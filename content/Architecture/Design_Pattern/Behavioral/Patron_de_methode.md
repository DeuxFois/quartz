---
title: Patron de méthode / Template Method
updated: 2021-12-26 15:28:48Z
created: 2021-12-26 15:18:03Z
source: https://refactoring.guru/fr/design-patterns/template-method
tags:
  - behavioral
  - design_pattern
---

**Patron de Méthode** permet de mettre le squelette d’un algorithme dans la classe mère, mais laisse les sous-classes redéfinir certaines étapes de l’algorithme sans changer sa structure.

![Patron de conception patron de&nbsp;méthode](../../_resources/template-method_b69262f94731486b9b8ad05c2ad3b94c.png)

## Problème

Imaginez que vous êtes en train de créer une application de data mining (exploration de données) qui analyse les documents d’une entreprise. Les utilisateurs alimentent l’application avec différents formats (PDF, DOC, CSV) et celle-ci tente de récupérer les données utiles dans un format uniforme.

La première version de l’application ne fonctionnait qu’avec les fichiers DOC. Dans la version suivante, les fichiers CSV étaient acceptés. Un mois plus tard, vous lui avez « appris » à récupérer les données des fichiers PDF.

![Les classes de data mining contiennent beaucoup de code dupliqué](../../_resources/problem_6670b65736b8415586597473be4cd39d.png)

Les classes de data mining contiennent beaucoup de code dupliqué.

Au bout d’un moment, vous remarquez que ces trois classes comportent beaucoup de code similaire. Bien que le code qui gère les différents formats de données soit complètement différent d’une classe à l’autre, celui qui traite et analyse les données est presque identique. Ne serait-ce pas super de se débarrasser de tout ce code dupliqué tout en laissant la structure de l’algorithme intact ?

Un autre problème se pose avec le code client qui utilise ces classes. Il y a de gros blocs conditionnels qui choisissent un comportement en fonction de la classe de l’objet traité. Si ces trois classes avaient une interface commune (ou une classe de base), vous pourriez enlever toutes les conditions du code client et utiliser le polymorphisme lorsque vous appelez les méthodes sur un objet à traiter.

## Solution

Le patron de méthode vous propose de découper un algorithme en une série d’étapes, de transformer ces étapes en méthodes et de mettre l’ensemble des appels à ces méthodes dans une seule méthode socle, le *patron de méthode*. Les étapes peuvent être `abstraites` ou avoir une implémentation par défaut. Pour utiliser l’algorithme, le client doit fournir sa propre sous-classe, implémenter toutes les étapes abstraites et redéfinir certaines d’entre elles si besoin (mais pas la méthode socle elle-même).

Mettons ceci en application dans notre logiciel de data mining. Nous pouvons créer une classe de base pour les trois algorithmes d’analyse syntaxique (parsing). Cette classe définit une méthode socle, composée d’une série d’appels à plusieurs étapes qui traitent les documents.

<img width="600" height="0" src="../../_resources/solution-fr_607cb7fa61f24fd78b5331b5bc71ebf3.png"/>

La méthode socle scinde l’algorithme en plusieurs étapes, permettant aux sous-classes de les redéfinir, mais on ne redéfinit pas la méthode socle elle-même.

En premier lieu, nous pouvons déclarer toutes les étapes `abstraites` afin de forcer les sous-classes à définir leur propre implémentation pour ces méthodes. Dans notre cas, les sous-classes ont déjà les implémentations nécessaires. Nous devons donc uniquement ajuster les signatures des méthodes en les faisant correspondre à celles de la classe mère.

À présent, voyons ce que l’on peut faire pour se débarrasser du code dupliqué. Il semblerait que le code pour ouvrir/fermer les fichiers et récupérer/parser les données est différent pour chaque format de données, il n’y a donc aucune raison de toucher à ces méthodes. En revanche, les autres étapes (analyser les données brutes et établir les rapports) se ressemblent de près et leur implémentation peut donc être déplacée dans la classe de base, où les sous-classes se partagent le code.

Comme vous pouvez le voir, nous avons deux types d’étapes :

- les *opérations abstraites* qui doivent être implémentées dans chaque sous-classe.
- les *opérations facultatives* qui possèdent déjà une implémentation par défaut, mais peuvent être redéfinies si besoin.

Toutefois, un autre type d’étape existe : les *crochets* (hooks). Un crochet est une étape facultative dont le corps de méthode est laissé vide. Un patron de méthode peut fonctionner même si un crochet n’est pas redéfini. En général, les crochets sont placés avant ou après les étapes cruciales des algorithmes et procurent aux sous-classes des points d’extension supplémentaires.

## Analogie

<img width="590" height="0" src="../../_resources/live-example_cbe119a3086644f38bd0d442e8e55b25.png"/>

Un plan architectural typique peut être légèrement remanié pour mieux répondre aux besoins d’un client.

Le patron de méthode peut être utilisé pour construire des maisons en série. Le plan architectural pour construire une maison standard peut être doté de plusieurs points d’extensions qui permettent à un propriétaire potentiel d’ajuster certains détails dans la maison.

Chaque étape de construction (poser les fondations, poser la charpente, monter les murs, installer la plomberie pour l’eau et les câbles pour l’électricité, etc.) peut être légèrement modifiée pour différencier un peu la maison des autres.

## Structure

<img width="340" height="0" src="../../_resources/structure_c023936c9a544ad499dec4c7354b5d01.png"/>

1.  La **Classe Abstraite** déclare des méthodes (qui représentent les étapes d’un algorithme) et la méthode patronDeMéthode qui appelle toutes ces méthodes dans un ordre spécifique. Les étapes peuvent être déclarées abstraites ou posséder une implémentation par défaut.
    
2.  Les **Classes Concrètes** peuvent redéfinir toutes les étapes, mais pas patronDeMéthode.
    

## Pseudo-code

Dans cet exemple, le patron de conception **Patron de Méthode** fournit un squelette pour les branches de l’IA (intelligence artificielle) d’un jeu vidéo de stratégie simple.

<img width="430" height="0" src="../../_resources/example_0b7f81a0eab240d192fa73be34d75645.png"/>

Les classes d’IA dans un jeu vidéo simple.

Toutes les races du jeu ont à peu près les mêmes unités et bâtiments. Vous pouvez donc réutiliser la même structure d’IA pour les différentes races, tout en personnalisant certains détails. Grâce à cette approche, vous pouvez redéfinir l’IA des orcs pour la rendre plus agressive, obtenir des humains plus défensifs, et faire en sorte que les monstres ne puissent pas construire de bâtiments. Pour ajouter une autre race au jeu, vous devez simplement créer une nouvelle sous-classe d’IA et redéfinir les méthodes par défaut déclarées dans la classe de base de l’IA.

```
// La classe abstraite définit un patron de méthode qui contient
// le squelette d’un algorithme. Ce dernier est généralement
// composé d’appels à des opérations primitives abstraites. Les
// sous-classes concrètes implémentent ces opérations, mais ne
// touchent pas au patron de méthode.
class GameAI is
    // Le patron de méthode définit le squelette d’un
    // algorithme.
    method turn() is
        collectResources()
        buildStructures()
        buildUnits()
        attack()

    // Certaines étapes peuvent être implémentées directement
    // dans une classe de base.
    method collectResources() is
        foreach (s in this.builtStructures) do
            s.collect()

    // Et certaines peuvent être abstraites.
    abstract method buildStructures()
    abstract method buildUnits()

    // Une classe peut avoir plusieurs patrons de méthode.
    method attack() is
        enemy = closestEnemy()
        if (enemy == null)
            sendScouts(map.center)
        else
            sendWarriors(enemy.position)

    abstract method sendScouts(position)
    abstract method sendWarriors(position)

// Les classes concrètes doivent implémenter toutes les
// opérations abstraites de la classe de base, mais elles ne
// doivent pas redéfinir le patron de méthode.
class OrcsAI extends GameAI is
    method buildStructures() is
        if (there are some resources) then
            // Construit des fermes, des casernes, puis une
            // forteresse.

    method buildUnits() is
        if (there are plenty of resources) then
            if (there are no scouts)
                // Construit un ouvrier et l’ajoute au groupe
                // d’éclaireurs.
            else
                // Construit un combattant et l’ajoute au groupe
                // des guerriers.

    // ...

    method sendScouts(position) is
        if (scouts.length > 0) then
            // Envoie les éclaireurs à la position.

    method sendWarriors(position) is
        if (warriors.length > 5) then
            // Envoie les guerriers à la position.

// Les sous-classes peuvent redéfinir certaines opérations avec
// une implémentation par défaut.
class MonstersAI extends GameAI is
    method collectResources() is
        // Les monstres ne récoltent pas de ressources.

    method buildStructures() is
        // Les monstres ne fabriquent pas de bâtiments.

    method buildUnits() is
        // Les monstres ne construisent pas d'unités.

```

## Possibilités d’application

Utilisez le patron de méthode si vous voulez que vos clients puissent étendre des étapes spécifiques d’un algorithme, mais pas l’algorithme entier ou sa structure.

Le patron de méthode vous permet de transformer un algorithme monolithique en une série d’étapes individuelles qui peuvent être facilement étendues par des sous-classes, tout en gardant intacte la structure établie dans une classe mère.

Utilisez ce patron si vous avez plusieurs classes qui contiennent des algorithmes presque identiques, avec seulement quelques différences mineures. Par conséquent, vous risqueriez de devoir retoucher toutes les classes lorsque vous modifiez l’algorithme.

Lorsque vous transformez un tel algorithme en un patron de méthode, vous pouvez également remonter les étapes dotées d’implémentations similaires dans la classe mère, afin d’éviter la duplication de code. Vous pouvez laisser le reste du code dans les sous-classes.

## Mise en œuvre

1.  Analysez l’algorithme ciblé pour voir si vous pouvez le décomposer en étapes. Déterminez les étapes communes à toutes les sous-classes et celles qui sont uniques.
    
2.  Créez une classe de base abstraite et déclarez le patron de méthode et un ensemble de méthodes abstraites pour représenter les opérations de l’algorithme. Faites une ébauche de la structure de l’algorithme dans ce patron de méthode en appelant les opérations correspondantes. Rendez ce patron `final` pour empêcher les sous-classes de la redéfinir.
    
3.  Cela ne pose aucun problème si toutes les opérations sont abstraites, mais une implémentation par défaut bénéficierait à certaines opérations. Les sous-classes n’ont pas besoin d’implémenter ces méthodes.
    
4.  Pensez à ajouter des crochets entre les étapes cruciales de votre algorithme.
    
5.  Pour chaque variante de l’algorithme, créez une nouvelle sous-classe. Elle *doit* implémenter toutes les opérations abstraites, mais *peut* également redéfinir les opérations facultatives.
    

## Avantages et inconvénients

- Vous permettez aux clients de redéfinir certaines parties d’un grand algorithme. Elles sont ainsi moins affectées par les modifications apportées aux autres parties de l’algorithme.
- Vous pouvez remonter le code dupliqué dans la classe mère.

- Certains clients peuvent être limités à cause du squelette de l’algorithme.
- Vous ne respectez pas le *Principe de substitution de Liskov*, si vous supprimez l’implémentation d’une étape par défaut dans une sous-classe.
- Plus vous avez d’étapes, plus le patron de méthode devient difficile à maintenir.

## Liens avec les autres patrons

- La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est une spécialisation du [Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method). Une *fabrique* peut aussi faire office d’étape dans un grand *patron de méthode*.
    
- Le [Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method) est basé sur l’héritage : il vous laisse modifier certaines parties d’un algorithme en les étendant dans les sous-classes. La [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) est basée sur la composition : vous pouvez modifier certaines parties du comportement de l’objet en lui fournissant différentes stratégies qui correspondent à ce comportement. Le *patron de méthode* agit au niveau de la classe, il est donc statique. La *stratégie* agit au niveau de l’objet et vous laisse permuter les comportements à l’exécution.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_9aaa4c2e3ccc4b699f62a3670f7c250c.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/java/example "Patrons de conception : Patron de méthode en Java") [<img width="43" height="56" src="../../_resources/csharp_5e4dcb193c364f6c8dc7bda670ba13e3.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/csharp/example "Patrons de conception : Patron de méthode en C#")[<img width="43" height="56" src="../../_resources/cpp_d2dbb1f82cdc489185f3eb0aac85c6f9.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/cpp/example "Patrons de conception : Patron de méthode en C++")[<img width="43" height="56" src="../../_resources/php_478bbb4e306d412cbedc242da92146b2.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/php/example "Patrons de conception : Patron de méthode en PHP")[<img width="43" height="56" src="../../_resources/python_7c8201aaa5ab480586871511009e1aaf.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/python/example "Patrons de conception : Patron de méthode en Python")[<img width="43" height="56" src="../../_resources/ruby_94ffc034621748abb6963cda8992d566.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/ruby/example "Patrons de conception : Patron de méthode en Ruby")[<img width="43" height="56" src="../../_resources/swift_4dc78ba3db184f39b74c2eab1d5ffdf2.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/swift/example "Patrons de conception : Patron de méthode en Swift")[<img width="43" height="56" src="../../_resources/typescript_fde7d6ee001c4e5d8a0621199fb0a23b.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/typescript/example "Patrons de conception : Patron de méthode en TypeScript")[<img width="43" height="56" src="../../_resources/go_6181061eef244e298b461e0c83c82544.svg"/>](https://refactoring.guru/fr/design-patterns/template-method/go/example "Patrons de conception : Patron de méthode en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_751def784d3742ae96793690326.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Visiteur](https://refactoring.guru/fr/design-patterns/visitor)

#### Retour

[Stratégie](https://refactoring.guru/fr/design-patterns/strategy)

[<img width="245" height="375" src="../../_resources/web-cover-fr_d6f958870a4148b88a932eddd32657d8.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/template-method "English") [Español](https://refactoring.guru/es/design-patterns/template-method "Español") [Français](https://refactoring.guru/fr/design-patterns/template-method "Français") [Polski](https://refactoring.guru/pl/design-patterns/template-method "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/template-method "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/template-method "Русский") [Українська](https://refactoring.guru/uk/design-patterns/template-method "Українська") [中文](https://refactoringguru.cn/design-patterns/template-method "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_1e7e0d1c04304ed98cc2c8cdc8387fa0.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_8240bf2ec4974f00b06fcbf1deb727d3.png)

- [Contenu premium](https://refactoring.guru/fr/store)
- [Patrons de conception](https://refactoring.guru/fr/design-patterns)
    - [Introduction](https://refactoring.guru/fr/design-patterns/what-is-pattern)
    - [Catalogue](https://refactoring.guru/fr/design-patterns/catalog)
    - [Patrons de création](https://refactoring.guru/fr/design-patterns/creational-patterns)
    - [Patrons structurels](https://refactoring.guru/fr/design-patterns/structural-patterns)
    - [Patrons comportementaux](https://refactoring.guru/fr/design-patterns/behavioral-patterns)
        - [Chaîne de responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility)
        - [Commande](https://refactoring.guru/fr/design-patterns/command)
        - [Itérateur](https://refactoring.guru/fr/design-patterns/iterator)
        - [Médiateur](https://refactoring.guru/fr/design-patterns/mediator)
        - [Mémento](https://refactoring.guru/fr/design-patterns/memento)
        - [Observateur](https://refactoring.guru/fr/design-patterns/observer)
        - [État](https://refactoring.guru/fr/design-patterns/state)
        - [Stratégie](https://refactoring.guru/fr/design-patterns/strategy)
        - [Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method)
        - [Visiteur](https://refactoring.guru/fr/design-patterns/visitor)
    - [Exemples de code](https://refactoring.guru/fr/design-patterns/examples)
- [Refactorisation (commenter bientôt)](https://refactoring.guru/fr/refactoring)

[Connexion](https://refactoring.guru/fr/login "Connexion") [Nous contacter](https://feedback.refactoring.guru/?lang=en "Commentaires")

- [Accueil](https://refactoring.guru/fr)
- [Refactorisation](https://refactoring.guru/fr/refactoring)
- [Patrons de conception](https://refactoring.guru/fr/design-patterns)
- [Contenu premium](https://refactoring.guru/fr/store)
- [Forum](https://feedback.refactoring.guru/?lang=en)
- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true)

2014-2021 [Refactoring.Guru](https://refactoring.guru/fr). Tous droits réservés.
 ![Organization address](../../_resources/fa-building_448d9b061b40475ea98fc157d7462b8f.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)