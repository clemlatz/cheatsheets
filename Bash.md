# Maîtriser le shell Bash

## 1. Découvrez votre système d'exploitation

### A11 Qu'est-ce que la ligne de commande ?

* Shell interprète une ligne de commande
* Une commande lance un programme (interne au Bash, outils de l'OS ou programmes de l'utilisateur)

La présence de l'invite de commande indique que le shell est prêt à recevoir des commandes.

    alice@order-alice ~/Documents $
    
À l'appuie sur la touche Entrée :
* La commande lance l'exécution d'un programme
* Le résultat s'affiche à l'écran
* L'invite s'affiche à nouveau

Une ligne de commande est constitué par des mots, séparé par des espaces. Le format général est le nom de la commande, suivi par des arguments qui seront interprété par cette dernière. Le nombre d'argument dépend de la commande appelée. 

    $ commande arg1 arg2 arg3 arg4 ...
    $ echo Bonjour à tous
    
Lorsqu'un argument est précédé d'un tiret, il est appelé une option. Une option a souvent un équivalent plus long séparé par deux tirets.

    $ date -u
    $ date --utc
    
### A12 Trouver de l'aide

Il est possible d'obtenir de l'aide directement dans le terminal de plusieurs manières :

* en effectuant une erreur d'une syntaxe
* en appelant une commande avec l'option -h ou --help
* en appelant la commande man (exemple : `man date`)

La commande `man` est la plus complète et propose le nom, un synopsis, une description et des exemples.

    man date

La commande `apropos` permet de lancer une recherche et retourne les commandes dont le mot-clé figure dans la partie description.

    apropos encoding
    

### A13 Gérer les répertoires et les fichiers

Commandes pour naviguer dans l'arborescence :

* `pwd` : retourner le chemin de la racine au répertoire courant
* `ls` : lister le répertoire du courant (-p pour signaler les répertoires)
* `cd` : changer de répertoire
    * sans argument pour revenir au répertoire utilisateur
    * suivi du nom d'un répertoire pour y entrer (`cd Documents`)
    * suivi de `..` pour revenir au répertoire (`cd ..`)
    * suivi du chemin de n'importe quel dossier de l'arborescence (`cd /home/clement/Documents/cv`)
* `mkdir` : créer un répertoire (`mkdir notes`)
* `touch` : créer un fichier vide (`touch notes/memo.txt`)
* `rm` : pour supprimer un fichier (`rm notes/memo.txt`)
* `rmdir` : pour supprimer un répertoire vide (`rmdir notes`)
* `cp` : copier un répertoire ou un fichier (`cp tom jerry`)
* `mv` : déplacer un répertoire ou un fichier (`mv jerry souris`)


### A14 Les utilisateurs et leurs droits

Plusieurs personnes peuvent travailler sur une même machine. Le système doit pouvoir distinguer chaque utilisateur, gérer la propriété des fichiers et partager les ressources. Chaque utilisateur se voit attribuer un identifiant de connexion (login), un mot de passe et un point d'entrée dans l'arborescence (son répertoire personnel).

Le système définit deux types d'utilisateurs :
* super-utilisateur root : il peut tout faire et modifier le système
* utilisateurs : ils ont des droits limités et sont répartis dans des groupes définis par root

La commande `id` permet d'obtenir des informations sur l'utilisateur actuel.

Le système gère l'accès au fichier selon des droits défini selon trois publics différents :
* le propriétaire
* les membres du groupe principal du propriétaire
* les autres utilisateurs

Pour chacun de ces publics, il y a trois trois d'accès :
* r : lecteur (read)
* w : écriture (write)
* x : exécution (execute)

On peut visualiser ces droits d'accès à l'aide de la commande `ls -l`. `-rwxrw-r--` signifie par exemple que le fichier est accessible en lecture, écriture et exécution pour le propriétaire, lecture et écriture pour son groupe principal pour son groupe principal et lecture uniquement pour les autres utilisateurs. Le premier tiret signifie qu'il s'agit d'un fichier (`d` sinon).

Le droit x sur un répertoire indiqu qu'on peut s'y positionner ou le traverser.

Les droits accès d'un fichier peuvent être modifié par son propriétaire ou par root à l'aide de la commande `chmod` dont les arguments sont `public opération droits`. Le public est exprimé par une lettre (u pour le propriétaire, g pour le groupe, o pour les autres). Le deuxième caratère est exprimé par un signe (+ pour ajouter le droit, - pour le retirer) et le troisième indique le droit (r, w, x).

Retirer le droit de lecture aux autres utilisateur :

    chmod o-r fichier1.txt
    
On peut modifier plusieurs droits en séparent les arguments par des virgules :

    chmod g+w,o-rx Documents/
    
Notation numérique octale : on peut également exprimer les droits sous les formes de nombre en additionant les droits (4 pour r, 2 pour w, x pour 1).

Pour donner les droits rwx (4+2+1=7) à l'utilisateur, r-x (4+1=5) au groupe et r-- (4) aux autres :

    chmod 754 fichier1.txt


### A15 Traiter un fichier texte

Un fichier texte est une suite de caractère sans mise en forme.

* `cat` : afficher le contenu du fichier
* `less` : permet de naviger dans le fichier
    * `less -N` permet d'afficher les numéros de ligne
* `head` : permet d'afficher les premières lettres du fichier
* `tail` : permet d'afficher les dernières lettres du fichier

Pour nommer un fichier, il est recommandé d'utiliser des caractères alphanumérique (a-zA-Z0-9), des tirets bas (_) et des traits d'union (-) sauf au début, le reste (espace, accents, symboles de ponctuation) est à proscrire.

L'utilitaire `vi` permet d'éditer les fichiers.






