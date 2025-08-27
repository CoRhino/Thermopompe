
# Documentation de Références - Système Thermopompe

## Vue d'Ensemble

Cette documentation regroupe toutes les références externes, spécifications techniques officielles, bibliothèques utilisées et ressources communautaires utiles pour le développement et la maintenance du système Thermopompe.

---

## Documentation des Composants Principaux

### Microcontrôleurs

#### ESP32
- **Documentation officielle Espressif** : https://docs.espressif.com/projects/esp-idf/en/latest/
- **Spécifications techniques** : https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf
- **Guide de démarrage Arduino** : https://docs.espressif.com/projects/arduino-esp32/en/latest/
- **Schémas de référence** : https://www.espressif.com/en/support/documents/technical-documents

#### ESP32-S3 (Version Avancée)
- **Datasheet ESP32-S3** : https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf
- **Guide technique** : https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/
- **Exemples de code** : https://github.com/espressif/esp-idf/tree/master/examples

#### ESP8266 (Alternative)
- **Documentation Arduino** : https://arduino-esp8266.readthedocs.io/en/latest/
- **Spécifications** : https://www.espressif.com/sites/default/files/documentation/0a-esp8266ex_datasheet_en.pdf

### Composants Infrarouge

#### LED Infrarouge TSAL6400
- **Datasheet TSAL6400** : https://www.vishay.com/docs/81010/tsal6400.pdf
- **Guide d'application** : https://www.vishay.com/docs/80071/80071.pdf
- **Calcul résistance de limitation** : https://www.digikey.com/en/resources/conversion-calculators/conversion-calculator-led-series-resistor

#### Récepteur IR TSOP4838
- **Datasheet TSOP4838** : https://www.vishay.com/docs/82459/tsop48.pdf
- **Guide d'utilisation** : https://www.vishay.com/docs/82662/tsop382.pdf
- **Schémas d'application** : https://www.vishay.com/docs/80071/80071.pdf

### Capteurs

#### DHT22 (AM2302)
- **Spécifications DHT22** : https://www.adafruit.com/datasheets/Digital%20humidity%20and%20temperature%20sensor%20AM2302.pdf
- **Guide d'utilisation** : https://learn.adafruit.com/dht-humidity-sensing-on-raspberry-pi-with-gdocs-logging
- **Bibliothèque Arduino** : https://github.com/adafruit/DHT-sensor-library

#### BME280 (Alternative Avancée)
- **Datasheet BME280** : https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme280-ds002.pdf
- **Guide API** : https://github.com/BoschSensortec/BME280_driver
- **Exemples Arduino** : https://github.com/adafruit/Adafruit_BME280_Library

#### SHT30/SHT40 (Sensirion)
- **Documentation Sensirion** : https://www.sensirion.com/sensors/
- **Datasheet SHT3x** : https://www.sensirion.com/fileadmin/user_upload/customers/sensirion/Dokumente/2_Humidity_Sensors/Datasheets/Sensirion_Humidity_Sensors_SHT3x_Datasheet_digital.pdf

---

## Spécifications Techniques Officielles

### Pompe à Chaleur Elios

#### Elios DE12HIW23230E3
- **Manuel utilisateur** : [Documentation constructeur - à obtenir du fabricant]
- **Spécifications techniques** : [Fiche technique officielle - contact Elios requis]
- **Codes IR documentés** : Voir [Guide matériel](../guide-technique/guide-materiel.md)
- **Support technique Elios** : [Contact constructeur - informations locales requises]

### Protocoles de Communication

#### Protocole NEC (Infrarouge)
- **Spécification NEC** : https://www.sbprojects.net/knowledge/ir/nec.php
- **Documentation technique** : https://techdocs.altium.com/display/FPGA/NEC+Infrared+Transmission+Protocol
- **Exemples d'implémentation** : https://github.com/Arduino-IRremote/Arduino-IRremote/blob/master/src/ir_NEC.hpp

#### WiFi 802.11
- **Standard IEEE 802.11** : https://standards.ieee.org/standard/802_11-2020.html
- **WPA3 Security** : https://www.wi-fi.org/discover-wi-fi/security
- **Guide sécurité WiFi** : https://www.cisa.gov/wifi-security-considerations

---

## Bibliothèques et Frameworks

### Bibliothèques Arduino Essentielles

#### IRremoteESP8266
- **Repository GitHub** : https://github.com/crankyoldgit/IRremoteESP8266
- **Documentation complète** : https://crankyoldgit.github.io/IRremoteESP8266/doxygen/html/
- **Exemples d'utilisation** : https://github.com/crankyoldgit/IRremoteESP8266/tree/master/examples
- **Protocoles supportés** : https://github.com/crankyoldgit/IRremoteESP8266/blob/master/SupportedProtocols.md

#### WiFiManager
- **Repository GitHub** : https://github.com/tzapu/WiFiManager
- **Documentation** : https://github.com/tzapu/WiFiManager/blob/master/README.md
- **Exemples de configuration** : https://github.com/tzapu/WiFiManager/tree/master/examples

#### ArduinoJson
- **Site officiel** : https://arduinojson.org/
- **Documentation API** : https://arduinojson.org/v6/api/
- **Assistant de configuration** : https://arduinojson.org/v6/assistant/
- **Guide performance** : https://arduinojson.org/v6/how-to/improve-performance/

#### AsyncWebServer (ESP32)
- **Repository GitHub** : https://github.com/me-no-dev/ESPAsyncWebServer
- **Documentation** : https://github.com/me-no-dev/ESPAsyncWebServer/blob/master/README.md
- **Exemples** : https://github.com/me-no-dev/ESPAsyncWebServer/tree/master/examples

### Bibliothèques Complémentaires

#### PubSubClient (MQTT)
- **Repository GitHub** : https://github.com/knolleary/pubsubclient
- **Documentation API** : https://pubsubclient.knolleary.net/api
- **Exemples MQTT** : https://github.com/knolleary/pubsubclient/tree/master/examples

#### Time/NTP
- **TimeLib Arduino** : https://github.com/PaulStoffregen/Time
- **NTP Client ESP32** : https://github.com/arduino-libraries/NTPClient
- **Documentation ESP32 NTP** : https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/system_time.html

---

## Standards et Normes

### Protocoles Réseau

#### HTTP/HTTPS
- **RFC 7231 (HTTP/1.1)** : https://tools.ietf.org/html/rfc7231
- **RFC 8446 (TLS 1.3)** : https://tools.ietf.org/html/rfc8446
- **Guide sécurité HTTPS** : https://developer.mozilla.org/en-US/docs/Web/Security/Transport_Layer_Security

#### REST API
- **Guide REST API** : https://restfulapi.net/
- **OpenAPI Specification** : https://swagger.io/specification/
- **HTTP Status Codes** : https://httpstatuses.com/

#### WebSocket
- **RFC 6455 (WebSocket)** : https://tools.ietf.org/html/rfc6455
- **Guide WebSocket** : https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API

#### JSON Web Tokens (JWT)
- **RFC 7519 (JWT)** : https://tools.ietf.org/html/rfc7519
- **JWT.io (outils)** : https://jwt.io/
- **Guide sécurité JWT** : https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/

### Protocoles IoT

#### MQTT
- **Spécification MQTT 5.0** : https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html
- **MQTT.org** : https://mqtt.org/
- **Guide Eclipse Mosquitto** : https://mosquitto.org/documentation/

#### Matter/Thread
- **Connectivity Standards Alliance** : https://csa-iot.org/all-solutions/matter/
- **Thread Group** : https://www.threadgroup.org/
- **Spécification Matter** : https://github.com/project-chip/connectedhomeip

---

## Ressources Communautaires

### Forums et Communautés

#### ESP32/Arduino
- **Forum ESP32** : https://esp32.com/
- **Arduino Forum** : https://forum.arduino.cc/
- **Reddit r/esp32** : https://www.reddit.com/r/esp32/
- **ESP32 Discord** : https://discord.gg/esp32

#### Domotique
- **Forum Home Assistant** : https://community.home-assistant.io/
- **OpenHAB Community** : https://community.openhab.org/
- **Jeedom Community** : https://community.jeedom.com/

#### Électronique
- **Forum EEVblog** : https://www.eevblog.com/forum/
- **Reddit r/electronics** : https://www.reddit.com/r/electronics/
- **All About Circuits** : https://forum.allaboutcircuits.com/

### Ressources d'Apprentissage

#### Tutoriels ESP32
- **Random Nerd Tutorials** : https://randomnerdtutorials.com/projects-esp32/
- **DroneBot Workshop** : https://dronebotworkshop.com/esp32-intro/
- **ESP32.com Tutorials** : https://esp32tutorials.com/

#### Guides Électronique
- **SparkFun Learn** : https://learn.sparkfun.com/
- **Adafruit Learn** : https://learn.adafruit.com/
- **Circuit Basics** : https://www.circuitbasics.com/

#### Cours en Ligne
- **Coursera IoT** : https://www.coursera.org/specializations/iot
- **edX Embedded Systems** : https://www.edx.org/learn/embedded-systems
- **Udemy ESP32** : https://www.udemy.com/topic/esp32/

---

## Outils de Développement

### Environnements de Développement

#### Arduino IDE
- **Téléchargement** : https://www.arduino.cc/en/software
- **Guide installation ESP32** : https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html
- **Gestionnaire de cartes** : https://github.com/espressif/arduino-esp32

#### PlatformIO
- **Site officiel** : https://platformio.org/
- **Documentation** : https://docs.platformio.org/en/latest/
- **Support ESP32** : https://docs.platformio.org/en/latest/platforms/espressif32.html

#### ESP-IDF (Avancé)
- **Documentation ESP-IDF** : https://docs.espressif.com/projects/esp-idf/en/latest/
- **Guide démarrage** : https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/
- **Exemples** : https://github.com/espressif/esp-idf/tree/master/examples

### Outils de Test et Debug

#### Analyseurs Protocol
- **Wireshark** : https://www.wireshark.org/
- **MQTT Explorer** : https://mqtt-explorer.com/
- **Postman** : https://www.postman.com/

#### Outils Matériel
- **Logic Analyzer** : https://www.saleae.com/
- **Oscilloscope virtuel** : https://www.velleman.eu/products/view/?id=435102
- **Multimètre** : Fluke, Keysight (recommandations professionnelles)

---

## Fournisseurs et Sources d'Approvisionnement

### Distributeurs Électroniques

#### Internationaux
- **Mouser Electronics** : https://www.mouser.com/
- **DigiKey** : https://www.digikey.com/
- **Farnell** : https://www.farnell.com/
- **RS Components** : https://www.rs-online.com/

#### Spécialisés ESP32
- **Espressif Store** : https://www.espressif.com/en/products/devkits
- **Adafruit** : https://www.adafruit.com/category/946
- **SparkFun** : https://www.sparkfun.com/categories/tags/esp32

#### Budget/Import
- **AliExpress** : https://www.aliexpress.com/
- **Banggood** : https://www.banggood.com/
- **LCSC** : https://www.lcsc.com/

### Services PCB

#### Fabrication PCB
- **JLCPCB** : https://jlcpcb.com/
- **PCBWay** : https://www.pcbway.com/
- **Seeed Studio** : https://www.seeedstudio.com/fusion_pcb.html
- **Aisler** : https://aisler.net/ (Europe)

#### Assemblage SMT
- **JLCPCB Assembly** : https://jlcpcb.com/smt-assembly
- **PCBWay Assembly** : https://www.pcbway.com/setinvite.aspx?inviteid=374841

### Impression 3D et Boîtiers

#### Services d'Impression
- **Sculpteo** : https://www.sculpteo.com/
- **Shapeways** : https://www.shapeways.com/
- **Ponoko** : https://www.ponoko.com/

#### Modèles 3D Gratuits
- **Thingiverse** : https://www.thingiverse.com/
- **Printables** : https://www.printables.com/
- **MyMiniFactory** : https://www.myminifactory.com/

---

## Logiciels de Conception

### Conception Électronique

#### KiCad (Open Source)
- **Site officiel** : https://www.kicad.org/
- **Documentation** : https://docs.kicad.org/
- **Bibliothèques communautaires** : https://github.com/KiCad

#### Altium Designer (Professionnel)