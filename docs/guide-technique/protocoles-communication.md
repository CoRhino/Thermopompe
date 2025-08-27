
# Protocoles de Communication - Thermopompe

## Vue d'Ensemble

Ce document détaille tous les protocoles de communication utilisés dans le système de contrôle de la thermopompe Elios. Le système utilise plusieurs couches de communication pour assurer un contrôle fiable et sécurisé.

## Prérequis

- Connaissances en protocoles réseau TCP/IP
- Bases des communications infrarouge
- Familiarité avec les protocoles de sécurité web
- Compréhension des formats de données JSON

## Architecture de Communication

### Diagramme des Protocoles

```mermaid
graph TB
    subgraph "Couche Application"
        A[Interface Web] --> B[API REST]
        C[App Mobile] --> B
        D[Système Domotique] --> B
    end
    
    subgraph "Couche Transport"
        B --> E[HTTP/HTTPS]
        B --> F[WebSocket]
        B --> G[MQTT]
    end
    
    subgraph "Couche Réseau"
        E --> H[TCP/IP]
        F --> H
        G --> H
        H --> I[WiFi 802.11n]
    end
    
    subgraph "Couche Physique"
        I --> J[Module ESP32]
        J --> K[Protocole IR]
        K --> L[Pompe Elios]
    end
    
    style A fill:#e1f5fe
    style L fill:#ffcdd2
    style J fill:#c8e6c9
    style B fill:#fff3e0
```

## 1. Protocole Infrarouge (IR)

### 1.1 Spécifications Techniques

#### Caractéristiques Physiques
- **Fréquence de porteuse** : 38 kHz
- **Longueur d'onde** : 940 nm
- **Modulation** : PWM (Pulse Width Modulation)
- **Portée maximale** : 8-12 mètres
- **Angle de diffusion** : 30-60°

#### Format des Trames IR

```mermaid
sequenceDiagram
    participant ESP32
    participant LED_IR as LED IR
    participant RX_Pompe as Récepteur Pompe
    participant Pompe as Pompe Elios
    
    Note over ESP32,Pompe: Format trame Elios DE12HIW23230E3
    
    ESP32->>LED_IR: Préambule (9ms HIGH)
    LED_IR->>RX_Pompe: Signal 38kHz modulé
    ESP32->>LED_IR: Pause (4.5ms LOW)
    LED_IR->>RX_Pompe: Silence
    ESP32->>LED_IR: Données (32 bits)
    LED_IR->>RX_Pompe: Bits encodés
    ESP32->>LED_IR: Stop bit (562µs)
    LED_IR->>RX_Pompe: Fin de trame
    
    RX_Pompe->>Pompe: Décodage commande
    Pompe-->>RX_Pompe: Feedback sonore (bip)
```

#### Structure des Commandes IR

```cpp
// Structure d'une commande IR Elios
typedef struct {
    uint8_t address;      // Adresse appareil (0x02)
    uint8_t command;      // Code commande
    uint8_t parameter;    // Paramètre (température, vitesse, etc.)
    uint8_t checksum;     // Somme de contrôle
} EliosIRCommand;

// Exemples de codes de commande
#define ELIOS_POWER_ON      0x01
#define ELIOS_POWER_OFF     0x02
#define ELIOS_TEMP_UP       0x03
#define ELIOS_TEMP_DOWN     0x04
#define ELIOS_MODE_HEAT     0x05
#define ELIOS_MODE_COOL     0x06
#define ELIOS_MODE_FAN      0x07
#define ELIOS_FAN_SPEED     0x08
#define ELIOS_TIMER_ON      0x09
#define ELIOS_TIMER_OFF     0x0A
```

### 1.2 Protocole de Timing

#### Timing des Bits

```
Bit 0 : 562µs HIGH + 562µs LOW  (total: 1.124ms)
Bit 1 : 562µs HIGH + 1687µs LOW (total: 2.249ms)

Préambule : 9000µs HIGH + 4500µs LOW
Répétition : 9000µs HIGH + 2250µs LOW + 562µs HIGH
```

#### Exemple de Trame Complète

```cpp
// Commande : Chauffage 22°C
void sendHeatCommand(int temperature) {
    EliosIRCommand cmd;
    cmd.address = 0x02;           // Adresse Elios
    cmd.command = ELIOS_MODE_HEAT; // Mode chauffage
    cmd.parameter = temperature;   // Température cible
    cmd.checksum = calculateChecksum(&cmd);
    
    sendIRCommand(&cmd);
}

// Calcul checksum (XOR simple)
uint8_t calculateChecksum(EliosIRCommand* cmd) {
    return cmd->address ^ cmd->command ^ cmd->parameter;
}
```

### 1.3 Gestion des Erreurs IR

#### Stratégies de Retry

```cpp
// Configuration retry logic
#define MAX_IR_RETRIES 3
#define RETRY_DELAY_MS 500

typedef enum {
    IR_SUCCESS,
    IR_ERROR_TIMEOUT,
    IR_ERROR_NO_ACK,
    IR_ERROR_CHECKSUM
} IRResult;

IRResult sendCommandWithRetry(EliosIRCommand* cmd) {
    for (int attempt = 0; attempt < MAX_IR_RETRIES; attempt++) {
        IRResult result = sendIRCommand(cmd);
        
        if (result == IR_SUCCESS) {
            return IR_SUCCESS;
        }
        
        // Attendre avant retry
        delay(RETRY_DELAY_MS * (attempt + 1));
    }
    
    return IR_ERROR_TIMEOUT;
}
```

## 2. Communication Réseau

### 2.1 Protocole HTTP/HTTPS

#### Configuration HTTPS

```cpp
// Configuration serveur HTTPS
const char* cert_pem = "-----BEGIN CERTIFICATE-----\n"
                       "MIIDXTCCAkWgAwIBAgIJAKZ...\n"
                       "-----END CERTIFICATE-----\n";

const char* key_pem = "-----BEGIN PRIVATE KEY-----\n"
                      "MIIEvgIBADANBgkqhkiG9w0B...\n"
                      "-----END PRIVATE KEY-----\n";

// Initialisation serveur sécurisé
WiFiServerSecure server(443);
server.setRSACert(new BearSSL::X509List(cert_pem), 
                  new BearSSL::PrivateKey(key_pem));
```

#### Headers HTTP Standards

```http
# Requête type
POST /api/v1/temperature HTTP/1.1
Host: thermopompe.local
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Length: 45
User-Agent: ThermopompeWeb/1.0

{
  "temperature": 22,
  "mode": "heat",
  "fan_speed": "auto"
}

# Réponse type
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-cache
Access-Control-Allow-Origin: *
Content-Length: 156

{
  "status": "success",
  "data": {
    "temperature": 22,
    "mode": "heat",
    "timestamp": "2024-01-15T14:30:00Z"
  }
}
```

### 2.2 Protocole WebSocket

#### Connexion WebSocket

```javascript
// Côté client JavaScript
const ws = new WebSocket('wss://thermopompe.local/ws');

ws.onopen = function(event) {
    console.log('Connexion WebSocket établie');
    
    // S'abonner aux mises à jour
    ws.send(JSON.stringify({
        type: 'subscribe',
        channels: ['temperature', 'status']
    }));
};

ws.onmessage = function(event) {
    const data = JSON.parse(event.data);
    handleRealTimeUpdate(data);
};
```

```cpp
// Côté serveur ESP32
void handleWebSocketMessage(uint8_t num, WStype_t type, uint8_t * payload, size_t length) {
    switch(type) {
        case WStype_CONNECTED:
            Serial.printf("Client %u connecté\n", num);
            break;
            
        case WStype_TEXT:
            String message = String((char*)payload);
            DynamicJsonDocument doc(1024);
            deserializeJson(doc, message);
            
            if (doc["type"] == "command") {
                processWebSocketCommand(num, doc);
            }
            break;
    }
}
```

#### Messages WebSocket

```json
// Message de commande
{
  "type": "command",
  "action": "set_temperature",
  "payload": {
    "temperature": 23,
    "mode": "heat"
  },
  "timestamp": "2024-01-15T14:30:00Z"
}

// Message de mise à jour d'état
{
  "type": "status_update",
  "payload": {
    "current_temperature": 21.5,
    "target_temperature": 23,
    "mode": "heat",
    "fan_speed": "medium",
    "power": true
  },
  "timestamp": "2024-01-15T14:30:15Z"
}

// Message d'erreur
{
  "type": "error",
  "error": {
    "code": "IR_TIMEOUT",
    "message": "Impossible de communiquer avec la pompe",
    "retry_count": 3
  },
  "timestamp": "2024-01-15T14:30:30Z"
}
```

### 2.3 Protocole MQTT (Optionnel)

#### Configuration MQTT

```cpp
#include <PubSubClient.h>

// Configuration client MQTT
WiFiClient espClient;
PubSubClient mqttClient(espClient);

const char* mqtt_server = "192.168.1.100";
const char* mqtt_username = "thermopompe";
const char* mqtt_password = "secure_password";

// Topics MQTT
const char* TOPIC_TEMPERATURE = "thermopompe/temperature";
const char* TOPIC_STATUS = "thermopompe/status";
const char* TOPIC_COMMAND = "thermopompe/command";
```

#### Messages MQTT

```json
// Publication état (thermopompe/status)
{
  "device_id": "thermopompe_salon",
  "temperature": {
    "current": 21.5,
    "target": 23.0
  },
  "mode": "heat",
  "power": true,
  "last_update": "2024-01-15T14:30:00Z"
}

// Commande (thermopompe/command)
{
  "action": "set_temperature",
  "temperature": 24,
  "mode": "heat"
}
```

## 3. Sécurité et Authentification

### 3.1 Authentification JWT

#### Génération Token

```cpp
#include <ArduinoJWT.h>

String generateJWT(String username) {
    ArduinoJWT jwt("votre_secret_key_256_bits");
    
    // Payload JWT
    String payload = "{\"sub\":\"" + username + "\","
                    "\"exp\":" + String(WiFi.getTime() + 3600) + ","
                    "\"iat\":" + String(WiFi.getTime()) + "}";
    
    return jwt.encodeJWT(payload);
}

bool validateJWT(String token) {
    ArduinoJWT jwt("votre_secret_key_256_bits");
    return jwt.decodeJWT(token);
}
```

### 3.2 Chiffrement TLS

#### Configuration Certificats

```cpp
// Génération certificat auto-signé
void generateSelfSignedCert() {
    br_ec_private_key *sk;
    br_ec_public_key *pk;
    
    // Génération clé privée
    sk = br_ec_keygen(&br_prng_seeder_system, BR_EC_secp256r1);
    
    // Génération certificat
    br_x509_certificate cert;
    // ... configuration certificat
}
```

### 3.3 Contrôle d'Accès

#### Système de Rôles

```cpp
typedef enum {
    ROLE_ADMIN = 0,
    ROLE_USER = 1,
    ROLE_READONLY = 2
} UserRole;

typedef struct {
    String username;
    String password_hash;
    UserRole role;
    uint32_t last_login;
} User;

bool hasPermission(User* user, String action) {
    if (user->role == ROLE_ADMIN) return true;
    
    if (action == "read" && user->role >= ROLE_READONLY) return true;
    if (action == "control" && user->role >= ROLE_USER) return true;
    
    return false;
}
```

## 4. Format des Messages et Structures de Données

### 4.1 Schémas JSON

#### État du Système

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "État Thermopompe",
  "type": "object",
  "properties": {
    "device_info": {
      "type": "object",
      "properties": {
        "model": { "type": "string", "const": "Elios DE12HIW23230E3" },
        "firmware_version": { "type": "string" },
        "uptime": { "type": "integer" }
      }
    },
    "temperature": {
      "type": "object",
      "properties": {
        "current": { "type": "number", "minimum": -40, "maximum": 60 },
        "target": { "type": "number", "minimum": 16, "maximum": 30 },
        "unit": { "type": "string", "enum": ["C", "F"] }
      }
    },
    "mode": {
      "type": "string",
      "enum": ["off", "heat", "cool", "fan", "auto"]
    },
    "fan_speed": {
      "type": "string",
      "enum": ["auto", "low", "medium", "high"]
    },
    "power": { "type": "boolean" },
    "timer": {
      "type": "object",
      "properties": {
        "enabled": { "type": "boolean" },
        "remaining_minutes": { "type": "integer", "minimum": 0 }
      }
    }
  }
}
```

#### Commande de Contrôle

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Commande Thermopompe",
  "type": "object",
  "properties": {
    "action": {
      "type": "string",
", "power_off", "set_temperature", "set_mode", "set_fan_speed", "set_timer"]
    },
    "parameters": {
      "type": "object",
      "properties": {
        "temperature": { "type": "integer", "minimum": 16, "maximum": 30 },
        "mode": { "type": "string", "enum": ["heat", "cool", "fan", "auto"] },
        "fan_speed": { "type": "string", "enum": ["auto", "low", "medium", "high"] },
        "timer_minutes": { "type": "integer", "minimum": 0, "maximum": 1440 }
      }
    }
  },
  "required": ["action"]
}
```

### 4.2 Structures de Données C++

#### État Global du Système

```cpp
typedef struct {
    struct {
        float current;
        float target;
        char unit; // 'C' ou 'F'
    } temperature;
    
    enum {
        MODE_OFF = 0,
        MODE_HEAT = 1,
        MODE_COOL = 2,
        MODE_FAN = 3,
        MODE_AUTO = 4
    } mode;
    
    enum {
        FAN_AUTO = 0,
        FAN_LOW = 1,
        FAN_MEDIUM = 2,
        FAN_HIGH = 3
    } fan_speed;
    
    bool power;
    
    struct {
        bool enabled;
        uint16_t remaining_minutes;
    } timer;
    
    uint32_t last_update;
    bool ir_communication_ok;
    
} SystemState;
```

## 5. Gestion des Erreurs et Retry Logic

### 5.1 Codes d'Erreur Standardisés

```cpp
typedef enum {
    // Erreurs générales
    ERROR_SUCCESS = 0,
    ERROR_INVALID_PARAMETER = 1001,
    ERROR_TIMEOUT = 1002,
    ERROR_NETWORK_UNAVAILABLE = 1003,
    
    // Erreurs IR
    ERROR_IR_TRANSMISSION_FAILED = 2001,
    ERROR_IR_NO_RESPONSE = 2002,
    ERROR_IR_INVALID_COMMAND = 2003,
    ERROR_IR_CHECKSUM_MISMATCH = 2004,
    
    // Erreurs authentification
    ERROR_AUTH_INVALID_TOKEN = 3001,
    ERROR_AUTH_EXPIRED_TOKEN = 3002,
    ERROR_AUTH_INSUFFICIENT_PERMISSIONS = 3003,
    
    // Erreurs système
    ERROR_SYSTEM_LOW_MEMORY = 4001,
    ERROR_SYSTEM_OVERHEATING = 4002,
    ERROR_SYSTEM_WIFI_DISCONNECTED = 4003
    
} ErrorCode;

const char* getErrorMessage(ErrorCode code) {
    switch(code) {
        case ERROR_SUCCESS: return "Succès";
        case ERROR_IR_TRANSMISSION_FAILED: return "Échec transmission IR";
        case ERROR_AUTH_INVALID_TOKEN: return "Token d'authentification invalide";
        // ... autres messages
        default: return "Erreur inconnue";
    }
}
```

### 5.2 Stratégie de Retry Adaptative

```cpp
typedef struct {
    uint8_t max_retries;
    uint16_t base_delay_ms;
    float backoff_multiplier;
    uint16_t max_delay_ms;
} RetryConfig;

class AdaptiveRetry {
private:
    RetryConfig config;
    uint8_t consecutive_failures;
    
public:
    AdaptiveRetry(RetryConfig cfg) : config(cfg), consecutive_failures(0) {}
    
    bool executeWithRetry(std::function<bool()> operation) {
        for (uint8_t attempt = 0; attempt < config.max_retries; attempt++) {
            if (operation()) {
                consecutive_failures = 0;
                return true;
            }
            
            // Calcul délai avec backoff exponentiel
            uint16_t delay = config.base_delay_ms * 
                           pow(config.backoff_multiplier, attempt + consecutive_failures);
            delay = min(delay, config.max_delay_ms);
            
            delay(delay);
        }
        
        consecutive_failures++;
        return false;
    }
};
```

## 6. Exemples de Trames de Communication

### 6.1 Capture de Codes IR Réels

```cpp
// Codes IR capturés pour Elios DE12HIW23230E3
const uint32_t IR_CODES[] = {
    0x02FD08F7,  // Power ON
    0x02FD8877,  // Power OFF
    0x02FD48B7,  // Mode Heat
    0x02FDC837,  // Mode Cool
    0x02FD28D7,  // Mode Fan
    0x02FDA857,  // Temperature UP
    0x02FD6897,  // Temperature DOWN
    0x02FDE817,  // Fan Speed
    0x02FD18E7,  // Timer ON
    0x02FD9867   // Timer OFF
};

// Fonction d'envoi avec timing précis
void sendEliosCommand(uint32_t code) {
    // Préambule
    digitalWrite(IR_PIN, HIGH);
    delayMicroseconds(9000);
    digitalWrite(IR_PIN, LOW);
    delayMicroseconds(4500);
    
    // Données (32 bits, MSB first)
    for (int i = 31; i >= 0; i--) {
        digitalWrite(IR_PIN, HIGH);
        delayMicroseconds(562);
        
        if (code & (1UL << i)) {
            // Bit 1
            digitalWrite(IR_PIN, LOW);
            delayMicroseconds(1687);
        } else {
            // Bit 0
            digitalWrite(IR_PIN, LOW);
            delayMicroseconds(562);
        }
    }
    
    // Stop bit
    digitalWrite(IR_PIN, HIGH);
    delayMicroseconds(562);
    digitalWrite(IR_PIN, LOW);
}
```

### 6.2 Exemple de Session HTTP Complète

```http
# 1. Authentification
POST /api/v1/auth/login HTTP/1.1
Host: thermopompe.local
Content-Type: application/json

{
  "username": "admin",
  "password": "hashedpassword123"
}

# Réponse
HTTP/1.1 200 OK
Content-Type: application/json

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "Bearer"
}

# 2. Consultation de l'état
GET /api/v1/status HTTP/1.1
Host: thermopompe.local
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# Réponse
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "success",
  "data": {
    "temperature": {
      "current": 21.5,
      "target": 22.0
    },
    "mode": "heat",
    "power": true,
    "last_update": "2024-01-15T14:30:00Z"
  }
}

# 3. Changement de température
PUT /api/v1/temperature HTTP/1.1
Host: thermopompe.local
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json

{
  "target": 24,
  "mode": "heat"
}

# Réponse
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "success",
  "data": {
    "temperature": 24,
    "ir_command_sent": true,
    "timestamp": "2024-01-15T14:31:00Z"
  }
}
```

## 7. Monitoring et Diagnostics

### 7.1 Métriques de Communication

```cpp
typedef struct {
    uint32_t ir_commands_sent;
    uint32_t ir_commands_failed;
    uint32_t http_requests_received;
    uint32_t websocket_connections;
    uint32_t authentication_failures;
    uint32_t uptime_seconds;
    float average_response_time_ms;
} CommunicationMetrics;

void updateMetrics(CommunicationMetrics* metrics) {
    // Calcul du taux de succès IR
    float ir_success_rate = (float)metrics->ir_commands_sent / 
                           (metrics->ir_commands_sent + metrics->ir_commands_failed);
    
    // Log périodique
    if (millis() % 60000 == 0) { // Chaque minute
        Serial.printf("IR Success Rate: %.2f%%\n", ir_success_rate * 100);
        Serial.printf("Active WebSocket connections: %d\n", metrics->websocket_connections);
    }
}
```

### 7.2 Logs Structurés

```cpp
typedef enum {
    LOG_DEBUG = 0,
    LOG_INFO = 1,
    LOG_WARNING = 2,
    LOG_ERROR = 3
} LogLevel;

void logCommunication(LogLevel level, const char* protocol, const char* message) {
    String timestamp = String(WiFi.getTime());
    String logEntry = "{\"timestamp\":\"" + timestamp + 
                     "\",\"level\":\"" + String(level) + 
                     "\",\"protocol\":\"" + String(protocol) + 
                     "\",\"message\":\"" + String(message) + "\"}";
    
    Serial.println(logEntry);
    
    // Sauvegarde dans fichier log si SPIFFS disponible
    if (SPIFFS.begin()) {
        File logFile = SPIFFS.open("/logs/communication.log", "a");
        if (logFile) {
            logFile.println(logEntry);
            logFile.close();
        }
    }
}
```

## 8. Tests et Validation

### 8.1 Tests de Communication IR

```cpp
bool testIRCommunication() {
    bool allTestsPassed = true;
    
    // Test 1: Envoi commande basique
    logCommunication(LOG_INFO, "IR", "Test envoi commande Power ON");
    if (!sendEliosCommand(IR_CODES[0])) {
        logCommunication(LOG_ERROR, "IR", "Échec test Power ON");
        allTestsPassed = false;
    }
    
    // Test 2: Séquence de commandes
    logCommunication(LOG_INFO, "IR", "Test séquence commandes");
    for (int i = 0; i < 5; i++) {
        if (!sendEliosCommand(IR_CODES[i])) {
            logCommunication(LOG_ERROR, "IR", "Échec séquence position " + String(i));
            allTestsPassed = false;
        }
        delay(1000); // Délai entre commandes
    }
    
    return allTestsPassed;
}
```

### 8.2 Tests de Performance Réseau

```cpp
void performanceTest() {
    uint32_t start_time = millis();
    uint32_t request_count = 0;
    
    // Test pendant 10 secondes
    while (millis() - start_time < 10000) {
        // Simulation requête
        if (processHTTPRequest()) {
            request_count++;
        }
        delay(10);
    }
    
    float requests_per_second = (float)request_count / 10.0;
    logCommunication(LOG_INFO, "HTTP", "Performance: " + String(requests_per_second) + " req/s");
}
```

## 9. Intégration avec Systèmes Externes

### 9.1 Home Assistant (MQTT Discovery)

```json
{
  "name": "Thermopompe Salon",
  "device_class": "temperature",
  "state_topic": "homeassistant/climate/thermopompe_salon/state",
  "command_topic": "homeassistant/climate/thermopompe_salon/set",
  "temperature_state_topic": "homeassistant/climate/thermopompe_salon/temperature",
  "temperature_command_topic": "homeassistant/climate/thermopompe_salon/temperature/set",
  "modes": ["off", "heat", "cool", "fan_only", "auto"],
  "min_temp": 16,
  "max_temp": 30,
  "temp_step": 1,
  "unique_id": "thermopompe_salon_climate",
  "device": {
    "identifiers": ["thermopompe_salon"],
    "name": "Thermopompe Salon",
    "model": "Elios DE12HIW23230E3",
    "manufacturer": "DIY IoT"
  }
}
```

## 10. Dépannage des Protocoles

### 10.1 Problèmes IR Courants

**Symptôme** : Commandes IR ignorées
- **Vérifications** :
  - Alignement LED IR vers récepteur pompe
  - Obstacle physique bloquant le signal
  - Niveau de batterie (si alimenté par pile)
- **Diagnostic** : Tester avec caméra smartphone (LED doit être visible)
- **Solution** : Réajuster position, nettoyer lentilles

**Symptôme** : Réponse intermittente
- **Vérifications** :
  - Qualité de l'alimentation
  - Interférences électromagnétiques
  - Timing des commandes
- **Solution** : Ajouter condensateurs de filtrage, espacer les commandes

### 10.2 Problèmes Réseau

**Symptôme** : Connexions WebSocket instables
- **Diagnostic** : Vérifier logs de déconnexion
- **Solution** : Implémenter reconnexion automatique
```cpp
void maintainWebSocketConnection() {
    if (!webSocket.isConnected()) {
        logCommunication(LOG_WARNING, "WebSocket", "Reconnexion nécessaire");
        webSocket.begin("thermopompe.local", 81, "/ws");
    }
}
```

## Références

- [Spécifications IR Elios DE12HIW23230E3](guide-materiel.md)
- [Architecture Système](architecture-systeme.md)
- [RFC 7519 - JSON Web Tokens](https://tools.ietf.org/html/rfc7519)
- [RFC 6455 - WebSocket Protocol](https://tools.ietf.org/html/rfc6455)
- [MQTT Version 3.1.1 Specification](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html)

---

**Note** : Ce document constitue la référence technique complète pour tous les protocoles de communication du système. Maintenir cette documentation à jour lors de modifications du code.
      "enum": ["power_on