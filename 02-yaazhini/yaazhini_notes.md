# Notes d'analyse Yaazhini

## Éléments identifiés

### Élément 1: Insecure communication [High]
- **Localisation**: Findings 1 - com\android\insecurebankv2\ (ChangePassword.java:53, DoLogin.java:42, DoTransfer.java:56)
- **Description**: Utilisation du protocole HTTP non chiffré en clair dans le code source pour les fonctionnalités critiques (connexion, transfert, changement de mot de passe).
- **Impact potentiel**: Risque élevé d'interception et de vol de données sensibles (identifiants, mots de passe, transactions) via des attaques de type Man-in-the-Middle (MitM).
- **Remédiation suggérée**: Remplacer toutes les instances de "http://" par "https://" et forcer l'utilisation de protocoles SSL/TLS à jour.

### Élément 2: Android debuggable enabled [Medium]
- **Localisation**: Findings 3 - AndroidManifest.xml (Ligne 17)
- **Description**: L'attribut ndroid:debuggable="true" est activé dans la configuration de l'application.
- **Impact potentiel**: Permet à un attaquant ou à un utilisateur malveillant de connecter un débogueur à l'application, d'analyser son comportement à l'exécution et d'extraire des données en mémoire.
- **Remédiation suggérée**: Modifier la propriété pour passer à ndroid:debuggable="false" dans le fichier AndroidManifest.xml avant la mise en production.

### Élément 3: Android backup vulnerability [Medium]
- **Localisation**: Findings 4 - AndroidManifest.xml (Ligne 17)
- **Description**: L'option de sauvegarde est activée via ndroid:allowBackup="true".
- **Impact potentiel**: Un utilisateur ou un attaquant ayant un accès physique au terminal (via ADB) peut exporter et cloner l'intégralité des données privées de l'application stockées dans /data/data/....
- **Remédiation suggérée**: Désactiver la sauvegarde en configurant ndroid:allowBackup="false" dans le manifeste.

### Élément 4: Improper export of providers & receivers [Medium]
- **Localisation**: Findings 5 & 6 - AndroidManifest.xml (Lignes 30 et 31)
- **Description**: Le ContentProvider (TrackUserContentProvider) et le BroadcastReceiver (MyBroadCastReceiver) possèdent l'attribut ndroid:exported="true" sans restrictions.
- **Impact potentiel**: Des applications tierces malveillantes installées sur le même appareil peuvent intercepter les diffusions système ou interroger/modifier directement la base de données de l'application.
- **Remédiation suggérée**: Restreindre l'accès en passant l'attribut à ndroid:exported="false" ou définir des permissions personnalisées strictes si le partage inter-applicatif est requis.

### Élément 5: Android external storage [Warning]
- **Localisation**: Findings 11 - com\android\insecurebankv2\ (DoTransfer.java:174, ViewStatement.java:22)
- **Description**: Écriture et lecture de fichiers de relevés bancaires (Statements_*.html) sur le stockage externe (carte SD / espace partagé).
- **Impact potentiel**: Fuite d'informations financières critiques. Les fichiers sur le stockage externe sont globalement lisibles et modifiables par n'importe quelle autre application disposant des droits de lecture de stockage.
- **Remédiation suggérée**: Migrer le stockage de ces fichiers sensibles vers le stockage interne privé de l'application (Context.getFilesDir()) et chiffrer les données au repos si le stockage externe est indispensable.
