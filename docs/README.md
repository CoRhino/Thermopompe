# Documentation Système Thermopompe

## Vue d'Ensemble

Bienvenue dans la documentation complète du système de contrôle domotique pour pompe à chaleur Elios. Cette documentation vous guide de l'installation initiale jusqu'à l'utilisation avancée et la maintenance du système.

## Structure de la Documentation

La documentation est organisée en trois sections principales :

### 📚 [Guide Utilisateur](guide-utilisateur/)
Documentation destinée aux utilisateurs finaux pour installer, configurer et utiliser le système.

- **[Installation](guide-utilisateur/installation.md)** - Guide d'installation complet pas à pas
- **[Configuration](guide-utilisateur/configuration.md)** - Configuration initiale et paramétrage
- **[Utilisation](guide-utilisateur/utilisation.md)** - Guide d'utilisation quotidienne
- **[Dépannage](guide-utilisateur/depannage.md)** - Solutions aux problèmes courants

### 🔧 [Guide Technique](guide-technique/)
Documentation technique pour développeurs et intégrateurs.

- **[Architecture Système](guide-technique/architecture-systeme.md)** - Vue d'ensemble de l'architecture
- **[Guide Matériel](guide-technique/guide-materiel.md)** - Composants et spécifications techniques
- **[Protocoles Communication](guide-technique/protocoles-communication.md)** - Protocoles IR et réseau
- **[Référence API](guide-technique/api-reference.md)** - Documentation complète de l'API REST

### 📖 [Ressources](ressources/)
Ressources complémentaires et références.

- **[FAQ](ressources/faq.md)** - Questions fréquemment posées
- **[Glossaire](ressources/glossaire.md)** - Terminologie technique
- **[Références](ressources/references.md)** - Liens et ressources externes

## Navigation Rapide

### Pour Débuter
Si vous installez le système pour la première fois :

1. 📋 **Prérequis** : Consultez les [prérequis matériels](guide-utilisateur/installation.md#prérequis)
2. 🔨 **Installation** : Suivez le [guide d'installation](guide-utilisateur/installation.md)
3. ⚙️ **Configuration** : Configurez votre système avec le [guide de configuration](guide-utilisateur/configuration.md)
4. 🎯 **Première utilisation** : Apprenez les bases dans le [guide d'utilisation](guide-utilisateur/utilisation.md)

### Pour Résoudre un Problème
Si vous rencontrez des difficultés :

1. 🔍 **FAQ** : Consultez les [questions fréquentes](ressources/faq.md)
2. 🛠️ **Dépannage** : Suivez le [guide de dépannage](guide-utilisateur/depannage.md)
3. 📊 **Diagnostic** : Utilisez les outils de [diagnostic système](guide-utilisateur/depannage.md#diagnostic-rapide)

### Pour Développer ou Intégrer
Si vous souhaitez développer ou intégrer le système :

1. 🏗️ **Architecture** : Comprenez l'[architecture système](guide-technique/architecture-systeme.md)
2. 🔌 **API** : Explorez la [référence API](guide-technique/api-reference.md)
3. 📡 **Protocoles** : Maîtrisez les [protocoles de communication](guide-technique/protocoles-communication.md)
4. 🔧 **Matériel** : Consultez le [guide matériel](guide-technique/guide-materiel.md)

## Fonctionnalités Principales

### ✅ Contrôle Infrarouge
- Remplacement complet de la télécommande originale
- Portée étendue jusqu'à 15+ mètres
- Codes IR optimisés pour Elios DE12HIW23230E3

### ✅ Interface Web Responsive
- Contrôle depuis tout navigateur web
- Interface adaptée mobile et desktop
- Mises à jour en temps réel

### ✅ API REST Complète
- Intégration domotique facilitée
- Authentification sécurisée JWT
- Documentation OpenAPI

### ✅ Monitoring et Historique
- Suivi des températures
- Logs détaillés des commandes
- Diagnostic automatique

### 🔄 Fonctionnalités en Développement
- Support multi-pompes
- Intégration Google Assistant/Alexa
- Application mobile native
- Intelligence artificielle embarquée

## État du Projet

| Composant | État | Version |
|-----------|------|---------|
| Firmware ESP32 | ✅ Stable | 1.2.3 |
| API REST | ✅ Stable | v1 |
| Interface Web | ✅ Stable | 2.1.0 |
| Documentation | ✅ Complète | Phase 4 |
| Tests Hardware | ✅ Validé | - |

## Compatibilité

### Pompes à Chaleur Supportées
- **Elios DE12HIW23230E3** ✅ Entièrement supporté
- **Autres modèles Elios** 🔄 Adaptation possible
- **Autres marques** ⚠️ Nécessite capture codes IR spécifiques

### Plateformes Domotiques
- **Home Assistant** ✅ Intégration REST/MQTT
- **OpenHAB** ✅ Via API REST
- **Jeedom** ✅ Plugin possible
- **Node-RED** ✅ Nodes personnalisés

## Installation Rapide

Pour une installation express, suivez ces étapes essentielles :

```bash
# 1. Matériel minimum requis
ESP32 + LED IR TSAL6400 + Récepteur TSOP4838 + Composants de base

# 2. Réseau
WiFi 2.4GHz avec signal > -70dBm

# 3. Configuration
Connexion à "Thermopompe-Setup" → 192.168.4.1
```

📖 **Guide complet** : [Installation détaillée](guide-utilisateur/installation.md)

## Support et Contributions

### Obtenir de l'Aide
1. **Documentation** : Consultez cette documentation complète
2. **FAQ** : Vérifiez les [questions fréquentes](ressources/faq.md)
3. **Diagnostic** : Utilisez les outils intégrés `/diagnostics/system`
4. **Communauté** : Consultez les [ressources communautaires](ressources/references.md#ressources-communautaires)

### Contribuer au Projet
- 🐛 **Signaler des bugs** : Issues GitHub
- 💡 **Proposer des améliorations** : Pull requests
- 📝 **Améliorer la documentation** : Contributions bienvenues
- 🧪 **Tester sur nouveaux matériels** : Retours d'expérience

📋 **Guide de contribution** : [CONTRIBUTING.md](../CONTRIBUTING.md)

## Sécurité et Confidentialité

- 🔒 **Authentification** : JWT avec expiration configurable
- 🛡️ **Chiffrement** : HTTPS recommandé en production
- 🔐 **Données locales** : Aucune donnée transmise vers l'extérieur
- 🔄 **Mises à jour** : OTA sécurisées avec vérification d'intégrité

## Licence et Crédits

- **Licence** : [À préciser - voir LICENSE file]
- **Développement** : Projet communautaire open-source
- **Bibliothèques** : Basé sur IRremoteESP8266, Arduino Framework
- **Remerciements** : Communautés ESP32 et domotique

---

## Mise à Jour de la Documentation

Cette documentation suit le plan de restructuration Phase 4 et est maintenue à jour avec les évolutions du projet.

**Dernière mise à jour** : Phase 4 - Finalisation et Ressources
**Version documentation** : 2.0.0
**Compatibilité firmware** : 1.2.3+

Pour toute suggestion d'amélioration de cette documentation, consultez le [guide de contribution](../CONTRIBUTING.md).