# Guide Matériel - Projet Thermopompe

## Vue d'Ensemble

Ce guide détaille tous les composants matériels nécessaires pour construire le système de contrôle infrarouge (IR) de la pompe à chaleur Elios. Le projet consiste à remplacer la télécommande traditionnelle par un module électronique connecté.

## Prérequis

- Connaissances de base en électronique
- Capacité de soudure (recommandée)
- Accès à un ordinateur pour la programmation
- Connexion WiFi domestique

## Composants Principaux

### 1. Microcontrôleur

#### Options Recommandées

**ESP32 (Recommandé)**
- **Modèle** : ESP32-DevKitC ou équivalent
- **Avantages** :
  - WiFi et Bluetooth intégrés
  - Processeur double cœur
  - Nombreuses GPIO (broches d'entrée/sortie)
  - Consommation énergétique optimisée
  - Large communauté de développement
- **Prix approximatif** : 10-15€

**ESP8266 (Alternative économique)**
- **Modèle** : NodeMCU ou Wemos D1 Mini
- **Avantages** :
  - WiFi intégré
  - Coût réduit
  - Compatible avec de nombreuses bibliothèques
- **Limitations** : Moins de GPIO, processeur simple cœur
- **Prix approximatif** : 5-10€

**Arduino (Non recommandé pour ce projet)**
- **Limitation** : Pas de connectivité WiFi native
- **Utilisation** : Uniquement si WiFi externe ajouté

**Raspberry Pi (Surdimensionné)**
- **Limitation** : Consommation énergétique élevée, complexité excessive
- **Utilisation** : Pas recommandé pour ce projet simple

### 2. Module Infrarouge (IR)

#### Émetteur IR
**LED Infrarouge**
- **Modèles recommandés** :
  - TSAL6400 (portée longue, haute puissance)
  - TSAL6200 (usage général)
  - SFH4550 (alternative fiable)
- **Spécifications** :
  - Longueur d'onde : 940nm ou 850nm
  - Angle de rayonnement : 20-60°
  - Courant direct maximum : 100mA
- **Prix** : 1-3€ par unité

#### Récepteur IR (pour capture des codes)
**Module Récepteur**
- **Modèles recommandés** :
  - TSOP4838 (38kHz, très populaire)
  - VS1838B (38kHz, économique)
  - TSOP4856 (56kHz, si nécessaire)
- **Spécifications** :
  - Fréquence de porteuse : 38kHz (standard)
  - Tension d'alimentation : 3.3V ou 5V
  - Portée : jusqu'à 8 mètres
- **Prix** : 2-5€

### 3. Alimentation

#### Options d'Alimentation
**Adaptateur secteur (Recommandé)**
- **Spécifications** : 5V DC, 1A minimum
- **Connecteur** : Micro-USB ou USB-C selon le microcontrôleur
- **Avantages** : Stable, fiable pour usage permanent

**Alimentation par pile**
- **Type** : 4x AA (6V) avec régulateur 3.3V
- **Utilisation** : Tests ou installation temporaire
- **Autonomie** : 1-3 mois selon l'usage

**Alimentation USB**
- **Source** : Port USB d'un autre appareil
- **Limitation** : Dépendant d'un autre équipement

### 4. Composants Électroniques

#### Résistances
- **220Ω à 1kΩ** : Limitation de courant pour LED IR
- **10kΩ** : Pull-up pour boutons (si utilisés)
- **Quantité** : Assortiment de résistances recommandé

#### Condensateurs
- **10µF à 100µF** : Stabilisation de l'alimentation
- **100nF** : Découplage haute fréquence
- **Type** : Électrolytiques et céramiques

#### Transistors (optionnel)
- **2N2222 ou équivalent** : Amplification du signal IR pour portée étendue
- **Utilisation** : Si la LED IR seule n'a pas assez de portée

### 5. Matériel de Prototypage

#### Breadboard
- **Type** : 830 points ou plus
- **Usage** : Tests et développement initial

#### Jumper Wires (Fils de connexion)
- **Types** : Mâle-mâle, mâle-femelle, femelle-femelle
- **Quantité** : Kit d'au moins 40 pièces

#### PCB (Circuit Imprimé) - Version finale
- **Option 1** : PCB universel pour soudure manuelle
- **Option 2** : PCB personnalisé (pour production en série)

### 6. Boîtier et Montage

#### Boîtier de Protection
- **Matériau** : Plastique ABS ou impression 3D
- **Exigences** :
  - Fenêtre transparente pour l'IR
  - Ventilation pour le microcontrôleur
  - Accès aux ports de programmation
- **Dimensions** : 80x60x25mm (approximatif)

#### Fixation
- **Vis** : M3 pour assemblage du boîtier
- **Support** : Adhésif double-face ou vis murales

## Capteurs Additionnels (Optionnels)

### Capteur de Température et Humidité
**DHT22 (AM2302)**
- **Précision** : ±0.5°C pour température, ±2-5% pour humidité
- **Interface** : Une seule broche de données
- **Prix** : 8-12€

**Alternative : BME280**
- **Avantages** : Température, humidité et pression
- **Interface** : I2C ou SPI
- **Précision** : Supérieure au DHT22

### Capteur de Luminosité
**LDR (Light Dependent Resistor)**
- **Usage** : Adaptation automatique selon l'éclairage ambiant
- **Prix** : 1-2€

## Outils Nécessaires

### Outils de Base
- **Fer à souder** : 25-40W avec panne fine
- **Soudure** : Diamètre 0.8-1mm, avec flux
- **Multimètre** : Pour vérifications électriques
- **Tournevis** : Jeu de tournevis de précision

### Outils Optionnels
- **Station de soudage** : Contrôle précis de la température
- **Loupe** : Pour soudures de précision
- **Aspirateur de soudure** : Correction d'erreurs

## Bibliothèques et Ressources Logicielles

### Bibliothèque IR pour ESP32/ESP8266
- **IRremoteESP8266** : https://github.com/crankyoldgit/IRremoteESP8266
- **Fonctionnalités** :
  - Support de nombreux protocoles IR
  - Capture et émission de codes
  - Compatible avec les pompes à chaleur

### Autres Bibliothèques Utiles
- **WiFiManager** : Configuration WiFi simplifiée
- **ArduinoJson** : Gestion des données JSON
- **AsyncWebServer** : Serveur web asynchrone

## Estimation des Coûts

### Configuration Minimale
- ESP32 : 12€
- LED IR + Récepteur : 5€
- Composants électroniques : 5€
- Breadboard et câbles : 8€
- **Total** : ~30€

### Configuration Complète
- ESP32 : 12€
- Modules IR : 8€
- Capteurs (température, humidité) : 12€
- PCB personnalisé : 15€
- Boîtier : 10€
- Composants et outils : 20€
- **Total** : ~77€

## Spécifications Techniques

### Consommation Énergétique
- **Mode veille** : 10-50mA
- **Mode transmission IR** : 100-200mA (pics courts)
- **Mode WiFi actif** : 80-160mA

### Portée IR
- **LED simple** : 3-5 mètres
- **LED avec transistor** : 8-12 mètres
- **Multiple LEDs** : 15+ mètres

### Conditions de Fonctionnement
- **Température** : -10°C à +60°C
- **Humidité** : 10% à 90% (sans condensation)
- **Alimentation** : 3.3V à 5V DC

## Dépannage Matériel

### Problèmes Courants

**LED IR ne fonctionne pas**
- Vérifier le sens de branchement (anode/cathode)
- Contrôler la résistance de limitation
- Tester avec une caméra de smartphone (la LED doit être visible)

**Portée insuffisante**
- Ajouter un transistor d'amplification
- Utiliser plusieurs LEDs en parallèle
- Vérifier l'alignement avec la pompe à chaleur

**Problèmes de connectivité WiFi**
- Vérifier la qualité du signal
- S'assurer de la compatibilité 2.4GHz
- Redémarrer le microcontrôleur

## Alternatives et Améliorations

### Modules Intégrés
- **ESP32-CAM** : Ajout de caméra pour surveillance
- **ESP32-S3** : Version plus puissante avec plus de mémoire

### Fonctionnalités Avancées
- **Écran OLED** : Affichage local des informations
- **Buzzer** : Notifications sonores
- **LED de statut** : Indication visuelle du fonctionnement

## Références

- [Documentation ESP32](https://docs.espressif.com/projects/esp-idf/en/latest/)
- [Bibliothèque IRremoteESP8266](https://github.com/crankyoldgit/IRremoteESP8266)
- [Guide de soudure pour débutants](https://learn.adafruit.com/adafruit-guide-excellent-soldering)
- [Calculateur de résistances](https://www.digikey.com/en/resources/conversion-calculators/conversion-calculator-led-series-resistor)

---



---

## Alternatives de Composants et Optimisations

### Alternatives Microcontrôleurs

#### ESP32 Variantes Avancées

**ESP32-S3 (Recommandé pour projets avancés)**
- **Avantages** :
  - Processeur dual-core Xtensa LX7 @ 240MHz
  - 512KB SRAM, jusqu'à 32MB PSRAM externe
  - Support natif USB-OTG
  - Meilleure gestion énergétique
  - Vector instructions pour traitement signal
- **Applications** : Projets nécessitant traitement audio/vidéo
- **Prix** : 15-20€

**ESP32-C3 (Alternative RISC-V)**
- **Avantages** :
  - Architecture RISC-V 32-bit @ 160MHz
  - WiFi 802.11b/g/n + Bluetooth 5.0 LE
  - Consommation réduite
  - Support cryptographie matérielle
- **Limitations** : Single-core, moins de GPIO
- **Prix** : 8-12€

#### Comparatif Performance Microcontrôleurs

| Modèle | CPU | RAM | Flash | WiFi | Bluetooth | Prix |
|--------|-----|-----|-------|------|-----------|------|
| ESP32 | Dual 240MHz | 520KB | 4MB | 802.11n | 4.2/LE | 12€ |
| ESP32-S3 | Dual 240MHz | 512KB | 8MB | 802.11n | 5.0/LE | 18€ |
| ESP32-C3 | Single 160MHz | 400KB | 4MB | 802.11n | 5.0/LE | 10€ |
| ESP8266 | Single 80MHz | 80KB | 4MB | 802.11n | Non | 6€ |

### Alternatives Capteurs de Température

#### Capteurs Haute Précision

**SHT30/SHT40 (Sensirion)**
- **Précision** : ±0.2°C température, ±2% humidité
- **Interface** : I2C
- **Avantages** :
  - Excellente stabilité long terme
  - Calibration d'usine précise
  - Boîtier protégé
- **Prix** : 15-25€

**BME680 (Bosch)**
- **Mesures** : Température, humidité, pression, qualité d'air (VOC)
- **Précision** : ±1°C, ±3% humidité
- **Applications** : Monitoring qualité air intérieur
- **Prix** : 20-30€

**DS18B20 (Maxim)**
- **Type** : Capteur température uniquement, étanche
- **Précision** : ±0.5°C
- **Interface** : 1-Wire (un seul fil de données)
- **Avantages** :
  - Adressage multiple sur un bus
  - Versions étanches disponibles
  - Alimentation parasite possible
- **Prix** : 5-8€

### Optimisations Émetteurs IR

#### Émetteurs Haute Puissance

**Configuration TSAL6400 Optimisée**
- **Courant nominal** : 100mA continu, 1A pulsé
- **Angle de rayonnement** : ±17° (idéal pour ciblage précis)
- **Portée effective** : 15+ mètres avec driver approprié

```cpp
// Driver haute puissance pour TSAL6400
class HighPowerIRDriver {
private:
    const uint8_t IR_PIN = 4;
    const uint8_t TRANSISTOR_PIN = 5;
    
public:
    void setup() {
        pinMode(TRANSISTOR_PIN, OUTPUT);
        ledcSetup(0, 38000, 8); // 38kHz PWM
        ledcAttachPin(IR_PIN, 0);
    }
    
    void sendPulse(uint16_t duration_us) {
        digitalWrite(TRANSISTOR_PIN, HIGH); // Activation transistor
        ledcWrite(0, 128); // 50% duty cycle pour 38kHz
        delayMicroseconds(duration_us);
        ledcWrite(0, 0);
        digitalWrite(TRANSISTOR_PIN, LOW);
    }
};
```

#### Array d'Émetteurs Directionnels

**Configuration 4 émetteurs en croix**
- **Avantages** : Couverture 360°, redondance
- **Schéma de câblage** :

```
     LED1 (Nord)
        |
LED4 ─ ESP32 ─ LED2 (Est)
        |
     LED3 (Sud)
```

### Améliorations Alimentation

#### Alimentation Commutée Intégrée

**HLK-PM01 (220V AC vers 5V DC)**
- **Sortie** : 5V / 600mA
- **Avantages** :
  - Très compact (34x20x15mm)
  - Isolation galvanique
  - Rendement >80%
  - Protection surcharge
- **Applications** : Intégration directe secteur
- **Prix** : 8-12€

**Schéma d'intégration sécurisée :**
```
230V AC ──[Fusible 1A]──[HLK-PM01]──[Condensateur 1000µF]──[ESP32]
            │                         │
            └─[Varistance]            └─[Régulateur 3.3V]
```

#### Système UPS avec Batterie

**Configuration batterie de secours :**
- **Batterie** : Li-Ion 18650 (3.7V, 2500mAh)
- **Autonomie** : 8-12 heures en fonctionnement normal
- **Chargeur** : TP4056 avec protection

```cpp
// Gestionnaire UPS intelligent
class BackupPowerManager {
private:
    const uint8_t BATTERY_VOLTAGE_PIN = 35;
    const uint8_t CHARGING_PIN = 26;
    const uint8_t POWER_GOOD_PIN = 25;
    
public:
    float getBatteryPercentage() {
        int adcValue = analogRead(BATTERY_VOLTAGE_PIN);
        float voltage = (adcValue * 3.3 / 4095.0) * 2.0;
        
        // Conversion Li-Ion en pourcentage
        if (voltage >= 4.1) return 100.0;
        if (voltage <= 3.2) return 0.0;
        return (voltage - 3.2) / (4.1 - 3.2) * 100.0;
    }
    
    bool isMainPowerAvailable() {
        return digitalRead(POWER_GOOD_PIN);
    }
};
```

### Configuration Multi-Capteurs Redondante

#### Système de Validation Croisée

```cpp
class RedundantSensorSystem {
private:
    struct SensorNode {
        SensorInterface* sensor;
        float lastReading;
        float confidence;
        uint32_t lastUpdate;
        bool isHealthy;
    };
    
    std::vector<SensorNode> sensors;
    
public:
    void addSensor(SensorInterface* sensor, float baseConfidence = 1.0) {
        SensorNode node = {
            sensor, 0.0, baseConfidence, 0, false
        };
        sensors.push_back(node);
    }
    
    float getValidatedReading() {
        updateAllSensors();
        
        // Élimination des outliers
        std::vector<float> validReadings;
        for (auto& node : sensors) {
            if (node.isHealthy) {
                validReadings.push_back(node.lastReading);
            }
        }
        
        if (validReadings.empty()) return NAN;
        if (validReadings.size() == 1) return validReadings[0];
        
        // Moyenne pondérée après filtrage
        return calculateWeightedAverage(validReadings);
    }
    
private:
    void updateAllSensors() {
        for (auto& node : sensors) {
            if (node.sensor->readData()) {
                node.lastReading = node.sensor->getLastValue();
                node.lastUpdate = millis();
                node.isHealthy = validateReading(node);
            } else {
                node.isHealthy = false;
            }
        }
    }
    
    bool validateReading(const SensorNode& node) {
        // Validation plage acceptable
        if (node.lastReading < -40 || node.lastReading > 85) return false;
        
        // Validation contre autres capteurs
        float deviation = calculateDeviation(node.lastReading);
        return deviation < 5.0; // Seuil d'écart acceptable
    }
};
```

### Optimisations PCB et Intégration

#### Layout PCB Professionnel

**Spécifications recommandées :**
- **Épaisseur** : 1.6mm standard
- **Couches** : 2 couches minimum (4 pour projets complexes)
- **Largeur pistes** : 0.2mm minimum, 0.5mm pour alimentation
- **Via** : 0.2mm drill, 0.45mm pad
- **Finition** : HASL ou ENIG pour connecteurs

**Zones spécialisées :**
```
┌─────────────────────────────────────┐
│ [Zone WiFi]    [ESP32]    [Alim]    │
│ Keep-out       CPU        Switching │
│                                     │
│ [Capteurs]     [IR]       [Conn]    │
│ Analogique     High-Power I/O       │
└─────────────────────────────────────┘
```

#### Boîtier Impression 3D Optimisé

**Matériaux recommandés :**
- **PETG** : Résistance température, transparence IR
- **ABS** : Résistance mécanique, finition
- **ASA** : Résistance UV pour usage extérieur

**Caractéristiques du boîtier :**
- **Dimensions** : 85x65x28mm (compatible boîtier standard)
- **Fenêtre IR** : 12x12mm avec diffuseur
- **Ventilation** : Grilles latérales 3mm
- **Fixation** : Trous 3mm pour vis ou adhésif
- **Accès** : Port micro-USB accessible

### Optimisations Énergétiques Avancées

#### Gestion Dynamique de la Fréquence

```cpp
class PowerOptimizer {
private:
    enum PowerState {
        HIGH_PERFORMANCE,  // 240MHz - Traitement intensif
        BALANCED,         // 160MHz - Usage normal
        ECONOMY,          // 80MHz  - Économie d'énergie
        ULTRA_LOW         // 40MHz  - Veille active
    };
    
    PowerState currentState;
    uint32_t lastActivity;
    
public:
    void optimizePowerConsumption() {
        uint32_t inactivity = millis() - lastActivity;
        
        if (inactivity > 300000) { // 5 minutes
            setPowerState(ULTRA_LOW);
        } else if (inactivity > 60000) { // 1 minute
            setPowerState(ECONOMY);
        } else if (getCurrentLoad() < 0.3) {
            setPowerState(BALANCED);
        } else {
            setPowerState(HIGH_PERFORMANCE);
        }
    }
    
private:
    void setPowerState(PowerState state) {
        if (state == currentState) return;
        
        switch (state) {
            case HIGH_PERFORMANCE:
                setCpuFrequencyMhz(240);
                WiFi.setSleep(false);
                break;
            case BALANCED:
                setCpuFrequencyMhz(160);
                WiFi.setSleep(WIFI_PS_MIN_MODEM);
                break;
            case ECONOMY:
                setCpuFrequencyMhz(80);
                WiFi.setSleep(WIFI_PS_MAX_MODEM);
                break;
            case ULTRA_LOW:
                setCpuFrequencyMhz(40);
                WiFi.setSleep(WIFI_PS_MAX_MODEM);
                break;
        }
        
        currentState = state;
    }
};
```

### Diagnostic et Maintenance

#### Système d'Auto-Diagnostic

```cpp
class HardwareDiagnostic {
public:
    struct DiagnosticResult {
        bool wifiModule;
        bool irEmitter;
        bool temperatureSensor;
        bool powerSupply;
        bool flashMemory;
        String detailedReport;
    };
    
    DiagnosticResult performFullDiagnostic() {
        DiagnosticResult result = {};
        
        result.wifiModule = testWiFiModule();
        result.irEmitter = testIREmitter();
        result.temperatureSensor = testTemperatureSensor();
        result.powerSupply = testPowerSupply();
        result.flashMemory = testFlashMemory();
        
        generateReport(result);
        
        return result;
    }
    
private:
    bool testWiFiModule() {
        return WiFi.mode(WIFI_STA) && WiFi.scanNetworks() >= 0;
    }
    
    bool testIREmitter() {
        // Test basique de l'émetteur IR
        pinMode(4, OUTPUT);
        digitalWrite(4, HIGH);
        delay(10);
        digitalWrite(4, LOW);
        return true; // Nécessite récepteur pour test complet
    }
    
    bool testTemperatureSensor() {
        // Test lecture capteur température
        return !isnan(sensorManager.getCurrentTemperature());
    }
    
    bool testPowerSupply() {
        float voltage = (analogRead(35) * 3.3 / 4095.0);
        return voltage > 3.0 && voltage < 3.6; // Plage acceptable
    }
    
    bool testFlashMemory() {
        File testFile = SPIFFS.open("/test.txt", "w");
        if (!testFile) return false;
        
        testFile.println("test");
        testFile.close();
        
        testFile = SPIFFS.open("/test.txt", "r");
        bool success = testFile.available() > 0;
        testFile.close();
        
        SPIFFS.remove("/test.txt");
        return success;
    }
};
```

### Évolutions et Mises à Niveau

#### Module d'Extension GPIO

```cpp
// Expansion I/O avec MCP23017
class GPIOExpander {
private:
    const uint8_t MCP23017_ADDRESS = 0x20;
    
public:
    void setup() {
        Wire.begin();
        
        // Configuration des ports en sortie
        writeRegister(0x00, 0x00); // IODIRA - Port A en sortie
        writeRegister(0x01, 0x00); // IODIRB - Port B en sortie
    }
    
    void digitalWrite(uint8_t pin, bool state) {
        uint8_t reg = (pin < 8) ? 0x12 : 0x13; // GPIOA ou GPIOB
        uint8_t bit = pin % 8;
        
**Note** : Les prix sont approximatifs et peuvent varier selon les fournisseurs et la localisation.
        uint8_t currentValue = readRegister(reg);
        
        if (state) {
            currentValue |= (1 << bit);
        } else {
            currentValue &= ~(1 << bit);
        }
        
        writeRegister(reg, currentValue);
    }
    
private:
    void writeRegister(uint8_t reg, uint8_t value) {
        Wire.beginTransmission(MCP23017_ADDRESS);
        Wire.write(reg);
        Wire.write(value);
        Wire.endTransmission();
    }
    
    uint8_t readRegister(uint8_t reg) {
        Wire.beginTransmission(MCP23017_ADDRESS);
        Wire.write(reg);
        Wire.endTransmission();
        
        Wire.requestFrom(MCP23017_ADDRESS, 1);
        return Wire.read();
    }
};
```

#### Module Écran OLED

**SSD1306 128x64 OLED**
- **Interface** : I2C
- **Consommation** : 20mA actif, <1µA veille
- **Applications** : Affichage local d'informations
- **Prix** : 8-15€

```cpp
// Gestionnaire d'affichage local
class LocalDisplay {
private:
    SSD1306Wire display(0x3c, 21, 22); // SDA, SCL
    
public:
    void setup() {
        display.init();
        display.flipScreenVertically();
        display.setFont(ArialMT_Plain_10);
    }
    
    void showStatus(float temperature, const String& mode, bool wifiConnected) {
        display.clear();
        
        // Température principale
        display.setFont(ArialMT_Plain_24);
        display.drawString(0, 0, String(temperature, 1) + "°C");
        
        // Mode actuel
        display.setFont(ArialMT_Plain_16);
        display.drawString(0, 30, "Mode: " + mode);
        
        // Indicateurs de statut
        display.setFont(ArialMT_Plain_10);
        display.drawString(0, 50, wifiConnected ? "WiFi: OK" : "WiFi: --");
        display.drawString(70, 50, "IP: " + WiFi.localIP().toString());
        
        display.display();
    }
    
    void showNetworkInfo() {
        display.clear();
        display.setFont(ArialMT_Plain_10);
        
        display.drawString(0, 0, "SSID: " + WiFi.SSID());
        display.drawString(0, 12, "IP: " + WiFi.localIP().toString());
        display.drawString(0, 24, "Signal: " + String(WiFi.RSSI()) + "dBm");
        display.drawString(0, 36, "Uptime: " + getUptimeString());
        
        display.display();
    }
};
```

### Kits de Développement Recommandés

#### Kit Débutant (Budget: 40€)

**Composants inclus :**
- ESP32 DevKit C
- LED IR TSAL6200
- Récepteur IR TSOP4838
- Capteur DHT22
- Breadboard et câbles
- Résistances assortiment
- Alimentation USB

**Fonctionnalités :**
- Contrôle IR basique
- Lecture température/humidité
- Interface web simple
- Configuration WiFi

#### Kit Intermédiaire (Budget: 80€)

**Composants supplémentaires :**
- ESP32-S3 DevKit
- LED IR haute puissance TSAL6400
- Transistor amplification 2N2222
- Capteur BME280 (température/humidité/pression)
- Module alimentation HLK-PM01
- PCB prototype
- Boîtier impression 3D

**Fonctionnalités avancées :**
- Portée IR étendue
- Capteurs multiples
- Alimentation secteur
- Boîtier professionnel

#### Kit Expert (Budget: 150€)

**Composants professionnels :**
- ESP32-S3 avec PSRAM
- Array 4x LED IR directionnelles
- Capteurs redondants (SHT30 + BME680)
- Écran OLED 128x64
- Système UPS avec batterie
- PCB personnalisé 4 couches
- Boîtier aluminium usiné

**Fonctionnalités expertes :**
- Système redondant
- Diagnostic intégré
- Alimentation de secours
- Interface locale
- Qualité industrielle

### Fournisseurs et Sources d'Approvisionnement

#### Fournisseurs Recommandés

**Composants Électroniques :**
- **Mouser Electronics** : Composants de qualité, livraison rapide
- **DigiKey** : Large sélection, documentation technique
- **LCSC** : Prix compétitifs, composants asiatiques
- **Conrad** : Disponibilité locale, support technique

**Modules ESP32 :**
- **Espressif Direct** : Modules officiels, dernières versions
- **AliExpress** : Prix attractifs, délais plus longs
- **Amazon** : Livraison rapide, retours faciles

**PCB et Boîtiers :**
- **JLCPCB** : PCB économiques, assemblage SMT
- **PCBWay** : Finitions spéciales, prototypes rapides
- **Thingiverse** : Modèles 3D gratuits
- **Ponoko** : Découpe laser personnalisée

#### Estimation Coûts par Configuration

| Configuration | Composants | PCB | Boîtier | Main d'œuvre | Total |
|---------------|------------|-----|---------|--------------|--------|
| Basique | 35€ | 5€ | 8€ | 2h | 48€ |
| Standard | 65€ | 15€ | 15€ | 4h | 95€ |
| Avancée | 120€ | 35€ | 25€ | 8h | 180€ |
| Professionnelle | 200€ | 60€ | 40€ | 12h | 300€ |

### Maintenance et Évolution

#### Planning de Maintenance

**Mensuel :**
- Vérification connexions
- Nettoyage lentille IR
- Test fonctionnalités basiques
- Sauvegarde configuration

**Trimestriel :**
- Calibration capteurs
- Mise à jour firmware
- Vérification étanchéité
- Test alimentation de secours

**Annuel :**
- Remplacement piles/batteries
- Inspection composants
- Mise à niveau matérielle
- Documentation modifications

#### Évolutions Futures

**Version 2.0 - Améliorations Prévues :**
- Support protocole Matter/Thread
- Intégration caméra thermique
- Intelligence artificielle embarquée
- Communication LoRaWAN

**Version 3.0 - Vision Long Terme :**
- Capteurs distribués sans fil
- Prédiction consommation énergétique
- Intégration réseau électrique intelligent
- Maintenance prédictive

### Conclusion Matérielle

Le choix des composants dépend largement de l'application visée et du budget disponible. Les configurations proposées offrent une évolutivité permettant de commencer avec un prototype simple et d'évoluer vers un système professionnel.

**Points clés à retenir :**

1. **ESP32** reste le choix optimal pour la majorité des applications
2. **Capteurs redondants** améliorent significantly la fiabilité
3. **Émetteurs IR haute puissance** essentiels pour portée étendue
4. **Alimentation de secours** recommandée pour usage critique
5. **PCB professionnel** nécessaire pour fiabilité long terme

Cette approche modulaire permet d'adapter le système aux besoins spécifiques tout en maintenant une architecture cohérente et évolutive.

### Références Matérielles

- [Datasheets ESP32 Family](https://www.espressif.com/en/products/socs)
- [Sensirion Sensor Guide](https://www.sensirion.com/sensors/)
- [IR LED Application Notes](https://www.osram-os.com/products/infrared/)
- [PCB Design Guidelines](https://www.altium.com/documentation/)
- [3D Printing Materials Guide](https://www.prusa3d.com/materials/)

---

*Dernière mise à jour : Phase 3 - Documentation Technique Approfondie*