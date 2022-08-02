---
title: Prototype
updated: 2021-12-26 15:28:06Z
created: 2021-12-26 14:01:28Z
source: https://refactoring.guru/fr/design-patterns/prototype
tags:
  - création
  - design_pattern
---

**Prototype** crée de nouveaux objets à partir d’objets existants sans rendre le code dépendant de leur classe.

![Patron de conception prototype](../../_resources/prototype_6f91352eb56041039dfcda0dddb25f97.png)

## Problème

Nous voulons une copie exacte d’un objet. Comment vous y prendriez-vous ? Tout d’abord, vous devez créer un nouvel objet de cette même classe. Ensuite, vous devez parcourir tous les attributs de l’objet d’origine et copier leurs valeurs dans le nouvel objet.

Super ! Mais il y a un hic. Certains objets ne peuvent pas être copiés ainsi, car leurs attributs peuvent être privés et ne pas être visibles en dehors de l’objet.

![Ce qui peut mal tourner lorsque l’on construit un objet « depuis l’extérieur »](../../_resources/prototype-comic-1-fr_8dbe0f974a95480197f30256be021.png)

Copier un objet « depuis l’extérieur » [n’est pas](https://refactoring.guru/cargo-cult) toujours possible.

Un autre problème se profile avec l’approche directe. Étant donné que vous devez connaître la classe de l’objet pour le dupliquer, votre code devient dépendant de cette classe. Même si cette dépendance ne vous effraie pas, il y a un autre hic. Parfois, vous ne connaissez que l’interface implémentée par l’objet, mais pas sa classe concrète. Si le paramètre d’une méthode accepte tous les objets qui implémentent une interface donnée, vous pouvez rencontrer des problèmes.

## Solution

Le patron de conception prototype délègue le processus de clonage aux objets qui vont être copiés. Il déclare une interface commune pour tous les objets qui pourront être clonés. Cette interface vous permet de cloner un objet sans coupler votre code à la classe de cet objet. En général, une telle interface ne contient qu’une seule méthode `clone`.

L’implémentation de cette méthode se ressemble dans toutes les classes. Elle crée un objet de la classe actuelle et reporte les valeurs des attributs de l’objet d’origine dans le nouveau. Vous pouvez également copier des attributs privés, car la plupart des langages de programmation autorisent l’accès à des objets d’une même classe.

Un objet qui peut être cloné est appelé un *prototype*. Lorsque vos objets possèdent des dizaines d’attributs et des centaines de configurations possibles, le clonage est une alternative au sous-classage.

<img width="343" height="0" src="../../_resources/prototype-comic-2-fr_26f400eae0ec4ddcbe1bf6f963983.png" class="jop-noMdConv">

Les prototypes préconstruits sont une alternative au sous-classage.

Leur fonctionnement est le suivant : vous créez un ensemble d’objets configurés différemment. Dès que vous avez besoin de l’un de ces objets, vous clonez un prototype plutôt que d’en construire un nouveau.

## Analogie

Dans la vie réelle, des prototypes sont utilisés pour exécuter des tests avant de lancer les chaînes de production d’un produit. Dans ce cas toutefois, les prototypes ne font pas partie intégrante de la fabrication, ils ne jouent qu’un rôle passif.

<img width="600" height="0" src="../../_resources/prototype-comic-3-fr_f4912030fde8454caa063fd0f3625.png" class="jop-noMdConv">

La division cellulaire.

Puisque les prototypes industriels ne se clonent pas eux-mêmes, étudions le processus de la mitose d’une cellule (petit rappel de vos cours de biologie). Une fois la division mitotique terminée, une paire de cellules identiques est créée. La cellule originale sert de prototype et joue un rôle actif dans la création de la copie.

## Structure

#### Implémentation de base

<img width="500" height="0" src="../../_resources/structure_485673edbf7f471a840dbf222bb1dc19.png" class="jop-noMdConv">

1.  L’interface **Prototype** déclare les méthodes de clonage. Dans la majorité des cas, il n’y a qu’une seule méthode `clone`.
    
2.  La classe **Prototype Concret** implémente la méthode de clonage. En plus de copier les données de l’objet original dans le clone, cette méthode peut gérer des cas particuliers du processus de clonage, comme des objets imbriqués, démêler les dépendances récursives, etc.
    
3.  Le **Client** peut produire une copie de n’importe quel objet implémentant l’interface prototype.
    

#### Implémentation du registre de prototypes

<img width="550" height="0" src="../../_resources/structure-prototype-cache_43603c8c5b524486bd04847b.png" class="jop-noMdConv">

1.  Le **Registre de Prototypes** facilite l’accès aux prototypes fréquemment utilisés. Il stocke un ensemble d’objets préconstruits qui ont déjà été copiés. Le registre de prototypes le plus simple est une table de hachage (hash map) `nom → prototype`. Si vous voulez de meilleurs critères de recherche qu’un simple nom, construisez une version plus robuste du registre.

## Pseudo-code

Dans cet exemple, le **Prototype** produit des copies exactes d’objets géométriques, sans coupler le code à leurs classes.

<img width="470" height="0" src="../../_resources/example_69c0d1e1531f4466b2db3f71c2e209f8.png" class="jop-noMdConv">

Cloner un ensemble d’objets qui appartiennent à une hiérarchie de classes.

Toutes les formes implémentent la même interface, et cette dernière fournit une méthode de clonage. Une sous-classe peut appeler la méthode de clonage de son parent avant de copier les valeurs de ses propres attributs dans l’objet.

```
// Prototype de base.
abstract class Shape is
    field X: int
    field Y: int
    field color: string

    // Un constructeur normal.
    constructor Shape() is
        // ...

    // Le constructeur du prototype. Un nouvel objet est
    // initialisé avec les valeurs de l’objet existant.
    constructor Shape(source: Shape) is
        this()
        this.X = source.X
        this.Y = source.Y
        this.color = source.color

    // Le traitement de clonage retourne l’une des sous-classes
    // Forme (Shape)
    abstract method clone():Shape

// Prototype concret. La méthode de clonage crée un nouvel objet
// et le passe au constructeur. Il garde une référence à un
// nouveau clone tant que le constructeur n’a pas terminé. Il
// est donc impossible de se retrouver avec un clone incomplet
// et les résultats du clonage sont toujours fiables.
class Rectangle extends Shape is
    field width: int
    field height: int

    constructor Rectangle(source: Rectangle) is
        // Un appel au constructeur parent est requis pour
        // copier les attributs privés définis dans la classe
        // mère.
        super(source)
        this.width = source.width
        this.height = source.height

    method clone():Shape is
        return new Rectangle(this)

class Circle extends Shape is
    field radius: int

    constructor Circle(source: Circle) is
        super(source)
        this.radius = source.radius

    method clone():Shape is
        return new Circle(this)

// Quelque part dans le code client.
class Application is
    field shapes: array of Shape

    constructor Application() is
        Circle circle = new Circle()
        circle.X = 10
        circle.Y = 10
        circle.radius = 20
        shapes.add(circle)

        Circle anotherCircle = circle.clone()
        shapes.add(anotherCircle)
        // La variable `autreCercle` contient la copie exacte de
        // l’objet `Cercle`.

        Rectangle rectangle = new Rectangle()
        rectangle.width = 10
        rectangle.height = 20
        shapes.add(rectangle)

    method businessLogic() is
        // Le prototype est très pratique, car il vous permet de
        // créer une copie d’un objet en ignorant tout de son
        // type.
        Array shapesCopy = new Array of Shapes.

        // Par exemple, nous ne savons pas exactement quels
        // éléments figurent dans le tableau des formes. Nous
        // savons juste que ce sont tous des formes. Grâce au
        // polymorphisme, lorsque nous appelons la méthode
        // `clone` sur une forme, le programme vérifie sa classe
        // et lance la méthode clone appropriée de cette classe.
        // C’est pour cela que nous fabriquons des clones,
        // plutôt qu’un ensemble d’objets forme.
        foreach (s in shapes) do
            shapesCopy.add(s.clone())

        // Le tableau `CopiesFormes` (shapesCopy) contient les
        // copies exactes du tableau des `formes`.

```

## Possibilités d’application

Utilisez le prototype si vous ne voulez pas que votre code dépende des classes concrètes des objets que vous clonez.

Vous rencontrerez ce cas si votre code manipule des objets obtenus via l’interface d’une application externe. Il arrive que votre code ne puisse pas se baser sur les classes concrètes de ces objets car elles sont inconnues.

Le prototype procure une interface générale au code client et lui permet de travailler avec tous les objets clonables. Cette interface rend le code client indépendant des classes concrètes des objets qu’il clone.

Utilisez ce patron si vous voulez réduire le nombre de sous-classes quand elles ne diffèrent que dans leur manière d’initialiser leurs objets respectifs. Ces sous-classes pourraient avoir été prévues pour créer des objets similaires dans des configurations spécifiques.

Le patron de conception prototype vous permet d’utiliser un ensemble d’objets préconstruits (des prototypes) de différentes manières.

Plutôt que d’instancier une sous-classe qui correspond à une configuration précise, le client peut se contenter de chercher le prototype approprié et de le cloner.

## Mise en œuvre

1.  Créez l’interface du prototype et déclarez-y la méthode `clone`, mais si vous avez une hiérarchie de classes existante, ajoutez juste la méthode à toutes ses classes.
    
2.  Une classe prototype doit définir un autre constructeur capable de prendre un objet de cette classe comme paramètre. Le constructeur doit copier les valeurs des attributs définis dans la classe de l’objet dans l’instance qui vient d’être créée. Si une sous-classe est concernée, vous devez appeler le constructeur du parent pour gérer le clonage de ses attributs privés.
    
    Si le langage de programmation que vous utilisez ne gère pas la surcharge, vous pouvez définir une méthode spéciale pour copier les données de l’objet. Faire tout ceci à l’intérieur du constructeur est plus pratique, car il retourne le résultat directement après avoir appelé l’opérateur `new`.
    
3.  La méthode de clonage ne contient qu’une seule ligne de code : un opérateur `new` avec la version prototype du constructeur. Notez bien que chaque classe doit redéfinir la méthode de clonage et utiliser le nom de sa propre classe avec l’opérateur `new`. Si vous ne le faites pas, la méthode de clonage sera capable de produire des objets de la classe mère.
    
4.  Vous pouvez également créer un registre centralisé des prototypes pour garder un catalogue de prototypes fréquemment utilisés.
    
    Ce registre peut être implémenté avec une classe fabrique, ou mis dans une classe prototype de base avec une méthode statique qui récupère le prototype. Cette méthode ira chercher un prototype en fonction du critère de recherche que le code client passe à la méthode. Ce critère peut être une chaîne de caractères ou un ensemble de paramètres de recherche. Dès que le prototype recherché est trouvé, le registre crée un clone et le retourne au client.
    
    Pour finir, remplacez tous les appels directs aux constructeurs des sous-classes par des appels à la méthode fabrique du registre de prototypes.
    

## Avantages et inconvénients

- Vous pouvez cloner les objets sans les coupler avec leurs classes concrètes.
    
- Vous clonez des prototypes préconstruits et vous pouvez vous débarrasser du code d’initialisation redondant.
    
- Vous pouvez créer des objets complexes plus facilement.
    
- Vous obtenez une alternative à l’héritage pour gérer les modèles de configuration d’objets complexes.
    
- Cloner des objets complexes dotés de références circulaires peut se révéler très difficile.
    

## Liens avec les autres patrons

- La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est souvent utilisée dès le début de la conception (moins compliquée et plus personnalisée grâce aux sous-classes) et évolue vers la [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory), le [Prototype](https://refactoring.guru/fr/design-patterns/prototype), ou le [Monteur](https://refactoring.guru/fr/design-patterns/builder) (ce dernier étant plus flexible, mais plus compliqué).
    
- Les classes [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) sont souvent basées sur un ensemble de [Fabriques](https://refactoring.guru/fr/design-patterns/factory-method), mais vous pouvez également utiliser le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) pour écrire leurs méthodes.
    
- Le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) se révèle utile lorsque vous voulez sauvegarder des copies de [Commandes](https://refactoring.guru/fr/design-patterns/command) dans l’historique.
    
- Les conceptions qui reposent énormément sur le [Composite](https://refactoring.guru/fr/design-patterns/composite) et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) tirent des avantages à utiliser le [Prototype](https://refactoring.guru/fr/design-patterns/prototype). Il vous permet de cloner les structures complexes plutôt que de les reconstruire à partir de rien.
    
- Le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) n’est pas basé sur l’héritage, il n’a donc pas ses désavantages. Mais le *prototype* requiert une initialisation compliquée pour l’objet cloné. La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est basée sur l’héritage, mais n’a pas besoin d’une étape d’initialisation.
    
- Parfois le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) peut venir remplacer le [Mémento](https://refactoring.guru/fr/design-patterns/memento) et proposer une solution plus simple. Cela n’est possible que si l’objet (l’état que vous voulez stocker dans l’historique) est assez direct et ne possède pas de liens vers des ressources externes, ou que les liens sont faciles à recréer.
    
- Les [Fabriques abstraites](https://refactoring.guru/fr/design-patterns/abstract-factory), [Monteurs](https://refactoring.guru/fr/design-patterns/builder) et [Prototypes](https://refactoring.guru/fr/design-patterns/prototype) peuvent tous être implémentés comme des [Singletons](https://refactoring.guru/fr/design-patterns/singleton).
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_c2b09c2022114d5892b15708c638c083.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/java/example "Patrons de conception : Prototype en Java") [<img width="43" height="56" src="../../_resources/csharp_ec7ad2cb9f464827b5d909be3769f4c3.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/csharp/example "Patrons de conception : Prototype en C#")[<img width="43" height="56" src="../../_resources/cpp_1e2c5ca244414ad89f2a6132c93d3f67.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/cpp/example "Patrons de conception : Prototype en C++")[<img width="43" height="56" src="../../_resources/php_c1fe797d36c546b49f1a3aa96f5d29e6.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/php/example "Patrons de conception : Prototype en PHP")[<img width="43" height="56" src="../../_resources/python_7bf6c5526c5e465dae6477efd55ec5c0.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/python/example "Patrons de conception : Prototype en Python")[<img width="43" height="56" src="../../_resources/ruby_bd61ac41674241b59960f6ebf06d0b64.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/ruby/example "Patrons de conception : Prototype en Ruby")[<img width="43" height="56" src="../../_resources/swift_bd68a943b14244a9ae4dcc718b0d817d.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/swift/example "Patrons de conception : Prototype en Swift")[<img width="43" height="56" src="../../_resources/typescript_36277facebfd4ff0b4b37f53b0b712a9.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/typescript/example "Patrons de conception : Prototype en TypeScript")[<img width="43" height="56" src="../../_resources/go_1f3ef7216c0242d5bfb13d223e4d73ca.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/prototype/go/example "Patrons de conception : Prototype en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_9b6d79ff01ae47a9b0c94ef4a3c.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Singleton](https://refactoring.guru/fr/design-patterns/singleton)

#### Retour

[Monteur](https://refactoring.guru/fr/design-patterns/builder)

[<img width="245" height="375" src="../../_resources/web-cover-fr_d757fd6f86f249eaa256070c13526574.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/prototype "English") [Español](https://refactoring.guru/es/design-patterns/prototype "Español") [Français](https://refactoring.guru/fr/design-patterns/prototype "Français") [Polski](https://refactoring.guru/pl/design-patterns/prototype "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/prototype "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/prototype "Русский") [Українська](https://refactoring.guru/uk/design-patterns/prototype "Українська") [中文](https://refactoringguru.cn/design-patterns/prototype "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_2ca4640891ab460392c35b3a5cdd6326.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_d7ab682aec5545b5af3b31577a4ca2e0.png)

- [Contenu premium](https://refactoring.guru/fr/store)
- [Patrons de conception](https://refactoring.guru/fr/design-patterns)
    - [Introduction](https://refactoring.guru/fr/design-patterns/what-is-pattern)
    - [Catalogue](https://refactoring.guru/fr/design-patterns/catalog)
    - [Patrons de création](https://refactoring.guru/fr/design-patterns/creational-patterns)
        - [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method)
        - [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory)
        - [Monteur](https://refactoring.guru/fr/design-patterns/builder)
        - [Prototype](https://refactoring.guru/fr/design-patterns/prototype)
        - [Singleton](https://refactoring.guru/fr/design-patterns/singleton)
    - [Patrons structurels](https://refactoring.guru/fr/design-patterns/structural-patterns)
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

2014-2021 [Refactoring.Guru](https://refactoring.guru/fr). Tous droits réservés.
![Organization address](../../_resources/fa-building_a61ea197b4a94a1e88cda3c8db312289.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)