
# DreamScape - Plateforme de voyage immersive

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/dreamscape)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-0.1.0--alpha-orange)](https://github.com/dreamscape)

## 📘 Présentation

DreamScape est une plateforme innovante de voyage combinant intelligence artificielle contextuelle et expériences panoramiques immersives pour offrir une expérience de planification et de découverte de voyage entièrement repensée. Notre solution permet aux utilisateurs de prévisualiser leurs destinations grâce à des panoramas 360° tout en recevant des recommandations personnalisées basées sur leurs préférences et le contexte de leur voyage.

## 🌟 Caractéristiques principales

- **Recommandations IA personnalisées**: Suggestions ultra-ciblées de destinations, hébergements et activités
- **Expériences panoramiques immersives**: Prévisualisation 360° des destinations et points d'intérêt
- **Navigation Globe-Panorama**: Passage fluide d'une vue globe terrestre aux panoramas détaillés
- **Intégration Amadeus complète**: Accès à un catalogue mondial de vols, hébergements et activités
- **Analyse contextuelle en temps réel**: Prise en compte de facteurs comme la météo et les événements locaux
- **Architecture microservices**: Services autonomes, découplés et indépendamment déployables

## 🏗️ Architecture microservices

DreamScape est construit selon une véritable architecture microservices :

- **Indépendance des services** : Chaque service possède son propre dépôt Git, base de données et cycle de déploiement
- **Communication asynchrone** : Communication principalement événementielle via Kafka
- **Résilience** : Tolérance aux pannes avec Circuit Breaker pattern
- **Containerisation** : Chaque service est packagé dans son propre conteneur Docker
- **Orchestration** : Déploiement via Kubernetes (K3s) sur Oracle Cloud Infrastructure
- **API Gateway** : Centralisation des requêtes et routage vers les services appropriés
- **Base de données par service** : Chaque service gère sa propre persistance de données

### Liste des microservices

| Service | Description | Technologies | Repo |
|---------|-------------|--------------|------|
| API Gateway | Point d'entrée unique, routage des requêtes | NGINX | [dreamscape-gateway](https://github.com/DREAMSCAPE-AI/dreamscape-gateway) |
| Auth Service | Authentification et autorisation | Node.js, JWT, OAuth2 | [dreamscape-auth](https://github.com/dreamscape/auth-service) |
| User Service | Gestion des profils et préférences | Node.js, Express, PostgreSQL | [dreamscape-user](https://github.com/dreamscape/user-service) |
| Voyage Service | Intégration Amadeus, réservations | Node.js, Express, MongoDB | [dreamscape-voyage](https://github.com/dreamscape/voyage-service) |
| AI Service | Recommandations personnalisées | Node.js/Python, TensorFlow.js | [dreamscape-ai](https://github.com/dreamscape/ai-service) |
| Panorama Service | Gestion des expériences panoramiques | Node.js, Marzipano, CesiumJS | [dreamscape-panorama](https://github.com/dreamscape/panorama-service) |
| Payment Service | Intégration Stripe, transactions | Node.js, Express, PostgreSQL | [dreamscape-payment](https://github.com/dreamscape/payment-service) |
| Frontend | Interface utilisateur web | React, Redux Toolkit, TailwindCSS | [dreamscape-web](https://github.com/dreamscape/web-client) |

### Communication inter-services

- **Synchrone (API)** : REST/GraphQL pour les requêtes nécessitant une réponse immédiate
- **Asynchrone (Events)** : Kafka pour la communication événementielle
- **Patterns implémentés** :
  - Saga Pattern pour les transactions distribuées
  - CQRS pour séparation lecture/écriture
  - Circuit Breaker pour la résilience
  - Event Sourcing pour l'historisation des changements d'état

## 🚀 Pour démarrer (pour les développeurs)

Chaque service possède son propre repository et instructions de démarrage. Voici comment configurer l'environnement de développement local :

### Configuration de l'environnement

```bash
# Cloner le repo d'infrastructure
git clone https://github.com/dreamscape/infrastructure.git
cd infrastructure/local

# Démarrer l'infrastructure locale (Kafka, DBs, monitoring)
docker-compose up -d

# Cloner et configurer les services individuels
./setup-local-dev.sh  # Script qui clone les repos nécessaires
```

### Démarrage des services individuels

Chaque service se démarre indépendamment. Exemple pour le service utilisateur :

```bash
cd ../user-service
cp .env.example .env
# Éditer .env avec les configurations locales
npm install
npm run dev
```

### Utilitaires de développement

```bash
# Interface de visualisation Kafka
npm run kafka-ui

# Tableau de bord local Kubernetes
npm run k8s-dashboard

# Tests d'intégration multi-services
cd infrastructure/tests
npm run integration-tests
```

## 📂 Structure de l'écosystème

Notre architecture microservices est organisée de cette façon :

```
dreamscape/
│
├── infrastructure/         # Repo de configuration globale
│   ├── k8s/                # Manifestes Kubernetes
│   ├── terraform/          # Configuration OCI et Cloudflare
│   ├── monitoring/         # Prometheus, Grafana, ELK
│   ├── messaging/          # Configuration Kafka
│   └── local/              # Environnement de développement local
│
├── service-template/       # Template pour nouveaux microservices
│
├── shared-libraries/       # Bibliothèques partagées (publiées via npm)
│   ├── event-schemas/      # Schémas des événements Kafka
│   ├── api-standards/      # Standards d'API et validateurs
│   └── observability/      # Outils de logging et monitoring
│
├── tests/                  # Tests d'intégration inter-services
│   ├── e2e/                # Tests end-to-end
│   ├── performance/        # Tests de charge et performance
│   └── contract/           # Tests de contrat d'API
│
└── documentation/          # Documentation centralisée
    ├── architecture/       # Documentation d'architecture
    ├── api/                # Documentation API (OpenAPI)
    ├── events/             # Catalogue d'événements
    └── guidelines/         # Guidelines et bonnes pratiques
```

## 🌐 Expériences panoramiques

Notre approche pour les expériences immersives combine:

- **CesiumJS**: Pour la navigation sur globe terrestre et carte 3D
- **Marzipano**: Pour les panoramas 360° haute qualité
- **Sources d'images gratuites**: 
  - Wikimedia Commons
  - KartaView/Mapillary (API gratuite)
  - Photos panoramiques propres (sous licence libre)

Cette approche hybride permet une expérience immersive de qualité tout en respectant nos contraintes budgétaires et techniques. Elle offre aux utilisateurs une prévisualisation des destinations avant leur voyage, réduisant ainsi l'incertitude et augmentant le taux de conversion des réservations.

## 🧠 Intelligence artificielle

Notre système de recommandation utilise une approche hybride:

- **Filtrage collaboratif**: Recommandations basées sur les comportements similaires entre utilisateurs
- **Filtrage basé sur le contenu**: Analyse des caractéristiques intrinsèques des destinations et activités
- **Neural Collaborative Filtering**: Modèle avancé combinant deep learning et filtrage collaboratif
- **Analyse contextuelle**: Prise en compte des facteurs comme la météo, les événements locaux et la saisonnalité

Ces algorithmes s'adaptent en temps réel pour fournir des recommandations personnalisées exceptionnelles.

## ⚙️ Modèle de développement

- **Approche agile**: Sprints de 4 semaines (8 jours de travail effectif)
- **Rythme de développement**: 2 jours de travail par semaine
- **Phasage**: Documentation (Janvier-Juin 2025) → MVP (Juin 2025-Avril 2026)
- **Livraison continue**: Chaque service suit son propre cycle de CI/CD
- **Feature Flags**: Activation progressive des fonctionnalités

## 📝 Conventions de code

- **TypeScript**: Utilisez la dernière version et respectez les règles ESLint du projet
- **Commits**: Format conventionnel `type(scope): message` (ex: `feat(auth): add oauth support`)
- **Branches**: Nommage `feature/nom-feature`, `bugfix/description-bug`, `release/x.y.z`
- **Documentation**: JSDoc pour les fonctions et composants principaux
- **Internationalisation**: Utilisez les clés i18n pour tous les textes visibles
- **Accessibilité**: Conformité WCAG 2.1 niveau AA

## 🧪 Stratégie de test

- **Tests unitaires**: Jest pour le backend, React Testing Library pour le frontend
- **Tests d'intégration**: Tests entre services via environnement d'intégration dédié
- **Tests contract**: Vérification des contrats d'API entre services (Pact)
- **Tests end-to-end**: Cypress pour les parcours utilisateur complets
- **Tests d'accessibilité**: Axe pour la conformité WCAG niveau AA
- **Tests de performance**: k6 + Lighthouse

## 🛡️ Sécurité et observabilité

- **Centralisation des logs**: ELK Stack pour agrégation des logs de tous les services
- **Distributed Tracing**: Jaeger pour suivre les requêtes à travers les services
- **Métriques**: Prometheus/Grafana pour monitoring en temps réel
- **Alerting**: Alertmanager avec intégration Slack
- **RGPD**: Mécanismes de consentement, gestion des données personnelles
- **Sécurité web**: Protection Cloudflare, WAF, HTTPS obligatoire
- **Multilingue**: Support français/anglais avec structure extensible

## 🔄 Workflow d'intégration

1. Développer et tester localement dans le repository du service concerné
2. Soumettre une Pull Request vers la branche `develop` du service
3. Revue de code par les pairs
4. Tests automatisés (unitaires, lint, contract tests)
5. Merge et déploiement continu sur l'environnement de développement du service
6. Tests d'intégration entre services
7. Promotion vers l'environnement de staging puis production

## ☁️ Infrastructure cloud

- **Oracle Cloud Infrastructure**: Utilisation du Free Tier pour le MVP
  - 4 instances ARM Ampere A1 (24 GB RAM total)
  - 200 GB de stockage bloc
- **Cloudflare**:
  - Pages pour l'hébergement frontend
  - CDN pour la distribution globale des assets
  - R2 pour le stockage des panoramas et médias
- **Kubernetes (K3s)**: Version légère adaptée aux ressources limitées
- **Kafka Lite**: Version légère pour la communication événementielle

## 📅 Feuille de route

### MVP (Juin 2025 - Avril 2026)
- **Infrastructure**: Oracle Cloud, Cloudflare, Kafka Lite
- **Core**: Authentification, profils, préférences
- **Voyage**: Intégration Amadeus (vols, hébergements, activités)
- **IA**: Recommandations personnalisées, analyse contextuelle
- **Panoramas**: Paris et Barcelone avec CesiumJS + Marzipano
- **Accessibilité**: Conformité WCAG AA, internationalisation FR/EN

### Phase d'enrichissement (Mai 2026 - Octobre 2026)
- Extension à 5+ destinations majeures
- Amélioration des algorithmes IA
- Système de paiement Stripe avancé
- Optimisations d'interface et performance

### Phase d'expansion (Novembre 2026 - Avril 2027)
- Fonctionnalités sociales et partage
- Neural Collaborative Filtering avancé
- Expériences panoramiques interactives augmentées
- Scaling multilingue et multi-région

## 🌍 Intégrations externes

- **Amadeus API**: Vols, hébergements, activités
- **Stripe**: Traitement des paiements
- **Cloudflare**: CDN, Pages, R2 Storage
- **Open-Meteo API**: Données météorologiques pour l'analyse contextuelle
- **KartaView/Mapillary**: Panoramas 360° des zones urbaines
- **Wikimedia Commons**: Panoramas sous licence libre

## 📚 Documentation

Chaque service maintient sa propre documentation technique dans son repository.
La documentation centralisée et les guides d'architecture sont disponibles dans le repository [dreamscape-docs](https://github.com/dreamscape/documentation).

Le guide d'onboarding complet pour les nouveaux développeurs est disponible à [documentation/guidelines/onboarding.md](https://github.com/dreamscape/documentation/blob/main/guidelines/onboarding.md).

## 👥 Équipe et contribution

- **Product Owner**: Kevin Coutellier
- **Tech Lead**: [Nom du Tech Lead]
- **Équipe**: Des équipes dédiées par domaine (Auth, Voyage, IA, Panorama, Frontend)

### Comment contribuer

Consultez notre [Guide de contribution](https://github.com/dreamscape/documentation/blob/main/CONTRIBUTING.md) pour en savoir plus sur comment participer au projet.

## 📜 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

Les panoramas 360° utilisent diverses licences (CC-BY, CC-BY-SA, etc.) - voir le fichier [ATTRIBUTIONS.md](ATTRIBUTIONS.md) pour les détails et crédits spécifiques.

## 🤝 Support

Pour toute question ou assistance:
- Slack: #dreamscape-team
- Email: support@dreamscape-travel.com

## 🙏 Remerciements

- Toute l'équipe DreamScape pour leur vision et leur travail exceptionnel
- Nos partenaires Amadeus pour leur support technique
- Oracle Cloud Infrastructure pour l'hébergement
- Les contributeurs de Wikimedia Commons et KartaView pour les panoramas
- Et tous nos utilisateurs early-adopters pour leurs précieux retours

---

*DreamScape - Voyagez avant même de partir*
