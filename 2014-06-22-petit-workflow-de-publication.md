---
layout: post
title: "Petit workflow de publication superflu à l'usage de l'élite et des bien nantis"
date: 2014-06-22
category: devops
tags: git, travis, ci, gh-pages, quality
lang: fr
---

_J'ose espérer que Desproges me pardonnera cet odieu repompage, mais que voulez-vous, on ne se refait pas…_

Il y a peu, [@nhoizey](https://twitter.com/nhoizey) se posait sur twitter [l'intérêt de versionner le répertoire de _build_](https://twitter.com/nhoizey/statuses/480039702285008896) dans nos projets, quand celui-ci ne contient finalement que la version distribuable de nos applications, sans aucune valeur ajoutée.

[Ma réponse](https://twitter.com/m4d_z/statuses/480056775551758336) a tenu en un tweet : pour publier sur une autre branche avec _subtree_. Là, visiblement, j'en ai perdu plusieurs sur ce coup-ci. C'est donc l'occasion de vous expliquer rapidement ce qui constitue, à mon avis, un workflow de publication propre, simple et efficace[^desproges].

## C'est quoi, un workflow ?

C'est peut-être bien par là qu'il faut commencer. Un workflow, ce sera l'ensemble des régles et procédures qui vont diriger vos méthodes de travail. Celle-ci vont pouvoir être rejouées indéfiniment pour obtenir des résultats identiques. Vous allez normalement pouvoir les automatiser ; mais surtout, elles font partie intégrante de la brique de **confiance** de votre processus de travail. C'est grâce à ce workflow que vous aller pouvoir garantir une partie de la qualité de ce que vous produisez.

Dans notre cas, nous nous intéressons au développement d'une _webapp_, probablement de type SPA[^spa]. Le workflow associé devrait respecter les points suivants :

* offrir une branche _master_ propre, sinon stable, qui puisse servir de base à toute nouvelle branche, ou à tout nouveau _fork_ (qui n'est jamais qu'une branche, en soi)
* respecter une procédure de [taggage sémantique des versions](http://semver.org/lang/fr/)
* ne pas parasiter les branches de code inutile, comme les résultats de pré-compilation ou les sources distribuables
* utiliser une procédure de déploiement continu

Ces quelques pré-requis simples vont nous servir de base pour assurer la solidité de notre processus.

## Structure

Pour que nous soyons tous raccord[^plombier], voilà la structure de base sur laquelle vont s'appuyer nos sources :

```
my_project
  |- .tmp/
  |- dist/
  |
  |- src/
  |    |- assets
  |    |- templates
  |    `- index.html
  |
  |- tests/
  |    ` index.html
  |
  |- .gitignore
  |- .travis.yml
  `- README.md
```

J'ai fait simple en ne mettant que les éléments indispensables, votre projet sera certainement plus complexe. Globalement :

* la racine contient les fichiers de réglages, configurations et description de votre projet ;
* les sources sont isolées dans un répertoire `src` ;
* les sources distribuables (celles issues de la compilation) sont dans le répertoire `dist`[^fiction].

## Développement

Bon, nous avons notre structure, développons ! Il va falloir respecter quelques bonnes pratiques en terme de nommage de branches :

* master : c'est la version de base de chacune des branches de développement. Elle se doit d'être au moins propre, au mieux stable (nous nous plaçons dans un contexte _open-source_) ;
* feature-<name> : les fonctionnalités sont développées en sandbox dans des branches dédiées. Ce fonctionnement va vous permettre de travailler sur plusieurs fonctionnalitées simultanément sans vous parasiter ;
* release : votre branche de publication, j'y reviens après.

Donc, tout développement part d'une branche master propre. Celle-ci ne contient **jamais** le répertoire `dist`. Celui-ci n'est dédié qu'à la publication. Vous allez donc développer un nouvelle fonctionnalité (au hasard : `login`) en tirant une nouvelle branche depuis le master :

```shell
git checkout -b feature-login master
```

Lors de votre développement, vous allez devoir tester votre travail. Vous allez probablement utiliser un tasker (comme [grunt](http://gruntjs.com/) ou [gulp](http://gulpjs.com/) par exemple) chargé d'effectuer des tâches de `watch`, `livereload`, `test`… Ces tâches vont compiler et tester votre code vers un répertoire `.tmp` qui ne sera **pas** versionné (il est dans le `.gitignore` du projet). De cette façon, vous ne polluerez jamais vos branches de code jetable.

## Stabilisation

Féliciations ! Vous venez de terminer votre fonctionnalité. Il est temps de la reverser dans le master pour l'inclure dans le _base trunk_. Selon que vous souhaiterez conserver ou non l'historique de développement, vous opterez pour un `rebase` ou un `merge` (avec ou sans `ff`). Je vous conseille de votre reporter à l'excellent article de Christophe concernant ce choix décisif souvent mal compris sur le [choix entre `merge` et `rebase`](http://www.git-attitude.fr/2014/05/04/bien-utiliser-git-merge-et-rebase/).

A ce niveau, nous n'avons toujours que notre code source et nos fichiers de configuration qui sont versionnés, aucun code jetable paraiste n'est présent[^karma].

## Semver

Vous arrivez normalement au terme d'un cycle itératif : toutes les fonctionnalités que vous aviez prévu de déployer dans cette nouvelles version ont été développées chacunes dans leurs branches, et ces dernières ont été rassemblées dans le `master`. Vous noterez qu'à n'importe quel moment, vous avez pu repartir de `master` sans vous prendre les pieds dans le tapis, et n'importe qui a pu _forker_ votre projet sans avoir d'effet indésirable.

Votre code étant stabilisé (vous pouvez toujours lancer vos tests pour vous en assurer : ils sont effectuées dans le répertoire `.tmp` qui n'est pas versionné), il convient de le figer. C'est le rôle des tags. Je vous incite à respecter la convention [semver](http://semver.org/lang/fr/) qui vous garantira une homgénéité dans votre nommage.

```shell
git checkout master
git tag v0.1.0
```

Hop ! Vos versions sont isolées dans des tags, ce qui d'une part montre que votre projet est proprement mené, et d'autre part vous permettra de repatir de ce tag pour une nouvelle branche en cas de besoin (comme de réaliser des quick-fix, j'en parle ensuite).

## Livraison

Vous avez vore `master` propre et vos tags correctement versionés. Il est temps de réaliser la livraison associée. Pour ça, nous allons passer par une branche spécifique, nommée `release`. Dans cette branche, nous allons verser le tag de la version à livrer, puis exécuter une tâche de _build_. Cette tâche, contrairement à celles de dev (`watch`, `livreload`…) va produire du code compilé dans le répertoire `dist`.

Pourquoi ? Simplement parce que nous en aurons besoin pour le déploiement continu. Mais nous ne souhaitons pas publier du code parasite jetable et `dist`ne va contenir aucune valeur ajoutée : il est uniquement la résultante de la compilation de vos sources. Il s'agit là d'un compromis : nous en avons besoin pour la suite, mais nous ne souhaitons pas polluer nos branches avec. Nous l'isolons donc uniquement dans la branche de `release`. C'est pour cette raison que nous ne lançons la tâche de _build_ que **depuis cette branche**.

_ProTipTime_ : Si vous voulez être sûrs de vous, vous pouvez rajouter une sécurité à votre tâche en controllant la branche courante : si vous lancez un `build` depuis une autre branche que `release` la compilation ne s'éxécutera pas et votre tasker retournera une erreur.

## Déploiement continu

Votre branche `release` contient désormais votre résultat compilé (et commité) isolé dans le répertoire `dist`. C'est là qu'intervient ce _fameux trick_ soulevé par Nicolas (et qui vous a conduit à lire cette édifiante prose). Nous allons déployer le code contenu dans `dist` dans une autre branche.

Dans le cas de GitHub, vous allez pouvoir déployer vers la branche `gh-pages` qui déploiera automatiquement votre projet à l'adresse `http://<username>.github.io/<my_project>`. Pratique :).

Nous allons donc recourir à `subtree` tel que le décrit la documentation de [Yeoman](http://yeoman.io/) dans leur [procédure de déploiement](https://github.com/yeoman/yeoman/wiki/Deployment#git-subtree-command).

Il faudra, la première fois, associer la branche `gh-pages` au répertoire `dist` :

```shell
git add dist && git commit -m "Initial dist subtree commit"
git subtree push --prefix dist origin gh-pages
```

Par la suite, il suffira de _pusher_ vers `gh-pages` le contenu de `dist` :

```shell
git subtree push --prefix dist origin gh-pages
```

♪♫ TaDa!

Votre application compilée est donc désormais déployée depuis votre branche de `release` vers votre plateforme de publication, en passant par la branche `gh-pages`.

## En bref

Reprenons depuis le début :

1. Notre `master` est une source clean, sans le répertoire `dist`, où que nous
nous trouvions dans le temps
2. Nous dévelopons chaque _feature_ dans des branches isolées `feature-<name>` partant de `master`
3. Nous exécutons nos tests (et _cie_) dans chaque branche de dev depuis un répertoire `.tmp` non-versionné
4. Nous rassemblons les branches de dev dans le `master` une fois l'ensemble stabilisé
5. Nous créons un tag versionné depuis le master pour figer la version
6. Nous reversons ce tag dans la branche de `release` pour la livraison, depuis laquelle nous effectuons une tâche de `build` qui va peupler le répetoire `dist` et le versionner
7. Nous poussons le contenu de `dist` vers la branche de déploiement `gh-pages` _via subtree_.

Hop, du code propre à tous les étages, et des procédures bien isolées :).

## Cas des quick-fix

Bon, ça c'est l'idéal. Dans la réalité, il arrive (fréquemment ?) que nous poussions du code en production, mais que nous constations, un peu tard[^lafontaine], qu'un bug nous est passé sous le nez…

Dans ce cas, je vous conseille de faire partir une branche depuis votre tag publié (pratique, hein ?), de réaliser vos corrections, puis de marquer un nouveau tag sur cette base une fois le tout corrigé. S'en suit la procédure habituelle : `release > build > déploy`, comme précédemment.

Eventuellement, vous pourriez repartir de la branche `release` pour réaliser un _quickwin_ (une correction en 1 commit), mais veillez dans ce cas à _cherry-picker_ votre commit vers `master` pour le reporter vers le _base trunk_ sans importer au passage le répertoire `dist` que nous ne voulons pas voir en dehors de `relase` (beurk beurk…).

## Pour aller encore plus loin

Si vous voulez vous amuser encore un peu, je vous propose d'automatiser la procédure de `build > deploy` avec un outil d'intégration continue comme [Travis](https://travis-ci.org/). En ajoutant cette brique, vous n'avez plus qu'à merger votre tag dans la branche `release` et à repouser celle-ci vers le dépôt central. Travis (ou votre outil d'intégration continue) va écouter cette branche, la récupérer, lancer la tâche de _build_, pousser le contenu du `dist` ainsi obtenu vers la branche de déploiement (`gh-pages`) et repousser cette dernière vers le dépôt central. **COMBO!**

Avec cette procédure, vous ne lancez jamais manuellement le _build_, c'est la CI qui s'en charge. Tout ce qui vous incombe reste de verser le bon tag dans la branche de _release_. Vous gagnez encore des points de confiance[^mage].

## Merci

Je ne sais pas si vous êtes arrivés jusqu'ici sans perdre le fil, mais si c'est le cas, j'espère que vous aurez au moins appris un petit quelque chose…

Bon dév à tout(e)s !

---

[^desproges]: _à l'usage de l'élite et des biens nantis_, donc.
[^spa]: Single Page Application, une webapp servant une page simple au départ et dont la logique va s'effectuer dans le navigateur.
[^plombier]: comme le dit si bien mon plombier, un homme fort sympathique au demeurant.
[^fiction]: ces noms sont fictifs. Toute ressemblance avec un projet existant ou ayant existé bla bla bla…
[^karma]: si c'est le cas, vous avez probablement perdu des points de karma, rendez-vous en page 5.
[^lafontaine]: qu'on ne l'y prendait plus.
[^mage]: et avec un bon MJ, vous passez _Grand Mage Git_, niveau _Architect_.
