Création ebuild Hello World
Je vais d’abord faire une version basique en imitant juste et en faisant directement l’ebuild à partir du lien du site.

On crée d’abord le repo perso  : 
emerge -a app-eselect/eselect-repository
eselect repository create custom_hello


Puis on crée le chemin et l’ebuild adéquat. 
chown -R portage:portage /var/db/repos/custom_hello
cd /var/db/repos/custom_hello
mkdir -p app-misc/hello
chown -R portage:portage app-misc/hello


Je crée ensuite l’ebuild adéquat que je documenterais après avoir réussi.
cd app-misc/hello
vim ./hello-2.6.90.ebuild


EAPI=7
 
DESCRIPTION="Création de premier ebuild"
HOMEPAGE="https://alpha.gnu.org/"
SRC_URI="https://alpha.gnu.org/gnu/hello/hello-2.6.90.tar.gz"
 
LICENSE=""
SLOT="0"
KEYWORDS="amd64 x86"
 
DEPEND=""
RDEPEND="${DEPEND}"
BDEPEND=""



On change ensuite les droits dessus : 
chown -R portage:portage hello-2.6.90.ebuild


A la place de repoman j’utilise ebuild : 
ebuild hello-2.6.90.ebuild manifest


Puis on emerge avec : 
emerge app-misc/hello::custom_hello


Et on test avec la commande : 
hello

