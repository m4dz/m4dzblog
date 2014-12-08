---
layout: post
title: "Proxy Socks : Gagnez la Liberté"
lang: fr
category: tech
tags:
- sysadmin
- tools
- linux
---

Je suis, comme beaucoup, abonné chez Free depuis de nombreuses années. Je suis, comme certains, victime de ce bridage éhonté du réseau appliqué au streaming sur de plus en plus de plateformes (Youtube en tête, mais Dailymotion morfle aussi, sans compter Facebook, l'AppStore, etc). T'as qu'à changer d'opérateur ! Oui, certes, j'avoue… Mais de mon côté, pas facile de trouver un opérateur digne de confiance sans aller s'engager dans une lutte complexe. En attendant, la situation stagnante nous faisant invariablement reculer[^dupneu], il a bien fallu trouver une solution pour pallier aux difficultés d'accès. Petite proposition…

<-- more -->

J'ai monté cette petite infra en deux heures il y a quelques semaines, et quelle coïncidence tout de même, [@mauriz] lance le [débat quelques jours plus tard] pour finalement [nous offrir ce petit résumé]. Aujourd'hui, c'est [@mariejulien] qui [remet le couvert]. Je vous propose donc ma petite solution maison (celle retenue par [@mauriz] au passage), parce que je suis comme ça moi, je suis un mec sympa :)…

## Proxy Socks, pourquoi ?

La solution se nomme proxy SOCKS. Comme je suis une super-feignasse, je [cite directement Wikipédia] :

> SOCKS est un protocole réseau qui permet à des applications client-serveur d'employer d'une manière transparente les services d'un pare-feu. SOCKS est l'abréviation du terme anglophone « sockets » et « Secured Over Credential-based Kerberos ».

Grosso-modo (pour ceux du fond) le proxy SOCKS va agir comme une passerelle : vous allez balancer là-dedans tout votre flux réseau, et celui-ci va passer au travers de votre machine distante pour ressortir de l'autre côté. Si vous avez bien choisi la machine qui va faire office de proxy, vous allez bénéficier de la qualité de son infrastructure. Et Free, qui est bien gentil, mais un peu con quand même, ne fera que faire transiter vos données entre votre machine et votre proxy distant, sans voir ce qui circule dessus… Malin malin[^malin] !

## Let's go, Marco !

Mon choix s'est tourné vers OVH pour héberger mon proxy SOCKS pour une raison simple : ils proposent une offre de [VPS] d'entrée de gamme franchement pas chère[^cheap]. Certes, la machine virtuelle fournie n'est pas une bête de course, mais on va juste lui demander de relayer du flux, pas de raison de se ruiner dans le service non-plus.

J'ai donc commandé un joli petit VPS Classic livré quelques heures plus tard, sur lequel j'ai déployé une Debian 6x "Wheezy" 64bits. Pour l'installation, rien de plus facile…

Tout d'abord, petite mise à jour :

	# aptitude update && aptitude upgrade

Ensuite, installation de ’[danted] qui est donc la solution SOCKS retenue (parce que c'est la plus courante et celle sur laquelle on trouve le plus de doc) :

	# aptitude install danted

## Configuration, aux petits oignons

L'avantage de gérer sa propre solution (c'est ce qui me plait dans l'histoire), c'est la flexibilité qu'on peut avoir dans le choix de ses outils. Je suis, notamment, régulièrement coincé derrière un proxy filtrant sur lequel je n'ai pas la main, et qui s'avère être particulièrement restrictif[^security]. J'ai donc choisi de faire tourner mon proxy SOCKS sur le port 443 (qui abrite normalement HTTPS) pour être sûr de pouvoir l'atteindre quelque soit mon emplacement[^meriletfou]. De toutes façons, rien d'autre ne tournera sur cette machine, donc je fais comme je veux. Et comme je suis un mec sympa, mais que je ne compte pas ouvrir mon relais réseau à la terre entière non-plus, je filtre sur un pool d'adresses IP définies celles autorisées à se connecter au proxy :).

Vu que j'ai un poil ramé à trouver la config qui va bien, je la colle ci-après dans un petit Gist pour vous laisser tout le plaisir de la réutiliser comme bon vous semble :

{% gist 5564720 %}

## Next

Une fois en place, il vous suffit de définir votre proxy socks, soit au niveau système (pour passer tout votre trafic depuis les applis supportant le protocole SOCKS), soit au niveau de votre navigateur (dans les réglages de connexion réseau, type de proxy `SOCKS 5`). Renseignez l'IP de votre serveur proxy et le port choisi pour recevoir les connexions. Dans mon cas, je n'ai pas activé d'authentification vu que le filtrage se fait sur IP.

Pour être sûr, allez faire un tour sur [canihazip] : l'IP affichée doit être celle de votre machine relais.

Bien évidemment, comme vous êtes des gens prudents[^prudence], vous penserez bien à sécuriser votre serveur en réalisant des opérations de base :

- changer le port SSH et interdire la connexion `root` ;
- mettre en place `iptables` pour sécuriser vos connexions derrière le firewall ;
- installer des alertes mails sur les connexions `root` pour détecter les intrusions ;
- régulièrement faire passer `rkhunter` ;
- et plein d'autres petites choses[^guidesecu].

## Verdict

Je vous laisse juge, mais j'ai eu droit à un bel effet _wahouuuu_ lorsque j'ai montré à la maison qu'une vidéo Youtube, ça existait aussi en 720p, et sans accroc ! Free ne pouvant pas brider son débit sur la connexion vers votre proxy vu qu'il ignore ce qui transite dans le flux (et que vous n'êtes pas en connexion directe vers Youtube), vous bénéficiez de la qualité des infrastructures d'OVH (100 Mbits)… **WIN \o/** !

Ça n'est pas grand-chose, ça ne coûte pas très cher, et en attendant mieux, ça permet de contourner le problème :).


[^dupneu]: Car "qui n'avance pas recule, comme dit monsieur Dupneu…"
[^malin]: Et gourmand, croquant… y'a de l'audace dans cette recette !
[^cheap]: 4,99 euros H.T. à l'heure où j'écris ces lignes.
[^security]: En gros, je peux taper en HTTP/HTTPS (ports 80 et 443) et en IMAP. Ne comptez pas faire du SMTP ou toute autre folie de ce genre, non, non, non…
[^meriletfou]: Celui qui me bloquera le port 443 en entreprise est un dangereux cinglé et ferait mieux d'aller vendre des pulls sur le marché.
[^prudence]: Je n'en doute pas une seconde…
[^guidesecu]: J'avais trouvé un excellent pas à pas de [sécurisation basique de serveur sur Alsacréations]


[@mauriz]: http://svay.com/
[débat quelques jours plus tard]: https://twitter.com/mauriz/status/322440068205772801
[nous offrir ce petit résumé]:http://svay.com/blog/quelles-solutions-pour-regarder-des-videos-youtube-sur-une-connexion-free-fr/
[@mariejulien]: http://www.mariejulien.com/
[remet le couvert]: https://twitter.com/mariejulien/status/333518024734822400
[cite directement Wikipédia]: http://fr.wikipedia.org/wiki/SOCKS
[VPS]: http://www.ovh.com/fr/vps/
[canihazip]: http://canihazip.com/
[sécurisation basique de serveur sur Alsacréations]: http://www.alsacreations.com/tuto/lire/622-Securite-firewall-iptables.html
[danted]: http://www.inet.no/dante/
