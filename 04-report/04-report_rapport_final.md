# Rapport d'analyse de sécurité mobile

## A. Informations générales
- **Date**: 2026-05-21
- **Analyste**: SecOps Auditor
- **Cible**: InsecureBank2 (com.android.insecurebankv2)
- **Version/Hash**: Version 2.0 / Android SDK Target 26
- **Outils utilisés**: BeVigil, Yaazhini v1.0.0

## B. Résumé exécutif
L'analyse de l'application InsecureBank2 révèle un niveau de risque global **Très Élevé (High)** avec un total de 14 vulnérabilités confirmées ou à auditer. Les failles critiques incluent l'utilisation de protocoles réseau non chiffrés pour l'authentification et les virements, ainsi qu'un stockage non sécurisé d'informations bancaires (relevés au format HTML) sur le stockage externe partagé de l'appareil. De plus, de graves défauts de configuration (mode débogage actif, composants exposés publiquement et sauvegardes système autorisées) permettent l'extraction locale intégrale ou la manipulation des données par des applications tierces malveillantes.

## C. Top 5 constats

### 1. Communication Réseau Non Sécurisée (HTTP Cleartext Traffic) - VULN-001
- **Sévérité**: High
- **Preuve**: `com/android/insecurebankv2/ChangePassword.java` (L53), `DoLogin.java` (L42), `DoTransfer.java` (L56) -> `String protocol = "http://";`
- **Impact**: Interception, lecture et modification malveillante de l'ensemble des flux d'authentification (mots de passe) et de transactions monétaires en clair par une attaque de type Man-in-the-Middle (MitM).
- **Remédiation**: Remplacer obligatoirement toutes les chaînes d'URL par le protocole sécurisé `https://` et implémenter un fichier `network_security_config.xml` configuré avec `cleartextTrafficPermitted="false"`.
- **Référence OWASP**: MASVS-NETWORK-1

### 2. Fuite de Données Financières sur le Stockage Externe Public - VULN-009
- **Sévérité**: Medium
- **Preuve**: `DoTransfer.java` (L174/190), `ViewStatement.java` (L22/26) -> `Environment.getExternalStorageDirectory() + "/Statements_..."`
- **Impact**: Les relevés de compte et d'opérations bancaires sont générés au format HTML sur l'espace de stockage externe partagé de l'appareil Android, les rendant lisibles, modifiables ou exfiltrables par n'importe quelle application tierce installée disposant des droits d'accès au stockage.
- **Remédiation**: Migrer l'enregistrement de ces données exclusivement vers le stockage interne privé de l'application via `Context.getFilesDir()`. Si le stockage externe est inévitable, chiffrer l'ensemble des fichiers au repos via l'API Android Jetpack Security.
- **Référence OWASP**: MASVS-STORAGE-1

### 3. Activation du Mode Débogage en Production - VULN-002
- **Sévérité**: Medium
- **Preuve**: `AndroidManifest.xml` (L17) -> `android:debuggable="true"`
- **Impact**: Permet à un utilisateur malveillant ou à un malware colocalisé de connecter un débogueur (comme jdb ou ADB) sur le processus actif de la banque, d'examiner/modifier la mémoire vive en temps réel et de contourner les contrôles de sécurité.
- **Remédiation**: Modifier impérativement la propriété pour passer à `android:debuggable="false"` dans le fichier manifeste final, ou automatiser sa désactivation via les variantes de build Gradle (build types de production).
- **Référence OWASP**: MASVS-CODE-1

### 4. Vulnérabilité de Sauvegarde Système des Données Privées - VULN-003
- **Sévérité**: Medium
- **Preuve**: `AndroidManifest.xml` (L17) -> `android:allowBackup="true"`
- **Impact**: Toute personne ayant un accès physique à l'appareil (ou par une interface USB de débogage ADB active) peut cloner, exporter et extraire l'intégralité du répertoire de données chiffrées/claires de la banque locale (`/data/data/com.android.insecurebankv2`).
- **Remédiation**: Désactiver explicitement la sauvegarde en configurant l'attribut à `android:allowBackup="false"` dans la balise `<application>` du fichier manifeste.
- **Référence OWASP**: MASVS-STORAGE-8

### 5. Exposition Publique Inappropriée des Composants Applicatifs - VULN-004 & VULN-005
- **Sévérité**: Medium
- **Preuve**: `AndroidManifest.xml` (L30/31) -> `<provider ... android:exported="true">` et `<receiver ... android:exported="true">`
- **Impact**: Le ContentProvider de suivi des utilisateurs et le BroadcastReceiver système sont ouverts à l'échelle globale du système. Des applications malveillantes tierces peuvent interroger de force la base de données interne ou injecter de faux événements (Intents falsifiés).
- **Remédiation**: Basculer l'attribut à `android:exported="false"` pour tous les composants à usage strictement interne ou implémenter un filtrage de signatures rigoureux par permissions d'accès.
- **Référence OWASP**: MASVS-PLATFORM-2

## D. Faux positifs notables
- **Occurrences HTTP dans les bibliothèques tierces (Analytics/GMS)**: Les résultats de Yaazhini (occurrences 4 à 15, 18 à 36) rapportent des URL HTTP pointant vers des schémas de métadonnées (ex: `http://schema.org/ActiveActionStatus`) ou des documentations obsolètes dans les SDK Google Analytics / Play Services. Bien qu'il s'agisse de liens en clair, ils ne transportent pas de données applicatives métiers ou d'identifiants utilisateurs. Ils sont classés en **Faux Positifs / Faible priorité** opérationnelle vis-à-vis du code source de la banque.

## E. Recommandations prioritaires
1. **Migration Réseau Immédiate (HTTPS)** : Remplacer l'ensemble des protocoles `http://` par `https://` dans l'ensemble des classes Java gérant l'authentification, le changement de mot de passe et l'exécution des virements.
2. **Durcissement et Clôture du Manifeste Android** : Corriger simultanément les attributs d'application dans le fichier `AndroidManifest.xml` en forçant `android:debuggable="false"`, `android:allowBackup="false"` et `android:exported="false"` pour sécuriser l'environnement d'exécution local.
3. **Sécurisation des Données au Repos (Stockage Interne)** : Supprimer l'usage de `Environment.getExternalStorageDirectory()` pour la génération des relevés HTML et migrer ces fichiers vers l'espace de stockage sandbox interne de l'application Android.

## F. Annexes
- [Lien vers exports BeVigil](../01-bevigil/bevigil_notes.md)
- [Lien vers rapport Yaazhini](../02-yaazhini/yaazhini_notes.md)
- [Lien vers triage.csv](../03-triage/triage.csv)
- [Lien vers owasp_mapping.md](../03-triage/owasp_mapping.md)
