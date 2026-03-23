# Minishell

Minishell est un projet 42 implémentant un shell Bash simplifiée en C, recréant les fonctionnalités essentielles du terminal comme les pipes, les redirections, l'implementation des commandes, création de built-ins et les variables d'environnement. Ce projet est typiquement réalisé dans le cadre de formations comme 42 School pour maîtriser les processus, les signaux et le parsing de commandes.
Fonctionnalités

- Lecture et parsing de commandes avec gestion des espaces, quotes et escapes.
- Support des pipes "|" pour chaîner les commandes.
  - Support des pipes avec attente de commandes. "|\n" 
- Redirections d'entrée, "<", Heredocs ""<<", sortie truncate ">" et append ">>".
- Variables d'environnement ($VAR) et built-ins comme echo, cd, pwd, export, unset, env, exit.
- Gestion des signaux (Ctrl+C, Ctrl+D).
- Expansion de $? pour le code de retour de la dernière commande.
- Prise en charge de $SHLVL pour connaitre le niveau du terminal.

## Prérequis :

Système Unix-like (Linux/macOS).
Compilateur GCC.
Bibliothèques : readline (pour l'édition de ligne), libc.

## Installation :

Clonez le dépôt :

    git clone https://github.com/Sellith/Minishell.git
    cd Minishell

Installez les dépendances (sur Debian/Ubuntu) :

    sudo apt update
    sudo apt install libreadline-dev

Compilez le projet :

    make

Cela génère l'exécutable minishell dans un dossier bin/ généré dans le dossier local.

## Utilisation

Lancez le shell :

    bin/minishell

Exemples de commandes :

    $ echo "Hello World"
    Hello World
    $ ls -la | grep Makefile > files.txt
    $ cat < files.txt
    $ export MY_VAR=42
    $ echo $MY_VAR
    42
    $ exit

Pour quitter : exit ou Ctrl+D.

## Structure du Projet

    Minishell/
    ├── Dependencies   # Contient ma libft ameliorée  
    ├── includes/      # Headers (minishell.h)
    ├── bin/           # Fichiers binaires du projet (libft.a, minishell)
    ├── build/         # Fichiers objets du projet une fois compilé 
    ├── src/
    │   ├── parsing/   # Séparation tokens, quotes, redirections
    │   ├── execution/ # Pipes, forks, execve
    │   ├── built-ins/ # echo, cd, export, etc.
    │   ├── exit_free  # Contiens les fonctions permettant une sortie du programe sans leaks de mémoire.
    │   └── utils/     # Libft-like functions.
    ├── Makefile
    └── README.md

## Liste des Built-ins supportés

Notre Minishell supporte plusieurs Commandes Built-ins avec certains parametres telles que :

- echo : Ecrit une chaine de charactères dans le sortie standart suivi d'un retour chariot.
  - -n : Supprime le retour chariot
- pwd : Affiche le repertoire courant dans la sortie standart.
- cd : Change le repertoire courant, gère les chemins relatifs et absoluts
  - Sans arguments : change le repertoire courant sur "home/".
  - "~" : Change le repertoire courant sur "home/".
  - "-" : Permet de revenir sur l'ancien repertoire courant.
- env : Affiche toutes les variables d'environement.
- export : Rajoute la variable passée en parametres dans les variables d'environement du shell actuel.
  - Sans arguments : Agis comme "env".
- unset : Retire la variable passée en paramètres des variables d'environement.
- exit : Quitte la session courante du shell bash.
  - Sans arguments : renvoi l'exit code de la précédante commande.
  - Avec un exit code : renvoi l'exit code passé en paramètre modulo 255. 

## Règles du Makefile

Le makefile a été confectionné de sorte que presque tout peut-etre contrôlé à partir de ses règles :

### Règles générales 
- clean : Supprime tous les fichiers objets existants (libft, et sources). 
- fclean : Supprime tous les fichiers objets et binaires existant (libft et sources).
- re : Supprime tous les fichiers objets et binaires et recompile l'ensemble du code.

### Règles de la libft
- libft : Clone la libft de mon repo github, la compile, copies les fichiers headers dans include/ et le fichier binaire dans bin/archives/.
- rmlib : Supprime tous les fichiers de la libft.
- udlib : Supprime tous les fichiers de la libft et la reclone. 
- cleandep : Supprime tous les fichiers objets de la libft.
- fcleandep : Supprime tous les fichiers objets et binaires de la libft.
- mkdep : Compile la libft.
- redep : Supprime tous les fichiers de la libft objets et binaires et la recompile.

### Règle des fichiers sources
- cleansrc : Supprime tous les fichiers objets du code sources.
- fcleansrc : Supprime tous les fichiers objets et binaires du code source.
- resrc : Supprime tous les fichiers objets et binaires du code source et recompile .

### Règle de normes et de tests
- val : Lance minishell avec valgrind.
- normy : Lance la norminette avec les flags et fait un diff.
- val_env_less : Lance minishell avec valgrind et avec env -i


## Normes

- Utilisation des flags stricts pour la compilation (-Wall -Wextra -Werror).
- Respecte la norme 42 (pas de globals excessives, fonctions courtes).
- Pas de termcaps (version pré-mise à jour du sujet 42).
- Leaks : make val ou Valgrind : le projet contient un .valgrindrc à la base du dossier
