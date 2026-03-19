# Minishell

Minishell est un projet 42 implémentant un shell Bash simplifiée en C, recréant les fonctionnalités essentielles de bash comme les pipes, les redirections et les variables d'environnement. Ce projet est typiquement réalisé dans le cadre de formations comme 42 School pour maîtriser les processus, les signaux et le parsing de commandes.
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

Utilisation

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
Structure du Projet


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

## Règles du Makefile

Le makefile a été confectionné de sorte que tout peut etre contrôlé à partir de ses règles :
$(NAME)

### Règles générales 
- clean : supprime tous les fichiers objets existants (libft, et sources). 
- fclean : supprime tous les fichiers objets et binaires existant (libft et sources).
- re : supprime tous les fichiers objets et binaires et recompile l'ensemble du code.

### Règles de la libft
- libft : clone la libft de mon repo github, la compile, copies les fichiers headers dans include/ et le fichiers binaire dans bin/.
- rmlib : supprime tous les fichiers de la libft.
- udlib : supprime tous les fichiers de la libft et la reclone. 
- cleandep : supprime tous les fichiers objets de la libft.
- fcleandep : supprime tous les fichiers objets et binaires de la libft.
- mkdep : compile la libft.
- redep : supprime tous les fichiers de la libft objets et binaires et la recompile.

### Règle des fichiers sources
- cleansrc : supprime tous les fichiers objets du code sources.
- fcleansrc : supprime tous les fichiers objets et binaires du code source.
- resrc : supprime tous les fichiers objets et binaires du code source et recompile .

### Règle de normes et de tests
- val : lance minishell avec valgrind.
- normy : lance la norminette avec les flags et fait un diff.
- val_env_less : lance minishell avec valgrind et avec env -i


## Normes

- Utilisation des flags stricts pour la compilation (-Wall -Wextra -Werror).
- Respecte la norme 42 (pas de globals excessives, fonctions courtes).
- Pas de termcaps (version pré-mise à jour du sujet 42).
- Leaks : make val ou Valgrind : le projet contient un .valgrindrc à la base du dossier
