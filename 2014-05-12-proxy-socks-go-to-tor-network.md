---
published: false
layout: post
title: "Proxy Socks: Go to Tor Network"
lang: fr
category: tech
tags:
- sysadmin
- tools
- linux
- archlinux
- raspberrypi
---

Suite au précédent article []() (qui date d'un an, je sais, je ne suis pas très assidu, mais je vais changer… promis), j'ai continué à utiliser ce proxy socks qui m'a rendu service à bien des occasions, que ce soit pour contourner des limitations de mon précédent FAI ou pour contourner les PALC chez certains clients un poil trop frileux côté sécurité.

<-- more -->

Récemment, suite à un petit changement d'infra, j'avais supprimé mon petit VPS et je m'étais juré de remonter mon proxy Socks dans mon copious-spare-time®. Du coup, passage en v2 oblige, le proxy utilise désormais [Tor]() pour se connecter et du même coup offrir un accès au subnet à l'oignon. Et pour faire encore plus propre, le tout est désormais propulsé sur un petit RaspberryPi hébergé à domicile.

Autant pour vous que pour moi, je laisse ici la recette de mise en route de la machine. _ENJOY_ :].

## You Said _RPi_?

J'avais un RPi sous la main, je ne m'en servais pas pour le moment, c'est un usage idéal (même si sa carte réseau n'est pas la meilleure que vous puissiez trouver). Cette partie est dédié à l'installation et à la configuration du petit-pc-qui-tient-dans-la-main. Si vous utilisez un autre serveur / votre NAS / _whateveryouwant_, foncez à la partie suivante.

### Récupération des sources

J'utilise une ArchLinux sur mon RPi parce que j'aime bien : ça me rappelle mes amours coupables avec Gentoo sans toute sa complexité et ses temps de compil' interminables. Et puis soyons franc : la communauté Arch est très agréable, réactive, et [sa documentation]() est sans aucun doute un modèle de référence dans l'univers unix. Dont acte.

Première étape, récupérer les sources du projet ALARMPI (_Arch Linux ARM Raspberry Pi_, j'ai mis du temps à saisir l'accronyme) :

    curl -O http://archlinuxarm.org/os/ArchLinuxARM-rpi-latest.zip
    curl -O http://archlinuxarm.org/os/ArchLinuxARM-rpi-latest.zip.md5
    unzip ./ArchLinuxARM-rpi-latest.zip

Vérifiez le checksum MD5 au passage, puis brûlez l'image sur la carte SD du RPi. Je suis sous Mac OS, donc une ou deux subtilités avec `dd`, mais globalement vous vous y retrouverez sous n'importe quel Unix :

    diskutils list
    # Idenitifiez votre carte SD (probablement /dev/disk2) et démontez-là
    diskutils unmount /dev/disk2s1

    sudo dd bs=1024 if=./ArchLinuxARM-rpi-latest/ArchLinuxARM-*.img of=/dev/rdisk2

Insérez ensuite la carte SD dans votre RPi, démarrez-le et connectez-vous en ssh (`root`/`root` par défaut) pour la suite.

### Configuration RPi / ArchLinux de base

Votre RPi a booté correctement ? Super. On continue donc ! Toutes les commandes suivantes sont éxecutées directement sur le RPi (via une connection SSH par exemple).

#### Redimensionnez votre partition principale

Il y a des chances que vous utilisiez une carte SD d'une capacité supérieure à 2Go (qui est la configuration _de base_ du projet ALARMPI), il va donc falloir redimensionner à chaud la partition principale pour utiliser tout l'espace disponible.

1. Affichez vos partitions actuelles. La partition `/` devrait faire 1.2G

        df -h

2. Ouvrez l'outil de partitionnement sur la carte SD

        fdisk /dev/mmcblk0

3. Supprimez la partition racine

        Command (m for help): p

        Disk /dev/mmcblk0: 7969 MB, 7969177600 bytes, 15564800 sectors
        Units = sectors of 1 * 512 = 512 bytes
        Sector size (logical/physical): 512 bytes / 512 bytes
        I/O size (minimum/optimal): 512 bytes / 512 bytes
        Disk label type: dos
        Disk identifier: 0x00057540

                Device Boot      Start         End      Blocks   Id  System
        /dev/mmcblk0p1            2048      186367       92160    c  W95 FAT32 (LBA)
        /dev/mmcblk0p2          186368     3667967     1740800    5  Extended
        /dev/mmcblk0p5          188416     3667967     1739776   83  Linux

        Command (m for help): d
        Partition number (1,2,5, default 5): 2
        Partition 2 is deleted

4. Créez une nouvelle partition étendue utilisant toute l'espace disponible (validez les valeurs par défaut)

        Command (m for help): n
        Partition type:
           p   primary (1 primary, 0 extended, 3 free)
           e   extended
        Select (default p): e
        Partition number (2-4, default 2): 2
        First sector (186368-15564799, default 186368):
        Using default value 186368
        Last sector, +sectors or +size{K,M,G} (186368-15564799, default 15564799):
        Using default value 15564799
        Partition 2 of type Extended and of size 7.3 GiB is set

        Command (m for help): n
        Partition type:
           p   primary (1 primary, 1 extended, 2 free)
           l   logical (numbered from 5)
        Select (default p): l
        Adding logical partition 5
        First sector (188416-15564799, default 188416):
        Using default value 188416
        Last sector, +sectors or +size{K,M,G} (188416-15564799, default 15564799):
        Using default value 15564799
        Partition 5 of type Linux and of size 7.3 GiB is set

5. Inscrivez la table de partitionnement

        Command (m for help): w
        The partition table has been altered!

        Calling ioctl() to re-read partition table.

        WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
        The kernel still uses the old table. The new table will be used at
        the next reboot or after you run partprobe(8) or kpartx(8)
        Syncing disks.

6. Rebootez

        reboot

7. Redimensionnez la partition à chaud

        resize2fs /dev/mmcblk0p5

Et voilà ! Votre installation occupe désormais l'intégralité de votre carte SD, on peut passer à la configuration.

#### Utilisateurs

Changez le mot de passe `root` par défaut :

    passwd

et ajoutez un compte utilisateur non-root :

    useradd -m -G users,wheel -s /bin/bash casper
    passwd casper

Pensez également à transférer votre clé SSH publique vers ce nouvel utilisateur et à désactiver le compte root en SSH (`PermitRootLogin no` dans le fichier `/etc/ssh/sshd_config`).


#### Update & Packages

Commencez par mettre à jour votre ArchLinux et installez `packer` pour la gestion des paquets sur les dépôts standards et sur _aur_ :

    pacman -Syu
    pacman -S haveged
    haveged -w 1024
    pacman-key --init
    pkill haveged
    pacman -Rs haveged

    pacman -S base-devel binutils

    wget https://aur.archlinux.org/packages/pa/packer/packer.tar.gz
    tar xvzf packer.tar.gz
    cd packer
    makepkg -s --asroot -i

#### Config système

Configurez les locales, localtime…

    # Fichier /etc/vconsole.conf
    KEYMAP=en
    FONT=lat9w-16
    FONT_MAP=8859-1_to_uni

    # Fichier /etc/locale.gen
    en_US.UTF-8 UTF-8
    fr_FR.UTF-8 UTF-8

    # Générez les locales
    locale-gen
    localectl set-locale LANG="fr_FR.UTF-8"

    # Configurez localtime
    timedatectl set-timezone Europe/Paris

    # Configurez le hostname
    hostnamectl set-hostname mymachinename

#### Pensez à faire un backup

Pour le backup, rien de plus simple avec DD : il suffit de dumper une image disque prête à être brûlée à nouveau sur la carte SD, c'est aussi simple que ça. Pensez juste à enregistrer l'image sur un support externe (disque, NFS…) pour ne pas remplir la partition principale (DD enregistrant aussi les secteurs zeros) :

    dd bs=1M if=/dev/mmcblk0 of=/mnt/backup/mymachinename.backup-$(date +"%y%m%d-%H%M").img


## Torification

Vous voilà avec un système tout propre tout beau et prêt à prendre du service. Étape suivante, installer et configurer le proxy. Mon choix s'est orienté sur Tor pour une raison simple : je recherchais un proxy _Socks_ parce qu'il s'agit d'un protocle supporté _a minima_ par tous les navigateurs, voire utilisable directement au niveau sysème. Les quelques solutions qu'on peut trouver ici ou là ont l'air soit veillissantes, soit peu ou mal maintenues.

### Tor : c'est quoi ?

Pour rappel (et pour faire simple), Tor est un _subnet_ (un réseau dans le réseau) capable de faire transiter des flux TCP chiffrés. Le principe peut être perçu de la façon suivante : vous demandez à consulter une ressource. Votre navigateur va contacter un proxy Tor local. Celui-ci va chiffrer votre requête, et l'envoyer vers un premier point de routage ; qui va à son tour la chiffrer (différemment) et ainsi de suite sur plusieurs rebonds. Tant que vous naviguez entre les relais Tor, vous êtes dans son _subnet_ (certains sites ne sont d'ailleurs accessibles que depuis Tor). Finalement, si la ressource demandée ne fait pas partie du réseau Tor, vous allez sortir par un point de sortie du réseau qui va aller se connecter lui-même à la ressource demandée. Puis la réponse fera le chemin en sens inverse, en empruntant d'autres relais.

Ce dispositif implique plusieurs choses en terme de sécurité :

- vos échanges sont chiffrés, et ce différemment entre chaque point du réseau Tor : il est impossible à un attaquant de lire votre requête, même s'il l'intercepte en plusieurs endroits ;
- un attaquant n'a aucun moyen de savoir d'où vient la requête et où elle va ;
- vos requêtes sortent systématiquement à différents endroits du réseau (des adresses IP différentes) ce qui vous rend intraçable : vous n'êtes jamais au même endroit ;
- vos requêtes ne passent jamais par les mêmes trajets, ce qui implique qu'on ne peut se mettre en embuscade pour vous attraper.

Ces points (et bien d'autres) font de Tor un excellent dispositif d'anonymisation sur le net (si on prend soin de cacher ses requêtes DNS à l'intérieur) qui est particulièrement utile aux journalises, hacktivistes, etc pour contourner les pressions gouvernementales. C'est un très bon outil.

Pourquoi l'utiliser ici ? Pour une raison simple : quand vous utilisez Tor, vous installez normalement en local (sur votre machine) un serveur qui va d'une part se connecter au subnet pour faire transiter vos requêtes ; et d'autre part va offrir à votre machine un serveur Socks local sur lequel connecter vos applications.

Nous y voilà !

L'idée est ici non-plus de faire tourner votre proxy socks en local mais sur votre RPi distant, et de vous y connecter depuis n'importe quelle machine. Vous aurez à la fois un proxy Socks de qualité et un accès au subnet Tor.

### Installation et configuration

Là encore, rien de plus simple (c'est l'avantage avec un outil universellement reconnu) :

    packer -S tor

Pour la confugration, il vous suffit d'activer les options suivantes dans votre fichier `/etc/tor/torrc` :


J'ai chosi de laisser le serveur Socks écouter sur le port 443 (9050 par défaut) car il s'agit du port normalement utilisé par HTTPS. De fait, ce port n'est jamais bloqué par les PALC (ce qui reviendrait à couper tout accès au web) et l'idée est quand même d'utiliser ce relais quand on ne peut pas passer par une connection directe (coucou les sysadmins des clients casse-pieds :)).

Le port 443 étant réservé à l'usage exclusif de `root`, il faut modifier le script de `systemd` pour assurer le lancement de Tor avec le bon couple utilisateur / groupe (`tor`/`tor` par défaut sur ArchLinux, crée automatiquement à l'install). Modifiez donc le fichier `/etc/___` :



Voilà ! Plus qu'à enregistrer et éxecuter le service :

    systemctl enable tor.service
    systemctl start tor.service

### Connecter votre système

Maintenant que le proxy est disponible, il vous suffit de configurer vos applications pour l'utiliser. Deux solutions s'offrent à vous :

**Vous ne souhaitez passer par le proxy que pour votre consultation web** Le menu de réglages de réseau de votre navigateur est fait pour vous. Sous Firefox par exemple, il vous suffit de renseigner le champ `proxy socks` avec l'adresse IP de votre RPi et le port que vous avez choisi :

[INSERT FF NETWORK]

**Vous voulez faire transiter tous les flux réseaux** de votre machine via le proxy. Il faut alors vous tourner vers les réglages réseau de votre OS. Sous OS X, c'est dans la partie préférences réseau que ça se passe :

[INSERT OS X NETWORK PREFS]

Pour vérifier que tout ça fonctionne bien, faites un tour sur [la page de checks Tor](), un joli message devrait vous signaler que vous passez bien par le proxy Tor pour vos connections :] !


### Superviser l'ensemble

Le fait d'ouvrir le proxy sur l'extérieur et de ne plus le limiter à la boucle locale implique que cet accès (non-filtré, j'y reviens après) peut être facilement exploité depuis l'extérieur.

Pour garder un œil sur son foncctionnement et être capable de visualiser rapidement la consommation de données transittant sur le proxy, j'utilise _XXX_ (nom de code _ARM_), un outil de visualisation de l'activité du proxy Tor [développé dans le cadre du projet]().

Pour l'installer, comme toujours, c'est simple comme une ligne de commande :

    packer -S arm

Vous pourrez alors lancer l'utilitaire avec la commande `arm` et obtenir la vue suivante :

[INSERT IMAGE]


### Limiter l'accès

Pour le moment, j'avoue, j'ai laissé le serveur Socks de Tor ouvert librement, sans filtrage.

Une première solution consisterait à réaliser un filtrage sur IP en utilisant la clé de configuration `` dans le fichier `torrc`. Mais cette solution imposera de connaître à l'avance les IP autorisées à se connecter au système, ce qui n'est pas forcément possible (quand j'arrive chez un client, je n'ai pas toujours son IP à l'avance — c'est même assez rare). Cette solution impose donc des contorsions pour accéder au serveur en SSH, ajouter l'IP, puis se connecter au proxy.

La seconde solution est un filtrage par username/password. Depuis la version _2.3_ de Tor, le serveur Socks autorise une authentification par identifiants et s'en sert au cloisonnement des sessions utilisateur. Je n'ai pas encore mis en place cette solution (qui me semble la plus efficace) car malgré de longues fouilles dans les entrailles du Trac du projet, je n'ai pas encore mis la main sur les options de configuration concernées. _Coming Soon_ donc…

