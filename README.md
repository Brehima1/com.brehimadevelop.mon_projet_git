# Mon Projet sur le Git 

```bash
mkdire mon_projet_git
cd mon_projet_git
git init
Création du fichier README.md
```



Exercices Git — Progression par niveau

Niveau : Débutant
Objectif : apprendre les bases de Git en pratiquant progressivement les commandes essentielles.

Sommaire
Prérequis
Exercice 1 — Initialisation d’un dépôt
Exercice 2 — Suivi des modifications
Exercice 3 — Historique et navigation
Exercice 4 — Ignorer des fichiers
Exercice 5 — Connexion à un dépôt distant
Commandes Git à retenir
Objectif final
Prérequis

Avant de commencer, assure-toi d'avoir Git installé sur ton ordinateur.

Vérifie l'installation avec :

```bash 

git --version
```
Tu devrais obtenir une réponse similaire à :
```bash 
git version 2.x.x
```
Il est également recommandé de disposer d'un compte sur une plateforme Git distante comme GitHub.

## Exercice 1 — Initialisation d’un dépôt
## Objectif

## Apprendre à :

## créer un projet ;
## initialiser un dépôt Git ;
## créer un fichier ;
## ajouter un fichier à l'index ;
## effectuer son premier commit.
## Consignes
1. Créer le dossier du projet

Crée un nouveau dossier nommé mon_projet_git :
```bash 

mkdir mon_projet_git
Puis entre dans le dossier :
cd mon_projet_git
```
2. Initialiser Git

Initialise un nouveau dépôt Git :
```bash 
git init
```
Git crée alors un dossier caché .git contenant les informations nécessaires au suivi du projet.

3. Créer le fichier README.md

Crée un fichier README.md :
touch README.md

Ajoute ensuite une courte description du projet.

Par exemple :

# Mon projet Git

Ce projet me permet d'apprendre et de pratiquer les commandes Git.

4. Vérifier l'état du dépôt

```bash 
git status
```
Git devrait indiquer que README.md est un fichier non suivi.

5. Ajouter le fichier à l'index

```bash 
git add README.md
```
6. Effectuer le premier commit

```bash
git commit -m "Initialisation du projet"

```
## Compétences acquises

À la fin de cet exercice, tu dois savoir utiliser :

```bash 
git init
git status
git add
git commit
```
## Exercice 2 — Suivi des modifications

## Objectif

Apprendre à suivre les modifications apportées aux fichiers d'un projet.

## Consignes

1. Créer un fichier notes.txt

Crée le fichier :

```bash 
touch notes.txt
```
Ajoute quelques lignes :

Première note.

Deuxième note.

Je découvre Git.

2. Vérifier l'état du dépôt
git status

Le fichier notes.txt doit apparaître comme non suivi.

3. Ajouter le fichier et créer un commit

Ajoute le fichier :

```bash 
git add notes.txt

Puis crée un commit :

git commit -m "Ajout du fichier de notes"

```
4. Modifier le fichier

Ajoute une nouvelle ligne dans notes.txt :

Git permet de suivre les modifications d'un projet.

5. Consulter les différences

Utilise :

```bash 
git diff
```
Cette commande affiche les modifications effectuées depuis le dernier commit.

## À retenir

git diff permet notamment de répondre à la question :

Qu'est-ce qui a changé depuis mon dernier commit ?

# Compétences acquises

Tu dois maintenant comprendre le fonctionnement de :

```bash 
git status
git add
git commit
git diff
```
## Exercice 3 — Historique et navigation

## Objectif

Apprendre à consulter l'historique d'un dépôt et à explorer les anciennes versions des fichiers.

## Consignes

1. Créer plusieurs fichiers

Crée par exemple :

```bash 
touch fichier1.txt

Ajoute le fichier :

git add fichier1.txt

Puis crée un commit :

git commit -m "Ajout de fichier1"

Fais la même chose avec d'autres fichiers :

touch fichier2.txt

git add fichier2.txt

git commit -m "Ajout de fichier2"

Puis :

touch fichier3.txt

git add fichier3.txt

git commit -m "Ajout de fichier3"

```

Tu dois maintenant avoir plusieurs commits dans ton historique.

2. Consulter l'historique

Utilise :

```bash 
git log
```
Tu verras notamment :

l'identifiant du commit ;
l'auteur ;
la date ;
le message du commit.

Pour obtenir un affichage plus compact :
```bash 
git log --oneline

Exemple :

a82f391 Ajout de fichier3
b71d245 Ajout de fichier2
c64a912 Ajout de fichier1
```
3. Afficher les modifications d'un fichier

Utilise :
```bash 
git log -p -- fichier1.txt
```
Cette commande affiche l'historique du fichier ainsi que les modifications apportées à chaque version.

4. Explorer une ancienne version

Pour consulter temporairement un ancien commit, récupère son identifiant avec :

```bash 
git log --oneline

Puis utilise :

git switch --detach <ID_DU_COMMIT>

Par exemple :

git switch --detach c64a912

```
Tu peux alors explorer l'état du projet tel qu'il existait à ce moment-là.

## Attention : cet état est temporaire. Tu es alors en mode HEAD détachée (detached HEAD).

Pour revenir à ta branche principale :

```bash 
git switch main
```

Si ta branche principale s'appelle master :
```bash 
git switch master
```
## À propos de git checkout

L'ancienne commande :

```bash 
git checkout <commit>
```
permet également d'explorer un ancien commit.

Cependant, pour les versions récentes de Git, git switch est généralement plus clair pour changer de branche ou explorer un commit.

## Compétences acquises

Tu dois maintenant savoir utiliser :

```bash 
git log
git log --oneline
git log -p
git switch --detach
```
## Exercice 4 — Ignorer des fichiers

# Objectif

Apprendre à empêcher Git de suivre certains fichiers ou dossiers.

Cette fonctionnalité est particulièrement importante pour éviter de versionner :

des fichiers contenant des informations sensibles ;
des fichiers temporaires ;
des fichiers générés automatiquement ;
des dépendances ;
des fichiers propres à ton environnement local.
1. Créer un fichier secret.txt

Crée le fichier :

```bash 
touch secret.txt

Ajoute ensuite cette règle dans .gitignore :

secret.txt

Si le fichier .gitignore n'existe pas encore :

touch .gitignore
```

2. Vérifier que Git ignore le fichier

Utilise :

```bash 
git status
```
Le fichier secret.txt ne doit normalement plus apparaître comme fichier non suivi.

Tu peux également vérifier directement :

```bash 
git check-ignore -v secret.txt
```
Git indiquera quelle règle du .gitignore ignore le fichier.

3. Ignorer un dossier temporaire

Crée un dossier :

```bash 
mkdir temp

Puis ajoute quelques fichiers :

touch temp/fichier1.txt
touch temp/fichier2.txt
```
4. Modifier le .gitignore

Ajoute cette règle :
```bash 
temp/

Ton fichier .gitignore peut maintenant ressembler à ceci :

secret.txt
temp/

Vérifie :

git status
```

Les fichiers contenus dans temp/ doivent être ignorés.

## Attention

.gitignore empêche principalement Git de commencer à suivre des fichiers qui ne sont pas encore suivis.

Si un fichier a déjà été ajouté à Git et commité, l'ajouter ensuite au .gitignore ne suffit pas à le retirer du suivi.

## Compétences acquises

Tu dois maintenant comprendre :

```bash 
.gitignore
git status
git check-ignore
```

## Exercice 5 — Connexion à un dépôt distant

## Objectif

Apprendre à connecter un dépôt Git local à un dépôt distant et à envoyer son travail en ligne.

1. Créer un dépôt distant

Crée un dépôt vide sur une plateforme Git comme GitHub.

Par exemple, nomme-le :

mon_projet_git

 Pour cet exercice, évite de générer automatiquement un nouveau README.md sur le dépôt distant si ton dépôt local en possède déjà un.

2. Connecter le dépôt local au dépôt distant

Dans ton projet local, ajoute le dépôt distant :

```bash 
git remote add origin <URL_DU_DEPOT>

Par exemple :

git remote add origin https://github.com/USERNAME/mon_projet_git.git

Vérifie la connexion :

git remote -v
```

Tu devrais voir quelque chose comme :

```bash 
origin  https://github.com/Brehima1/mon_projet_git.git (fetch)
origin  https://github.com/Brehima1/mon_projet_git.git (push)
```

3. Envoyer les commits vers le dépôt distant

Si ta branche principale s'appelle main :

```bash 
git push -u origin main
```
L'option -u permet d'associer ta branche locale à la branche distante.

Par la suite, tu pourras simplement utiliser :

```bash 
git push
```

4. Récupérer un dépôt public

Choisis un dépôt public sur GitHub et clone-le :

```bash 
git clone <URL_DU_DEPOT>

Par exemple :

git clone https://github.com/Brehima1/mon-projet.git

Git crée automatiquement un nouveau dossier contenant le projet.

Entre ensuite dans le dossier :

cd mon-projet

Vérifie son état :

git status
```

## Commandes Git à retenir

Voici les principales commandes rencontrées dans ces exercices :

Commande	Utilité 

git init :	Initialise un dépôt Git
git status : Affiche l'état du dépôt
git add	: Ajoute des fichiers à l'index
git commit	: Enregistre les modifications
git diff : Affiche les modifications non commitées
git log :	Affiche l'historique
git log --oneline : Affiche l'historique de manière compacte
git log -p	: Affiche l'historique avec les modifications
git switch : Change de branche ou explore un commit
git remote add	: Ajoute un dépôt distant
git remote -v	: Affiche les dépôts distants
git push : Envoie les commits vers le dépôt distant
git clone: Copie un dépôt distant en local
git check-ignore : Vérifie pourquoi un fichier est ignoré

## Le cycle Git fondamental

```bash 
La logique principale de Git peut être résumée ainsi :

       Fichiers
          │
          ▼
     git add
          │
          ▼
       Index
          │
          ▼
    git commit
          │
          ▼
   Historique local
          │
          ▼
     git push
          │
          ▼
  Dépôt distant
```
Le cycle de travail classique est donc :

git status
git add .
git commit -m "Description du changement"
git push

## Objectif final

À la fin de ces exercices, tu dois être capable de :

créer un dépôt Git ;

créer et modifier des fichiers ;

vérifier l'état d'un dépôt ;

ajouter des fichiers à l'index ;

créer des commits ;

consulter l'historique ;

comparer les modifications ;

explorer une ancienne version du projet ;

utiliser un fichier .gitignore ;

connecter un dépôt local à un dépôt distant ;

envoyer des commits avec git push ;

récupérer un projet avec git clone.

# Défi final

Une fois les cinq exercices terminés, crée ton propre petit projet Git.

Contraintes

Ton projet doit contenir :

```bash 
mon_projet_git/
├── README.md
├── notes.txt
├── .gitignore
├── fichier1.txt
├── fichier2.txt
└── temp/
```

Le dossier temp/ et le fichier secret.txt doivent être ignorés par Git.

Tu dois également avoir au moins 5 commits avec des messages explicites.

Exemple :

Initialisation du projet
Ajout des notes
Ajout du premier fichier
Ajout du deuxième fichier
Configuration du gitignore

Enfin, connecte ton projet à un dépôt distant et envoie-le avec :

git push