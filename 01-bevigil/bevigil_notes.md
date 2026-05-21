# Notes d'analyse BeVigil

## Ce qui est certain
- Nom du package : com.android.insecurebankv2
- Nom de l'application : InsecureBankv2
- Version : 2.0
- Note de sécurité : 7.4 (Average)
- Total des issues détectées : 5
  - Stockage d'informations sensibles dans Shared Preferences : 91%
  - Informations sensibles dans les logs : 7.2%
  - Activité exportée : 1%
  - Secret possible détecté : 0.5%
  - Client HTTP non sécurisé utilisé : 0.3%
- Permissions : 2 Safe, 8 Risky, 0 Dangerous
- Trackers détectés : Google AdMob, Google Analytics, Google Tag Manager
- Bibliothèques tierces : Android Support Library, Google Ads, Google Mobile Services, Google Fit, Google Maps API, etc.

## Ce qui est hypothèse
- L'utilisation de bibliothèques tierces pourrait exposer des vulnérabilités indirectes.
- Les secrets détectés dans es/values/strings.xml pourraient être exploitables si mal protégés.

## Points d'intérêt
- Exported activity dans esources/AndroidManifest.xml (LOW)
- Weak crypto algorithms dans sources/com/google/android/gms/internal/zzbl.java (LOW)
- Possible secret dans esources/res/values/strings.xml (LOW)
- Chemins de fichiers sensibles dans sources/com/android/insecurebankv2/ChangePassword.java (LOW)

## Domaines et sous-domaines
- Mobilité / Applications Android
  - Gestion des informations sensibles
  - Communication réseau

## Endpoints et APIs
- Google Maps API
- Google Fit API
- Google Play Services Drive API
- Google Tag Manager

## URLs HTTP/HTTPS
- Aucune URL spécifique listée dans le rapport, mais l'application utilise des services Google via HTTPS

## Emails et identifiants
- Aucun email explicite détecté
- Possibles secrets dans strings.xml

## Technologies détectées
- Android SDK
- Android Support Library (v4, v7)
- Google Mobile Services
- Google Ads / Analytics / Tag Manager
