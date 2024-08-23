# Création ebuild Hello World

L'objectif ici est de se familiariser avec le fonctionnement de l'ebuild en commancant par l'ebuild le plus simple étant Hello World.

## Prérequis

Si vous avez correctement installé votre OS Gentoo il n'y aura pas besoin d'installer des outils supplémentaires. Sauf pour un cas que je vais vous présenter sous peu.


### Création de son repository perso 

Il va falloir votre propre repository pour Portage, si vous en avez déjà un vous pouvez directement allez à la prochaine partie.

Il est possible de créer manuellement son repository et si vous souhaitez le faire vous pouvez suivre ce guide sur la documentation officielle de Gentoo : 
https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Defining_a_custom_ebuild_repository

De mon côté, l'outil eselect permet de créer automatiquement le repository en créeant et modifiant les fichiers nécessaires. Pour se faire il faut installer la fonctionnalité repository de eselect avec cette commande : 

    emerge -a app-eselect/eselect-repository

Puis créer votre repository en remplaçant `custom_hello` par le nom de votre choix : 

    eselect repository create custom_hello

Et on met portage comme propriétaire du repository pour lui accorder les permissions nécessaires.

    chown -R portage:portage /var/db/repos/custom_hello

Une fois cela fait on peut passer à la création de l'ebuild.


### Création de l'ebuild

Tout d'abord on crée les directories adéquat pour le programme qui sont de la forme `{CATEGORY}/{NOM_PROGRAMME}` en donnant les permissions nécessaires à portage.  
``Remarque`` : Il est possible de créer une catégorie perso  

    cd /var/db/repos/custom_hello
    mkdir -p app-misc/hello
    chown -R portage:portage app-misc/hello

Puis on crée l'ebuild adéquat.

    cd app-misc/hello
    vim ./hello-2.6.90.ebuild

Dans notre cas, nous allons créer l'ebuild pour installer le programme hello de GNU ayant pour version 2.6.90. La structure du nom du fichier n'est pas faite au hasard et permet à l'ebuild de fonctionner correctement. Il faut suivre la structure `{NOM_PROGRAMME}-{VERSION}.ebuild`

Vous pourrez copier directement l'ebuild que j'ai fourni dans ce dépôt.

Son fonctionnement est assez simple car il y'a peu d'options importantes à mettre en place. La plus importante étant `SRC_URI` indiquant à portage où récupérer le code source du programme.

Une fois l'ebild conçu, changez ses droits pour être sur que portage ait la main mise dessus. 

    chown portage:portage hello-2.6.90.ebuild


### Execution de l'ebuild

Enfin nous pouvons installer le programme tout d'abord en créant le Manifest associé à l'ebuild. 

    ebuild hello-2.6.90.ebuild manifest

Puis en emergeant le programme. 

    emerge app-misc/hello::custom_hello

N'oubliez pas de mettre le nom de votre repository à la place de `cutom_hello` pour préciser à Portage de chercher dans votre repository et non pas celui par défaut.

Après l'installation vérifier le bon fonctionnement du programme avec la commande :

    hello

C'est la fin de la création de ce premier ebuild, merci pour votre attention.
