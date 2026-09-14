# 🎯 Plan d'apprentissage CI/CD — Learning by Doing
**Outil : GitHub Actions**

> Règle : chaque notion théorique est immédiatement suivie d'une pratique sur le **même projet fil rouge**.

---

## 📦 Projet fil rouge (à créer une seule fois)

Un petit projet simple, mais avec de vrais tests, pour bien sentir la douleur "avant CI" puis le soulagement "avec CI".

- [ ] Créer un dépôt GitHub `learning-cicd`
- [ ] Choisir un langage simple (suggestion : **Python** avec `pytest`, ou **Node.js** avec `jest`)
- [ ] Créer une petite app : ex. une calculatrice (`add`, `subtract`, `divide`) avec 3-4 fonctions
- [ ] Écrire 4-5 tests unitaires qui couvrent ces fonctions (y compris un cas limite : division par zéro)
- [ ] Vérifier que `pytest` (ou `npm test`) fonctionne en local avant de continuer

---

## PHASE 1 — CI (Continuous Integration)

### 1️⃣ Avant CI — comprendre le problème
- [ ] Lire/comprendre le concept d'**"integration hell"** : quand plusieurs devs poussent du code sans vérification automatique, les bugs s'accumulent et se découvrent tard (souvent en prod)
- [ ] Comprendre le workflow "à l'ancienne" : coder → tester à la main (si on y pense) → push → merge → espérer que ça marche
- [ ] Identifier les risques : oubli de test, code cassé mergé sur `main`, conflits découverts trop tard

### 2️⃣ Appliquer un exemple "avant CI"
- [ ] Sur ton projet, **introduis volontairement un bug** dans une fonction (ex. modifie `divide` pour qu'elle ne gère pas la division par zéro)
- [ ] Ne lance PAS les tests localement (simule l'oubli)
- [ ] Fais un `git commit` + `git push` direct sur `main`
- [ ] Constate : rien ne t'arrête, le code cassé est sur `main` sans que personne ne le sache
- [ ] Note ce ressenti — c'est exactement le problème que le CI va résoudre

### 3️⃣ Comprendre pourquoi CI
- [ ] Comprendre la définition : CI = exécuter automatiquement les tests (et autres vérifications) à **chaque push / pull request**
- [ ] Comprendre les bénéfices concrets :
  - détection immédiate des bugs
  - feedback rapide (pas besoin d'attendre la prod)
  - confiance pour merger sans peur
  - base pour automatiser encore plus (linting, build, sécurité...)
- [ ] Comprendre le principe : "si ça casse, on le sait tout de suite, pas 2 semaines après"

### 4️⃣ Comprendre le concept de CI indépendamment des outils
> Objectif : comprendre le "quoi" et le "comment ça fonctionne" en théorie, avant de voir la syntaxe d'un outil précis. Ces concepts sont les mêmes que tu utilises GitHub Actions, GitLab CI, Jenkins ou CircleCI.

- [ ] Comprendre le principe du **"pipeline"** : une suite d'étapes automatisées qui s'exécutent dans un ordre défini
- [ ] Comprendre les **déclencheurs (triggers)** : qu'est-ce qui lance le pipeline ? (un push, une pull request, un tag, une planification horaire...)
- [ ] Comprendre les **étapes génériques d'un pipeline CI** :
  1. **Checkout** : récupérer le code source
  2. **Install/Setup** : installer les dépendances et l'environnement (langage, versions...)
  3. **Build** (si applicable) : compiler / préparer le code
  4. **Test** : exécuter les tests automatisés (unitaires, parfois lint/qualité de code)
  5. **Report** : remonter le résultat (succès/échec) et rendre visible ce statut
- [ ] Comprendre la notion de **runner / agent d'exécution** : une machine (souvent une VM éphémère) qui exécute le pipeline, indépendante de ton PC
- [ ] Comprendre la notion de **pipeline as code** : la configuration du pipeline est un fichier versionné (YAML en général) dans le dépôt, pas une config cachée dans une interface
- [ ] Comprendre la notion de **statut / badge de build** : succès ✅ ou échec ❌, visible sur le repo, la PR, etc.
- [ ] (Optionnel) Vérifie que tu peux dessiner sur papier/tableau un schéma générique : `push → trigger → runner → checkout → install → test → rapport de statut`, sans mentionner un seul outil

### 5️⃣ Comprendre comment appliquer ces concepts avec GitHub Actions
- [ ] Comprendre la structure d'un workflow : dossier `.github/workflows/`, fichier `.yml`
- [ ] Faire correspondre les concepts génériques (étape 4) à la syntaxe GitHub Actions :
  - déclencheur → `on:` (push, pull_request...)
  - pipeline / runner → `jobs:` (chaque job tourne sur un `runner`, ex. `ubuntu-latest`)
  - étapes séquentielles → `steps:` (checkout, install, test...)
  - checkout du code → action officielle `actions/checkout@v4`
  - setup de l'environnement → `actions/setup-python@v5` (ou `setup-node@v4`)
  - rapport de statut → automatique, visible dans l'onglet **Actions**, sur les commits et les PR
- [ ] Comprendre où voir les résultats : onglet **Actions** du repo GitHub

### 6️⃣ Appliquer CI sur le même projet
- [ ] Corrige le bug introduit à l'étape 2 (remets `divide` correcte)
- [ ] Crée `.github/workflows/ci.yml` avec un workflow qui :
  - se déclenche sur `push` et `pull_request` vers `main`
  - installe les dépendances
  - exécute les tests (`pytest` ou `npm test`)
- [ ] Push ce fichier, vérifie que le workflow se lance dans l'onglet **Actions** et qu'il passe ✅
- [ ] Réintroduis volontairement le même bug qu'à l'étape 2, push-le → observe le workflow **échouer** ❌
- [ ] (Bonus) Crée une branche + une Pull Request avec ce bug, observe que GitHub bloque/alerte visuellement sur la PR avant merge

### 7️⃣ Résumé CI
- [ ] Reformule avec tes mots, **sans citer d'outil** : c'est quoi le CI, pourquoi ça existe, quelles sont ses étapes génériques
- [ ] Reformule ensuite comment GitHub Actions implémente concrètement ces concepts
- [ ] Vérifie que tu peux répondre sans notes à :
  - "Que se passe-t-il si je push du code cassé maintenant ?"
  - "Quelles sont les étapes génériques d'un pipeline CI, indépendamment de l'outil ?"
  - "Où est défini mon pipeline CI dans GitHub Actions ?"
  - "Quels sont les 3 éléments clés d'un fichier de workflow GitHub Actions ?"

---

## PHASE 2 — CD (Continuous Delivery/Deployment)
*(à démarrer seulement une fois la Phase 1 solide — même structure : avant / exemple / pourquoi / concepts génériques / GitHub Actions / appliquer / résumé)*

- [ ] 1. Avant CD (déploiement manuel, ses risques)
- [ ] 2. Exemple pratique "avant CD" sur le même projet (déployer à la main)
- [ ] 3. Pourquoi CD
- [ ] 4. Comprendre les concepts de CD indépendamment des outils (environnements — dev/staging/prod, artefact déployable, delivery vs deployment, approbation manuelle vs automatique, rollback)
- [ ] 5. Comment appliquer ces concepts avec GitHub Actions (ex. déployer sur GitHub Pages, ou un service simple type Render/Vercel selon le projet)
- [ ] 6. Appliquer CD sur le même projet fil rouge
- [ ] 7. Résumé CD + résumé global CI/CD

---

## ✅ Suivi
Coche chaque case au fur et à mesure. Dis-moi quand tu commences l'étape 1 et on avance ensemble, étape par étape, en pratiquant à chaque fois avant de passer à la suite.
