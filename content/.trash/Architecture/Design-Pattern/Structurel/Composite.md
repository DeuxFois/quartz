---
title: Composite / Composite
updated: 2022-01-18 19:44:08Z
created: 2021-12-26 15:10:21Z
source: https://refactoring.guru/fr/design-patterns/composite
tags:
  - design_pattern
  - structurel
---

**Composite** permet d’agencer les objets dans des arborescences afin de pouvoir traiter celles-ci comme des objets individuels.

![Patron de conception composite](../../_resources/composite_9d0fd2b82f3248b2a4bbbe88c8faff48.png)

## Problème

L’utilisation de ce patron doit être réservée aux applications dont la structure principale peut être représentée sous la forme d’une arborescence.

Prenons les deux objets suivants : `Produits` et `Boîtes`. Une boîte peut contenir plusieurs produits ainsi qu’un certain nombre de boîtes plus petites. Ces petites boîtes peuvent également contenir quelques produits ou même d’autres boîtes encore plus petites, et ainsi de suite.

Vous décidez de mettre au point un système de commandes qui utilise ces classes. Les commandes peuvent être composées de produits simples sans emballage, d’autres boîtes remplies de produits… et d’autres boîtes. Comment allez-vous déterminer le coût total d’une telle commande ?

<img width="370" height="0" src="../../_resources/problem-fr_580f92b2debd4408874c5dbd7277f3c2.png" class="jop-noMdConv">

Une commande peut contenir divers produits empaquetés à l’intérieur de boîtes, elles-mêmes rangées dans de plus grosses boîtes, etc. La structure complète ressemble à un arbre inversé.

Vous pouvez tenter l’approche directe qui consiste à déballer toutes les boîtes, prendre chaque produit et en faire la somme pour obtenir le total. Ce mode de calcul peut facilement se mettre en place dans le monde réel mais dans un programme, ce n’est pas aussi simple que de créer une boucle. Il faut connaître à l’avance la classe des `Produits` et des `Boîtes` que l’on parcourt, le niveau d’imbrication des boîtes ainsi que d’autres détails. Tout ceci rend l’approche directe assez compliquée et même parfois impossible.

## Solution

Le patron de conception composite vous propose de manipuler les `Produits` et les `Boîtes` à l’aide d’une interface qui déclare une méthode de calcul du prix total.

Comment cette méthode peut-elle fonctionner ? Pour un produit, on retourne simplement son prix. Pour une boîte, on parcourt chacun de ses objets, on leur demande leur prix, puis on retourne un total pour la boîte. Si l’un de ces objets est une boîte plus petite, cette dernière va aussi parcourir son propre contenu et ainsi de suite, jusqu’à ce que tous les prix aient été calculés. Une boîte peut même ajouter des frais supplémentaires, comme le prix de l‘emballage.

<img width="600" height="0" src="../../_resources/composite-comic-1-fr_b0e5f413de0e4ab6a74ea7bf76583.png" class="jop-noMdConv">

Le patron de conception composite emploie une méthode récursive afin de parcourir tous les composants d’une arborescence.

La cerise sur le gâteau est que vous n’avez même pas besoin de connaître la classe concrète des objets de l’arborescence. Vous n’avez pas besoin de savoir si un objet est un produit tout simple ou une boîte sophistiquée, vous les manipulez de la même manière grâce à une interface commune. Lorsque vous faites appel à une méthode, les objets s’occupent de faire transiter la requête en descendant vers les feuilles de l’arbre.

## Analogie

<img width="280" height="0" src="../../_resources/live-example_80e4964736e14dd9a7650bd9726c82f3.png" class="jop-noMdConv">

Un exemple de structure militaire.

En général, les armées d’un pays sont structurées en hiérarchies. Une armée comporte plusieurs divisions, une division est composée de brigades, une brigade est composée de compagnies, qui peuvent elles-mêmes être divisées en escouades. Pour finir, une escouade est un petit groupe de soldats. Les ordres sont donnés au sommet de la hiérarchie et passés au niveau directement inférieur à chaque soldat, qui sait quoi en faire.

## Structure

<img width="360" height="0" src="../../_resources/structure-fr_b1c7adcbd2c0414e90db59731cc46613.png" class="jop-noMdConv">

1.  L’interface **Composant** décrit les opérations communes aux objets simples et complexes de l’arborescence.
    
2.  Une **Feuille** est un élément de base d’une branche qui n’a pas de sous-élément.
    
    En général les feuilles font le plus gros du travail, car elles n’ont personne à qui le déléguer.
    
3.  Le **Conteneur** (alias *composite*) est un élément composé de sous-éléments : des feuilles ou d’autres conteneurs. Un conteneur ne connait pas les classes de ses enfants. Il passe par l’interface composant pour interagir avec ses sous-éléments.
    
    Lorsqu’il reçoit une requête, un conteneur délègue la tâche à ses sous-éléments, traite les résultats intermédiaires, puis renvoie le résultat final au client.
    
4.  Le **Client** manipule les éléments depuis l’interface composant, ce qui lui permet de fonctionner de la même manière pour les éléments simples et complexes de l’arborescence.
    

## Pseudo-code

Dans cet exemple, le patron de conception **Composite** nous permet de gérer des imbrications de formes géométriques dans un éditeur graphique.

<img width="370" height="0" src="../../_resources/example_34b02f2e13e44c3db158e38965b13a02.png" class="jop-noMdConv">

Exemple de l’éditeur des formes géométriques.

La classe `CompositionGraphique` est un conteneur doté d’un certain nombre de formes, incluant même des formes composées. Une forme composée possède les mêmes méthodes qu’une forme simple. Mais plutôt que de tout gérer toute seule, une forme composée envoie une requête récursive à tous ses enfants et « totalise » le résultat.

Le code client manipule ces formes en passant par l’interface commune. De ce fait, le client ne sait jamais s’il est en train de manipuler une forme ou une composition. Le client peut manipuler des structures d’objets très complexes sans jamais être couplé avec les classes concrètes de cette structure.

```
// L’interface du composant déclare des opérations communes pour
// les objets simples et complexes d’une composition.
interface Graphic is
    method move(x, y)
    method draw()

// La classe feuille représente les objets finaux d’une
// composition. Un objet feuille ne peut pas avoir de sous-
// objets. En général, ce sont les feuilles qui lancent les
// traitements. Les objets composite ne font que déléguer le
// travail à leurs sous-composants.
class Dot implements Graphic is
    field x, y

    constructor Dot(x, y) { ... }

    method move(x, y) is
        this.x += x, this.y += y

    method draw() is
        // Dessine un point aux coordonnées X et Y.

// Toutes les classes composant peuvent étendre d’autres
// composants.
class Circle extends Dot is
    field radius

    constructor Circle(x, y, radius) { ... }

    method draw() is
        // Trace un cercle de rayon R aux coordonnées X et Y.

// La classe composite représente les composants complexes qui
// peuvent avoir des enfants. Les objets composite délèguent
// généralement les tâches à leurs enfants et « additionnent »
// ensuite le résultat.
class CompoundGraphic implements Graphic is
    field children: array of Graphic

    // Un composite peut ajouter ou retirer d’autres composants
    // (simples ou complexes) de la liste de ses enfants.
    method add(child: Graphic) is
        // Ajoute un enfant au tableau d’enfants.

    method remove(child: Graphic) is
        // Retire un enfant du tableau d’enfants.

    method move(x, y) is
        foreach (child in children) do
            child.move(x, y)

    // Un composite exécute sa logique principale d’une certaine
    // manière : il parcourt récursivement tous ses enfants,
    // puis récupère et additionne leurs résultats. L’objet est
    // entièrement parcouru, car les enfants du composite
    // passent ces appels à leurs propres enfants et ainsi de
    // suite.
    method draw() is
        // 1. Pour chaque composant enfant :
        //     - Dessine le composant.
        //     - Met à jour le rectangle de délimitation.
        // 2. Dessine un rectangle en pointillé en utilisant les
        // coordonnées de la délimitation.

// Le code client manipule les composants grâce à leur interface
// de base. Ainsi, le code client peut aussi bien prendre en
// charge les composants simples que les complexes.
class ImageEditor is
    field all: CompoundGraphic

    method load() is
        all = new CompoundGraphic()
        all.add(new Dot(1, 2))
        all.add(new Circle(5, 3, 10))
        // ...

    // Combine les composants sélectionnés en un seul composant
    // complexe.
    method groupSelected(components: array of Graphic) is
        group = new CompoundGraphic()
        foreach (component in components) do
            group.add(component)
            all.remove(component)
        all.add(group)
        // Tous les composants vont être dessinés.
        all.draw()

```

## Possibilités d’application

Utilisez le composite si vous devez gérer une structure d’objets qui ressemble à une arborescence.

Le patron de conception composite vous propose deux éléments de base qui partagent la même interface : des feuilles simples et des conteneurs complexes. Un conteneur peut être composé de feuilles et d’autres conteneurs. Grâce à cela, vous pouvez construire une structure récursive composée d’objets imbriqués qui ressemble à un arbre.

Utilisez ce patron si vous voulez que le client interagisse avec les éléments simples aussi bien que complexes de façon uniforme.

Tous les éléments définis dans le patron composite partagent une interface commune. En utilisant cette interface, le client n’a pas besoin de connaître la classe concrète des objets qu’il manipule.

## Mise en œuvre

1.  Assurez-vous que votre application possède bien la forme d’une arborescence. Décomposez-la en conteneurs et en éléments simples. Rappelez-vous que les conteneurs doivent pouvoir accueillir des éléments simples et d’autres conteneurs.
    
2.  Déclarez l’interface composant avec une liste de méthodes qui fonctionnent à la fois avec les composants simples et complexes.
    
3.  Créez une classe feuille pour représenter les éléments simples. Un même programme peut avoir plusieurs classes feuille différentes.
    
4.  Créez une classe conteneur pour représenter les éléments complexes. Fournissez un attribut de type tableau à cette classe, afin de stocker les références aux sous-éléments. Ce tableau doit pouvoir stocker les feuilles et les conteneurs, assurez-vous donc qu’il est bien déclaré avec le type d’interface du composant.
    
    Tout en implémentant les méthodes de l’interface composant, gardez en tête que les conteneurs sont censés déléguer la majeure partie du travail à leurs sous-éléments.
    
5.  Enfin, définissez des méthodes pour ajouter ou retirer des éléments enfants du conteneur.
    
    Elles peuvent être placées à l’intérieur de l’interface composant, mais ceci ne respecte pas le *principe de ségrégation des interfaces*, car la classe feuille contiendra des méthodes vides. L’avantage est que le client pourra traiter ces éléments uniformément, même lorsqu’il construit l’arborescence.
    

## Avantages et inconvénients

- Vous pouvez travailler dans des structures arborescentes complexes plus facilement en utilisant les avantages du polymorphisme et de la récursivité.
    
- *Principe ouvert/fermé*. Vous pouvez introduire de nouveaux types d’éléments dans l’application qui pourront directement être intégrés dans l’arborescence, sans avoir à réécrire l’existant.
    
- Vous rencontrerez parfois des difficultés pour définir une interface commune à certaines classes dont les fonctionnalités sont trop différentes. Dans certains scénarios, vous devez créer une interface composant bien trop générique, rendant le fonctionnement difficile à comprendre.
    

## Liens avec les autres patrons

- Vous pouvez utiliser le [Monteur](https://refactoring.guru/fr/design-patterns/builder) lorsque vous créez des arbres [Composites](https://refactoring.guru/fr/design-patterns/composite) complexes, car vous pouvez programmer les étapes de la construction récursivement.
    
- La [Chaîne de Responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility "Chaîne de responsabilité") est souvent utilisée en conjonction avec le [Composite](https://refactoring.guru/fr/design-patterns/composite). Dans ce cas, lorsqu’une feuille reçoit une demande, elle la passe le long de la chaîne de ses composants parent, jusqu’à la racine de l’arborescence.
    
- Vous pouvez utiliser les [Itérateurs](https://refactoring.guru/fr/design-patterns/iterator) pour parcourir des arbres [Composites](https://refactoring.guru/fr/design-patterns/composite).
    
- Vous pouvez utiliser le [Visiteur](https://refactoring.guru/fr/design-patterns/visitor) pour lancer une opération sur un arbre [Composite](https://refactoring.guru/fr/design-patterns/composite) entier.
    
- Vous pouvez transformer des nœuds de feuilles de l’arbre [Composite](https://refactoring.guru/fr/design-patterns/composite) en [Poids mouches](https://refactoring.guru/fr/design-patterns/flyweight) et les partager pour économiser de la RAM.
    
- Le [Composite](https://refactoring.guru/fr/design-patterns/composite) et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) ont des diagrammes de structure similaires puisqu’ils reposent sur la composition récursive pour organiser un nombre variable d’objets.
    
    Un *décorateur* est comme un *composite*, mais avec un seul composant enfant. Il y a une autre différence importante : Le *décorateur* ajoute des responsabilités supplémentaires à l’objet emballé, alors que le *composite* se contente d’« additionner » les résultats de ses enfants.
    
    Mais ces patrons de conception peuvent également coopérer : vous pouvez utiliser le *décorateur* pour étendre le comportement d’un objet spécifique d’un arbre *Composite*.
    
- Les conceptions qui reposent énormément sur le [Composite](https://refactoring.guru/fr/design-patterns/composite) et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) tirent des avantages à utiliser le [Prototype](https://refactoring.guru/fr/design-patterns/prototype). Il vous permet de cloner les structures complexes plutôt que de les reconstruire à partir de rien.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_cddd45c358d44950bc20aa720f3073e9.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/java/example "Patrons de conception : Composite en Java") [<img width="43" height="56" src="../../_resources/csharp_9ee8f1ab64754c309327678c40a2eac6.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/csharp/example "Patrons de conception : Composite en C#")[<img width="43" height="56" src="../../_resources/cpp_e109cacad19646fa97256bddbecca1ef.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/cpp/example "Patrons de conception : Composite en C++")[<img width="43" height="56" src="../../_resources/php_f55a4a1644624ab190464f495b13fb46.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/php/example "Patrons de conception : Composite en PHP")[<img width="43" height="56" src="../../_resources/python_bf895152b2334ec5bee680903a241b88.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/python/example "Patrons de conception : Composite en Python")[<img width="43" height="56" src="../../_resources/ruby_f43914161d714c68a3c23dcba231dbcb.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/ruby/example "Patrons de conception : Composite en Ruby")[<img width="43" height="56" src="../../_resources/swift_0ff1aa551f734f3486fa1e165415c235.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/swift/example "Patrons de conception : Composite en Swift")[<img width="43" height="56" src="../../_resources/typescript_e7a822d44f51485baa0f0b6732bd5973.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/typescript/example "Patrons de conception : Composite en TypeScript")[<img width="43" height="56" src="../../_resources/go_700610496e944b678f790fe980867e82.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/composite/go/example "Patrons de conception : Composite en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_b7186cc19bb1492aa7fbba8752e.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Décorateur](https://refactoring.guru/fr/design-patterns/decorator)

#### Retour

[Pont](https://refactoring.guru/fr/design-patterns/bridge)

[<img width="245" height="375" src="../../_resources/web-cover-fr_b0303e5e113d4c42a31c5869bb0afeba.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook **Plongée au cœur des PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/composite "English") [Español](https://refactoring.guru/es/design-patterns/composite "Español") [Français](https://refactoring.guru/fr/design-patterns/composite "Français") [Polski](https://refactoring.guru/pl/design-patterns/composite "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/composite "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/composite "Русский") [Українська](https://refactoring.guru/uk/design-patterns/composite "Українська") [中文](https://refactoringguru.cn/design-patterns/composite "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_9446b885daab426cb6a5aef9e562fd4a.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_631d205432054cf48d112ccb24ee09ea.png)

- [Contenu premium](https://refactoring.guru/fr/store)
- [Patrons de conception](https://refactoring.guru/fr/design-patterns)
    - [Introduction](https://refactoring.guru/fr/design-patterns/what-is-pattern)
    - [Catalogue](https://refactoring.guru/fr/design-patterns/catalog)
    - [Patrons de création](https://refactoring.guru/fr/design-patterns/creational-patterns)
    - [Patrons structurels](https://refactoring.guru/fr/design-patterns/structural-patterns)
        - [Adaptateur](https://refactoring.guru/fr/design-patterns/adapter)
        - [Pont](https://refactoring.guru/fr/design-patterns/bridge)
        - [Composite](https://refactoring.guru/fr/design-patterns/composite)
        - [Décorateur](https://refactoring.guru/fr/design-patterns/decorator)
        - [Façade](https://refactoring.guru/fr/design-patterns/facade)
        - [Poids mouche](https://refactoring.guru/fr/design-patterns/flyweight)
        - [Procuration](https://refactoring.guru/fr/design-patterns/proxy)
    - [Patrons comportementaux](https://refactoring.guru/fr/design-patterns/behavioral-patterns)
    - [Exemples de code](https://refactoring.guru/fr/design-patterns/examples)
- [Refactorisation (commenter bientôt)](https://refactoring.guru/fr/refactoring)

[Connexion](https://refactoring.guru/fr/login "Connexion") [Nous contacter](https://feedback.refactoring.guru/?lang=en "Commentaires")

- [Accueil](https://refactoring.guru/fr)
- [Refactorisation](https://refactoring.guru/fr/refactoring)
- [Patrons de conception](https://refactoring.guru/fr/design-patterns)
- [Contenu premium](https://refactoring.guru/fr/store)
- [Forum](https://feedback.refactoring.guru/?lang=en)
- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true)

2014-2021 [Refactoring.Guru](https://refactoring.guru/fr). Tous droits réservés. ![Organization address](../../_resources/fa-building_2ef73d2cc24c48378a5ecb3c7bd5cf91.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305 Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)