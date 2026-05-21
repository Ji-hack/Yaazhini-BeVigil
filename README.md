# Lab: Audit de Sécurité Mobile — InsecureBank2

Ce laboratoire documente l'analyse de sécurité statique et dynamique de l'application Android **InsecureBank2** (version 2.0). L'objectif est d'identifier les vulnérabilités majeures, de les trier par sévérité et de les aligner avec le standard international **OWASP MASVS**.

## 📁 Structure du Projet

```text
├── 01-bevigil/          # Notes d'analyse et résultats issus de BeVigil
├── 02-yaazhini/         # Rapports d'identification de vulnérabilités (Yaazhini)
├── 03-triage/
│   ├── triage.csv       # Matrice de corrélation consolidée (14 vulnérabilités)
│   └── owasp_mapping.md # Correspondance détaillée avec les critères OWASP MASVS v2
└── 04-report/
    └── rapport_final.md # Rapport d'audit final avec Top 5 et recommandations
```
📊 Résumé des Constats Majeurs
L'application présente un niveau de risque global Très Élevé (High). Les 5 vulnérabilités critiques prioritaires identifiées sont :

VULN-001 (High — MASVS-NETWORK-1) : Utilisation de protocoles HTTP en clair pour l'authentification et les transferts de fonds.

VULN-009 (Medium — MASVS-STORAGE-1) : Stockage des relevés bancaires HTML sur l'espace externe partagé (carte SD) accessible par toutes les applications.

VULN-002 (Medium — MASVS-CODE-1) : Mode débogage actif (android:debuggable="true") en production.

VULN-003 (Medium — MASVS-STORAGE-8) : Autorisation des sauvegardes système via ADB permettant le clonage complet de la sandbox de l'application.

VULN-004/005 (Medium — MASVS-PLATFORM-2) : Exposition publique non restreinte de composants applicatifs (ContentProvider et BroadcastReceiver).

🛠️ Plan de Remédiation Prioritaire
Passer au chiffrement intégral (HTTPS) : Remplacer l'ensemble des endpoints http:// par https://.

Durcir le Manifeste Android : Configurer les attributs à false (debuggable, allowBackup, exported) dans le fichier AndroidManifest.xml.

Sécuriser le stockage des données au repos : Migrer les fichiers sensibles vers l'espace de stockage interne privé de l'application (Context.getFilesDir()).

Analyse réalisée le 21 Mai 2026.
