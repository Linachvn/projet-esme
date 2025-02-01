# projet-esme - Lina CHAUVIN
Projet individuel

Producer :
Le producteur Kafka écrit des données météorologiques depuis l'API OpenWeatherMap pour cinq villes, puis les envoie à un topic Kafka.

1. Configuration
On définit une clé API pour OpenWeatherMap.
On liste cinq villes dont on souhaite récupérer les données météo.
On spécifie le topic Kafka (topic-weather) où les données seront envoyées.
On définit l'adresse du serveur Kafka, indiquant que Kafka tourne en local.

2. Initialisation du producteur Kafka 
Notre producer kafka:
Se connecte au serveur Kafka spécifié.
Il utilise une fonction de sérialisation qui transforme les données en JSON avant de les envoyer.

3. Fonction de récupération des données météo
get_weather_data(city) :
Construit l'URL d'appel API pour obtenir la météo d'une ville.
Envoie une requête HTTP GET à l'API.
Retourne les données météo au format JSON.

4. Boucle d'envoi des données vers Kafka 
Le code tourne en continu, pour chaque ville dans CITIES :
Il appelle get_weather_data(city) pour obtenir la météo.
Si les données sont valides, il les envoie au topic Kafka.
Les données peuvent être consommer par un kafka consumer pour analyser ou afficher les données en temps réel.

Consumer : 
Le consumer Kafka permet d'ingérer, traiter et republier des données météo en temps réel.

Il lit des messages depuis le topic Kafka contenant des données météo issues de l'API OpenWeather.
Il parse et transforme ces données avec PySpark.
Il ajoute des indicateurs météo.
Il réécrit les données enrichies dans un nouveau topic Kafka.
Il affiche aussi les résultats en temps réel dans la console.

On commence par définir le chemin vers Spark et crée une session Spark. Puis on définit la structure des messages JSON de Kafka pour faciliter le parsing, elle contient des champs comme température, pression, humidité, vitesse du vent. On se connecte à Kafka et au topic "topic-weather". Cela permet de lire les messages en continu, ensuite on décode les messages JSON pour extrait leur contenu.
Cela extrait la ville, le pays, la température, la pression, l'humidité, vent.


heat_index : Calcule un indice de chaleur en fonction de la température et de l'humidité pour estimer la sensation thermique.

severity_index : Évalue la sévérité des conditions météorologiques en combinant la vitesse du vent, la pression atmosphérique et l'humidité.

time_of_day : Catégorise l'heure de la journée en matin, après-midi, soirée ou nuit en fonction de l'horaire du timestamp.

Ensuite, on ransforme les données en JSON pour les renvoyer vers Kafka. On écrit les données traitées dans le topic Kafka final "topic-weather-final"

Le projet vise à développer un pipeline de traitement des données météorologiques en temps réel en combinant Kafka et Spark. L’objectif est de collecter des données depuis l’API OpenWeatherMap, de les stocker dans un topic nommé topic-weather, puis de les traiter avec Spark pour enrichir les informations en générant de nouvelles variables comme un indice de chaleur et un indice de sévérité météorologique. Les données transformées sont ensuite envoyées vers un second topic topic-weather-final. Ce projet permet le suivi en temps réel de la météo.



