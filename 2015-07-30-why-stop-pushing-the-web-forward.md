---
layout: post
title: "Why Stop Pushing The Web Forward"
lang: fr
category: tech
tags:
- technology
- browsers
---

Je suis tombé aujourd'hui sur un article de PPK rédigé il y a deux jours, nommé [Stop pushing the web forward](http://www.quirksmode.org/blog/archives/2015/07/stop_pushing_th.html). J'avoue que je suis globalement plutôt d'accord avec son propos (j'y reviens après) et j'ai donc [diffusé à mon tour l'article sur Twitter](https://twitter.com/m4d_z/status/626727321219805184). Quelques copains ont réagi (à ma grande surprise, je n'étais probablement pas le premier à le _tweeter_), notamment [@dioxmat](https://twitter.com/dioxmat/status/626736386058657792) qui trouve l'article un poil condescendant envers les implémenteurs de nos butineurs. Twitter n'étant toujours pas le bon endroit pour une conversation sérieuse, voici ma réponse :).


# Pushing the web forward

Petit retour en arrière, pour ceux qui n'ont pas eu la chance de le vivre : à une lointaine époque vivait IE6[^ie6]. Avec lui s'achevait la _browsers war_ et s'engageait un temps sombre comparable à l'obscurantisme médiéval appliqué au web. Son déclin a engagé le web dans une nouvelle voie, qui a trouvé un nom depuis peu : [_Move the web forward_](http://movethewebforward.org/). La philosophie derrière tout ça est simple : faisons avancer le web aussi vite que possible pour le doter de nouvelles fonctionnalités qui permettront d'enrichir toujours plus l'écosystème. Charge aux devs web comme aux implémenteurs des navigateurs de s'entendre sur les priorités pour donner le sens du vent.

J'avoue que j'ai, comme tout le monde, été très sensible à ça. Après avoir passé des années à galérer avec des navigateurs peu respecteux des standards, pouvoir enfin produire un code non seulement de qualité, mais en plus doté de fonctionnalités qui évitaient pas mal de hacks[^hacks][^br], c'était Byzance ! Aujourd'hui, et depuis presque 2 ans, je reviens un peu sur cette première idée, et j'avais du mal à comprendre le malaise…


# Stop pushing the web forward

C'est ici qu'intervient l'article de PPK. Son point de vue est simple : dans la course aux nouvelles fonctionnalités, on s'est probablement perdus en route, et il est temps de serrer le frein à main avant qu'il ne soit trop tard. Il propose donc un moratoire d'un an sur ces nouvelles API pour qu'on puisse tous se donner le temps de réfléchir[^think]. Je résume et vous invite à lire l'article pour bien comprendre.

Son ton est comme toujours assez tranché[^ppk], et on pourrait croire qu'il accuse les implémenteurs des butineurs de se lancer tête baissée sans réflechir dans la bataille à la fonctionnalité. L'autre aspect de son article, c'est l'accent sur le moratoire d'un an, et c'est clairement sur ce sujet que (comme me le fait justement remarquer [@jailvestre](https://twitter.com/jailvestre)), l'article se prend une volée de bois vert.

Proposer (voire imposer) un moratoire sur le développement des nouvelles fonctionnalités, c'est comme signifier qu'on va passer en jugement tout le travail réalisé précédemment sur les implémentations, qui sont quand même réfléchies. Ça a un petit goût de Sainte Inquisition, et je comprends que ça hérisse le poil de mal de gens, développeurs de navigateurs en première ligne.

Pourtant, sur le fond, il a raison et je le rejoins.


# Le syndrome _James Dean_

Il y a presque 2 ans, en discutant avec [@jeremiepat](https://twitter.com/jeremiepat), on faisait l'amer constat que dans la jungle foisonnante des nouvelles API, nous étions désormais contraints de devoir choisir sur lequelles nous allions pousser nos investigations. Après avoir pu, pendant plusieurs années, maîtriser la quasi-totalité de l'éco-système _frontend_, il devenait impossible de couvrir le champ des possibles. La corollaire, c'est que cette richesse entraîne la spécialisation. D'un domaine composés de gens aux niveaux de compétences variées, nous migrions vers un secteur composés de généralistes et de spécialistes.

Soyons bien d'accord que c'est là une évolution naturelle, et je n'y vois pas malice. Nous connaissons ça côté _backend_, sans en souffrir. Au détail près peut-être que le _backend_, c'est une affaire de spécialistes depuis le début, là où le _front_ passe par une phase de mutation.

Accélérer encore, c'est faire avancer cette mutation à marche forcée, là ou les processus naturels ont besoin de temps. À trop tirer sur la corde, on risque de se prendre l'élastique dans la gueule. À mon sens, les risques se situent à plusieurs niveaux :

## Toi, jeune padawan, tu veux faire du web ?

Le premier, le plus visible, et celui qui est rapidement mis en avant quand je parle de tout ça, est lié facteur humain : plus on accélère et on met de choses dans la balance, plus il devient difficile pour un _frontiste_ de maîtriser les différents aspects de son métier. Il faut soit rester généraliste et accepter de ne pas maîtriser tous les aspects liés au évolutions _front_ les plus récentes ; soit se spécialiser dans un secteur (offline-first, webgl, votre-techno-favorite-ici), et se mettre à distance du reste des technos disponibles.

Malheureusement, les domaines restent étroitement liés, et même en choisissant une spécialité, il va falloir réaliser une veille permanente sur ce qui gravite autour et pour comprendre les nouvelles fonctionnalités (pas les **maîtriser**, hein, juste les **comprendre**) pour voir comment cela pourra influencer notre propre spécialité. C'est un travail titanesque de rester en veille, de tester, lire et relire, comparer… J'aime ça, mais ce n'est pas à la portée de tout le monde. On va laisser des gens sur le carreau[^road]. Si certains pensent que "tant pis pour eux, il n'ont qu'à suivre, ça fera de la place pour ceux qui suivent", moi, ça m'emmerde. Parce que ces gens qu'on va laisser derrière, ils ne vont pas quitter le web. Ils vont continuer à produire des sites. Des sites mal ficelés, de mauvaise qualité, par manque de connaissances. Et tout ça contribue à un mauvais état du web en général.

## C'est pas grave, y'a un _polyfill_

Secundo, l'outillage. C'est un point que PPK met particulièrement en avant dans son article, c'est même son angle d'attaque principal. Le problème des fonctionnalités récentes, c'est qu'elles ne sont pas disponibles dans les navigateurs anciens qui parcourent encore la toile, et bien souvent pas encore dans tous les navigateurs récents. Je lisais pas plus tard que ce matin un article mettant en avant une techno uniquement dispo sur safari 9, ce qui peut sembler insignifiant mais comprend quand même des millions d'iPhones et d'iPad à compter de septembre. La bonne pratique voudra qu'on dégrade élégamment l'usage de cette fonctionnalité, mais la majorité des devs qui se rueront dessus feront du _shit-programming_ à base de _webkit-only_ et _go fuck_… Personnellement, ça me lève le cœur.

Pour la plupart des nouvelles fonctionnalités, on verra fleurir quantité de _polyfills_ : des hacks encapsulés dans de micro-libs _js_ ou _CSS_ qui viendront émuler, ou implémenter (avec plus ou moins de succès) la _spec_ dans les navigateurs ne la supportant pas nativement. Là encore, ça m'inquiète, parce qu'on est en train de bâtir un web à bases de béquilles. Leur usage a un coût, que nous, _web developers_, faisons directement porter sur nos utilisateurs finaux : les visiteurs de nos sites.

Notre pile technique, et avec elle sa maintenance et son évolution, devient trop complexe, et trop lourde. Comment justifier les dizaines ou centaines de _kilo-octets_ que nous imposons à nos visiteurs uniquement pour supporter une fonctionnalité qu'on tenait absolument à avoir dans les mains ?

## Feature-fun isn't funny

Cette réflexion me conduit au troisième aspect : pourquoi utiliser une fonctionnalité récente ? Un outil reste un outil, seul l'usage que l'on en fait le rend indispensable. Evidemment, _quand on n'a qu'un marteau, tout ressemble à un clou_, mais multiplier les alternatives dans l'autre-sens n'est pas une meilleure solution. J'ai vu trop de projets utiliser des bibliothèques toutes neuves à peine disponibles[^justreleased] juste parce qu'elles semblaient sexy à utiliser.

Evidemment, je comprends le goût et l'attrait de la nouveauté qui motive de nombreux développeurs, moi le premier. Mais sachons les utiliser avec discernement. Utiliser une fonctionnalité, alors qu'elle est à peine disponible, en patchant le navigateur à grands coups de polyfills, ou en faisant du _webkit-only_, c'est creuser le lit d’une nouvelle _browsers war_. C'est pourtant pas faute de prévenir : la guerre autour des préfixes CSS qui fait rage depuis plusieurs mois est la bataille d'introduction d'un nouveau conflit de grande envergure.

## Retro-compatibilité n'est pas jouer

Un des points qui m'inquiéte le plus, c'est l'aspect rétro-compatibilité, qui a une consonnance unique dans notre écosystème web. En mode de développement _long-run_, il y a (normalement) des phases de stabilisations du produit. Elles vont servir à faire le point sur le produit, à les évaluer, les renforcer et à supprimer celles qui ne sont plus nécessaires, ou qui n'ont aucune valeur ajoutée réelle. Cette phase d'élagage du code est nécessaire à son bon fonctionnement. Comme dans un jardin, elle va permettre de supprimer tout ce qui n'est pas indispensable pour laisser le reste prendre de la place.

Le web ne se permet pas ce darwinisme forcé : tout ce qui a été disponible le reste. C'est ce qui me permet d'affirmer que le code de mon premier site écrit il y a presque 20 ans fonctionne toujours. C'est un véritable aspect positif, si on sait en tirer parti. Mais si on n'y prend pas garde, la métamorphose risque d'être trop rapide et trop radicale : toutes les nouvelles API développées ces dernières années sont là, et elles le resteront. Elles le resteront parce que **nous** les avons utilisées, et à ce titre, l'axiome premier du web s'applique. Nous risquons à chaque ajout de transformer un peu plus le monstre.

## Web-natif ou nativement-web ?

Dernier point[^last] : l'orientation que nous souhaitons donner au web est primordiale. Une très grande partie des nouvelles API est développée dans le but de donner l'illusion d'une app native dans un environnement web. PPK donne en exemple _Navigation Transitions_ pour illustrer le phénomène.

Je défends depuis plusieurs années le concept de _browser-as-a-platform_, et c'est clairement dans cette direction que nous devons aller pour continuer de pousser le web dans la bonne direction. Mais le web a une identité propre. Il est regrettable à mon sens de pousser à l'adoption de fonctionnalités qui ne sont là que pour mimer les comportements des autres plateformes (des autres OS) plutôt que d'imposer et de cultiver notre différence. Nous souhaitons faire des _webapps_ ? Super ! Acceptons donc que le mot clé soit **web** et pas _apps_, et avançons dans ce sens. Le reste n'est que complexe d'infériorité ridicule.


# Alors, moratoire ou pas ?

Honnêtement, je n'en sais rien. Quand je relis mes propos précédents, je me dis que nous sommes peut-être déjà allés trop loin. Ce dont je suis sûr, c'est qu'_il est urgent de ralentir_ si on ne veut pas perdre le contrôle et creuser notre propre tombe. Arrêter la course à l'innovation et prendre le temps de penser ensemble à ce que nous voulons voir devenir le web n'est peut-être pas une idée si dramatique.


# J'accuse

Quoi qu'il en soit, _j'accuse_ !

J'accuse les implémenteurs d'ajouter à tour de bras des fonctionnalités dans les navigateurs, parfois trop vite, parfois imcomplétement, parfois avec trop peu de discernement, en cédant à la pression imposée par les développeurs web gourmands de nouveauté.

J'accuse les développeurs web de se jeter sur les nouvelles API, alors qu'elles ne sont pas encore stabilisée pour composer des produits de production de masse sur lesquels ils n'effectueront pas de maintenance à long terme. Je les accuse aussi de refuser depuis des années de réaliser un web accessible et performant gracieusement dégradé sous prétexte que c'est plus complexe à faire et que cela demande du temps et de l'expertise. Je les accuse enfin de céder à leurs client (pas les visiteurs, mais leurs commanditaires), en implémentant _polyfills_ et _libs_ uniquement pour simuler des apps natives au lieu de cultiver la différence du web.

J'accuse enfin les clients commanditaires de sites et d'apps web de refuser d'entendre toute l'expertise que nous avons acquise au fil du temps ; de refuser nos arguments quand nous défendons un web propre, standard et respecteux de ses utilisateurs ; pour préférer un web qui ne ressemble pas à un web pour _faire comme de vrai sur mon iPhone à $900_.

Lisez cette liste à l'envers maintenant, vous comprendrez.

Pour que le web reste cet endroit unique que nous avons bâti, il nous faut continuer à le défendre dans sa singularité. Pour qu'il puisse vivre et grandir encore, il nous faut en prendre soin. Et comme pour nos ressources naturelles, il devient urgent d'être, tous, écologiquement responsables de notre écosystème numérique.

Le web devient grand, il est toujours plus beau, ne le gâchons pas.


_Note: j'expérimente une façon différente de gérer les commentaires sur les billets. Si vous souhaitez participer à la discussion, je vous invite à commenter le [commit du billet dans le dépôt GitHub](https://github.com/m4dz/m4dzblog/commit/15187960b5881671b1d9bbd0d6d909de537c6b6f)._


[^twitlate]: que je lis toujours avec 2 jours de retards, on ne se refait pas…

[^ie6]: notez la façon dont un bon billet technoweb se doit de comporter une référence à IE6

[^hacks]: notez bien ça, j'y reviens ensuite, vous allez comprendre pourquoi c'est un problème

[^br]: qui a dit `border-radius` ?

[^think]: si possible en buvant des bières, ou tout autre breuvage de votre choix, entre copains, c'est toujours mieux

[^ppk]: il ne serait pas une des rock-stars du web sinon, de ceux qu'on écoute quand ils montent à la tribune

[^road]: même moi il m'arrive d'en avoir ras-le-bol de cette sensation de ne pas réussir à me maintenir à jour, la tête au moins hors de l'eau

[^justreleased]: et je ne parle même pas des frameworks JS tous neufs / trop fun / mainstream

[^last]: après je conclus, promis
