#  TP Git Avancé — Guide Pratique & Résolution d'Exercices

Bienvenue dans ce TP consacré aux fonctionnalités avancées de **Git**. Ce document regroupe des explications claires, des cas d'usage réels et des guides étape par étape pour réaliser les 5 exercices pratiques.

---

##  Table des Matières

- [Exercice 1 : Réécriture d’historique avec `rebase -i`](#exercice-1--réécriture-dhistorique-avec-rebase--i)
- [Exercice 2 : Débogage avec `git bisect`](#exercice-2--débogage-avec-git-bisect)
- [Exercice 3 : Gestion des Sous-modules Git (`submodules`)](#exercice-3--gestion-des-sous-modules-git-submodules)
- [Exercice 4 : Hooks Git Personnalisés](#exercice-4--hooks-git-personnalisés)
- [Exercice 5 : Travail Hors-ligne et Résolution de Conflits Complexes](#exercice-5--travail-hors-ligne-et-résolution-de-conflits-complexes)

---

##  Exercice 1 : Réécriture d’historique avec `rebase -i`

### Objectif
Apprendre à nettoyer l'historique de commits avant de partager son code (`squash`, reformatage de messages, suppression de commits inutiles).

###  Pas-à-pas d'exécution

1. **Créer des commits volontairement désordonnés :**
   ```bash
   mkdir tp-git-avance && cd tp-git-avance
   git init
   
   echo "Code V1" > app.py && git add app.py && git commit -m "feat: ajout v1"
   echo "print('debug')" >> app.py && git add app.py && git commit -m "wip debug a supprimer"
   echo "Code V2" >> app.py && git add app.py && git commit -m "fix typos"
   echo "Code V3" >> app.py && git add app.py && git commit -m "Ajustement v2"
   ```

2. **Lancer le rebase interactif sur les 4 derniers commits :**
   ```bash
   git rebase -i HEAD~4
   ```

3. **Configurer les instructions dans l'éditeur (VS Code / Vim) :**
   L'éditeur s'ouvre avec la liste des commits de haut en bas (du plus ancien au plus récent) :
   ```text
   pick a1b2c3d feat: ajout v1
   drop e4f5g6h wip debug a supprimer
   pick i7j8k9l fix typos
   squash m0n1o2p Ajustement v2
   ```
   * **`reword` (ou `r`) :** Pour modifier le message du commit `fix typos`.
   * **`squash` (ou `s`) :** Pour fusionner `Ajustement v2` avec `fix typos`.
   * **`drop` (ou `d`) :** Pour supprimer `wip debug a supprimer`.

4. **Sauvegarder et fermer l'éditeur** pour valider la réécriture.

---

## Exercice 2 : Débogage avec `git bisect`

###  Objectif
Retrouver rapidement le commit exact qui a introduit un bug via une recherche dichotomique automatique.

###  Pas-à-pas d'exécution

1. **Préparer l'historique et introduire une erreur :**
   ```bash
   # Commits sains
   echo "def main(): return 0" > app.py && git add app.py && git commit -m "v1.0 stable"
   echo "# maj doc" >> README.md && git add README.md && git commit -m "docs: maj readme"
   
   # Commit fautif (introduction du bug)
   echo "def main(): return 1/0" > app.py && git add app.py && git commit -m "feat: optimisation calcul"
   
   # Commits ultérieurs
   echo "# autre changement" >> README.md && git add README.md && git commit -m "docs: ajout licence"
   ```

2. **Démarrer la session `bisect` :**
   ```bash
   git bisect start
   git bisect bad                 # Le commit actuel (HEAD) contient le bug
   git bisect good <hash-v1.0>    # Préciser un commit antérieur stable
   ```

3. **Tester et étiqueter à chaque étape :**
   Git va basculer automatiquement sur un commit intermédiaire.
   ```bash
   python3 app.py                 # Tester le code
   git bisect bad                 # Si l'erreur se produit
   # OU
   git bisect good                # Si l'application fonctionne
   ```

4. **Identifier le coupable et quitter `bisect` :**
   Une fois le commit fautif affiché par Git, notez son hash puis quittez le mode de débogage :
   ```bash
   git bisect reset
   ```

---

##  Exercice 3 : Gestion des Sous-modules Git (`submodules`)

###  Objectif
Intégrer et gérer un projet externe ou une dépendance sous forme de dépôt Git imbriqué.

### Pas-à-pas d'exécution

1. **Ajouter un sous-module au projet principal :**
   ```bash
   git submodule add https://github.com/octocat/Spoon-Knife.git lib/Spoon-Knife
   git commit -m "feat: ajout du sous-module Spoon-Knife"
   ```

2. **Cloner un projet contenant des sous-modules :**
   ```bash
   # Méthode 1 : Clonage récursif direct
   git clone --recursive <url-du-depot-parent>

   # Méthode 2 : Initialisation post-clonage classique
   git clone <url-du-depot-parent>
   cd <depot-parent>
   git submodule init
   git submodule update
   ```

3. **Modifier et pousser des changements depuis le sous-module :**
   ```bash
   cd lib/Spoon-Knife
   git checkout main
   echo "// correction" >> index.html
   git commit -am "fix: correctif local sous-module"
   git push origin main
   ```

4. **Mettre à jour le sous-module dans le projet parent :**
   ```bash
   cd ../.. # Retour à la racine du projet parent
   git submodule update --remote --merge
   git add lib/Spoon-Knife
   git commit -m "chore: mise a jour de la reference du sous-module"
   ```

---

##  Exercice 4 : Hooks Git Personnalisés

###  Objectif
Automatiser des contrôles de qualité (linter, vérifications de sécurité) avant et après les commits.

###  Pas-à-pas d'exécution

1. **Créer le hook `pre-commit` (Bloque si la chaîne `"debug"` est détectée) :**
   Créez le fichier `.git/hooks/pre-commit` :
   ```bash
   #!/bin/bash
   if grep -rnw --exclude-dir=.git -E "debug" .; then
       echo "Commit bloqué : le mot-clé 'debug' a été détecté dans le code !"
       exit 1
   fi
   ```

2. **Créer le hook `post-commit` (Notification post-enregistrement) :**
   Créez le fichier `.git/hooks/post-commit` :
   ```bash
   #!/bin/bash
   echo "✅ Commit enregistré avec succès dans la branche $(git rev-parse --abbrev-ref HEAD) !"
   ```

3. **Rendre les scripts d'accroche exécutables :**
   ```bash
   chmod +x .git/hooks/pre-commit
   chmod +x .git/hooks/post-commit
   ```

4. **Tester la validation :**
   ```bash
   # Test invalide (doit échouer)
   echo "print('debug mode')" > test.py
   git add test.py
   git commit -m "test hook"   # Le commit est rejeté !

   # Test valide
   echo "print('production mode')" > test.py
   git add test.py
   git commit -m "fix: code propre" # Déclenche le message du post-commit
   ```

---

##  Exercice 5 : Travail Hors-ligne et Résolution de Conflits Complexes

###  Objectif
Simuler un environnement de travail collaboratif distribué et gérer la résolution de conflits de fusion manuellement et visuellement.

###  Pas-à-pas d'exécution

1. **Initialiser un dépôt distant et deux clones locaux :**
   ```bash
   # Dépôt distant central
   mkdir depot-distant.git && cd depot-distant.git
   git init --bare && cd ..

   # Clone Alice
   git clone depot-distant.git clone_alice
   cd clone_alice
   echo -e "Ligne 1: Intro
Ligne 2: Original
Ligne 3: Fin" > main.py
   git add main.py && git commit -m "Commit initial" && git push origin main
   cd ..

   # Clone Bob
   git clone depot-distant.git clone_bob
   ```

2. **Générer des modifications divergentes :**
   ```bash
   # Alice modifie la ligne 2 et push
   cd clone_alice
   echo -e "Ligne 1: Intro
Ligne 2: Modif Alice
Ligne 3: Fin" > main.py
   git commit -am "Alice: modification ligne 2" && git push origin main
   cd ..

   # Bob modifie aussi la ligne 2 hors-ligne
   cd clone_bob
   echo -e "Ligne 1: Intro
Ligne 2: Modif Bob
Ligne 3: Fin" > main.py
   git commit -am "Bob: modification ligne 2"
   ```

3. **Déclencher le conflit de fusion :**
   ```bash
   cd clone_bob
   git fetch origin
   git merge origin/master # Déclenche CONFLICT (content): Merge conflict in main.py
   ```

4. **Résoudre le conflit :**

   #### Option A : Résolution Manuelle (Éditeur texte)
   Ouvrez `main.py`, repérez les marqueurs de conflit :
   ```python
  print('Fonctionnalite combinee A et B')
   ```
   Éditez le fichier pour garder la version souhaitée, supprimez les marqueurs (`<<<<<<<`, `=======`, `>>>>>>>`), puis validez :
   ```bash
   git add main.py
   git commit -m "fix: résolution manuelle du conflit"
   ```

   #### Option B : Outil Visuel (VS Code / Meld)
   Configurez votre outil de diff/merge :
   ```bash
   # Configuration pour VS Code
   git config merge.tool vscode
   git config mergetool.vscode.cmd 'code --wait $MERGED'
   
   # Lancement de l'outil
   git mergetool
   ```
   Dans **VS Code**, utilisez les boutons d'action au-dessus du conflit (*Accept Current*, *Accept Incoming*, ou *Merge Editor*) pour harmoniser le code, sauvegardez et finalisez :
   ```bash
   git add main.py
   git commit -m "fix: résolution visuelle du conflit"
   git push origin main
   ```

---
 *Note : Ce document est formaté pour un affichage optimal sous **Visual Studio Code** (support complet du Markdown et rendu des blocs de code).*