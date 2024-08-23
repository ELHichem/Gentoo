# Création ebuild Hello Penguin

L'objectif ici est de se familiariser avec le fonctionnement de l'ebuild en modifiant le code source du programme Hello et en manipulant la variable `SRC_URI`. Nous allons rajouter une option -p au programme affichant ce magnifique pingouin saluant : 

    ⠀⠀⠀⠀⠀⠀⠀⣠⣠⣶⣿⣷⣿⣿⣿⣷⣷⣶⣤⣀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
    ⠀⠀⠀⠀⠀⣤⣾⣿⢿⣻⡽⣞⣳⡽⠚⠉⠉⠙⠛⢿⣶⣄⠀⠀⠀⠀⠀⠀⠀⠀⠀
    ⠀⠀⠀⠀⣼⣿⣿⢻⣟⣧⢿⣻⢿⠀⠀⠀⠀⠀⠀⠀⠻⣿⣧⠀⠀⠀⠀⠀⠀⠀⠀
    ⠀⠀⢀⣾⣿⡿⠞⠛⠚⠫⣟⡿⣿⠀⠀⠀⠀⠀⠀⠀⠀⠘⢿⣧⠀⠀⠀⠀⠀⠀⠀
    ⠀⠀⣼⣿⡟⠀⠀⠀⠀⠀⠈⢻⡽⣆⠀⠀⣴⣷⡄⠀⠀⠀⠘⣿⡆⠀⠀⣀⣠⣤⡄
    ⠀⠀⣿⣿⠁⠀⠀⠀⠀⠀⠀⠈⣿⠿⢷⡀⠘⠛⠃⠀⠠⠀⠀⣿⣅⣴⡶⠟⠋⢹⣿
    ⠀⠀⢻⣿⡀⠀⠀⠀⢾⣿⡆⠀⢿⣴⣴⡇⠀⠀⠀⠀⠀⠀⢠⡟⠋⠁⠀⠀⠀⢸⣿
    ⠀⠀⠈⢿⣇⠀⠀⠀⠈⠉⠥⠀⠀⠉⠉⠀⠀⠀⠀⠀⠀⢀⡾⠁⠀⠀⠀⠀⠀⣾⡏
    ⠀⠀⠀⠈⢿⣦⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢠⠸⠁⠀⠀⠀⠀⠀⣼⡟⠀
    ⠀⠀⠀⠀⠀⣹⣿⣶⣤⣀⡀⠀⠀⠀⠀⠀⠀⠀⠀⠂⠁⠀⠐⢧⡀⠀⢀⣾⠟⠀⠀
    ⠀⠀⢀⣰⣾⠟⠉⠀⠀⠉⠉⠀⠐⠂⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⢿⣶⡟⠋⠀⠀⠀
    ⣠⣶⡿⠋⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⡀⠀⠈⣿⡆⠀⠀⠀⠀
    ⢻⣧⣄⠀⠀⠀⢰⡇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢸⣿⠀⠀⠀⠀
    ⠀⠉⠛⠿⣷⣶⣾⡇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢸⣿⠀⠀⠀⠀
    ⠀⠀⠀⠀⠀⠀⣿⡇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣤⣤⣾⣿⠀⠀⠀⠀
    ⠀⠀⠀⠀⠀⠀⢹⣿⣿⣿⣿⣷⣦⡀⠀⢀⣀⠀⠀⠀⣠⣴⣿⣿⣿⣿⣷⠀⠀⠀⠀
    ⠀⠀⠀⠀⠀⠀⠀⠻⢿⣿⣿⣿⣿⠿⠿⠿⠿⠿⠿⠿⠿⣿⣿⣿⠿⠟⠁⠀⠀⠀⠀

## Prérequis

Si vous avez correctement installé votre OS Gentoo il n'y aura pas besoin d'installer des outils supplémentaires. Il vous faudra votre propre repository donc si vous n'en avez pas vous pouvez aller voir mon dépôt `Gentoo/ebuilds/hello_world` et suivez la première partie du README.


### Téléchargement et modification du code source

Vous allez tout d'abord vous placez dans votre repository et dans le dossier `hello` pour créer le programme.

    cd /var/db/repos/custom_hello/hello

Puis téléchargez le tarball du programme et décompressez le dans ce meme dossier : 

    wget https://alpha.gnu.org/gnu/hello/hello-2.6.90.tar.gz
    tar -xzf hello-2.6.90.tar.gz

Ensuite remplacez le fichier `hello-2.6.90/src/hello.c` par celui que je vous ai fourni dans le dépôt.  
Pour une meilleur compréhension j'ai précisé via des commentaires les modifications effectuées dans le fichier.

### Création de l'ebuild

Maintenant compressez à nouveau le programme et placez le dans le directory adéquat tout en changeant ses permission pour permettre à Portage d'avoir la main dessus.

    tar -czvf hello-2.6.90.tar.gz hello-2.6.90/
    cp hello-2.6.90.tar.gz /var/cache/distfiles/
    chown -R portage:portage /var/cache/distfiles/hello-2.6.90.tar.gz

Puis copiez l'ebuild que j'ai fourni dans ce dépôt dans votre dossier `hello`. La différence importante de celui-ci par rapport au permier ebuild est `SCR_URI`. En effet, au lieu de chercher le programme sur Internet nous précisons à Portage que le programme se situe localement dans le dossier `/var/cache/distfiles`.

### Execution de l'ebuild

Enfin procédez à l'installation du programme de la même manière que le premier ebuild.

    chown -R portage:portage hello-2.6.90.ebuild
    ebuild hello-2.6.90.ebuild manifest
    emerge app-misc/hello::custom_hello

Et testez le fonctionnement de celui-ci avec la commande :

    hello -p

Vous affichant notre magnifique pingouin saluant et concluant ce petit tuto.
