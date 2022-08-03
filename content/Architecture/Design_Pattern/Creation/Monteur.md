---
title: Monteur / Builder
updated: 2021-12-26 15:27:58Z
created: 2021-12-26 14:01:32Z
source: https://refactoring.guru/fr/design-patterns/builder
tags:
  - création
  - design_pattern
---

**Monteur** permet de construire des objets complexes étape par étape. Il permet de produire différentes variations ou représentations d’un objet en utilisant le même code de construction.

![Patron de conception monteur](../../_resources/builder-fr_10d1ceb31d404af3ae224e04d7fda26d.png)

## Problème

Imaginez un objet complexe qui nécessite une initialisation fastidieuse, composée de plusieurs parties avec de nombreux champs et objets imbriqués. Le code d’initialisation va se retrouver dans un constructeur, enterré sous une pile monstrueuse de paramètres, ou encore pire : réparti un peu partout dans le code client.

![Créer de nombreuses sous-classes entraîne un autre problème](../../_resources/problem1_f320e715ef8046dabe552ca09d117bd0.png)

Créer une sous-classe pour chaque configuration possible d’un objet risque de rendre le programme trop complexe.

Réfléchissons à la manière de modéliser un objet `Maison`. Pour fabriquer une maison de base, vous devez construire quatre murs et un sol, installer une porte, poser quelques fenêtres et bâtir un toit. Mais comment procéder si vous voulez une plus grande maison avec plus de lumière, un peu de terrain et autres commodités (un système de chauffage, de la plomberie et des câbles électriques) ?

La solution la plus simple est d’étendre la classe de base `Maison` et de créer un ensemble de sous-classes pour couvrir toutes les combinaisons de paramètres. Mais au bout d’un certain temps, vous allez vous retrouver avec un nombre considérable de sous-classes. Le moindre paramètre supplémentaire comme le style du porche par exemple, va encore plus développer la hiérarchie.

Voici une autre approche qui n’implique pas de générer des sous-classes : vous pouvez créer un constructeur géant dans la classe de base `Maison` avec tous les paramètres contrôlant l’objet maison. Cette solution élimine le besoin de sous-classes, mais entraîne un autre problème.

<img width="600" height="0" src="../../_resources/problem2_b38c713d6e9f48d0bb8bda43b205cb0c.png" class="jop-noMdConv">

Un constructeur qui possède de nombreux paramètres a ses inconvénients : ces derniers ne sont pas toujours tous utilisés.

Dans la majorité des cas, la plupart des paramètres resteront inutilisés, rendant [l’appel au constructeur assez hideux](https://refactoring.guru/fr/smells/long-parameter-list "Long Parameter List"). Par exemple, le paramètre recensant les piscines se révèle inutile neuf fois sur dix, car peu de maisons en sont équipées.

## Solution

Le patron de conception monteur propose d’extraire le code du constructeur d’objet de sa classe et de le déplacer dans des objets distincts appelés *monteurs*.

<img width="410" height="0" src="../../_resources/solution1_716647eda6ae425687c46e111b7a7325.png" class="jop-noMdConv">

Le patron de conception monteur permet de construire des objets complexes étape par étape. Le monteur empêche les autres objets d’accéder au produit pendant sa construction.

Il organise la construction de l’objet à l’aide d’une série d’étapes (`construireMurs`, `construirePorte`, etc.). Pour créer un objet, vous allez effectuer une séquence d’étapes dans un objet monteur. Le gros avantage, c’est que vous n’avez pas besoin d’appeler toutes les étapes, mais seulement celles nécessaires à la création de la configuration particulière d’un objet.

Certaines étapes de la construction peuvent demander des implémentations variables en fonction des différentes représentations du produit. Par exemple, les murs d’une cabane peuvent être en bois, mais ceux d’un château seront en pierre.

Dans ce cas, vous pouvez créer plusieurs monteurs qui implémentent le même ensemble d’étapes de construction, mais d’une manière différente. Vous pouvez ensuite utiliser ces monteurs dans le processus de construction (c’est-à-dire une succession d’appels ordonnés des étapes) pour créer différents types d’objets.

<img width="600" height="0" src="../../_resources/builder-comic-1-fr_d7817eed29244d6081d463e024f8d16.png" class="jop-noMdConv">

Ces monteurs exécutent la même tâche, mais de manière différente.

Prenons un autre exemple : un premier monteur qui fabrique tout à partir de bois et de verre, un deuxième qui utilise de la pierre et du fer et un troisième qui se sert d’or et de diamants. En appelant les mêmes étapes, vous pouvez construire une maison avec le premier, un petit château avec le deuxième et un palais grâce au troisième. Mais tout ceci ne peut fonctionner que si le code client qui appelle les étapes de la construction peut interagir avec les monteurs via une interface commune.

#### Directeur (Director)

Vous pouvez aller encore plus loin en prenant tous les appels aux étapes utilisées pour construire un produit, et en les mettant dans une classe séparée que l’on nomme *directeur*. La classe directeur va définir l’ordre d’exécution des différentes étapes et le monteur fournit les implémentations de ces étapes.

<img width="343" height="0" src="../../_resources/builder-comic-2-fr_fe469a9751ad4dd58ac757b843c0f58.png" class="jop-noMdConv">

Le directeur connait les étapes à suivre pour construire un produit fonctionnel.

La classe directeur n’est pas obligatoire. Vous pouvez toujours appeler les étapes de construction dans un ordre spécifique depuis le code client. Cependant, la classe directeur se révèle idéale pour y placer les routines de construction et pouvoir les réutiliser ensuite dans votre programme.

De plus, le directeur cache au client les détails de la construction du produit. Le client doit juste associer un monteur avec un directeur, lancer la construction via le directeur, puis récupérer le résultat auprès du monteur.

## Structure

<img width="460" height="0" src="../../_resources/structure_9a9475c8558c45eeaf8daf13b6eab4d3.png" class="jop-noMdConv">

1.  L’interface du **Monteur** déclare les étapes communes de la construction du produit entre tous les monteurs.
    
2.  Les **Monteurs Concrets** fournissent différentes implémentations des étapes de la construction. Ils peuvent créer des produits qui ne reprennent pas l’interface commune.
    
3.  Les **Produits** sont les résultats retournés. Les produits construits par les différents monteurs ne sont pas obligés d’appartenir à la même hiérarchie de classes ni d’avoir la même interface.
    
4.  Le **Directeur** indique l’ordonnancement des étapes de construction et offre la possibilité de créer et de réutiliser des configurations spécifiques pour les produits.
    
5.  Le **Client** doit associer l’un des monteurs au directeur. En général cette association n’est réalisée qu’une seule fois, grâce aux paramètres du constructeur du directeur. Pour toute construction ultérieure, le directeur utilise l’objet monteur. En guise d’alternative, le client peut passer l’objet monteur à la méthode de production du directeur. Dans ce cas, vous pouvez utiliser un monteur différent chaque fois que vous lancez une production avec le directeur.
    

## Pseudo-code

Voici un exemple qui montre comment un **Monteur** peut réutiliser le même code de construction d’objet pour assembler différents types de produits comme des voitures, et créer leurs manuels respectifs.

<img width="500" height="0" src="../../_resources/example-fr_699f945931364654b111784029a1a2fc.png" class="jop-noMdConv">

Un exemple de construction de voitures étape par étape et le manuel d’utilisation qui correspond à leur modèle.

Une voiture est un objet complexe. Elle peut être fabriquée de cent manières différentes. Plutôt que d’encombrer la classe `Voiture` avec un énorme constructeur, nous avons extrait le code dans une classe monteur séparée pour la voiture. Cette classe est composée d’un ensemble de méthodes pour configurer les différentes parties d’une voiture.

Si le code client veut assembler un modèle spécial de voiture bénéficiant d’un réglage de précision, il peut s’adresser directement au monteur. Il peut également déléguer l’assemblage à la classe directeur qui connait le processus de fabrication à indiquer au monteur pour les modèles de voitures les plus populaires.

Ce qui suit va peut-être vous choquer, mais chaque voiture doit posséder son propre manuel (franchement, qui les lit ?). Le manuel décrit toutes les fonctionnalités de la voiture et les détails vont varier selon les modèles. Il semble donc approprié de réutiliser un processus de construction existant pour les voitures et leurs manuels. Bien entendu, la création d’un manuel et d’une voiture sont deux procédés complètement différents et nous devons concevoir des monteurs spécialisés pour les manuels. Cette classe va implémenter les mêmes méthodes de construction que sa cousine assembleuse de voitures, mais au lieu de fabriquer des pièces de voitures, elle se contente de les décrire. Nous pouvons construire une voiture ou un manuel en passant ces monteurs au même objet directeur.

L’étape finale consiste à récupérer l’objet qui en résulte. Même si une voiture en métal et un manuel en papier sont directement liés, ce sont deux choses complètement différentes. Nous ne pouvons pas insérer une méthode pour récupérer les résultats dans le directeur sans le coupler aux classes concrètes du produit. Par conséquent, c’est le monteur qui effectue le travail qui nous donne ensuite le résultat.

```
// L’utilisation du patron de conception monteur n’est
// conseillée que si vos produits sont complexes et nécessitent
// une configuration étendue. Bien qu’ils n’aient pas la même
// interface, les deux produits suivants sont liés.
class Car is
    // Une voiture est équipée d’un GPS, d’un ordinateur de bord
    // et d'un certain nombre de sièges. Les différents modèles
    // de voitures (sport, SUV, cabriolet) ont différentes
    // fonctionnalités installées ou activées.

class Manual is
    // Chaque voiture doit avoir un manuel d’utilisation qui
    // correspond à sa configuration et décrit toutes ses
    // fonctionnalités.

// L’interface du monteur contient des méthodes spécialisées
// pour créer les différentes parties des objets du produit.
interface Builder is
    method reset()
    method setSeats(...)
    method setEngine(...)
    method setTripComputer(...)
    method setGPS(...)

// Les classes concrètes du monteur suivent l’interface monteur
// et procurent des implémentations spécifiques pour les étapes
// de la fabrication. Votre programme peut contenir plusieurs
// variantes de monteurs, chacune avec sa propre implémentation.
class CarBuilder implements Builder is
    private field car:Car

    // Une instance monteur fraichement créée doit contenir un
    // objet produit vide qu’elle va ensuite assembler.
    constructor CarBuilder() is
        this.reset()

    // La méthode reset nettoie l’objet qui est construit.
    method reset() is
        this.car = new Car()

    // Toutes les étapes de la fabrication manipulent la même
    // instance du produit.
    method setSeats(...) is
        // Configure le nombre de sièges dans la voiture.

    method setEngine(...) is
        // Installe un moteur donné.

    method setTripComputer(...) is
        // Installe un ordinateur de bord.

    method setGPS(...) is
        // Installe un système de géolocalisation.

    // Les monteurs concrets sont censés fournir leurs propres
    // méthodes pour récupérer les résultats. Ce fonctionnement
    // permet aux différents types de monteurs de créer des
    // produits entièrement différents qui ne suivent pas la
    // même interface. Par conséquent, les méthodes ne peuvent
    // pas être déclarées dans l’interface du monteur (en tout
    // cas pas dans un langage de programmation doté d’un
    // système de typage statique).
    //
    // En général, après avoir retourné le résultat final au
    // client, une instance de monteur doit être prête à lancer
    // la fabrication d’un autre produit. C’est pour cette
    // raison que l’on appelle généralement la méthode reset à
    // la fin du corps de la méthode `getProduct`. Mais ce
    // comportement n’est pas obligatoire et votre monteur peut
    // attendre que le code client lance un appel explicite à un
    // reset pour vous débarrasser du résultat précédent.
    method getProduct():Car is
        product = this.car
        this.reset()
        return product

// Contrairement aux autres patrons de création, le monteur vous
// permet de fabriquer des produits qui ne suivent pas la même
// interface.
class CarManualBuilder implements Builder is
    private field manual:Manual

    constructor CarManualBuilder() is
        this.reset()

    method reset() is
        this.manual = new Manual()

    method setSeats(...) is
        // Fonctionnalités des sièges dans le manuel.

    method setEngine(...) is
        // Ajoute les informations concernant le moteur.

    method setTripComputer(...) is
        // Ajoute les instructions concernant l’ordinateur de
        // bord.

    method setGPS(...) is
        // Ajoute les instructions concernant le GPS.

    method getProduct():Manual is
        // Retourne le manuel et remet à zéro le monteur
        // (reset).

// Le directeur a pour seule responsabilité l’ordonnancement des
// étapes de la fabrication. Il se révèle utile lorsque vous
// fabriquez des produits avec un ordre précis ou dans une
// configuration particulière. À proprement parler, la classe
// Directeur n’est pas obligatoire, car le client peut manipuler
// les monteurs directement.
class Director is
    private field builder:Builder

    // Le directeur manipule n’importe quelle instance de
    // monteur que le code client lui envoie. Ainsi, le code
    // client peut modifier le type final du produit qui vient
    // d’être assemblé.
    method setBuilder(builder:Builder)
        this.builder = builder

    // Le directeur peut fabriquer plusieurs variantes de
    // produits en utilisant les mêmes étapes de fabrication.
    method constructSportsCar(builder: Builder) is
        builder.reset()
        builder.setSeats(2)
        builder.setEngine(new SportEngine())
        builder.setTripComputer(true)
        builder.setGPS(true)

    method constructSUV(builder: Builder) is
        // ...

// Le code client crée un objet monteur, le passe au directeur
// et lance le processus de fabrication. Le résultat final est
// récupéré dans l’objet monteur.
class Application is

    method makeCar() is
        director = new Director()

        CarBuilder builder = new CarBuilder()
        director.constructSportsCar(builder)
        Car car = builder.getProduct()

        CarManualBuilder builder = new CarManualBuilder()
        director.constructSportsCar(builder)

        // Le produit final est souvent récupéré depuis un objet
        // monteur, car le directeur est indépendant des
        // monteurs et des produits concrets et il ne les voit
        // donc pas.
        Manual manual = builder.getProduct()

```

## Possibilités d’application

Utilisez le patron de conception monteur afin de vous débarrasser d’un « constructeur télescopique ».

Prenons un constructeur avec dix paramètres facultatifs. Faire un appel à cette monstruosité n’est pas très pratique : vous surchargez le constructeur avec plusieurs petites versions, mais avec moins de paramètres. Ces constructeurs font toujours référence au constructeur principal en donnant des valeurs par défaut aux paramètres optionnels.

```
class Pizza {
    Pizza(int size) { ... }
    Pizza(int size, boolean cheese) { ... }
    Pizza(int size, boolean cheese, boolean pepperoni) { ... }
    // ...

```

Cette monstruosité ne peut être créée que dans certains langages qui permettent la surcharge tels que le C# ou le Java.

Le monteur vous permet de créer des objets étape par étape, en utilisant uniquement celles qui sont nécessaires. Après avoir implémenté le patron, vous n’avez plus besoin d’entasser les paramètres dans vos constructeurs.

Utilisez le monteur pour rendre votre code capable de créer différentes représentations de produits (par exemple des maisons en pierre et en bois).

Le monteur est utile lorsque les étapes de la construction des différentes représentations du produit se ressemblent (seuls quelques détails diffèrent).

L’interface de base du monteur définit toutes les étapes possibles de la construction. Les monteurs concrets implémentent les étapes pour construire les représentations particulières du produit. Le directeur quant à lui s’occupe de gérer l’ordonnancement des différentes étapes de construction.

Utilisez le monteur afin de construire une arborescence [composite](https://refactoring.guru/fr/design-patterns/composite "Composite") ou d’autres objets complexes.

Le monteur vous permet de construire des produits étape par étape. Vous pouvez déléguer l’exécution de certaines étapes sans endommager le produit final. Vous pouvez même appeler les étapes de manière récursive, ce qui est très pratique dans le cas de la construction d’un objet composite.

Le monteur ne met pas à disposition le produit tant que les étapes de la construction ne sont pas terminées. Par conséquent, le code client ne récupèrera jamais un résultat incomplet.

## Mise en œuvre

1.  Assurez-vous de définir clairement les étapes communes de la construction pour toutes les représentations de produits disponibles, sinon vous ne pourrez pas mettre en place le patron.
    
2.  Déclarez ces étapes dans l’interface du monteur.
    
3.  Pour chaque représentation du produit, créez une classe concrète monteur puis implémentez ses étapes de construction.
    
    N’oubliez pas de mettre en place une méthode pour récupérer le résultat de la construction. Cette méthode ne peut pas être déclarée dans l’interface du monteur, car les différents monteurs peuvent fabriquer des produits qui n’ont pas forcément une interface commune. Nous ne pouvons pas connaitre à l’avance le type de retour d’une telle méthode. En revanche, si vos produits appartiennent à une hiérarchie unique, la méthode qui récupère le résultat peut être ajoutée à l’interface de base sans problème.
    
4.  Réfléchissez à la création d’une classe directeur. Elle peut encapsuler différents processus de construction d’un produit en utilisant le même objet monteur.
    
5.  Le code client crée les objets monteur et directeur. Le client doit passer un objet monteur au directeur avant le début de la construction. En général, il ne le fait qu’une seule fois, via les paramètres du constructeur du directeur. Le directeur utilise l’objet monteur pour toute construction ultérieure. Nous pourrions également passer le monteur directement à la méthode de construction du directeur.
    
6.  Le résultat de la construction peut être obtenu directement à partir du directeur, si tous les produits sont branchés sur la même interface. Sinon, le client doit aller directement chercher le résultat auprès du monteur.
    

## Avantages et inconvénients

- Vous pouvez construire les objets étape par étape et les déléguer ou les exécuter récursivement.
    
- Vous pouvez réutiliser le même code de construction lorsque vous construisez différentes représentations des produits.
    
- *Principe de responsabilité unique*. Vous pouvez découpler le code complexe de la construction et la logique métier du produit.
    
- Le monteur nécessite de créer beaucoup nouvelles classes, ce qui accroit la complexité générale du code.
    

## Liens avec les autres patrons

- La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est souvent utilisée dès le début de la conception (moins compliquée et plus personnalisée grâce aux sous-classes) et évolue vers la [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory), le [Prototype](https://refactoring.guru/fr/design-patterns/prototype), ou le [Monteur](https://refactoring.guru/fr/design-patterns/builder) (ce dernier étant plus flexible, mais plus compliqué).
    
- Le [Monteur](https://refactoring.guru/fr/design-patterns/builder) se concentre sur la construction d’objets complexes étape par étape. La [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) se spécialise dans la création de familles d’objets associés. La *fabrique abstraite* retourne le produit immédiatement, alors que le *monteur* vous permet de lancer des étapes supplémentaires avant de récupérer le produit.
    
- Vous pouvez utiliser le [Monteur](https://refactoring.guru/fr/design-patterns/builder) lorsque vous créez des arbres [Composites](https://refactoring.guru/fr/design-patterns/composite) complexes, car vous pouvez programmer les étapes de la construction récursivement.
    
- Vous pouvez combiner le [Monteur](https://refactoring.guru/fr/design-patterns/builder) avec le [Pont](https://refactoring.guru/fr/design-patterns/bridge) : la classe directeur joue le rôle de l’abstraction, et les différents *monteurs* prennent le rôle des implémentations.
    
- Les [Fabriques abstraites](https://refactoring.guru/fr/design-patterns/abstract-factory), [Monteurs](https://refactoring.guru/fr/design-patterns/builder) et [Prototypes](https://refactoring.guru/fr/design-patterns/prototype) peuvent tous être implémentés comme des [Singletons](https://refactoring.guru/fr/design-patterns/singleton).
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_0f47e41057bb416f98ac991ccca739a3.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/java/example "Patrons de conception : Monteur en Java") [<img width="43" height="56" src="../../_resources/csharp_3ffe9aca15794347b14c5182c8823c9c.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/csharp/example "Patrons de conception : Monteur en C#")[<img width="43" height="56" src="../../_resources/cpp_2f7c9eea44b146d494964d53541ef647.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/cpp/example "Patrons de conception : Monteur en C++")[<img width="43" height="56" src="../../_resources/php_0a993845178545afacd302d7d4c2e1cd.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/php/example "Patrons de conception : Monteur en PHP")[<img width="43" height="56" src="../../_resources/python_950ae2abcde84b73b9285e3a8ba901b0.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/python/example "Patrons de conception : Monteur en Python")[<img width="43" height="56" src="../../_resources/ruby_500743e86143432880da5a96c180da2b.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/ruby/example "Patrons de conception : Monteur en Ruby")[<img width="43" height="56" src="../../_resources/swift_435bdd3372354fa6aa84d2ec3dfbeb7a.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/swift/example "Patrons de conception : Monteur en Swift")[<img width="43" height="56" src="../../_resources/typescript_d734ace797204596b8111300818f6ef4.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/typescript/example "Patrons de conception : Monteur en TypeScript")[<img width="43" height="56" src="../../_resources/go_56d708ad4eac48aaa2cc604add11e5dc.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/builder/go/example "Patrons de conception : Monteur en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_327d266b6c234d24a7ced48c2a5.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Prototype](https://refactoring.guru/fr/design-patterns/prototype)

#### Retour

[Comparaison des fabriques](https://refactoring.guru/fr/design-patterns/factory-comparison)

[<img width="245" height="375" src="../../_resources/web-cover-fr_b73e9ceda0934a518f4e61197c52c87a.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/builder "English") [Español](https://refactoring.guru/es/design-patterns/builder "Español") [Français](https://refactoring.guru/fr/design-patterns/builder "Français") [Polski](https://refactoring.guru/pl/design-patterns/builder "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/builder "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/builder "Русский") [Українська](https://refactoring.guru/uk/design-patterns/builder "Українська") [中文](https://refactoringguru.cn/design-patterns/builder "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_284aa320ea0e42a89c326a0cd6478238.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_2d47179d59484ad29bc8a628bee27e7a.png)

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
![Organization address](../../_resources/fa-building_ecc8c93279f142c49407d91ef3e63e50.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)