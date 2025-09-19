# Tutoriel Complet Git & Git LFS

## Table des Matières
1. [Installation](#installation)
2. [Configuration Initiale](#configuration-initiale)
3. [Commandes Git de Base](#commandes-git-de-base)
4. [Branches et Fusion](#branches-et-fusion)
5. [Travail avec les Dépôts Distants](#travail-avec-les-dépôts-distants)
6. [Git LFS (Large File Storage)](#git-lfs-large-file-storage)
7. [Commandes Avancées](#commandes-avancées)
8. [Résolution de Problèmes](#résolution-de-problèmes)

---

## Installation

### Windows
```bash
# Télécharger depuis https://git-scm.com/download/win
# Ou via Chocolatey
choco install git

# Ou via winget
winget install --id Git.Git -e --source winget
```

### macOS
```bash
# Via Homebrew
brew install git

# Via MacPorts
sudo port install git

# Xcode Command Line Tools
xcode-select --install
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install git

# Pour Git LFS
sudo apt install git-lfs
```

### Linux (CentOS/RHEL/Fedora)
```bash
# CentOS/RHEL
sudo yum install git
# ou
sudo dnf install git

# Fedora
sudo dnf install git git-lfs
```

### Vérification de l'installation
```bash
git --version
# Sortie attendue: git version 2.x.x

git lfs version
# Sortie attendue: git-lfs/3.x.x
```

---

## Configuration Initiale

### Configuration Globale
```bash
# Configurer votre nom et email (obligatoire)
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Configurer l'éditeur par défaut
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "nano"         # Nano
git config --global core.editor "vim"          # Vim

# Configurer la branche par défaut
git config --global init.defaultBranch main

# Activer la coloration
git config --global color.ui auto

# Configurer le comportement de merge
git config --global merge.tool vimdiff

# Configuration des fins de ligne (important pour cross-platform)
# Windows
git config --global core.autocrlf true
# macOS/Linux
git config --global core.autocrlf input
```

### Configuration Locale (par projet)
```bash
# Dans un dépôt spécifique
git config user.name "Nom Professionnel"
git config user.email "nom.professionnel@entreprise.com"
```

### Visualiser la Configuration
```bash
# Voir toute la configuration
git config --list

# Voir une configuration spécifique
git config user.name
git config --global user.email

# Voir où est stockée une configuration
git config --show-origin user.name
```

---

## Commandes Git de Base

### Initialisation d'un Dépôt
```bash
# Créer un nouveau dépôt
mkdir mon-projet
cd mon-projet
git init

# Cloner un dépôt existant
git clone https://github.com/utilisateur/depot.git
git clone https://github.com/utilisateur/depot.git mon-dossier-local

# Cloner seulement une branche spécifique
git clone -b nom-branche https://github.com/utilisateur/depot.git

# Cloner avec une profondeur limitée (shallow clone)
git clone --depth 1 https://github.com/utilisateur/depot.git
```

### État et Statut du Dépôt
```bash
# Voir l'état des fichiers
git status

# Version courte du statut
git status -s
git status --short

# Voir l'historique des commits
git log

# Log avec format personnalisé
git log --oneline
git log --graph --oneline --all
git log --pretty=format:"%h - %an, %ar : %s"

# Voir les différences
git diff                    # Différences non indexées
git diff --staged          # Différences indexées
git diff HEAD              # Toutes les différences
git diff commit1 commit2   # Entre deux commits
```

### Ajout et Commit
```bash
# Ajouter des fichiers à l'index
git add fichier.txt
git add dossier/
git add .                  # Tous les fichiers modifiés
git add -A                 # Tous les fichiers (y compris supprimés)
git add *.js               # Tous les fichiers .js

# Ajouter interactivement
git add -i
git add -p                 # Patch mode (par chunks)

# Supprimer des fichiers de l'index
git reset HEAD fichier.txt
git restore --staged fichier.txt

# Créer un commit
git commit -m "Message de commit"
git commit -a -m "Commit avec ajout automatique des fichiers modifiés"
git commit --amend         # Modifier le dernier commit
git commit --amend -m "Nouveau message"
```

### Exemple complet d'ajout et commit
```bash
# Créer un fichier
echo "# Mon Projet" > README.md

# Vérifier le statut
git status
# Sur la branche main
# Fichiers non suivis:
#   README.md

# Ajouter le fichier
git add README.md

# Vérifier le statut
git status
# Sur la branche main
# Modifications à commiter:
#   nouveau fichier: README.md

# Commiter
git commit -m "Ajout du README"

# Voir l'historique
git log --oneline
# a1b2c3d (HEAD -> main) Ajout du README
```

### Suppression et Déplacement de Fichiers
```bash
# Supprimer un fichier
git rm fichier.txt
git rm --cached fichier.txt  # Supprimer de l'index mais garder le fichier

# Supprimer un dossier
git rm -r dossier/

# Déplacer/renommer un fichier
git mv ancien-nom.txt nouveau-nom.txt

# Exemple
git mv README.txt README.md
git commit -m "Renommer README en markdown"
```

---

## Branches et Fusion

### Gestion des Branches
```bash
# Lister les branches
git branch                 # Branches locales
git branch -r             # Branches distantes
git branch -a             # Toutes les branches

# Créer une branche
git branch nouvelle-fonctionnalite
git branch feature/login   # Convention de nommage

# Créer et basculer sur une branche
git checkout -b nouvelle-fonctionnalite
git switch -c nouvelle-fonctionnalite  # Nouvelle syntaxe

# Basculer entre les branches
git checkout main
git switch main           # Nouvelle syntaxe

# Supprimer une branche
git branch -d nom-branche            # Suppression sécurisée
git branch -D nom-branche            # Suppression forcée
git push origin --delete nom-branche # Supprimer sur le serveur distant
```

### Exemple de Workflow avec Branches
```bash
# Créer une nouvelle fonctionnalité
git switch -c feature/authentification

# Travailler sur la fonctionnalité
echo "Code d'authentification" > auth.js
git add auth.js
git commit -m "Ajout du système d'authentification"

# Faire plusieurs commits
echo "Tests d'authentification" > auth.test.js
git add auth.test.js
git commit -m "Ajout des tests d'authentification"

# Revenir sur main
git switch main

# Fusionner la fonctionnalité
git merge feature/authentification

# Supprimer la branche de fonctionnalité
git branch -d feature/authentification
```

### Types de Fusion (Merge)
```bash
# Fusion normale (crée un commit de merge)
git merge nom-branche

# Fast-forward (pas de commit de merge si possible)
git merge --ff-only nom-branche

# Toujours créer un commit de merge
git merge --no-ff nom-branche

# Fusion avec message personnalisé
git merge nom-branche -m "Fusion de la fonctionnalité X"
```

### Rebase
```bash
# Rebaser sur une autre branche
git rebase main

# Rebase interactif (modifier l'historique)
git rebase -i HEAD~3      # 3 derniers commits

# Continuer un rebase après résolution de conflit
git rebase --continue

# Abandonner un rebase
git rebase --abort

# Exemple de rebase interactif
git rebase -i HEAD~3
# Dans l'éditeur:
# pick a1b2c3d Premier commit
# squash e4f5g6h Deuxième commit
# reword i7j8k9l Troisième commit
```

### Résolution de Conflits
```bash
# Quand un conflit survient
git status
# Both modified: fichier.txt

# Éditer le fichier pour résoudre les conflits
# Chercher les marqueurs <<<<<<<, =======, >>>>>>>

# Exemple de conflit dans fichier.txt:
# <<<<<<< HEAD
# Code de la branche courante
# =======
# Code de la branche à fusionner
# >>>>>>> nom-branche

# Après résolution
git add fichier.txt
git commit -m "Résolution du conflit"
```

---

## Travail avec les Dépôts Distants

### Configuration des Remotes
```bash
# Voir les remotes
git remote
git remote -v             # Avec les URLs

# Ajouter un remote
git remote add origin https://github.com/utilisateur/depot.git
git remote add upstream https://github.com/original/depot.git

# Modifier un remote
git remote set-url origin https://github.com/nouveau/depot.git

# Supprimer un remote
git remote remove nom-remote
```

### Push (Envoi vers le serveur)
```bash
# Push vers la branche par défaut
git push

# Push vers une branche spécifique
git push origin main
git push origin feature/nouvelle-fonctionnalite

# Premier push d'une nouvelle branche
git push -u origin nouvelle-branche
git push --set-upstream origin nouvelle-branche

# Push de tous les branches
git push --all origin

# Push des tags
git push --tags origin

# Push forcé (attention !)
git push --force origin main
git push --force-with-lease origin main  # Plus sûr
```

### Pull et Fetch
```bash
# Récupérer et fusionner les changements
git pull

# Récupérer d'une branche spécifique
git pull origin main

# Récupérer sans fusionner
git fetch
git fetch origin
git fetch --all

# Pull avec rebase au lieu de merge
git pull --rebase

# Exemple complet
git fetch origin
git status
git merge origin/main
# Équivalent à: git pull origin main
```

### Synchronisation avec Fork (Upstream)
```bash
# Configuration pour un fork
git remote add upstream https://github.com/original/projet.git

# Synchroniser avec l'upstream
git fetch upstream
git switch main
git merge upstream/main
git push origin main

# Ou avec rebase
git fetch upstream
git switch main
git rebase upstream/main
git push origin main
```

---

## Git LFS (Large File Storage)

### Installation et Configuration
```bash
# Installation sur Ubuntu/Debian
sudo apt install git-lfs

# Installation sur macOS
brew install git-lfs

# Installation sur Windows (inclus avec Git for Windows récent)
# Ou télécharger depuis: https://git-lfs.github.io/

# Vérifier l'installation
git lfs version
```

### Configuration de Git LFS
```bash
# Installer Git LFS dans votre compte utilisateur (une seule fois)
git lfs install

# Configuration globale
git lfs install --system

# Vérifier la configuration
git lfs env
```

### Utilisation de Git LFS
```bash
# Tracker des types de fichiers
git lfs track "*.psd"      # Fichiers Photoshop
git lfs track "*.zip"      # Archives
git lfs track "*.mp4"      # Vidéos
git lfs track "*.exe"      # Exécutables
git lfs track "assets/**"  # Dossier entier

# Voir quels fichiers sont trackés
git lfs track

# Ajouter le .gitattributes
git add .gitattributes
git commit -m "Ajout du tracking LFS"

# Ajouter des gros fichiers
git add gros-fichier.zip
git commit -m "Ajout du gros fichier"
git push origin main
```

### Exemple Complet d'Utilisation
```bash
# Initialiser un nouveau projet avec LFS
mkdir mon-projet-lfs
cd mon-projet-lfs
git init
git lfs install

# Tracker les gros fichiers
git lfs track "*.psd"
git lfs track "*.ai"
git lfs track "videos/*.mp4"

# Vérifier le .gitattributes créé
cat .gitattributes
# *.psd filter=lfs diff=lfs merge=lfs -text
# *.ai filter=lfs diff=lfs merge=lfs -text
# videos/*.mp4 filter=lfs diff=lfs merge=lfs -text

# Ajouter et commiter la configuration
git add .gitattributes
git commit -m "Configuration Git LFS"

# Ajouter des gros fichiers
cp /path/to/design.psd .
git add design.psd
git commit -m "Ajout du fichier de design"

# Push vers le serveur
git remote add origin https://github.com/utilisateur/projet.git
git push -u origin main
```

### Commandes LFS Spécifiques
```bash
# Voir les fichiers LFS
git lfs ls-files

# Voir l'état des fichiers LFS
git lfs status

# Télécharger les fichiers LFS manquants
git lfs pull

# Pousser seulement les fichiers LFS
git lfs push origin main

# Voir l'historique LFS
git lfs logs last

# Vérifier l'intégrité
git lfs fsck

# Nettoyer le cache local
git lfs prune

# Migrer des fichiers existants vers LFS
git lfs migrate import --include="*.zip"
```

### Cloner un Dépôt avec LFS
```bash
# Clone normal (télécharge automatiquement les fichiers LFS)
git clone https://github.com/utilisateur/projet-lfs.git

# Clone sans télécharger les fichiers LFS
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/utilisateur/projet-lfs.git

# Télécharger les fichiers LFS après
cd projet-lfs
git lfs pull
```

---

## Commandes Avancées

### Stash (Remisage)
```bash
# Sauvegarder les modifications en cours
git stash

# Sauvegarder avec un message
git stash push -m "Travail en cours sur la fonctionnalité X"

# Inclure les fichiers non suivis
git stash -u
git stash --include-untracked

# Lister les stash
git stash list

# Appliquer le dernier stash
git stash apply
git stash pop      # Applique et supprime le stash

# Appliquer un stash spécifique
git stash apply stash@{2}

# Supprimer un stash
git stash drop stash@{1}

# Supprimer tous les stash
git stash clear

# Créer une branche depuis un stash
git stash branch nouvelle-branche stash@{1}
```

### Cherry-pick
```bash
# Appliquer un commit spécifique
git cherry-pick a1b2c3d

# Cherry-pick multiple commits
git cherry-pick a1b2c3d e4f5g6h

# Cherry-pick une plage de commits
git cherry-pick a1b2c3d..e4f5g6h

# Cherry-pick sans créer de commit
git cherry-pick --no-commit a1b2c3d

# Résoudre les conflits lors du cherry-pick
git cherry-pick --continue
git cherry-pick --abort
```

### Reset et Revert
```bash
# Reset soft (garde les changements dans l'index)
git reset --soft HEAD~1

# Reset mixed (default, garde les changements dans le working directory)
git reset HEAD~1
git reset --mixed HEAD~1

# Reset hard (supprime tous les changements)
git reset --hard HEAD~1

# Reset vers un commit spécifique
git reset --hard a1b2c3d

# Revert (crée un nouveau commit qui annule les changements)
git revert HEAD
git revert a1b2c3d
git revert HEAD~3..HEAD  # Revert plusieurs commits
```

### Tags
```bash
# Créer un tag léger
git tag v1.0.0

# Créer un tag annoté
git tag -a v1.0.0 -m "Version 1.0.0 - Release stable"

# Créer un tag sur un commit spécifique
git tag -a v0.9.0 a1b2c3d -m "Version 0.9.0"

# Lister les tags
git tag
git tag -l "v1.*"     # Tags matchant un pattern

# Voir les détails d'un tag
git show v1.0.0

# Pousser les tags
git push origin v1.0.0
git push origin --tags  # Tous les tags

# Supprimer un tag
git tag -d v1.0.0                    # Local
git push origin --delete tag v1.0.0 # Remote

# Checkout sur un tag
git checkout v1.0.0
git switch --detach v1.0.0
```

### Reflog
```bash
# Voir l'historique des références
git reflog

# Reflog d'une branche spécifique
git reflog show main

# Récupérer un commit "perdu"
git reflog
# a1b2c3d HEAD@{0}: reset: moving to HEAD~1
# e4f5g6h HEAD@{1}: commit: Fonctionnalité importante

# Récupérer le commit perdu
git reset --hard e4f5g6h

# Nettoyer le reflog (attention!)
git reflog expire --expire=30.days refs/heads/main
```

### Bisect (Recherche de Bug)
```bash
# Commencer une session bisect
git bisect start

# Marquer le commit actuel comme mauvais
git bisect bad

# Marquer un commit connu comme bon
git bisect good a1b2c3d

# Git checkout automatiquement un commit au milieu
# Tester et marquer
git bisect bad    # Si le bug est présent
git bisect good   # Si le bug n'est pas présent

# Continuer jusqu'à trouver le commit fautif

# Terminer la session bisect
git bisect reset

# Bisect automatique avec un script
git bisect start HEAD a1b2c3d
git bisect run npm test  # Exécute les tests automatiquement
```

### Blame et Log Avancés
```bash
# Voir qui a modifié chaque ligne
git blame fichier.txt
git blame -L 10,20 fichier.txt  # Lignes 10 à 20 seulement

# Log d'un fichier spécifique
git log fichier.txt
git log --follow fichier.txt  # Suit les renommages

# Log avec statistiques
git log --stat
git log --shortstat

# Log graphique
git log --graph --pretty=oneline --abbrev-commit

# Log depuis une date
git log --since="2023-01-01"
git log --until="2023-12-31"

# Log par auteur
git log --author="Nom Auteur"

# Chercher dans les messages de commit
git log --grep="bugfix"

# Chercher dans le contenu des commits
git log -S "fonction_importante"
git log -G "regex_pattern"
```

### Hooks Git
```bash
# Les hooks sont dans .git/hooks/
ls .git/hooks/

# Exemple de hook pre-commit
vim .git/hooks/pre-commit

#!/bin/sh
# Exemple de hook pre-commit
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed, commit aborted"
    exit 1
fi

# Rendre le hook exécutable
chmod +x .git/hooks/pre-commit

# Hooks disponibles:
# - pre-commit: avant chaque commit
# - post-commit: après chaque commit
# - pre-push: avant chaque push
# - post-merge: après chaque merge
# - pre-receive: côté serveur, avant réception
# - post-receive: côté serveur, après réception
```

---

## Résolution de Problèmes

### Problèmes Courants et Solutions

#### Annuler le Dernier Commit (non pushé)
```bash
# Garder les changements
git reset --soft HEAD~1

# Supprimer les changements
git reset --hard HEAD~1
```

#### Modifier le Message du Dernier Commit
```bash
git commit --amend -m "Nouveau message"

# Si déjà pushé (attention!)
git push --force-with-lease origin main
```

#### Récupérer un Fichier Supprimé
```bash
# Si le fichier était committé
git checkout HEAD~1 -- fichier-supprime.txt

# Depuis un commit spécifique
git checkout a1b2c3d -- fichier-supprime.txt

# Si supprimé mais pas encore committé
git restore fichier-supprime.txt
```

#### Annuler les Modifications d'un Fichier
```bash
# Modifications non indexées
git checkout -- fichier.txt
git restore fichier.txt

# Modifications indexées
git reset HEAD fichier.txt
git restore --staged fichier.txt
```

#### Résoudre des Conflits de Merge
```bash
# Voir les fichiers en conflit
git status

# Utiliser un outil de merge
git mergetool

# Ou éditer manuellement et résoudre
# Puis ajouter et commiter
git add fichier-resolu.txt
git commit
```

#### Nettoyer les Fichiers Non Suivis
```bash
# Voir ce qui sera supprimé
git clean -n

# Supprimer les fichiers non suivis
git clean -f

# Supprimer aussi les dossiers
git clean -fd

# Supprimer aussi les fichiers ignorés
git clean -fx
```

#### Problèmes de Push
```bash
# Si rejeté à cause de l'historique
git pull --rebase origin main
git push origin main

# Ou avec merge
git pull origin main
git push origin main

# Push forcé (danger!)
git push --force-with-lease origin main
```

#### Changer l'URL du Remote
```bash
# De HTTPS vers SSH
git remote set-url origin git@github.com:utilisateur/depot.git

# De SSH vers HTTPS
git remote set-url origin https://github.com/utilisateur/depot.git
```

### Optimisation et Maintenance

#### Nettoyage du Dépôt
```bash
# Nettoyer les références
git gc

# Nettoyage agressif
git gc --aggressive --prune=now

# Vérifier l'intégrité
git fsck

# Voir la taille du dépôt
du -sh .git/

# Nettoyer les branches merged
git branch --merged main | grep -v main | xargs -n 1 git branch -d
```

#### Optimisation LFS
```bash
# Nettoyer le cache LFS
git lfs prune

# Voir l'utilisation LFS
git lfs ls-files --size

# Migrer des fichiers vers LFS
git lfs migrate import --include="*.zip" --everything
```

### Alias Git Utiles
```bash
# Configuration d'alias utiles
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.ignore '!gi() { curl -sL https://www.toptal.com/developers/gitignore/api/$@ ;}; gi'

# Utilisation des alias
git st        # au lieu de git status
git co main   # au lieu de git checkout main
git lg        # log coloré et formaté
```

### .gitignore Avancé
```bash
# Exemples de .gitignore

# Fichiers système
.DS_Store
Thumbs.db
*.tmp
*.log

# Environnements de développement
.vscode/
.idea/
*.swp
*.swo

# Dependencies
node_modules/
vendor/
*.pyc
__pycache__/

# Build artifacts
dist/
build/
*.o
*.exe

# Environnement
.env
.env.local
.env.production

# Bases de données
*.db
*.sqlite

# Ignorer tout sauf certains fichiers
*
!.gitignore
!src/
!src/**

# Patterns avancés
# Ignorer tous les .txt sauf important.txt
*.txt
!important.txt

# Ignorer dans tous les sous-dossiers
**/logs
**/node_modules

# Créer automatiquement un .gitignore
git config --global alias.ignore '!gi() { curl -sL https://www.toptal.com/developers/gitignore/api/$@ ;}; gi'
git ignore python,node,visualstudiocode > .gitignore
```

---

## Workflows Git Courants

### Git Flow
```bash
# Installation de git-flow
# Ubuntu: sudo apt install git-flow
# macOS: brew install git-flow-avh

# Initialisation
git flow init

# Nouvelles fonctionnalités
git flow feature start nouvelle-fonctionnalite
# Travailler sur la fonctionnalité...
git flow feature finish nouvelle-fonctionnalite

# Releases
git flow release start 1.0.0
# Finaliser la release...
git flow release finish 1.0.0

# Hotfixes
git flow hotfix start urgent-fix
# Corriger le bug...
git flow hotfix finish urgent-fix
```

### GitHub Flow (Simplifié)
```bash
# 1. Créer une branche depuis main
git checkout main
git pull origin main
git checkout -b feature/nouvelle-fonctionnalite

# 2. Travailler et commiter
# ... développement ...
git add .
git commit -m "Ajout de la nouvelle fonctionnalité"

# 3. Pousser et créer une Pull Request
git push -u origin feature/nouvelle-fonctionnalite

# 4. Après approbation, merger dans main
git checkout main
git pull origin main
git merge --no-ff feature/nouvelle-fonctionnalite
git push origin main

# 5. Nettoyer
git branch -d feature/nouvelle-fonctionnalite
git push origin --delete feature/nouvelle-fonctionnalite
```

---

## Conclusion

Ce tutoriel couvre les commandes essentielles et avancées de Git et Git LFS. Pour approfondir vos connaissances:

- [Documentation officielle Git](https://git-scm.com/doc)
- [Documentation Git LFS](https://git-lfs.github.io/)
- [Pro Git Book](https://git-scm.com/book)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

### Commandes de Référence Rapide
```bash
# Configuration
git config --global user.name "Nom"
git config --global user.email "email@example.com"

# Base
git init
git clone <url>
git status
git add .
git commit -m "message"
git push
git pull

# Branches
git branch
git checkout -b <branche>
git merge <branche>

# LFS
git lfs install
git lfs track "*.extension"
git lfs ls-files

# Utilitaires
git log --oneline
git diff
git stash
git reset --hard HEAD~1
```
