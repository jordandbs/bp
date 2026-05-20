# EC10 
---


## 1. Revues de code (C33)

### 1.1 Standards de développement

**Nommage**

| Élément | Convention | 
|---|---|
| Variables / fonctions | `camelCase` | 
| Classes / composants | `PascalCase` | 
| Constantes | `UPPER_SNAKE_CASE` | 
| Fichiers | `kebab-case` | 

**Formatage**

- Indentation : **2 espaces** (JS/TS/HTML) ou **4 espaces** (PHP/Python)
- Longueur de ligne maximale : **120 caractères**
- Fin de ligne : `LF` (Unix), pas `CRLF`
- Fichiers terminés par un saut de ligne vide
---

### 1.2 Processus de revue

Pull request :

**Checklist du revieweur**

Avant d'approuver une PR, le revieweur vérifie systématiquement :

- [ ] Le code respecte les conventions de nommage et de formatage du projet
- [ ] Aucune logique métier n'est dupliquée (principe DRY)
- [ ] Les fonctions ont une responsabilité unique (principe SRP)
- [ ] Les cas d'erreur sont gérés explicitement
- [ ] Aucun secret (clé API, mot de passe) n'est commité en dur
- [ ] Les tests correspondants sont présents et passent
- [ ] La PR ne dépasse pas **400 lignes modifiées** (sinon, la découper)

**Règles de communication en revue**

- Les commentaires sont formulés de manière constructive (suggestion, pas injonction)
- On distingue les **bloquants** (à corriger avant merge) des **suggestions** (optionnelles)
- L'auteur doit répondre à chaque commentaire, même pour indiquer qu'il a pris en compte
---

### 1.3 Maintenabilité du code

Dette technique 
code clair lisible et simple 
pour la maintenance 
---

## 2. Tests automatisés (C34)

### 2.1 Stratégie de couverture
3 types de test unitaire integration fonctionnel

**Objectifs de couverture minimaux**

| Type | Couverture cible | Outil |
|---|---|---|
| Unitaires | ≥ 80 % des fonctions métier |  PHPUnit |
| Intégration | Routes API critiques |  Postman |

**Fonctionnalités prioritaires à couvrir dans SkillHub Learning**

- Inscription / connexion utilisateur
- Accès et progression dans une session
- Soumission d'une formation
- Suivie des formations (en cours termine)

---

### 2.2 Bonnes pratiques de rédaction

**Structure d'un test : pattern AAA**

**Règles de nommage des tests**
Le nom d'un test doit décrire : **qui** fait **quoi** dans **quel contexte**.

**Couverture des cas limites**

Pour chaque fonctionnalité, prévoir systématiquement :

- Le cas nominal 
- Les cas limites 
- Les cas d'erreur 

**Isolation des tests**

- Chaque test est indépendant 
- Utiliser des mocks pour les dépendances externes (BDD, API)
- Réinitialiser l'état entre les tests 

---

### 2.3 Documentation des tests

**Rapport de couverture**

Générer et archiver le rapport à chaque CI 

Le rapport HTML est archivé dans `reports/coverage/` et consultable par toute l'équipe par exemple.

**Intégration continue**

Les tests sont exécutés automatiquement à chaque push via la pipeline CI :

Un merge est **bloqué** si un test échoue ou si la couverture descend sous le seuil minimal.

---

## 3. Documentation technique (C35)

### 3.1 Structure de la documentation

La documentation de SkillHub est organisée en quatre niveaux.

```
docs/
├── README.md               ← Point d'entrée (ce fichier)
├── architecture/
│   ├── overview.md         ← Vue d'ensemble du système
│   ├── database-schema.md  ← Schéma de la base de données
│   └── api-design.md       ← Décisions d'architecture API
├── guides/
│   ├── getting-started.md  ← Installation et lancement local
│   ├── contributing.md     ← Ce guide de bonnes pratiques
│   └── deployment.md       ← Procédure de déploiement
├── modules/
│   ├── learning/           ← Documentation du module Learning
│   └── mentoring/          ← Documentation du module Mentoring
└── adr/
    └── 001-choix-framework.md  ← Architecture Decision Records
```

**Ce qui doit obligatoirement être documenté**

- Configuration de l'environnement de développement (variables d'env, prérequis)
- Architecture générale et flux de données principaux
- Chaque endpoint d'API (méthode, paramètres, réponses, codes d'erreur)
- Procédures de déploiement et de rollback
- Guide de dépannage pour les erreurs fréquentes

---

### 3.2 Clarté et accessibilité

**Principes de rédaction**

- Écrire pour un développeur qui **rejoint l'équipe pour la première fois**
- Préférer les exemples concrets aux descriptions abstraites
- Une section = une idée principale
- Utiliser des titres hiérarchisés (`#`, `##`, `###`) pour faciliter la navigation

**Template de documentation d'un endpoint API**

```markdown
## GET /api/courses/:id/progress

Retourne la progression de l'utilisateur connecté pour un cours donné.
voir cour api rest sur moodle


### Erreurs possibles

401 403 404

### Documentation du code

JSDoc / PHPDoc 
---

### 3.3 Plateformes collaboratives

**Organisation recommandée**

| Usage | Outil | Accès |
|---|---|---|
| Documentation principale |  GitLab  | Toute l'équipe |
| Référence API | Swagger UI (auto-généré) | Dev + QA |
| Décisions techniques | ADR dans le repo | Toute l'équipe |
| Suivi des bugs | GitLab Issues | Toute l'équipe |

**Règles de mise à jour**

- Toute PR modifiant le comportement d'une API doit **mettre à jour la documentation correspondante** dans la même PR
- La documentation est revue en même temps que le code lors des revues
- Un fichier `CHANGELOG.md` est maintenu à la racine du projet

**Format du CHANGELOG**
Ajouté Modifié Corrigé

---

**Indicateurs de qualité à surveiller**

| Indicateur | Seuil d'alerte | Outil |
|---|---|---|
| Couverture de tests | < 80 % | Jest / SonarQube |
| Complexité cyclomatique | > 10 | ESLint / SonarQube |
| Dette technique | > 5 jours | SonarQube |
| Temps de build CI | > 10 min | GitLab CI |
| Issues ouvertes sans responsable | > 5 | GitLab Issues |

---

## Outils recommandés

| Catégorie | Outil | Usage |
|---|---|---|
| Linting JS | ESLint + Prettier | Formatage et qualité du code |
| Linting PHP | PHP_CodeSniffer | same |
| Tests unitaires | Jest / PHPUnit | Tests automatisés |
| Qualité de code | SonarQube | Analyse statique continue |
| Documentation API | Swagger / OpenAPI | Documentation auto-générée |
| Wiki collaboratif | GitLab Wiki | Documentation équipe |
| CI/CD | GitLab CI / GitHub Actions | Automatisation des contrôles |
---
