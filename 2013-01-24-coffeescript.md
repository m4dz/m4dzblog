---
layout: post
title: "CoffeeScript, le JS++"
lang: fr
category: tech
tags:
- javascript
- coffeescript
external-url: http://www.clever-age.com/veille/blog/du-javascript-avec-coffeescript.html
---

_Initialement [publié sur le blog de Clever Age](http://www.clever-age.com/veille/blog/du-javascript-avec-coffeescript.html), je reprends ici le contenu de mon article consacré à CoffeeScript et ses bienfaits…_

<-- more -->

JavaScript tient le haut du pavé dans le web moderne. On en parle beaucoup, on lui offre de nombreux développements, on le ramène à la vie après une traversée du désert. Mais JavaScript n’est pas exempt de défauts. Face à cette situation, Jeremy Ashkenas a conçu CoffeeScript, un micro-langage qui compile vers JavaScript. Petite présentation…

On parle beaucoup de JavaScript. L’émulation de la communauté autour de ce petit langage de développement "dans le navigateur" — initialement conçu en 10 jours (rappelons-le) par Brendan Eich — n’a jamais été aussi forte que ces dernières années. JavaScript a retrouvé ses lettres de noblesse, notamment grâce à des personnalités comme Douglas Crockford ([JavaScript, The Good Parts](http://shop.oreilly.com/product/9780596517748.do)) et Ryan Dahl ([Node.js](http://nodejs.org/)). Plus question de s’en passer aujourd’hui, JavaScript, c’est l’avenir dans le présent, aussi bien en front qu’en back.

# JavaScript, WTF ?

Oui, mais JavaScript, pour de nombreuses raisons, notamment historiques, souffre de défauts qui peuvent vite s’avérer être un véritable calvaire dès qu’il s’agit de les contourner. On moque beaucoup le langage (moi le premier), et ses plus ardents défenseurs sont aussi souvent les premiers à le brocarder.

Parce que souvenons-nous : JavaScript naît lors de l’hiver 1995 dans l’esprit de Brendan Eich. L’objectif est alors de concevoir un langage simple, embarquable dans la prochaine version de Netscape, et destiné à concurrencer le VBScript d’Internet Explorer dont la concurrence commence à se faire sentir, et qui pourrait donner un sérieux atout à Microsoft dans la guerre qui fait déjà rage. Mais il faut faire vite : le développement du navigateur est déjà bien avancé, et la livraison de la première beta ne devrait plus tarder. Le temps manque : Brendan Eich se voit confier cette délicate mission et publie en 10 jours un premier draft du langage. Pas le temps de revenir sur des éléments pensés rapidement, et qui manquent parfois de cohérence : on implémente le langage, on l’inclut dans la beta, 3… 2… 1… *Ignition* ! C’est déjà trop tard, et plein de choses sont cristallisées dans le code. Il est désormais hors de question de faire machine arrière : ce serait laisser sur le carreau un bon paquet de développeurs web qui commencent à faire honneur au nouveau venu. Tant pis, on va traîner les vieux dossiers, et s’arranger au quotidien pour contourner les problèmes qu’ils nous posent.

# A New Hope

Le choix est fait, on assurera la rétro-compatibilité. On fera évoluer le langage, mais ça prendra du temps. Heureusement, l’essentiel de JavaScript est bien pensé, solidement structuré, et c’est tout de même un plaisir de l’utiliser. Pour les lacunes qui demeurent, pas beaucoup de solutions. Ou plutôt si, une seule, évidente : développer un nouveau langage. Mais la tâche est impossible : celui-ci viendrait en confrontation directe avec un outil largement utilisé et répandu, qui plus est standardisé [ECMA](http://www.ecma-international.org/publications/standards/Ecma-262.htm). Alors la solution, c’est peut-être de concevoir, non pas un nouveau langage dans le navigateur, mais un langage **qui compile** vers javascript. Ainsi, on garde la puissance d’un langage repensé de zéro, et la compatibilité avec l’existant. Une forme de couche d’abstraction qui nous permettra de nous affranchir de tous les petits défauts et de nous tourner vers l’avenir.

C’est de cette réflexion que naît l’idée de CoffeeScript dans la tête de Jeremy Ashkenas. Il va prendre les logiques qu’il apprécie dans d’autres langages : Ruby, Python… et les appliquer à JavaScript.

Car oui, CoffeeScript, ce n’est *que* du JavaScript. Même puissance, mêmes avantages ; les défauts en moins.

# Petit tour d’horizon

CoffeeScript propose - et n’impose jamais - plusieurs concepts. Ceux d’entre-vous qui viennent de Ruby et Python notamment ne seront pas dépaysés.

## Pas de point virgule

En JavaScript, le point-virgule est optionnel (si, si, je vous assure). Avec CoffeeScript, il est franchement déconseillé :

```coffeescript
myVar = 'foo'
```

## L’indentation définit les blocs

Dans le même esprit de nettoyage des caractères superflus polluant la lecture, plus d’accolades (encore une fois, c’est une forte suggestion, pas une obligation). L’indentation définit les blocs (1 tab == 2 espaces) :

```coffeescript
myObj =
    name:   'MAD'
    agency: 'Clever Age'
    skills: [
        'javascript'
        'html'
        'css'
        'ruby'
    ]
```

## Définition et appels de fonction simplifiés

Les fonctions sont encore plus simplement définies en utilisant la simple flèche `->` précédée si besoin des arguments. Les fonctions sont systématiquement des expressions :

```coffeescript
myFunc = (a, b) ->
    result = a + b
    "La somme des arguments vaut #{result}"

console.log myFunc 3, 5
```

Ici, on défint une simple fonction `myFunc` qui prend 2 arguments `a` et `b`. Cette fonction réalise la somme des deux et retourne la phrase `La somme des arguments vaut #{result}` en substituant la valeur du résultat dans la sortie. Je précise que la fonction retourne la chaîne de caractères : en CoffeeScript, les fonctions retournent systématiquement le résultat de la dernière expression (et en CoffeeScript, tout est une expression).

On peut donc appeler la fonction avec ou sans parenthèses : elles sont optionnelles si on passe des arguments. :

```coffeescript
myFunc = ->
    'Hello World'        # == return 'Hello World'

message = myFunc         # message référence juste la méthode `myFunc` sans l'appeler
message = myFunc()       # message == 'Hello World'
```

En effet, comme en JavaScript pur, le fait d’appeler une fonction par sa variable (donc sans parenthèses, et sans arguments) référence simplement cette dernière sans l’appeler.

## JSLint compatible

Dans l’esprit de *JavaScript, The Good Parts*, CoffeeScript produit un code nativement compatible avec JSLint (le linter de Douglas Crockford). Il respecte donc une des premières règles : toujours utiliser `var` pour déclarer vos variables. Sauf que dans son cas, vous n’avez plus à vous en préoccuper : c’est CoffeeScript qui va se charger de déclarer explicitement vos variables en haut de leur scope. Pratique pour éviter les oublis…

Dans le même esprit, à de très rares exceptions près, les comparaisons se font systématiquement en valeur **et** en type (ce que vous devriez normalement faire, bande de coquins). Les comparaisons utilisent donc systématiquement , en interne `===` et `!==`. Pour éviter la confusion, vous pouvez utiliser les plutôt lisibles `is` et `isnt` au lieu de `==` et `!=`.

# Du sucre dans votre café ?

CoffeeScript, c’est ça, du *syntactic sugar for JavaScript*, une façon de se simplifier la vie. Petite revue de détails…

## Tout est une expression

CoffeeScript est pensé pour pouvoir être lu de façon littérale pour faciliter la compréhension de son code. C’est une mise en application d’une vieille idée de Donald Knuth qui propose de concevoir le code comme une suite d’expressions logiques lisibles, en opposition à une suite d’instructions machine parfois obscures. Vous pouvez donc écrire une expression ternaire de cette manière :

```coffeescript
wait = if coffee.hot then 5 else 0
console.log 'Just wait #{wait} minute(s) before drinking your coffee.'
```

## Opérateur d’existence

Vous avez besoin de tester si une variable est à `undefined` ou `null` ? Pas de problème. L’opérateur d’existence `?` est là pour vous aider :

```coffeescript
# return true if coffee is not undefined
ready = true if coffee?

# Teste l'existence d'une méthode et l'applique de façon chaînée
ready = machine.prepare?().coffee?.ready
```

## Bloc conditionnels

Les blocs conditionnels peuvent être écrits de différentes façons, pour être le plus clair possible :

```coffeescript
cream = true if coffee == 'cappuccino'

cream = if coffee == 'cappuccino' then true else false

if coffee == 'cappuccino' then
    cream = true
else
    cream = false

cream = false unless coffee is 'cappuccino'
```

## Opérations sur les tableaux

Vous pouvez utiliser la syntaxe Ruby `..` et `...` pour gérer les index de vos tableaux. On peut créer un tableau rempli des entiers de 0 à 9 (inclus) de cette façon :

```coffeescript
# jusqu'à 9 inclus
numbers = [0..9]

# jusqu'à 10 exclus
numbers = [0...10]
```

Vous pouvez également vous en servir pour "découper" (`slice`) ou modifier en place (`splice`) votre tableau, ou faire de l’affectation dynamique :

```coffeescript
coffees = [ "affogato", "caffè latte", "cappuccino", "carajillo", "macchiato", "mocha", "mazagran" ]

# Récupérer uniquement les noms commençant par "c" (index 1 à 3)
c_coffees = coffees[1...4]

# Récupérer tous les cafés à partir de l'index 4
m_coffees = coffees[4..]

# Remplacer les cafés aux index 3 à 6
coffees[3..6] = [ "irish coffee", "kopi susu", "eiskaffee", "yuanyang" ]
```

## Boucles `for` conditionnelles

CoffeeScript lève la confusion usuelle autour de la boucle `for…in` : en JavaScript, elle itère sur les propriétés d’un objet, alors que tout le monde s’attend à ce qu’elle fasse une boucle sur les valeurs, notamment pour les tableaux. En CoffeeScript, `for…in` fait donc bien une telle boucle, et c’est le nouveau `for…of` qui itère sur les propriétés d’un objet.

Personnellement, j’ai un petit faible pour la boucle `for` conditionnelle en fin d’expression (on parle de *compréhension conditionnelle*) qui illustre bien la puissance du langage :

```coffeescript
coffees = [ "affogato", "caffè latte", "cappuccino", "macchiato" ]

# Réalise une boucle `for` sur le tableau `coffees` et
# passe la valeur de chaque itération à la méthode `drink()`
# uniquement si la valeur ne vaut pas "capuccino" ou "affogato"
drink coffee for coffee in coffees when coffee not in ["cappuccino", "affogato"]
```

## Classes

Pour ceux qui sont déroutés par le fonctionnement objet prototypal de JavaScript, CoffeeScript apporte une syntaxe plus "traditionnelle" du langage orienté objet. C’est une simple encapsulation du `prototype`, rien d’autre.

Il offre un constructeur aux classes, avec une assignation automatique possible des variables passées en paramètre aux propriétés de l’objet créé (si ces paramètres sont précédés du symbole `@`, qui est un raccourci pour `this`). Il propose une logique d’héritage via le mot-clé `extends`, et un accès à l’objet parent via le mot-clé `super`.

```coffeescript
class Coffee
    constructor: (@name, @cream = null) ->

    prepare: ->
        extra = " with cream" if @cream
        "Go to prepare a #{@name}#{extra}"

class CoffeeCream extends Coffee
    constructor: (name) ->
        super name, true

affogato = new CoffeeCream "affogato"
alert affogato.prepare()
```

CoffeeScript permet également de gérer le binding facilement, en utilisant la *fat arrow* `=>` pour définir une méthode plutôt que la simple flèche.

Petit rappel sur le binding en JavaScript : lorsque vous définissez une méthode au sein d’un objet, le mot-clé `this` désigne l’objet courant, celui sur lequel votre méthode est appelée. Si votre fonction est appelée autrement que directement (`obj.method args`), par exemple en tant que fonction de rappel (*callback*), vous perdez le binding et le mot-clé `this` devient l’objet global (dans un navigateur, c’est `window`). Une bonne pratique consiste donc à stocker en référence l’objet courant dans une variable `self` ou `_this` pour continuer à l’utiliser plus tard, ou à forcer le binding à l’aide du `bind` de ES5/Prototype/jQuery.

Avec CoffeeScript, il suffit d’utiliser la *fat arrow* au lieu de la flèche simple :

```coffeescript
class Checkout
    constructor: ( @customer, @cart ) ->
        # A partir de là, `this` devient l'objet jQuery courant.
        # Pour conserver le binding @ === Checkout, on utilise `=>`
        $( '.cart_add' ).on 'click', (e) =>
            e.preventDefault()
            e.stopPropagation()

            @customer.purchase @cart
```

# One more thing

Tout ça n’est qu’un rapide survol de l’ensemble des capacités du langage. Dans les petites choses qui vous simplifient largement la vie au quotidien, on trouve notamment :

- La syntaxe heredoc ;
- Les blocs d’expressions régulières ;
- Les splats ;
- Une vraie plus-value sur le Duck Typing (avec un usage extensif de l’opérateur d’existence) ;
- Et encore plein d’autres petites pépites !

**Alors, c’est bien joli, mais…**

Oui, c’est bien joli. C’est même un vrai confort d’utilisation au quotidien *si vous en ressentez le besoin*. Le but de CoffeeScript n’est pas de remplacer JavaScript. CoffeeScript **est** JavaScript. Il est là pour combler certaines lacunes ou incohérences du langage et faciliter le développement au quotidien. Personnellement, j’apprécie de pouvoir me concentrer sur ma logique plutôt que de devoir trouver un truc pour contourner un comportement de JavaScript inattendu. Certains apprécieront, d’autres pas.

Le reproche le plus souvent formulé à l’encontre de CoffeeScript, c’est que le code qui va être exécuté n’est pas *votre* code. C’est celui *compilé* par CoffeeScript. Et c’est cette notion de lâcher-prise qui gêne beaucoup de développeurs. J’opposerai à cet argument que :

- votre logique n’est pas modifiée : vos fonctions et vos classes se comportent comme vous les avez définies, et elles portent les même nom. Si vous utilisez une charte de nommage cohérente, il n’y a aucune raison pour que vous ne retrouviez pas vos petits ;
- Les sources compilées sont correctement indentées et structurées : le code est donc lisible et propre, facile à relire et à comprendre ;
- Les sources générées sont particulièrement optimales en performance ;
- L’ensemble des instructions JavaScript sont comprises par CoffeeScript. Vous pouvez donc placer autant de fois que vous le souhaitez l’instruction `debugger` dans votre code. Si vous n’avez jamais joué avec `debugger`, vous perdez 10 point de karma. C’est une bonne occasion de vous y mettre ;) ;
- Le support de sourcemap dans CoffeeScript arrive bientôt, et l’implémentation commence à être de plus en plus mature dans les navigateurs récents. Ce sera encore un avantage de plus pour observer le fonctionnement de votre code.

Enfin, à titre d’exemple, vous pouvez facilement trouver des projets qui utilisent CoffeeScript IRL, et qui s’en sortent plutôt bien :

- Dropbox
- CloudApp
- Posterous
- Trello
- Airbnb mobile
- Basecamp mobile
- et plein d’autres [visibles ici](https://github.com/jashkenas/coffee-script/wiki/In-The-Wild)

De nombreux outils, frameworks et bibliothèques utilisent par ailleurs CoffeeScript comme langage de développement désormais, par exemple :

- BatmanJS
- ChaplinJS
- Brunch
- Docco
- Pow
- Stitch
- TowerJS
- Underscore.js

Sans compter que le compilateur lui-même est écrit en CoffeeScript…

**Ok, je suis convaincu, comment je fais maintenant ?**

Si vous êtes arrivé jusqu’ici, vous êtes aux portes de CoffeeScript. Le compilateur est disponible sous forme de paquet NPM, il vous faudra donc un environnement [Node.js](http://nodejs.org/) fonctionnel pour l’utiliser.

Pour l’installer, rien de plus simple :

```shell
$ npm -g install coffee-script
```

Pour l’utiliser, passez simplement vos scripts au compilateur avec le switch `-c` pour lancer la compilation.

```shell
$ coffee -c main.coffee
```

Il dispose également d’un *watcher* capable d’observer les changements sur vos fichiers sources et de relancer la compilation. Pratique en temps de développement.

```shell
$ coffee -w main.coffee
```

Vous n’avez plus qu’à vous plonger dans le langage dès maintenant, tout est disponible à l’adresse [http://www.coffeescript.org](http://www.coffeescript.org), y compris de quoi tester interactivement directement dans votre navigateur !

# À vous maintenant !

Vous avez les cartes en main. Libre à vous de l’utiliser si vous en avez envie, pour tester ou en production. D’expérience, son usage n’est certainement pas justifié pour tous les projets (dixit Jeremy Ashkenas lui-même) mais dès que vous aurez à vous plonger dans des logiques de code complexes, il pourra vous être d’un grand secours. Et puis, si Brendan Eich lui-même vante ses mérites et propose d’inclure directement dans le standard ECMAScript certaines de ses syntaxes, on peut globalement être rassuré sur la qualité et la pérennité du langage.

A vous donc : 3… 2… 1… *Ignition* !
