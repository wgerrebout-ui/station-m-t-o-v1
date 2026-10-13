# Station météo — APK Android

Projet Android minimaliste pour afficher les mesures météo d'un ESP32 ou d'un Raspberry Pi sur le même réseau local. La compilation de l'APK est automatisée par GitHub Actions.

## État du projet

L'application Android est prête comme interface de départ. Elle appelle `http://ADRESSE_LOCALE/api/weather` et attend une réponse JSON. **Il faut que le Raspberry Pi ou l'ESP32 fournisse cette API** ; ce dépôt ne configure pas encore le serveur de la station.

## Format de réponse attendu

Exemple :

```json
{
  "temperature_c": 21.7,
  "humidity_pct": 54.2,
  "pressure_hpa": 1013.4,
  "updated_at": "2026-10-13 18:45:00"
}
```

Les clés alternatives `temp_c`, `temperature`, `humidity`, `humidite`, `pressure` et `pression` sont également acceptées. `updated_at` ou `timestamp` sont facultatifs.

## Construire l'APK avec GitHub sur téléphone

1. Crée un dépôt GitHub public ou privé nommé `station-meteo`.
2. Téléverse les fichiers de ce projet en conservant leur arborescence, notamment le dossier `.github/workflows`.
3. Ouvre l'onglet **Actions** du dépôt et active les workflows si GitHub le demande.
4. Lance **Construire l'APK Android** avec **Run workflow**, ou attends la compilation déclenchée par l'envoi des fichiers sur `main`.
5. Ouvre l'exécution terminée avec une coche verte, descends jusqu'à **Artifacts**, puis télécharge `station-meteo-apk`.
6. Décompresse l'archive téléchargée : elle contient `app-debug.apk`. Ouvre le fichier sur le téléphone et autorise, si Android le demande, l'installation depuis cette source.

## Utiliser l'application

1. Connecte le téléphone au même réseau Wi-Fi que la station.
2. Trouve l'adresse IP locale du Raspberry Pi ou de l'ESP32 dans l'interface du routeur ou dans la configuration réseau de l'appareil.
3. Dans l'application, saisis par exemple `http://192.168.1.50` (remplace cette adresse par la vraie IP), puis touche **Actualiser les mesures**.
4. Le serveur doit répondre à `http://192.168.1.50/api/weather` avec le JSON ci-dessus.

## Réseau et sécurité

- L'application est conçue pour un accès local, mais le réseau local doit être correctement isolé et sécurisé.
- L'application accepte HTTP pour simplifier la première mise en service. Ne publie pas cette API sur Internet et ne redirige pas son port sur le routeur.
- L'IP d'exemple est fictive : elle doit être remplacée par l'adresse réelle de la station.

## Compiler localement (facultatif)

Avec Android Studio, ouvre ce dossier comme projet Gradle et lance la tâche `:app:assembleDebug` avec JDK 17 et Gradle 8.9.
