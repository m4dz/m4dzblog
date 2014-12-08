---
layout: post
title: Peinture en cours
lang: fr
category: blog
tags:
- tools
---

Bon, c'était prévisible, je n'ai pas tenu : j'ai décidé de refaire (déjà ?) la peinture ici :). Donc refonte en cours, si j'arrive à tenir le rythme que je me suis imposé, ça devrait venir rapidement… Concrètement, le wireframe est ok, le style aussi, j'ai commencé l'intégration. Donc on devrait pouvoir tenir le choc.

Au passage, j'en profiterai certainement pour passer d'Octopress à Jekyll. Petite explication…

<-- more -->

## Mes besoins de publication ##

Ce blog me sert de carnet perso, je l'utilise donc aussi pour expérimenter de nouvelles choses. Pour ça, mes pré-requis (en l'état actuel des choses) sont les suivants :

### Publication statique ###

Je cherche à générer un site statique. C'est une tendance que je défends de plus en plus, loin de nos CMS/CMF modernes sur lesquels nous appuyons la majorité de nos projets, perso et client, petits et grands.

Je trouve que nous avons tendance à les sur-utiliser, et à négliger les autres pistes de publication que nous avons à notre disposition. Je milite de plus en plus pour une contribution _à la roots_ (comprendre, à la main, dans des fichiers markdown), ou un peu plus évoluée en passant par des interfaces de contribution (je pense que je ferais un petit billet pour exprimer mon point de vue là-dessus assez rapidement). Derrière ces contributions simples, un moteur de génération statique et hop, pas besoin de plus.

C'est un point de vue qui se défend pour un blog — perso. C'est également facilement valable sur des mini-sites pour nos clients, lorsque nos besoins sont minces en terme de contribution, comme lorsqu'il nous arrive de concevoir des mini-sites vitrine _one-shot_.

### Socle Ruby ###

J'ai également besoin d'appuyer ce blog sur un socle _Ruby_. Pourquoi Ruby ? Parce que je fais du PHP depuis près de 15 ans, et que je continue d'en faire dans mon quotidien professionnel. Même si c'est un langage que j'affectionne (on ne renie pas ses premiers amours), j'ai aussi envie, et besoin, de travailler d'autres choses.

J'ai mis les mains dans Ruby il y a 7 ou 8 ans. A quelques projets près, j'ai eu peu d'occasions de le mettre en pratique professionnellement, et je pense que la situation perdurera longtemps encore ッ. Donc je profite de mon blog perso pour faire _comme j'ai envie_…

Donc Ruby : +1 !

### Gestion des commentaires ###

Là, le bât-blesse : le problème de la publication statique, c'est que par essence, elle est statique. Les contributions utilisateur deviennent donc compliquées.

J'ai pas mal d'idées en têtes et de petites choses dans les cartons. On en parle beaucoup [ici]() ou [là]() avec les copains sur twitter.

On en parlera aussi beaucoup avec [Karl](http://www.la-grange.net/) lors de son [élaboratoire](http://sudweb.fr/2013/#elaboratoires) prochain à SudWeb (venez nombreux, y'a de grandes plaines sauvages à conquérir).

En attendant, je laisse en friche, et je vous propose de réagir notamment sur des outils comme [branch.com](http://branch.com/), la solution disqus ne me convenant décidément pas…

## Octopress : état des lieux ##

J'ai donc choisi de partir sur [Octopress](http://octopress.org) sur la première mouture de ce blog. Il offre un nombre d'avantages certains :

- Il s'appuie sur [Jekyll](http://jekyllrb.com/) pour la génération des pages. C'est un moteur de génération statique, écrit en Ruby (yeah), utilisant la syntaxe de templates [Liquid](http://liquidmarkup.org/), particulièrement efficace (en tous cas, moi j'aime bien). C'est aussi Jekyll lui propulse les [pages Github](http://pages.github.com/) ;
- Il arrive pré-configuré pour l'instance de [Jekyll](http://jekyllrb.com/) : c'est un peu l'objectif, Jekyll nu, il faut le configurer, et tout créer. Octopress fourni une config de base sur laquelle démarrer promptement ;
- Il propose des thèmes, ce qui facile (normalement) l'intégration de vos propres créations.

Le problème à mon sens avec Octopress, c'est que le projet prend son temps, et certaines fonctionnalités (comme le [linklog](http://octopress.org/docs/blogging/linklog/) par exemple) se font largement attendre.

L'autre point de désaccord que je rencontre, c'est que comme tout système global, il embarque tout un tas de plugins par défaut. C'est certes pratique, mais ce n'est pas ma philosophie. Je cherche à mettre les mains dans le cambouis, parce que finalement, c'est bien là mon objectif : essayer, apprendre, hacker. Et là, il y a trop de choses toutes faites…

Enfin, j'ai un léger souci avec l'écosystème de thèmes gravitant autour du projet. Ils sont finalement extrêmement semblables, sur la même base, et tout ça nuit à la diversité. Quitte à monter un thème de zéro, autant tout faire de zéro.

Mais ne me faites pas dire ce que je ne pense pas : le projet me semble très intéressant, et il a sa place dès les que vous avez besoin d'une solution de blogging utilisable _out-of-the-box_ et embarquant une grande quantités de plugins. Je cherche juste plus de souplesse.

## Mon choix technique ##

Finalement, je tourne mon choix vers une solution Jekyll brute. En même temps que la peinture fraîche, ça me donnera l'occasion de poser les bases techniques telles que je l'entends. Ça sera donc :

- Propulsion Jekyll ;
- Plugins custom pour remonter les infos externes (tweets, repos github…) ;
- Thème entièrement réalisé par les mimines de votre serviteur ;
- Et sûrement plein de petites pépites pour aller avec :)

Comming soon donc, comme disent les bretons.

## Nota bene ##

Même si j'aimerai écrire plus ici, ce n'est pas mon seul lieu d'expression (et j'ai comme dans l'idée qu'il va y avoir plein d'autres canaux de communication dans les semaines et mois à venir). J'écris notamment pour le blog technique de mon GE (gentil employeur) [Clever Age](http://www.clever-age.com). Je mettrais donc les articles que je publie là-bas en lien dans ce carnet numérique, histoire de ne pas les perdre en route…
