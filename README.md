# Document technique – Plugin QGIS « Suivi des Travaux Communaux »

## 1. Contexte et objectifs

### 1.1 Contexte

Les collectivités territoriales ont besoin d’outils SIG pour **le suivi des travaux de voirie et de réseaux** afin de respecter les normes SCNIG (Système Commun National d’Information Géographique) et ACCESSIBILIT2 (normes accessibilité PMR et sécurité). Le plugin vise à **centraliser, suivre et restituer les données** dans un format compatible avec les standards SIG de transport.

### 1.2 Objectifs

* Permettre la **saisie et gestion des travaux** directement depuis QGIS.
* Respecter les standards **SCNIG** pour la structuration des données SIG.
* Intégrer les recommandations **ACCESSIBILITE** pour l’accessibilité et les informations PMR.
* Fournir un **export conforme aux standards transport** pour les applications métier et les rapports.

---

## 2. Périmètre fonctionnel

### 2.1 Utilisateurs cibles

* Services techniques et voirie des communes
* DGS / DST
* Chargés d’études mobilité et transport
* Agents terrain (collecte et suivi)

### 2.2 Cas d’usage principaux

* Saisie d’un chantier sur carte
* Mise à jour de l’état des travaux
* Suivi des chantiers PMR ou accessibles
* Détection des travaux en retard
* Export vers formats SCNIG/Transport (GPKG, GeoJSON, Excel)
* Production de fiches PDF pour réunions et reporting

---

## 3. Architecture technique

### 3.1 Schéma général

```
[ QGIS Desktop + Plugin ]
            ↓
[ PostgreSQL / PostGIS ]
            ↓
[ Export SCNIG / Transport ]
```

### 3.2 Choix techniques

* **Logiciel SIG** : QGIS
* **Base de données** : PostgreSQL + PostGIS
* **Langage plugin** : Python (PyQGIS / PyQt)
* **Projection** : RGF93 / Lambert-93 (EPSG:2154)
* **Normes** : SCNIG, ACCESSIBILITE, standards transport

---

## 4. Modèle de données (conforme SCNIG et standards transport)

### 4.1 Schéma

Schéma : `voirie`

### 4.2 Table principale : `voirie.travaux`

| Champ         | Type      | Description                       | Norme         |
| ------------- | --------- | --------------------------------- | ------------- |
| id            | UUID      | Identifiant unique                | SCNIG         |
| geom          | Geometry  | Point, ligne, polygone            | SCNIG         |
| type_travaux  | VARCHAR   | Voirie, Réseaux, Signalisation    | SCNIG         |
| etat          | VARCHAR   | À venir, En cours, Achevé, Annulé | SCNIG         |
| date_debut    | DATE      | Date de début                     | SCNIG         |
| date_fin      | DATE      | Date prévisionnelle               | SCNIG         |
| entreprise    | VARCHAR   | Nom entreprise                    | SCNIG         |
| responsable   | VARCHAR   | Responsable chantier              | SCNIG         |
| urgence       | VARCHAR   | Faible, Moyenne, Élevée           | SCNIG         |
| accessibilite | VARCHAR   | Conforme PMR, Dérogation          | ACCESSIBILITE |
| commentaire   | TEXT      | Observations                      | SCNIG         |
| date_creation | TIMESTAMP | Création                          | SCNIG         |
| date_maj      | TIMESTAMP | Dernière modification             | SCNIG         |

---

## 5. Fonctionnalités du plugin

### 5.1 Connexion aux données

* Connexion PostGIS existante
* Gestion multi-utilisateurs
* Respect des droits d’accès

### 5.2 Création et modification des travaux

* Formulaire guidé QGIS (type, état, accessibilité, urgence)
* Dessin sur carte : point, ligne, polygone
* Validation des champs selon SCNIG et ACCESSIBILITE

### 5.3 Gestion des états et alertes

* États : À venir / En cours / Achevé / Annulé
* Alertes travaux en retard
* Vérification conformité PMR (ACCESSIBILIT2)

### 5.4 Filtres et visualisation

* Travaux en cours / en retard
* Travaux par entreprise ou par période
* Travaux accessibles PMR

### 5.5 Export et impression

* Export GeoPackage / GeoJSON compatible SCNIG
* Export Excel standard transport
* Impression PDF des fiches travaux

---

## 6. Interface utilisateur

* Panneau latéral QGIS avec boutons métier
* Icônes claires, textes compréhensibles pour non-géomaticiens
* Infobulles explicatives et aide intégrée

---

## 7. Étapes de développement

1. Création du schéma PostGIS et des tables selon SCNIG / ACCESSIBILITE
2. Développement du plugin PyQGIS (formulaire, boutons, gestion états)
3. Logique métier : alertes, filtres, styles automatiques
4. Mise en page des fiches PDF et export standards
5. Tests et validation auprès des agents et DGS

---

## 8. Modèle de diffusion et licence

* Licence : **GPL v3**
* Projet open source, disponible sur GitHub
* Contribution volontaire (financière ou non financière)
* Maintien de la gratuité pour les collectivités

---

## 9. Évolutions prévues

* Connexion serveur SIG / QGIS Server
* Carte web Leaflet pour élus et agents terrain
* Accès terrain via QField
* Gestion des droits utilisateurs avancée
* Notifications automatiques

---
