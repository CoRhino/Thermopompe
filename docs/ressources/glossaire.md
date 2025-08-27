
# Glossaire Technique - Système Thermopompe

## Vue d'Ensemble

Ce glossaire définit la terminologie technique utilisée dans la documentation du système de contrôle domotique pour pompe à chaleur. Les termes sont organisés par ordre alphabétique avec leurs équivalents anglais quand pertinents.

---

## A

### API (Application Programming Interface)
**Définition** : Interface de programmation permettant l'interaction entre différents logiciels ou composants système.
**Contexte** : L'API REST du système Thermopompe expose des endpoints pour contrôler la pompe via HTTP.
**Voir aussi** : [Référence API](../guide-technique/api-reference.md)

### Authentification JWT
**Définition** : Méthode d'authentification utilisant des tokens JSON Web Tokens pour sécuriser l'accès aux ressources.
**Usage** : Tous les appels à l'API nécessitent un token JWT valide dans le header Authorization.
**Expiration** : Les tokens ont une durée de vie limitée (défaut: 1 heure).

### Automatisation
**Définition** : Programmation de tâches répétitives basées sur des conditions temporelles ou environnementales.
**Exemples** : Démarrage automatique à 7h00, arrêt si température > 25°C.

---

## B

### Breadboard
**Définition** : Plaque d'essai permettant de prototyper des circuits électroniques sans soudure.
**Usage** : Utilisée pour les tests initiaux avant assemblage final sur PCB.
**Spécifications** : 830 points recommandé pour ce projet.

### Broker MQTT
**Définition** : Serveur intermédiaire qui gère les messages MQTT entre clients.
**Contexte** : Utilisé pour l'intégration domotique avec Home Assistant ou autres systèmes.
**Port standard** : 1883 (non sécurisé), 8883 (TLS).

---

## C

### Capteur DHT22
**Définition** : Capteur numérique de température et d'humidité avec communication série.
**Spécifications** : Précision ±0.5°C (température), ±2-5% (humidité).
**Interface** : Communication sur une seule broche de données avec protocole propriétaire.

### Code IR (Infrarouge)
**Définition** : Séquence de signaux modulés transmise par infrarouge pour contrôler un appareil.
**Format** : Valeurs hexadécimales (ex: 0x02FD08F7 pour Power ON).
**Fréquence** : 38kHz pour la pompe Elios DE12HIW23230E3.

### COP (Coefficient de Performance)
**Définition** : Rapport entre l'énergie thermique produite et l'énergie électrique consommée par une pompe à chaleur.
**Calcul** : COP = Énergie thermique / Énergie électrique.
**Valeur typique** : 3-5 pour une pompe à chaleur moderne.

---

## D

### Domotique
**Définition** : Ensemble des technologies permettant d'automatiser et de contrôler les équipements d'un habitat.
**Synonyme** : Maison intelligente, Smart Home.
**Protocoles** : WiFi, Zigbee, Z-Wave, Matter/Thread.

### Driver IR
**Définition** : Circuit électronique amplifiant le signal de commande pour augmenter la portée de l'émetteur infrarouge.
**Composant** : Transistor 2N2222 ou équivalent.
**Gain** : Permet d'étendre la portée de 3-5m à 8-12m.

---

## E

### Elios DE12HIW23230E3
**Définition** : Modèle spécifique de pompe à chaleur pour lequel ce système est conçu.
**Fabricant** : Elios.
**Protocole IR** : Compatible 38kHz avec codes spécifiques documentés.

### Endpoint
**Définition** : Point d'accès spécifique d'une API REST identifié par une URL.
**Exemples** : `/api/v1/status`, `/api/v1/temperature`.
**Méthodes** : GET (lecture), POST (création), PUT (modification), DELETE (suppression).

### ESP32
**Définition** : Microcontrôleur avec WiFi et Bluetooth intégrés, développé par Espressif.
**Architecture** : Dual-core Xtensa LX6 @ 240MHz.
**Mémoire** : 520KB SRAM, 4MB Flash (typique).
**Avantages** : Faible coût, faible consommation, large communauté.

---

## F

### Firmware
**Définition** : Logiciel de bas niveau contrôlant directement le matériel du microcontrôleur.
**Langage** : C++ avec framework Arduino pour ce projet.
**Mise à jour** : Over-The-Air (OTA) ou via port série.

### Fréquence Porteuse
**Définition** : Fréquence de modulation du signal infrarouge.
**Standard** : 38kHz pour la plupart des télécommandes.
**Fonction** : Permet de distinguer le signal utile du bruit ambiant.

---

## G

### GPIO (General Purpose Input/Output)
**Définition** : Broches configurables du microcontrôleur pour interfacer avec des composants externes.
**ESP32** : 34 broches GPIO disponibles.
**Configuration** : Entrée, sortie, PWM, ADC, communication série.

### GraphQL
**Définition** : Langage de requête et runtime pour APIs, alternative à REST.
**Avantages** : Requêtes flexibles, réduction des appels réseau.
**Évolution** : Prévu pour la version 2.0.0 du système.

---

## H

### Home Assistant
**Définition** : Plateforme domotique open-source permettant de centraliser le contrôle d'équipements connectés.
**Intégration** : Via API REST ou MQTT.
**Configuration** : Fichier YAML avec sensors et rest_commands.

### HTTPS (HyperText Transfer Protocol Secure)
**Définition** : Version sécurisée du protocole HTTP utilisant TLS/SSL.
**Certificat** : Auto-signé ou Let's Encrypt pour ce projet.
**Port** : 443 (standard), configurable.

---

## I

### IoT (Internet of Things)
**Définition** : Réseau d'objets physiques connectés échangeant des données via Internet.
**Traduction** : Internet des Objets.
**Contexte** : Le système Thermopompe est un dispositif IoT.

### IR (Infrarouge)
**Définition** : Rayonnement électromagnétique de longueur d'onde supérieure à la lumière visible.
**Usage** : Communication sans fil avec la pompe à chaleur.
**Longueur d'onde** : 850nm ou 940nm pour les LEDs utilisées.

---

## J

### JSON (JavaScript Object Notation)
**Définition** : Format d'échange de données léger et lisible par l'humain.
**Usage** : Format de réponse de l'API REST.
**Structure** : Paires clé-valeur organisées en objets et tableaux.

### JWT (JSON Web Token)
**Définition** : Standard ouvert pour transmettre des informations de manière sécurisée.
**Structure** : Header.Payload.Signature encodés en Base64.
**Usage** : Authentification et autorisation dans l'API.

---

## L

### LED IR (Light Emitting Diode Infrarouge)
**Définition** : Diode électroluminescente émettant de la lumière infrarouge.
**Modèles** : TSAL6400 (haute puissance), TSAL6200 (usage général).
**Caractéristiques** : Courant direct 100mA max, angle 20-60°.

### Logs
**Définition** : Fichiers d'enregistrement des événements système pour diagnostic et audit.
**Types** : Logs système, logs commandes, logs erreurs.
**Rétention** : 500-1000 entrées en mémoire locale.

---

## M

### MCU (Microcontroller Unit)
**Définition** : Circuit intégré comprenant processeur, mémoire et périphériques.
**Synonyme** : Microcontrôleur.
**Exemple** : ESP32, ESP8266, Arduino.

### MQTT (Message Queuing Telemetry Transport)
**Définition** : Protocole de messagerie léger pour l'IoT basé sur publish/subscribe.
**Port** : 1883 (standard), 8883 (sécurisé).
**QoS** : Quality of Service avec niveaux 0, 1, 2.

---

## N

### NEC (Protocole IR)
**Définition** : Protocole de communication infrarouge développé par NEC Corporation.
**Caractéristiques** : 32 bits de données, fréquence porteuse 38kHz.
**Usage** : Protocole standard pour télécommandes, utilisé par Elios.

### NTP (Network Time Protocol)
**Définition** : Protocole de synchronisation d'horloge sur réseau.
**Usage** : Synchronisation de l'heure système pour programmations précises.
**Serveurs** : pool.ntp.org (public), serveurs locaux possible.

---

## O

### OTA (Over-The-Air)
**Définition** : Mise à jour du firmware via réseau sans connexion physique.
**Avantages** : Maintenance à distance, pas de démontage requis.
**Sécurité** : Authentification et vérification d'intégrité nécessaires.

---

## P

### PCB (Printed Circuit Board)
**Définition** : Circuit imprimé supportant et connectant électriquement les composants.
**Traduction** : Carte de circuit imprimé.
**Évolution** : De breadboard (prototype) vers PCB (version finale).

### Pompe à Chaleur
**Définition** : Système thermodynamique transférant la chaleur d'un milieu froid vers un milieu chaud.
**Types** : Air-air, air-eau, géothermique.
**Avantages** : Efficacité énergétique élevée (COP 3-5).

### Pull-up (Résistance)
**Définition** : Résistance connectée entre une entrée logique et l'alimentation positive.
**Valeur** : 10kΩ typique pour DHT22.
**Fonction** : Assure un état logique stable quand l'entrée n'est pas active.

---

## R

### Rate Limiting
**Définition** : Limitation du nombre de requêtes par unité de temps pour protéger l'API.
**Exemple** : 100 requêtes/minute maximum.
**Réponse** : Code HTTP 429 "Too Many Requests" si dépassé.

### REST (Representational State Transfer)
**Définition** : Style d'architecture pour services web utilisant HTTP.
**Principes** : Stateless, cacheable, interface uniforme.
**Méthodes** : GET, POST, PUT, DELETE, PATCH.

---

## S

### SoC (System on Chip)
**Définition** : Circuit intégré contenant tous les composants d'un système informatique.
**Exemple** : ESP32 (processeur + WiFi + Bluetooth + mémoire).
**Avantages** : Compacité, faible consommation, coût réduit.

### SSID (Service Set Identifier)
**Définition** : Nom d'identification d'un réseau WiFi.
**Longueur** : 0-32 caractères.
**Visibilité** : Peut être masqué pour la sécurité.

---

## T

### Thermopompe
**Définition** : Terme québécois pour pompe à chaleur.
**Usage** : Nom du projet pour respecter la localisation.
**Équivalent** : Heat pump (anglais), pompe à chaleur (français standard).

### Timer
**Définition** : Fonction de programmation temporelle pour actions automatiques.
**Modes** : Timer ON (démarrage différé), Timer OFF (arrêt différé).
**Durée** : 30 minutes à 24 heures selon modèle.

### TSOP4838
**Définition** : Récepteur infrarouge intégré avec démodulation 38kHz.
**Fonction** : Capture et décode les signaux IR pour apprentissage des codes.
**Alimentation** : 3.3V ou 5V.

---

## U

### UUID (Universally Unique Identifier)
**Définition** : Identifiant unique de 128 bits.
**Usage** : Traçabilité des requêtes API (request_id).
**Format** : 8-4-4-4-12 caractères hexadécimaux.

---

## V

### VCC/VDD
**Définition** : Tension d'alimentation positive d'un circuit.
**Valeurs** : 3.3V (logique), 5V (alimentation générale).
**Symbole** : VCC (Voltage Common Collector), VDD (Voltage Drain Drain).

---

## W

### WebSocket
**Définition** : Protocole de communication bidirectionnelle en temps réel sur TCP.
**Usage** : Mises à jour live de l'interface utilisateur.
**Avantages** : Faible latence, communication full-duplex.

### WiFi 2.4GHz
**Définition** : Bande de fréquence radio utilisée pour réseaux sans fil.
**Avantages** : Portée étendue, pénétration des obstacles.
**Inconvénients** : Débit limité, interférences possibles.

### WPA3 (WiFi Protected Access 3)
**Définition** : Standard de sécurité WiFi le plus récent.
**Amélioration** : Chiffrement renforcé par rapport à WPA2.
**Rétrocompatibilité** : Support WPA2 maintenu.

---

## Symboles et Unités

### °C (Degré Celsius)
**Définition** : Unité de température du système métrique.
**Conversion** : °F = (°C × 9/5) + 32.
**Plage système** : 16-30°C pour la configuration pompe.

### dBm (Décibel-milliwatt)
**Définition** : Unité de mesure de puissance exprimée en décibels.
**Usage** : Force du signal WiFi.
**Interprétation** : -30dBm (excellent), -70dBm (acceptable), -90dBm (faible).

### mA (Milliampère)
**Définition** : Unité de mesure du courant électrique (1/1000 ampère).
**Usage** : Consommation des composants électroniques.
**Exemples** : LED IR (100mA), ESP32 veille (10-50mA).

### MHz (Mégahertz)
**Définition** : Unité de fréquence équivalant à un million de hertz.
**Usage** : Fréquence processeur (ESP32: 240MHz), fréquence radio (WiFi: 2400MHz).

### V (Volt)
**Définition** : Unité de tension électrique.
**Valeurs courantes** : 3.3V (logique numérique), 5V (alimentation), 230V (