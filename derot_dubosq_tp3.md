William Dubosq
Robin Dérot
4ETI

 # Compte-rendu TP3 Administration système

## Exercice 1 :
### Commandes de base

1. #### Quels sont les 5 derniers paquets installés sur votre machine ?

  On utilise la commande suivante :
  
  ```bash
  cat /var/log/dpkg.log | grep " install " | tail -5
  ```
  
  On cherche les paquets contenant le mot *" install "*, puis on séléctionne les 5 derniers avec le mot *tail*.

2. #### Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel!).Comment explique-t-on la (petite) différence de comptage?

  Avec la commande *dpkg*, on utilise la commande qui suit :
  
  ```bash
  cd /var/log
  dpkg -l | grep "^ii" | wc -l
  ```
  
  Le résultat que nous obtenons est 543. Il faut préciser la commande avec *grep "^ii"* car il y a un en-tête avant les paquets installés. On ne compte donc que les lignes commençant par *ii*.
  
  Avec la commande *dpkg*, on utilise la commande qui suit :
  
  ```bash
  apt list --installed | wc -l
  ```
  
  Le résultat obtenu avec cette commande est 544. La différence est due à la première ligne *en train de lister* qui est comptabilisée. Il n'est pas possible de la filtrer avec *grep*.
  
3. #### Combien de paquets sont disponibles en téléchargement ?
  
  On peut connaître le nombre de paquets disponibles avce la commande suivante :
  
  ```bash
  apt list | wc -l
  ```
  
  Le résultat obtenu avec cette commande est 61 599.
  
4. #### Créer un alias “maj” qui met à jour le système

  On se place dans le répertoire personnel et on crée l'alias dans .bashrc:
  
  ```bash
  cd
  ```
  
  Dans .bashrc :
  
  ```bash
  alias maj="sudo apt full-upgrade"
  ```
  
  On sauve avec la commande :
  
  ```bash
  source ~/.bashrc
  ```

5. #### A quoi sert le paquet *fortunes*? Installez-le.

  Le paquet *fortunes* contient des "épigrammes". Ce sont des citations.
  
  Pour installer ce paquet, il faut utiliser la commande :
  ```bash
  sudo apt install fortunes
  ```
  
6. #### Quels paquets proposent de jouer au sudoku?

  Il s'agit du paquet *gnome-sudoku*.

7. #### Lister les derniers paquets installés explicitement avec la commandeapt install

  On utilise la commande suivante :
  
  ```bash
  cat /var/log/dpkg.log | grep " install " | tail -10
  ```
  
  On obtient alors les paquets *fortunes* :
  
  ```bash
  2020-02-28 14:14:49 install fortune-mod:amd64 <aucune> 1:1.99.1-7build
  2020-02-28 14:14:49 install fortune-min:all <aucune> 1:1.99.1-7build1
  2020-02-28 14:14:49 install fortune:all <aucune> 1:1.99.1-7build1
  ```
## Exercice 2 :
#### A partir de quel paquet est installée la commande ls? Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des commandes utiles)? Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension.sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.
  
  On se rend dans notre répertoire personnel. On utilise la commande suivante :
  ```bash
  which -a ls | tail -1 | xargs dpkg -S
  ```
  
  On utilise *tail -1* car on cherche le chemin de la commande. On utilise *xargs* pour passer le chemin de la command en argument de *dpkg -S*.
  
  Si on veut rélaiser cette opération avec un script, on crée un script *origine-commande* contenant le code suivant :
  
  ```bash
  #!/bin/bash
  echo $(which -a $1 | tail -1 | xargs dpkg -S)
  ```
  
  Le résultat de ces opérations donne :
  
  *coreutils: /bin/ls*
  

## Exercice 3 :  
#### Ecrire une commande qui aﬀiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.

  Nous réutilisons le script que nous avons imaginé à la question précédente. En effet, celui-ci renvoie le nom du package correspondant à la commande qui a été installée. On veut obtenir uniquement le nom du paquet, donc il nous faut couper ce nom avant le *:*.
  On utilise donc cut comme separateur, et on recherche le nom du paquet en executant grep dans dpkg -l.
  Pour la commande ls, on execute alors : 
  
  ```bash
  var=$(./origine-commande ls | cut -f 1 -d :);(dpkg -l | grep $var) && echo "INSTALLE" || echo "NON INSTALLE"
  ```
  
## Exercice 4 :  
#### Lister les programmes livrés avec coreutils. A quoi sert la commande '\[' et comment aﬀicher ce qu’elle retourne?

  On utilise la commande suivante :
  
  ```bash
  apt show coreutils
  ```
  
  Cela nous donne comme réponse les détails du paquet avec à la fin les commandes contenues dans le paquet :

  *arch base64 basename cat chcon chgrp chmod chown chroot cksum comm cp csplit cut date dd df dir dircolors dirname du echo env expand expr factor false flock fmt fold groups head hostid id install join link ln logname ls md5sum mkdir mkfifo mknod mktemp mv nice nl nohup nproc numfmt od paste pathchk pinky pr printenv printf ptx pwd readlink realpath rm rmdir runcon shasum seq shred sleep sort split stat stty sum sync tac tail tee test timeout touch tr true truncate tsort tty uname unexpand uniq unlink users vdir wc who whoami yes*

  Pour afficher ce qu'elle retourne :
  
  ```bash
  var=$(apt show coreutils);echo $var
  ```
  
## Exercice 5 :
## Aptitude
#### Installez le paquet emacs à l’aide de la version graphique d’aptitude.

  On commence par installer aptitude avec *apt install aptitude*
  on lance aptitude, on tape "/" puis on recherche " emacs".
  On l'installe enfin avec "+".
  
## Exercice 6 :
##  Installation d’un paquet par PPA
#### Vérifiez qu’un nouveau fichier a été créé dans/etc/apt/sources.list.d. Que contient-il ?
  
  On exécute les commandes de l'énoncé.
  On effectue les commandes suivantes :
  
  ```bash
  cd /etc/apt/sources.list.d 
  ls
  ```
  
  On obtient alors : *linuxuprising-ubuntu-java-eoan.list*

## Exercice 7 : 
## Creation d'un depot personalisé
#### Création d’un paquet Debian avec dpkg-deb

  Tout d'abord on créé l'arborescence demandée :
  
  ```bash
  cd
  cd scripts
  mkdir origine-commande
  cd origine-commande
  mkdir DEBIAN
  mkdir usr
  cd usr
  mkdir local
  cd local 
  mkdir bin
  ```
  
  On créé ensuite le fichier "control" suivant dans DEBIAN, qui contient la configuration de notre paquet : 
  
  ```bash
  Package: origine-commande
  Version: 0.1
  Maintainer: Derot_Dubosq
  Architecture: all
  Description: Cherche l'origine d'une commande
  Section: utils
  Priority : optional
  ```
  
  Enfin, on retourne dans notre dossier personnel pour créer le package :
  
  ```bash
  cd
  dpkg-deb --build origine-commande
  ```

#### Création du dépôt personnel avec reprepro

  On commence par créer l'arborescence demandée dans notre dossier personnel :
  
  ```bash
  cd
  mkdir repo-cpe
  cd repo-cpe
  mkdir conf
  mkdir packages
  ```
  
  Dans conf, qui contient la configuration du dépôt, on créé donc le fichier distribution suivant :
  
  ```bash
  Origin: Derot_Dubosq
  Label: origine-commande
  // Suite: stable
  Codename: cosmic
  Architectures: i386 amd64
  Components: universe
  Description: Cherche l'origine d'une commande
  ```
  
  On genere ensuite l'arborescence grâce à la commande :
  
  ```bash
  reprepro -b . export
  ```
  
  On copie "origine-commande.deb" dans le dossier packages, qui contient les paquets, puis on fait en sorte que le paquet soit inscrit dans le dépot :
  
  ```bash
  cp ~/scripts/origine-commande.deb ~/repo-cpe/packages
  reprepro -b . includedeb cosmic origine-commande.deb
  ```

Pour indiquer à apt qu'il existe un nouveau dépot, on va créer repo-cpe.list dans r /etc/apt/sources.list.d :

```bash
cd /etc/apt/sources.list.d
sudo nano repo-cpe.list
## Dans nano
deb file:/home/derot_dubosq/repo-cpe cosmic multiverse
##
```

Enfin on lance *sudo apt update* pour detecter notre nouveau paquet.

#### Signature du dépôt avec GPG
*GPG est la version GNU du protocole PGP (Pretty Good Privacy), qui permet d’échanger des données de
manière sécurisée. Ce système repose sur la notion de clés de chiffrement asymétriques (une clé publique et
une clé privée)*

On créé une nouvelle paire de clés grace à la commande :

```bash
gpg --gen-key
Real name: derot_dubosq
Email address: robin.derot@cpe.fr
okay
passphrase : gaz_ez_au_baby
```
Dans le fichier "distribution", on rajoute la ligne suivante :

```bash
SignWith: yes
```

On ajoute ensuite la clé à notre dépôt... :

```bash
reprepro --ask-passphrase -b . export
```

...puis la clé publique à notre dépôt :

```bash
gpg --export -a "auteur" > public.key
```

Enfin, on ajoute cette clé à la liste des clés connues de apt :

```bash
sudo apt-key add public.key
```

## Exercice 8.
## Installation d’un logiciel à partir du code source
*Lorsqu’un logiciel n’est disponible ni dans les dépôts officiels, ni dans un PPA, ou encore parce qu’on
souhaite n’installer qu’une partie de ses fonctionnalités, on peut se tourner vers la compilation du code
source.*
*Malheureusement, cette installation "à la main" fait qu’on ne propose pas des bénéfices de la gestion de
paquets apportée par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé "à la
main" en un paquet, et de le gérer ensuite avec apt; c’est ce que permet par exemple checkinstall.*

On commence par cloner le dépôt git suivant pour récuperer en local le code source du logiciel nudoku :
```bash
cd
git clone https://github.com/jubalh/nudoku
```

On lance ensuite la commande "autoreconf -i" en installant en chaque execution les paquets manquants jusqu'a ce qu'elle ss'exxecute sans erreur : on voit donc qu'il nous manque le paquet "gettext", mais nous n'avons pas réussi à l'installer.

