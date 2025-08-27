# FAQ - Questions Fréquentes - Système Thermopompe

## Vue d'Ensemble

Cette FAQ regroupe les questions les plus fréquemment posées concernant l'installation, la configuration, l'utilisation et le dépannage du système de contrôle domotique pour pompe à chaleur Elios.

## Questions sur l'Installation et Configuration

### Q1 : Quels sont les prérequis minimum pour installer le système ?

**Réponse :**
- **Matériel** : ESP32, LED IR TSAL6400, récepteur IR TSOP4838, composants électroniques de base
- **Réseau** : WiFi 2.4GHz (WPA2/WPA3) avec signal > -70dBm
- **Alimentation** : Prise 5V disponible près de la pompe
- **Compétences** : Connaissances de base en électronique et soudure
- **Outils** : Fer à souder, multimètre, tournevis de précision

### Q2 : Puis-je utiliser un ESP8266 au lieu d'un ESP32 ?

**Réponse :**
Oui, l'ESP8266 est compatible mais avec des limitations :
- **Avantages** : Coût réduit (5-10€ vs 12€), WiFi intégré
- **Inconvénients** : Moins de GPIO, processeur simple cœur, performances moindres
- **Recommandation** : ESP32 pour un projet stable et évolutif

### Q3 : Quelle est la portée maximale de l'émetteur IR ?

**Réponse :**
La portée dépend de la configuration :
- **LED simple** : 3-5 mètres
- **LED avec transistor d'amplification** : 8-12 mètres
- **Array de LEDs multiples** : 15+ mètres
- **Facteurs influents** : Alignement, obstacles, éclairage ambiant

### Q4 : Le système fonctionne-t-il avec d'autres marques de pompes à chaleur ?

**Réponse :**
Le système est conçu pour l'Elios DE12HIW23230E3 mais peut être adapté :
- **Adaptation requise** : Capture des codes IR spécifiques à votre modèle
- **Processus** : Utiliser le récepteur IR pour enregistrer les commandes de votre télécommande
- **Compatibilité** : La plupart des pompes utilisant l'IR à 38kHz sont supportables

## Questions sur l'Utilisation

### Q5 : Pourquoi la pompe émet-elle toujours un bip au démarrage ?

**Réponse :**
**Problème** : Utilisation du bouton "Power ON" au lieu du "Timer"
**Solution** : 
- Utiliser exclusivement le bouton "Timer 30min" pour un démarrage silencieux
- Séquence : Double-clic Timer → Attente 30min → Démarrage automatique sans bip
- **Avantage** : Évite les nuisances sonores

### Q6 : Comment optimiser la consommation énergétique de la pompe ?

**Réponse :**
**Réglages recommandés** :
- Réduire la température cible de 1-2°C
- Programmer des plages horaires optimisées
- Utiliser le mode automatique quand possible
- Maintenir les filtres propres

**Monitoring** : Utiliser l'historique des températures pour analyser les cycles

### Q7 : L'interface web est lente, comment l'optimiser ?

**Réponse :**
**Vérifications** :
- Signal WiFi : minimum -60dBm recommandé
- Navigateur : Vider le cache, utiliser Chrome/Firefox récent
- Module : Redémarrer l'ESP32 si uptime > 30 jours
- Réseau : Vérifier la bande passante disponible

## Questions Techniques

### Q8 : Les automatisations ne se déclenchent pas, que faire ?

**Réponse :**
**Vérifications système** :
- **Heure** : Synchronisation NTP correcte
- **Programmation** : Horaires configurés et mode automatique activé
- **Codes IR** : Tester manuellement les commandes
- **Logs** : Consulter `/logs/system` pour identifier les erreurs

### Q9 : Comment diagnostiquer les problèmes de connectivité WiFi ?

**Réponse :**
**Tests de diagnostic** :
1. **Signal** : Vérifier la force du signal (-60dBm minimum)
2. **Fréquence** : S'assurer du réseau 2.4GHz (pas 5GHz)
3. **Reset** : Mode récupération avec bouton BOOT 10 secondes
4. **Reconfiguration** : Se connecter à "Thermopompe-Setup" → 192.168.4.1

### Q10 : Que signifient les codes d'erreur du système ?

**Réponse :**
**Codes principaux** :
- **E001** : Échec WiFi → Reset configuration réseau
- **E002** : Capteur DHT22 non détecté → Vérifier connexions GPIO18
- **E003** : Mémoire insuffisante → Redémarrage ESP32
- **E004** : Erreur sauvegarde → Factory reset
- **E005** : Module IR défaillant → Test hardware complet

### Q11 : Comment étendre la portée IR si elle est insuffisante ?

**Réponse :**
**Solutions d'amplification** :
1. **Transistor 2N2222** : Ajouter un driver d'amplification
2. **LEDs multiples** : Configuration en parallèle
3. **Positionnement** : Vérifier l'alignement et éliminer les obstacles
4. **Alimentation** : S'assurer de la stabilité 5V

**Code d'optimisation** : Répéter les commandes 3 fois avec délai 100ms

## Questions sur la Sécurité et Maintenance

### Q12 : Le système est-il sécurisé ? Comment protéger l'accès ?

**Réponse :**
**Sécurité intégrée** :
- Authentification JWT avec expiration
- HTTPS recommandé en production
- Rate limiting sur les endpoints critiques
- Logs d'audit des actions sensibles

**Bonnes pratiques** :
- Changer les mots de passe par défaut
- Utiliser un réseau WiFi sécurisé (WPA3)
- Mettre à jour le firmware régulièrement

### Q13 : Quelle maintenance est nécessaire ?

**Réponse :**
**Maintenance mensuelle** :
- Vérifier les connexions
- Nettoyer la lentille IR
- Tester les fonctionnalités de base
- Sauvegarder la configuration

**Maintenance trimestrielle** :
- Calibrer les capteurs
- Mettre à jour le firmware
- Vérifier l'étanchéité du boîtier
- Tester l'alimentation de secours (si présente)

**Maintenance annuelle** :
- Remplacer les piles/batteries
- Inspection visuelle des composants
- Évaluer les améliorations possibles

## Questions sur l'Intégration Domotique

### Q14 : Comment intégrer le système avec Home Assistant ?

**Réponse :**
**Configuration REST** :
```yaml
rest:
  - resource: "https://thermopompe.local/api/v1/status"
    headers:
      Authorization: "Bearer YOUR_JWT_TOKEN"
    sensor:
      - name: "Thermopompe Temperature"
        value_template: "{{ value_json.data.temperature.current }}"
```

**Références** : Voir [documentation API](../guide-technique/api-reference.md)

### Q15 : Le système supporte-t-il MQTT ?

**Réponse :**
**Support MQTT** : Configuration via l'endpoint `/config/mqtt`
**Paramètres** :
- Broker MQTT (adresse IP)
- Port (défaut 1883)
- Authentification (username/password)
- Topic de base (ex: "home/thermopompe")

### Q16 : Peut-on contrôler plusieurs pompes avec un seul système ?

**Réponse :**
**Limitation actuelle** : Une pompe par module ESP32
**Solutions multi-pompes** :
- Installer un module par pompe
- Centraliser via système domotique
- **Évolution prévue** : Support multi-pompes dans version 1.3.0

## Questions sur les Performances

### Q17 : Quelle est la consommation électrique du module ?

**Réponse :**
**Consommation typique** :
- **Mode veille** : 10-50mA
- **WiFi actif** : 80-160mA
- **Transmission IR** : 100-200mA (pics courts)
- **Consommation annuelle** : ~5-10 kWh

### Q18 : Combien de temps conserve-t-il l'historique des données ?

**Réponse :**
**Stockage local** :
- **Historique température** : 7 jours en mémoire
- **Logs commandes** : 1000 dernières entrées
- **Logs système** : 500 entrées récentes

**Extension** : Intégration base de données externe pour historique long terme

### Q19 : Le système fonctionne-t-il hors ligne ?

**Réponse :**
**Fonctionnement hors ligne** :
- **Contrôle local** : Interface web accessible en local
- **Programmations** : Continuent de fonctionner
- **Limitations** : Pas de contrôle distant, pas de synchronisation NTP

**Recommandation** : Connexion internet pour optimisation complète

## Questions sur le Dépannage

### Q20 : La LED IR ne semble pas fonctionner, comment tester ?

**Réponse :**
**Test caméra smartphone** :
1. Ouvrir l'application caméra
2. Pointer vers la LED IR
3. Déclencher une commande
4. **Résultat attendu** : LED visible (lumière blanche/violette)

**Si invisible** :
- Vérifier polarité LED (anode = patte longue)
- Contrôler résistance 220Ω
- Tester continuité avec multimètre

### Q21 : L'ESP32 ne démarre pas, que faire ?

**Réponse :**
**Diagnostic alimentation** :
- Vérifier tension 5V ±0.2V avec multimètre
- Contrôler connexions GND
- Tester sans composants externes
- Vérifier consommation < 500mA

**Diagnostic firmware** :
- Reflasher le firmware
- Vérifier port série (115200 bauds)
- Mode recovery : maintenir BOOT + EN

### Q22 : Les capteurs donnent des valeurs erronées ?

**Réponse :**
**Validation DHT22** :
- Plage normale : -40°C à +80°C, 0-100% humidité
- Vérifier connexions 3.3V, GND, DATA (GPIO18)
- Résistance pull-up 10kΩ entre VCC et DATA
- Remplacer si écart > 5°C avec référence

**Calibration** : Ajuster les offsets dans la configuration

## Questions sur les Évolutions

### Q23 : Quelles améliorations sont prévues ?

**Réponse :**
**Version 1.3.0 (Prochaine)** :
- Support multi-pompes
- Intégration Google Assistant/Alexa  
- Algorithmes d'optimisation énergétique
- Interface mobile native

**Version 2.0.0 (Future)** :
- Refonte avec GraphQL
- Authentification OAuth2
- Intelligence artificielle embarquée
- Protocol Matter/Thread

### Q24 : Comment contribuer au projet ?

**Réponse :**
**Contributions possibles** :
- Signaler des bugs via les issues
- Proposer des améliorations
- Tester sur d'autres modèles de pompes
- Améliorer la documentation

**Processus** : Voir [CONTRIBUTING.md](../../CONTRIBUTING.md) pour les détails

### Q25 : Où trouver de l'aide supplémentaire ?

**Réponse :**
**Ressources disponibles** :
1. **Documentation** : [Guide complet](../guide-utilisateur/)
2. **API Reference** : [Documentation technique](../guide-technique/api-reference.md)
3. **Diagnostic** : Endpoint `/diagnostics/system` pour état détaillé
4. **Logs** : Consulter `/logs/system` pour erreurs spécifiques
5. **Communauté** : Forums et groupes de discussion dédiés

---

**Note** : Cette FAQ est mise à jour régulièrement. Pour des questions spécifiques non couvertes, consultez la documentation technique ou les logs système du module.