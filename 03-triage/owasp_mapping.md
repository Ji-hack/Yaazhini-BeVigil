# Mapping OWASP MASVS

## VULN-001: Insecure Communication (HTTP Cleartext Traffic)
- **Catégorie OWASP**: MASVS-NETWORK
- **Référence spécifique**: V5.1
- **Justification**: Toutes les connexions réseau sortantes transportant des données d'authentification ou des transactions financières doivent obligatoirement utiliser TLS pour empêcher les interceptions en clair.

## VULN-002: Android Debuggable Enabled
- **Catégorie OWASP**: MASVS-CODE
- **Référence spécifique**: V7.1
- **Justification**: Le mode débogage actif en production permet l'injection de code et la manipulation de variables à l'exécution par un processus tiers ou via l'interface ADB.

## VULN-003: Android Backup Vulnerability
- **Catégorie OWASP**: MASVS-STORAGE
- **Référence spécifique**: V2.8
- **Justification**: L'activation des sauvegardes automatiques système expose l'ensemble du répertoire privé de l'application à une extraction en clair via les outils de sauvegarde système standard.

## VULN-004: Improper Export of Content Providers
- **Catégorie OWASP**: MASVS-PLATFORM
- **Référence spécifique**: V6.2
- **Justification**: L'exposition publique de Content Providers sans permissions adaptées permet à des applications malveillantes colocalisées de lire ou corrompre les données applicatives.

## VULN-005: Improper Export of Receivers & Activities
- **Catégorie OWASP**: MASVS-PLATFORM
- **Référence spécifique**: V6.2
- **Justification**: Les composants système exposés (Broadcast Receivers/Activities) peuvent recevoir des intents forgés par des tiers pour contourner le flux de contrôle normal.

## VULN-006: Insecure Cryptographic Algorithms (MD5 & SHA-1)
- **Catégorie OWASP**: MASVS-CRYPTO
- **Référence spécifique**: V3.2
- **Justification**: L'application emploie des fonctions de hachage obsolètes et brisées (MD5, SHA-1) qui sont vulnérables aux attaques par collision et au cassage d'empreintes.

## VULN-007: Insecure Cryptographic Signature (SHA1withRSA)
- **Catégorie OWASP**: MASVS-CRYPTO
- **Référence spécifique**: V3.2
- **Justification**: L'utilisation de signatures basées sur SHA-1 pour valider des transactions ou des achats in-app fragilise la vérification d'intégrité et l'authenticité des données.

## VULN-008: Use of Insufficiently Random Values
- **Catégorie OWASP**: MASVS-CRYPTO
- **Référence spécifique**: V3.6
- **Justification**: La classe java.util.Random génère des séquences pseudo-aléatoires prédictibles, ce qui compromet la sécurité des identifiants uniques ou des jetons de session.

## VULN-009: Insecure Storage in External Storage & Shared Preferences
- **Catégorie OWASP**: MASVS-STORAGE
- **Référence spécifique**: V2.1
- **Justification**: Le stockage d'informations financières sur l'espace externe partagé ou dans des préférences claires rend les données sensibles lisibles par n'importe quelle app du système.

## VULN-010: JavaScript Enabled in WebView
- **Catégorie OWASP**: MASVS-PLATFORM
- **Référence spécifique**: V6.5
- **Justification**: L'activation globale de JavaScript au sein d'une WebView chargeant des fichiers locaux potentiellement altérables introduit un risque d'exécution de code malveillant via XSS local.

## VULN-011: Possible Hardcoded Secrets inside configuration files
- **Catégorie OWASP**: MASVS-STORAGE
- **Référence spécifique**: V2.1
- **Justification**: L'inscription de jetons et clés secrètes statiques au sein de fichiers de ressources comme strings.xml permet leur extraction immédiate par simple décompilation.

## VULN-012: Sensitive Information Leakage via Application Logs
- **Catégorie OWASP**: MASVS-STORAGE
- **Référence spécifique**: V2.3
- **Justification**: La présence d'informations sensibles écrites dans les journaux système standard (Logcat) expose ces métadonnées à des fuites vers des outils de surveillance tiers.

## VULN-013: Missing Copy-Paste Clipboard Protection from EditText Fields
- **Catégorie OWASP**: MASVS-STORAGE
- **Référence spécifique**: V2.11
- **Justification**: L'absence de restriction du presse-papiers permet la mise en mémoire tampon globale de secrets de l'utilisateur (mots de passe, codes), lisibles par le reste du système.

## VULN-014: Missing Protection Against Screenshots
- **Catégorie OWASP**: MASVS-PLATFORM
- **Référence spécifique**: V6.11
- **Justification**: Le manque de restriction d'arrière-plan ou de capture d'écran permet de capturer visuellement les données financières sensibles à l'écran de l'appareil.

