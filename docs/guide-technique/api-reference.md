
# Référence API - Thermopompe

## Vue d'Ensemble

Cette documentation détaille l'ensemble des endpoints de l'API REST du système de contrôle de la thermopompe Elios. L'API fournit un accès complet aux fonctionnalités de contrôle, surveillance et configuration du système.

## Prérequis

- Connaissances des protocoles HTTP/HTTPS
- Familiarité avec le format JSON
- Compréhension des tokens JWT pour l'authentification
- Accès réseau au module ESP32

## Configuration de Base

### URL de Base
```
https://thermopompe.local/api/v1
```

### Authentification
L'API utilise l'authentification par token JWT (Bearer Token) pour sécuriser les accès.

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Headers Standards
```http
Content-Type: application/json
Accept: application/json
User-Agent: ThermopompeClient/1.0
```

## Structure des Réponses

### Format de Réponse Standard

```json
{
  "status": "success|error",
  "data": {},
  "message": "Message descriptif (optionnel)",
  "timestamp": "2024-01-15T14:30:00Z",
  "request_id": "uuid-unique"
}
```

### Codes de Statut HTTP

| Code | Description | Usage |
|------|-------------|--------|
| 200 | OK | Requête réussie |
| 201 | Created | Ressource créée |
| 400 | Bad Request | Paramètres invalides |
| 401 | Unauthorized | Authentification requise |
| 403 | Forbidden | Permissions insuffisantes |
| 404 | Not Found | Endpoint non trouvé |
| 429 | Too Many Requests | Rate limit dépassé |
| 500 | Internal Server Error | Erreur serveur |

## 1. Authentification

### 1.1 Connexion

#### POST /auth/login

Authentifie un utilisateur et retourne un token JWT.

**Paramètres de requête :**
```json
{
  "username": "string",
  "password": "string"
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "refresh_token_here"
  }
}
```

**Exemple cURL :**
```bash
curl -X POST "https://thermopompe.local/api/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "motdepasse123"
  }'
```

### 1.2 Rafraîchissement de Token

#### POST /auth/refresh

Renouvelle un token JWT expiré.

**Paramètres de requête :**
```json
{
  "refresh_token": "string"
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "access_token": "nouveau_token_jwt",
    "expires_in": 3600
  }
}
```

### 1.3 Déconnexion

#### POST /auth/logout

Invalide le token JWT actuel.

**Headers requis :**
```http
Authorization: Bearer {token}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "message": "Déconnexion réussie"
}
```

## 2. État du Système

### 2.1 État Global

#### GET /status

Retourne l'état complet du système.

**Headers requis :**
```http
Authorization: Bearer {token}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "device_info": {
      "model": "Elios DE12HIW23230E3",
      "firmware_version": "1.2.3",
      "uptime": 86400,
      "wifi_signal": -45,
      "free_memory": 45000
    },
    "temperature": {
      "current": 21.5,
      "target": 22.0,
      "unit": "C"
    },
    "mode": "heat",
    "fan_speed": "medium",
    "power": true,
    "timer": {
      "enabled": false,
      "remaining_minutes": 0
    },
    "last_ir_command": {
      "command": "set_temperature",
      "timestamp": "2024-01-15T14:25:00Z",
      "success": true
    }
  }
}
```

**Exemple JavaScript :**
```javascript
fetch('https://thermopompe.local/api/v1/status', {
  headers: {
    'Authorization': 'Bearer ' + token,
    'Content-Type': 'application/json'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```

### 2.2 État Température

#### GET /temperature

Retourne uniquement les informations de température.

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "current": 21.5,
    "target": 22.0,
    "unit": "C",
    "sensor_readings": [
      {
        "timestamp": "2024-01-15T14:30:00Z",
        "value": 21.5
      }
    ]
  }
}
```

## 3. Contrôle de la Pompe

### 3.1 Contrôle de l'Alimentation

#### POST /power

Contrôle l'alimentation de la pompe à chaleur.

**Paramètres de requête :**
```json
{
  "power": true
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "power": true,
    "ir_command_sent": true,
    "previous_state": false
  }
}
```

#### GET /power

Retourne l'état actuel de l'alimentation.

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "power": true,
    "last_change": "2024-01-15T14:25:00Z"
  }
}
```

### 3.2 Contrôle de Température

#### PUT /temperature

Modifie la température cible.

**Paramètres de requête :**
```json
{
  "target": 24,
  "unit": "C"
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "target": 24,
    "previous_target": 22,
    "ir_commands_sent": [
      {
        "command": "temp_up",
        "count": 2,
        "success": true
      }
    ]
  }
}
```

**Exemple avec validation :**
```bash
curl -X PUT "https://thermopompe.local/api/v1/temperature" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "target": 24,
    "unit": "C"
  }'
```

### 3.3 Contrôle de Mode

#### PUT /mode

Change le mode de fonctionnement.

**Paramètres de requête :**
```json
{
  "mode": "heat|cool|fan|auto|off"
}
```

**Modes disponibles :**
- `heat` : Mode chauffage
- `cool` : Mode refroidissement
- `fan` : Ventilation seule
- `auto` : Mode automatique
- `off` : Arrêt

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "mode": "heat",
    "previous_mode": "cool",
    "ir_command_sent": true
  }
}
```

### 3.4 Contrôle Vitesse Ventilateur

#### PUT /fan-speed

Modifie la vitesse du ventilateur.

**Paramètres de requête :**
```json
{
  "speed": "auto|low|medium|high"
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "fan_speed": "medium",
    "previous_speed": "low",
    "ir_command_sent": true
  }
}
```

### 3.5 Contrôle Timer

#### POST /timer

Configure le timer de la pompe.

**Paramètres de requête :**
```json
{
  "enabled": true,
  "minutes": 120,
  "action": "on|off"
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "timer": {
      "enabled": true,
      "remaining_minutes": 120,
      "action": "off",
      "start_time": "2024-01-15T14:30:00Z"
    },
    "ir_command_sent": true
  }
}
```

#### DELETE /timer

Annule le timer actuel.

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "timer": {
      "enabled": false,
      "remaining_minutes": 0
    }
  }
}
```

## 4. Configuration

### 4.1 Configuration Système

#### GET /config

Retourne la configuration actuelle du système.

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "device_name": "Thermopompe Salon",
    "temperature_unit": "C",
    "auto_discovery": true,
    "ir_settings": {
      "retry_count": 3,
      "retry_delay": 500
    },
    "network": {
      "wifi_ssid": "MonWiFi",
      "ip_address": "192.168.1.100",
      "mac_address": "AA:BB:CC:DD:EE:FF"
    },
    "mqtt": {
      "enabled": false,
      "broker": "",
      "port": 1883
    }
  }
}
```

#### PUT /config

Met à jour la configuration du système.

**Paramètres de requête :**
```json
{
  "device_name": "string",
  "temperature_unit": "C|F",
  "auto_discovery": true,
  "ir_settings": {
    "retry_count": 3,
    "retry_delay": 500
  }
}
```

### 4.2 Configuration WiFi

#### PUT /config/wifi

Met à jour la configuration WiFi.

**Paramètres de requête :**
```json
{
  "ssid": "NouveauWiFi",
  "password": "motdepasse123",
  "auto_connect": true
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "message": "Configuration WiFi mise à jour. Redémarrage nécessaire.",
  "data": {
    "restart_required": true
  }
}
```

### 4.3 Configuration MQTT

#### PUT /config/mqtt

Configure l'intégration MQTT.

**Paramètres de requête :**
```json
{
  "enabled": true,
  "broker": "192.168.1.50",
  "port": 1883,
  "username": "thermopompe",
  "password": "mqttpass",
  "base_topic": "home/thermopompe"
}
```

## 5. Historique et Logs

### 5.1 Historique des Températures

#### GET /history/temperature

Retourne l'historique des températures.

**Paramètres de requête (query string) :**
- `from` : Date de début (ISO 8601)
- `to` : Date de fin (ISO 8601)
- `interval` : Intervalle d'agrégation (5m, 1h, 1d)
- `limit` : Nombre maximum d'entrées (défaut: 100)

**Exemple d'URL :**
```
GET /history/temperature?from=2024-01-15T00:00:00Z&to=2024-01-15T23:59:59Z&interval=1h
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "points": [
      {
        "timestamp": "2024-01-15T14:00:00Z",
        "current": 21.5,
        "target": 22.0
      },
      {
        "timestamp": "2024-01-15T15:00:00Z",
        "current": 22.1,
        "target": 22.0
      }
    ],
    "total_count": 24,
    "interval": "1h"
  }
}
```

### 5.2 Logs des Commandes

#### GET /logs/commands

Retourne l'historique des commandes IR envoyées.

**Paramètres de requête :**
- `limit` : Nombre d'entrées (défaut: 50, max: 200)
- `status` : Filtrer par statut (success, failed)

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "commands": [
      {
        "timestamp": "2024-01-15T14:30:00Z",
        "command": "set_temperature",
        "parameters": {
          "target": 24
        },
        "ir_code": "0x02FD48B7",
        "status": "success",
        "retry_count": 0,
        "response_time_ms": 45
      }
    ],
    "total_count": 150
  }
}
```

### 5.3 Logs Système

#### GET /logs/system

Retourne les logs système récents.

**Paramètres de requête :**
- `level` : Niveau minimum (debug, info, warning, error)
- `limit` : Nombre d'entrées (défaut: 100)

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "logs": [
      {
        "timestamp": "2024-01-15T14:30:00Z",
        "level": "info",
        "component": "IR",
        "message": "Commande envoyée avec succès",
        "details": {
          "command": "set_temperature",
          "value": 24
        }
      }
    ]
  }
}
```

## 6. Diagnostics

### 6.1 Test de Communication IR

#### POST /diagnostics/ir-test

Lance un test de communication infrarouge.

**Paramètres de requête :**
```json

{
  "test_sequence": ["power_on", "mode_heat", "temp_up", "power_off"],
  "delay_between_commands": 1000
}
```

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "test_results": [
      {
        "command": "power_on",
        "ir_code": "0x02FD08F7",
        "status": "success",
        "response_time_ms": 42
      },
      {
        "command": "mode_heat",
        "ir_code": "0x02FD48B7",
        "status": "success",
        "response_time_ms": 38
      }
    ],
    "overall_status": "success",
    "success_rate": 100.0
  }
}
```

### 6.2 Test de Connectivité Réseau

#### GET /diagnostics/network

Teste la connectivité réseau et retourne les informations de diagnostic.

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "wifi": {
      "connected": true,
      "ssid": "MonWiFi",
      "signal_strength": -45,
      "ip_address": "192.168.1.100",
      "gateway": "192.168.1.1",
      "dns": ["8.8.8.8", "8.8.4.4"]
    },
    "internet": {
      "reachable": true,
      "ping_google": 12,
      "ping_cloudflare": 8
    },
    "memory": {
      "free_heap": 45000,
      "total_heap": 320000,
      "fragmentation": 15
    }
  }
}
```

### 6.3 Information Système

#### GET /diagnostics/system

Retourne des informations détaillées sur le système.

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "hardware": {
      "chip_model": "ESP32-D0WDQ6",
      "cpu_frequency": 240,
      "flash_size": 4194304,
      "free_sketch_space": 1966080
    },
    "software": {
      "firmware_version": "1.2.3",
      "build_date": "2024-01-15",
      "arduino_version": "2.0.5",
      "sdk_version": "4.4.2"
    },
    "sensors": {
      "temperature_sensor": {
        "connected": true,
        "model": "DHT22",
        "last_reading": 21.5
      },
      "ir_emitter": {
        "pin": 4,
        "status": "operational"
      }
    }
  }
}
```

## 7. Gestion des Utilisateurs

### 7.1 Liste des Utilisateurs

#### GET /users

Retourne la liste des utilisateurs (Admin uniquement).

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "data": {
    "users": [
      {
        "id": 1,
        "username": "admin",
        "role": "admin",
        "last_login": "2024-01-15T14:30:00Z",
        "created_at": "2024-01-01T00:00:00Z"
      },
      {
        "id": 2,
        "username": "user1",
        "role": "user",
        "last_login": "2024-01-15T12:00:00Z",
        "created_at": "2024-01-05T10:00:00Z"
      }
    ]
  }
}
```

### 7.2 Création d'Utilisateur

#### POST /users

Crée un nouvel utilisateur (Admin uniquement).

**Paramètres de requête :**
```json
{
  "username": "nouvel_utilisateur",
  "password": "motdepasse123",
  "role": "user|admin|readonly"
}
```

**Réponse réussie (201) :**
```json
{
  "status": "success",
  "data": {
    "user": {
      "id": 3,
      "username": "nouvel_utilisateur",
      "role": "user",
      "created_at": "2024-01-15T14:30:00Z"
    }
  }
}
```

### 7.3 Modification d'Utilisateur

#### PUT /users/{id}

Modifie un utilisateur existant.

**Paramètres de requête :**
```json
{
  "role": "admin",
  "password": "nouveau_motdepasse"
}
```

### 7.4 Suppression d'Utilisateur

#### DELETE /users/{id}

Supprime un utilisateur (Admin uniquement).

**Réponse réussie (200) :**
```json
{
  "status": "success",
  "message": "Utilisateur supprimé avec succès"
}
```

## 8. WebSocket API

### 8.1 Connexion WebSocket

**URL de connexion :**
```
wss://thermopompe.local/ws
```

**Authentification :**
```javascript
const ws = new WebSocket('wss://thermopompe.local/ws');
ws.onopen = function() {
    // Authentification avec token JWT
    ws.send(JSON.stringify({
        type: 'auth',
        token: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'
    }));
};
```

### 8.2 Abonnements en Temps Réel

#### Abonnement aux Mises à Jour

```javascript
// S'abonner aux changements de température
ws.send(JSON.stringify({
    type: 'subscribe',
    channels: ['temperature', 'status', 'logs']
}));
```

#### Messages Reçus

```javascript
ws.onmessage = function(event) {
    const data = JSON.parse(event.data);
    
    switch(data.type) {
        case 'temperature_update':
            console.log('Nouvelle température:', data.payload.current);
            break;
        case 'status_change':
            console.log('Changement d\'état:', data.payload);
            break;
        case 'error':
            console.error('Erreur:', data.error);
            break;
    }
};
```

### 8.3 Envoi de Commandes via WebSocket

```javascript
// Envoyer une commande de changement de température
ws.send(JSON.stringify({
    type: 'command',
    action: 'set_temperature',
    payload: {
        target: 24,
        mode: 'heat'
    }
}));
```

## 9. Rate Limiting

### 9.1 Limites par Endpoint

| Endpoint | Limite | Fenêtre |
|----------|--------|---------|
| `/auth/login` | 5 tentatives | 15 minutes |
| `/temperature` | 10 requêtes | 1 minute |
| `/power` | 20 requêtes | 1 minute |
| `/mode` | 20 requêtes | 1 minute |
| `/status` | 60 requêtes | 1 minute |
| Autres endpoints | 100 requêtes | 1 minute |

### 9.2 Headers de Rate Limiting

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99
X-RateLimit-Reset: 1642262400
```

### 9.3 Réponse de Rate Limiting

**Erreur 429 :**
```json
{
  "status": "error",
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Trop de requêtes. Réessayez dans 60 secondes.",
    "retry_after": 60
  }
}
```

## 10. Codes d'Erreur Détaillés

### 10.1 Erreurs d'Authentification (3xxx)

| Code | Message | Description |
|------|---------|-------------|
| 3001 | INVALID_CREDENTIALS | Nom d'utilisateur ou mot de passe incorrect |
| 3002 | TOKEN_EXPIRED | Le token JWT a expiré |
| 3003 | INVALID_TOKEN | Token JWT invalide ou malformé |
| 3004 | INSUFFICIENT_PERMISSIONS | Permissions insuffisantes pour cette action |

### 10.2 Erreurs de Validation (4xxx)

| Code | Message | Description |
|------|---------|-------------|
| 4001 | INVALID_PARAMETER | Paramètre requis manquant ou invalide |
| 4002 | TEMPERATURE_OUT_OF_RANGE | Température hors de la plage autorisée (16-30°C) |
| 4003 | INVALID_MODE | Mode de fonctionnement non supporté |
| 4004 | INVALID_FAN_SPEED | Vitesse de ventilateur non supportée |

### 10.3 Erreurs Système (5xxx)

| Code | Message | Description |
|------|---------|-------------|
| 5001 | IR_COMMUNICATION_FAILED | Échec de communication infrarouge |
| 5002 | SENSOR_UNAVAILABLE | Capteur de température indisponible |
| 5003 | WIFI_DISCONNECTED | Connexion WiFi perdue |
| 5004 | MEMORY_INSUFFICIENT | Mémoire système insuffisante |

### 10.4 Exemple de Réponse d'Erreur

```json
{
  "status": "error",
  "error": {
    "code": "TEMPERATURE_OUT_OF_RANGE",
    "message": "La température doit être comprise entre 16 et 30°C",
    "details": {
      "provided_value": 35,
      "min_value": 16,
      "max_value": 30
    }
  },
  "timestamp": "2024-01-15T14:30:00Z",
  "request_id": "uuid-123456"
}
```

## 11. Exemples d'Intégration

### 11.1 Client JavaScript

```javascript
class ThermopompeAPI {
    constructor(baseUrl, token) {
        this.baseUrl = baseUrl;
        this.token = token;
    }
    
    async request(endpoint, method = 'GET', data = null) {
        const options = {
            method,
            headers: {
                'Authorization': `Bearer ${this.token}`,
                'Content-Type': 'application/json'
            }
        };
        
        if (data) {
            options.body = JSON.stringify(data);
        }
        
        const response = await fetch(`${this.baseUrl}${endpoint}`, options);
        return await response.json();
    }
    
    async getStatus() {
        return this.request('/status');
    }
    
    async setTemperature(target) {
        return this.request('/temperature', 'PUT', { target });
    }
    
    async setPower(power) {
        return this.request('/power', 'POST', { power });
    }
    
    async setMode(mode) {
        return this.request('/mode', 'PUT', { mode });
    }
}

// Utilisation
const api = new ThermopompeAPI('https://thermopompe.local/api/v1', 'your-jwt-token');

api.getStatus().then(status => {
    console.log('État actuel:', status.data);
});

api.setTemperature(24).then(result => {
    console.log('Température modifiée:', result);
});
```

### 11.2 Client Python

```python
import requests
import json

class ThermopompeAPI:
    def __init__(self, base_url, token):
        self.base_url = base_url
        self.session = requests.Session()
        self.session.headers.update({
            'Authorization': f'Bearer {token}',
            'Content-Type': 'application/json'
        })
    
    def get_status(self):
        response = self.session.get(f'{self.base_url}/status')
        return response.json()
    
    def set_temperature(self, target):
        data = {'target': target}
        response = self.session.put(f'{self.base_url}/temperature', json=data)
        return response.json()
    
    def set_power(self, power):
        data = {'power': power}
        response = self.session.post(f'{self.base_url}/power', json=data)
        return response.json()

# Utilisation
api = ThermopompeAPI('https://thermopompe.local/api/v1', 'your-jwt-token')

status = api.get_status()
print(f"Température actuelle: {status['data']['temperature']['current']}°C")

result = api.set_temperature(24)
print(f"Résultat: {result['status']}")
```

### 11.3 Intégration Home Assistant

```yaml
# configuration.yaml
rest:
  - resource: "https://thermopompe.local/api/v1/status"
    headers:
      Authorization: "Bearer YOUR_JWT_TOKEN"
    sensor:
      - name: "Thermopompe Temperature"
        value_template: "{{ value_json.data.temperature.current }}"
        unit_of_measurement: "°C"
      - name: "Thermopompe Target"
        value_template: "{{ value_json.data.temperature.target }}"
        unit_of_measurement: "°C"

rest_command:
  thermopompe_set_temp:
    url: "https://thermopompe.local/api/v1/temperature"
    method: "PUT"
    headers:
      Authorization: "Bearer YOUR_JWT_TOKEN"
      Content-Type: "application/json"
    payload: '{"target": {{ target }}}'
```

## 12. Versioning de l'API

### 12.1 Versioning par URL

L'API utilise un versioning par URL path:
- Version actuelle: `/api/v1/`
- Versions futures: `/api/v2/`, `/api/v3/`, etc.

### 12.2 Rétrocompatibilité

- Les versions précédentes restent supportées pendant au minimum 12 mois
- Les changements breaking sont uniquement introduits dans les nouvelles versions majeures
- Les dépréciations sont annoncées via les headers de réponse

### 12.3 Headers de Version

```http
API-Version: 1.0
API-Supported-Versions: 1.0
API-Deprecated-Versions: none
```

## 13. Support et Documentation

### 13.1 Endpoints de Documentation

#### GET /docs

Retourne la documentation interactive de l'API (Swagger/OpenAPI).

#### GET /docs/openapi.json

Retourne la spécification OpenAPI au format JSON.

### 13.2 Monitoring et Métriques

#### GET /health

Point de contrôle de santé du service.

**Réponse réussie (200) :**
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T14:30:00Z",
  "uptime": 86400,
  "version": "1.2.3"
}
```

#### GET /metrics

Retourne les métriques de performance (format Prometheus).

```
# HELP
thermopompe_api_requests_total Total number of API requests
# TYPE thermopompe_api_requests_total counter
thermopompe_api_requests_total{method="GET",endpoint="/status"} 1250
thermopompe_api_requests_total{method="PUT",endpoint="/temperature"} 45

# HELP thermopompe_ir_commands_total Total number of IR commands sent
# TYPE thermopompe_ir_commands_total counter
thermopompe_ir_commands_total{status="success"} 412
thermopompe_ir_commands_total{status="failed"} 8

# HELP thermopompe_temperature_celsius Current temperature reading
# TYPE thermopompe_temperature_celsius gauge
thermopompe_temperature_celsius 21.5
```

## 14. Sécurité

### 14.1 Bonnes Pratiques

- **Toujours utiliser HTTPS** en production
- **Stocker les tokens JWT de manière sécurisée** (jamais dans localStorage en production)
- **Implémenter un timeout** pour les requêtes (recommandé: 30 secondes)
- **Valider côté client ET serveur** tous les paramètres
- **Utiliser des tokens avec expiration courte** (1 heure recommandée)

### 14.2 Headers de Sécurité

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Content-Security-Policy: default-src 'self'
```

### 14.3 Audit et Logging

Toutes les actions sensibles sont loggées :
- Tentatives de connexion (réussies et échouées)
- Modifications de configuration
- Commandes de contrôle envoyées
- Accès aux endpoints d'administration

## 15. Limitations Connues

### 15.1 Limitations Techniques

- **Commandes IR séquentielles** : Délai minimum de 500ms entre les commandes
- **Connexions WebSocket simultanées** : Maximum 10 connexions
- **Taille des logs** : Maximum 1000 entrées en mémoire
- **Fréquence de lecture des capteurs** : Une fois toutes les 30 secondes

### 15.2 Limitations Fonctionnelles

- **Pas de support multi-pompes** dans cette version
- **Authentification locale uniquement** (pas d'OAuth2/SAML)
- **Historique limité** à 7 jours en mémoire locale
- **Configuration WiFi** nécessite un redémarrage

## 16. Migration et Évolutions

### 16.1 Changelog API

#### Version 1.2.3 (Actuelle)
- Ajout de l'endpoint `/diagnostics/system`
- Amélioration de la gestion d'erreurs IR
- Support des headers de rate limiting

#### Version 1.2.0
- Ajout du support WebSocket
- Endpoints de gestion des utilisateurs
- Configuration MQTT

#### Version 1.1.0
- API de base avec endpoints essentiels
- Authentification JWT
- Historique des températures

### 16.2 Évolutions Prévues

#### Version 1.3.0 (Prochaine)
- Support multi-pompes
- Intégration Google Assistant/Alexa
- Algorithmes d'optimisation énergétique

#### Version 2.0.0 (Future)
- Refonte complète avec GraphQL
- Authentification OAuth2
- Interface mobile native

## 17. Exemples Avancés

### 17.1 Monitoring avec Grafana

```json
{
  "dashboard": {
    "title": "Thermopompe Monitoring",
    "panels": [
      {
        "title": "Température",
        "type": "graph",
        "targets": [
          {
            "expr": "thermopompe_temperature_celsius",
            "legendFormat": "Température actuelle"
          }
        ]
      }
    ]
  }
}
```

### 17.2 Automatisation avec Node-RED

```json
[
  {
    "id": "temp_monitor",
    "type": "http request",
    "method": "GET",
    "url": "https://thermopompe.local/api/v1/status",
    "headers": {
      "Authorization": "Bearer {{token}}"
    }
  },
  {
    "id": "temp_controller",
    "type": "function",
    "func": "if (msg.payload.data.temperature.current < 20) { return {payload: {target: 22}}; }"
  }
]
```

### 17.3 Script Shell de Monitoring

```bash
#!/bin/bash

TOKEN="your-jwt-token"
API_URL="https://thermopompe.local/api/v1"

# Fonction de vérification de l'état
check_status() {
    curl -s -H "Authorization: Bearer $TOKEN" \
         "$API_URL/status" | jq '.data.temperature.current'
}

# Fonction d'alerte si température anormale
check_temperature() {
    temp=$(check_status)
    if (( $(echo "$temp < 18" | bc -l) )); then
        echo "ALERTE: Température trop basse: ${temp}°C" | mail -s "Alerte Thermopompe" admin@example.com
    fi
}

# Exécution
check_temperature
```

## Références

- [Protocoles de Communication](protocoles-communication.md)
- [Architecture Système](architecture-systeme.md)
- [Guide Matériel](guide-materiel.md)
- [RFC 7519 - JSON Web Tokens](https://tools.ietf.org/html/rfc7519)
- [OpenAPI Specification](https://swagger.io/specification/)

## Support

Pour toute question concernant l'API :
1. Consultez cette documentation
2. Vérifiez les logs système via `/logs/system`
3. Testez la connectivité avec `/diagnostics/network`
4. Utilisez l'endpoint `/health` pour vérifier l'état du service

---

**Note** : Cette documentation constitue la référence complète de l'API REST. Maintenir à jour lors des évolutions du firmware.