---
title: Fabrique abstraite / Abstract Factory
updated: 2021-12-26 15:27:53Z
created: 2021-12-26 14:01:25Z
source: https://refactoring.guru/fr/design-patterns/abstract-factory
tags:
  - création
  - design_pattern
---

**Fabrique abstraite** permet de créer des familles d’objets apparentés sans préciser leur classe concrète.

![Patron de conception fabrique abstraite](../../_resources/abstract-factory-fr_d627b05adce64634b9de210dc74587.png)

## Problème

Imaginons la création d’un simulateur pour un magasin de meubles. Votre code contient les classes suivantes :

1.  Une famille de produits appartenant à un même thème : `Chaise` \+ `Sofa` \+ `TableBasse`.
    
2.  Plusieurs variantes de cette famille. Par exemple, les produits `Chaise` \+ `Sofa` \+ `TableBasse` sont disponibles dans les variantes suivantes : `Moderne`, `Victorien`, `ArtDéco`.
    

![Familles de produits et leurs variantes](../../_resources/problem-fr_2064ebbba37e4f05818766362d9d1dd9.png)

Familles de produits et leurs variantes.

Vous devez trouver une solution pour créer des objets individuels (des meubles) et les faire correspondre à d’autres objets de la même famille. Les clients sont agacés lorsqu’ils reçoivent des meubles qui ne se marient pas.

<img width="600" height="0" src="../../_resources/abstract-factory-comic-1-fr_420a4a25fc424ff7ba7cd6.png" class="jop-noMdConv">

Un sofa moderne ne se marie pas avec des chaises de style victorien.

De plus, vous n’avez pas envie de réécrire votre code chaque fois que vous ajoutez de nouvelles familles de produits à votre programme. Les vendeurs de meubles alimentent régulièrement leurs catalogues et il n’est pas envisageable de restructurer le code à chaque mise à jour.

## Solution

La première chose que propose la fabrique abstraite est de déclarer explicitement des interfaces pour chaque produit de la famille de produits (dans notre cas : chaise, sofa, table basse). Toutes les autres variantes de produits peuvent ensuite se servir de ces interfaces. Par exemple, toutes les variantes de chaises peuvent implémenter l’interface `Chaise` ; toutes les variantes de tables basses peuvent implémenter `TableBasse`, etc.

<img width="420" height="0" src="../../_resources/solution1_cfcf64e484574e72bc8100689f8f92e2.png" class="jop-noMdConv">

Toutes les variantes du même objet peuvent être rangées dans la même hiérarchie de classe.

La prochaine étape est de déclarer la *fabrique abstraite* — une interface armée d’une liste de méthodes de création pour toutes les familles de produits (par exemple : `créerChaise`, `créerSofa` et `créerTableBasse`). Ces méthodes doivent renvoyer tous les types de produits **abstraits** des interfaces que nous avons créées précédemment : `Chaise`, `Sofa`, `TableBasse`, etc.

<img width="640" height="0" src="../../_resources/solution2_e6b5780abba24a85b2313f973507e5ad.png" class="jop-noMdConv">

Chaque fabrique concrète correspond à une variante d’un produit spécifique.

Mais que deviennent les variantes des produits ? Pour chaque variante d’une famille de produits, nous créons une classe fabrique qui implémente l’interface `FabriqueAbstraite`. Une fabrique est une classe qui retourne un certain type de produits. Par exemple, la `FabriqueMeubleModerne` peut uniquement créer des `ChaiseModerne`, des `SofaModerne` et des `TableBasseModerne`.

Le code client travaille simultanément avec les interfaces abstraites respectives des fabriques et des produits. Nous pouvons ainsi changer le type de fabrique passé au code client et la variante de produit qu’il reçoit, sans avoir à le modifier.

<img width="600" height="0" src="../../_resources/abstract-factory-comic-2-fr_30b3e8fb3eee4b6e86d1fa.png" class="jop-noMdConv">

Le client n’a pas besoin de connaître la classe concrète des fabriques qu’il utilise.

Imaginons un cas où le client désire une fabrique qui peut produire une chaise. Le client n’a pas à se préoccuper de la classe de la fabrique ni du type de chaise qu’il va obtenir. Il doit traiter les chaises de la même manière, que ce soit un modèle de style moderne ou victorien, en utilisant l’interface abstraite `Chaise`. Grâce à cette approche, le client ne sait qu’une seule chose à propos de la chaise, c’est qu’elle implémente la méthode `sasseoir`. De plus, quelle que soit la variante de la chaise renvoyée, elle correspondra systématiquement au type de sofa ou de table basse produit par le même objet fabrique.

Il ne reste plus qu’un point à éclaircir : si le client n’est lié qu’aux interfaces abstraites, comment les objets de la fabrique sont-ils créés ? En général, l’application crée un objet fabrique concret lors de l’initialisation. Mais avant cela, l’application doit choisir le type de la fabrique en fonction de la configuration ou des paramètres d’environnement.

## Structure

<img width="720" height="0" src="../../_resources/structure_70f1dd00d1474d72872306b7ca75bd65.png" class="jop-noMdConv">

1.  Les **Produits Abstraits** déclarent une interface pour un ensemble d’objets distincts mais apparentés, qui forment une famille de produits.
    
2.  Les **Produits Concrets** sont des implémentations des produits abstraits groupés par variantes. Chaque produit abstrait (chaise/sofa) doit être implémenté dans toutes les variantes (victorien/moderne).
    
3.  L’interface **Fabrique Abstraite** déclare un ensemble de méthodes pour créer chacun des produits abstraits.
    
4.  Les **Fabriques Concrètes** implémentent les opérations de création d’objets de la fabrique abstraite. Chaque fabrique concrète correspond à une variante spécifique de produits et ne crée que ces variantes.
    
5.  Bien que les fabriques concrètes instancient des produits concrets, leurs méthodes de création ont une valeur de retour correspondant aux produits *abstraits*. Le code client qui sollicite une fabrique est ainsi isolé de la variante du produit obtenu. Le **client** peut travailler avec n’importe quelle variante de fabrique ou produit, tant qu’il interagit avec les interfaces abstraites.
    

## Pseudo-code

Cet exemple montre comment la **Fabrique abstraite** peut être utilisée pour créer des éléments d’une UI multiplateforme sans coupler le code client avec les classes concrètes de l’UI, tout en gardant une cohérence entre les éléments créés et le système d’exploitation sélectionné.

<img width="640" height="0" src="../../_resources/example_183b183486524de08826bc72544798df.png" class="jop-noMdConv">

Exemple des classes d’une UI multiplateforme

Tous les éléments de l’UI d’une application multiplateforme sont censés avoir un comportement identique quel que soit le système d’exploitation, mais leur apparence peut légèrement varier. Vous devez faire en sorte que les éléments de l’interface correspondent bien au style du système d’exploitation. Vous ne voudriez pas vous retrouver avec des boutons macOS dans Windows.

L’interface de la fabrique abstraite déclare un ensemble de méthodes de création que le code client peut utiliser pour produire différents types d’éléments de l’UI. Chaque fabrique concrète correspond à un système d’exploitation particulier et crée ses propres éléments de l’UI en fonction de ce système d’exploitation.

Le fonctionnement est le suivant : lorsqu’une application est exécutée, elle vérifie le système d’exploitation utilisé et exploite cette information pour créer un objet de la fabrique qui y correspond. Cette fabrique est ensuite utilisée pour générer tout le reste des éléments de l’UI, ce qui évite les erreurs.

Grâce à cette approche, tant qu’il utilise leurs interfaces abstraites, le code client ne dépend plus des classes concrètes de la fabrique et des éléments de l’UI. Cela lui permet également d’exploiter les nouveaux éléments de l’UI ou les fabriques que vous pourriez ajouter dans le futur.

Nous n’avons ainsi plus besoin de modifier le code client à chaque nouvel élément de l’interface utilisateur que vous voulez ajouter dans votre application. Vous devrez simplement créer une nouvelle classe fabrique produisant ces éléments et apporter quelques modifications au code d’initialisation afin qu’il choisisse la classe appropriée.

```
// L’interface de la fabrique abstraite déclare un ensemble de
// méthodes qui retournent différents produits abstraits. Ces
// produits font partie de ce que l’on appelle une famille et
// sont reliés par un concept ou un thème de haut niveau. Les
// produits d’une famille sont généralement capables de
// collaborer les uns avec les autres. Une famille de produits
// peut avoir plusieurs variantes, mais les produits d’une
// variante ne sont pas compatibles avec les produits d’une
// autre.
interface GUIFactory is
    method createButton():Button
    method createCheckbox():Checkbox


// Les fabriques concrètes construisent une famille de produits
// qui appartiennent à une seule variante. La fabrique garantit
// la compatibilité des produits qui en sortent. Les signatures
// des méthodes des fabriques concrètes renvoient un produit
// abstrait, et à l’intérieur de la méthode, un produit concret
// est instancié.
class WinFactory implements GUIFactory is
    method createButton():Button is
        return new WinButton()
    method createCheckbox():Checkbox is
        return new WinCheckbox()

// Chaque fabrique concrète possède une variante de produit
// correspondante.
class MacFactory implements GUIFactory is
    method createButton():Button is
        return new MacButton()
    method createCheckbox():Checkbox is
        return new MacCheckbox()


// Chaque produit d’une famille de produits doit avoir une
// interface de base. Toutes les variantes du produit doivent
// implémenter cette interface.
interface Button is
    method paint()

// Les produits concrets sont créés par les fabriques concrètes
// correspondantes.
class WinButton implements Button is
    method paint() is
        // Affiche un bouton avec le style Windows.

class MacButton implements Button is
    method paint() is
        // Affiche un bouton avec le style macOS.

// Voici l’interface de base d’un autre produit. Tous les
// produits peuvent interagir les uns avec les autres, mais les
// interactions adéquates devraient être implémentées de
// préférence entre les produits d’une même variante concrète.
interface Checkbox is
    method paint()

class WinCheckbox implements Checkbox is
    method paint() is
        // Affiche une case à cocher avec le style Windows.

class MacCheckbox implements Checkbox is
    method paint() is
        // Affiche une case à cocher avec le style macOS.


// Le code client travaille uniquement avec les types abstraits
// des fabriques et des produits : FabriqueGUI, Bouton et
// CaseÀCocher. Ceci vous permet d’envoyer n’importe quelle
// sous-classe de fabrique ou de produit au code client sans
// toucher à son code.
class Application is
    private field factory: GUIFactory
    private field button: Button
    constructor Application(factory: GUIFactory) is
        this.factory = factory
    method createUI() is
        this.button = factory.createButton()
    method paint() is
        button.paint()


// L’application choisit le type de la fabrique en fonction de
// la configuration actuelle ou des paramètres d’environnement,
// et la crée à l’exécution (en général lors de la phase
// d’initialisation).
class ApplicationConfigurator is
    method main() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            factory = new WinFactory()
        else if (config.OS == "Mac") then
            factory = new MacFactory()
        else
            throw new Exception("Error! Unknown operating system.")

        Application app = new Application(factory)

```

## Possibilités d’application

Utilisez la fabrique abstraite si votre code a besoin de manipuler des produits d’un même thème, mais que vous ne voulez pas qu’il dépende des classes concrètes de ces produits — soit vous ne les connaissez pas encore, soit vous voulez juste rendre votre code évolutif.

La fabrique abstraite fournit une interface qui permet de créer des objets pour chaque classe de la famille de produits. Tant que votre code utilise cette interface pour créer ses objets, il prendra systématiquement les bonnes variantes des produits disponibles dans votre application.

- Étudiez la possibilité d’utiliser la fabrique abstraite lorsqu’une classe se retrouve avec un ensemble de patrons [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) qui occultent sa fonction principale.
    
- Dans un programme bien conçu *chaque classe n’a qu’une seule responsabilité*. Lorsqu’une classe gère plusieurs types de produits, déplacer les méthodes fabrique dans des fabriques individuelles ou dans une implémentation à part entière d’une fabrique abstraite est souvent plus pratique.
    

## Mise en œuvre

1.  Établissez une matrice de vos différents types de produits et leurs variantes.
    
2.  Déclarez des interfaces abstraites pour tous vos types de produits. Mettez en place vos produits concrets dans des classes qui implémentent ces interfaces.
    
3.  Déclarez une interface abstraite de la fabrique avec un ensemble de méthodes de création pour tous les produits abstraits.
    
4.  Implémentez une classe fabrique concrète pour chaque variante des produits.
    
5.  Insérez le code de l’initialisation de la fabrique quelque part dans votre application. Il doit instancier une des fabriques concrètes en fonction de la configuration de l’application ou de l’environnement d’exécution. Passez cette fabrique à toutes les classes qui construisent des produits.
    
6.  Parcourez votre code et repérez tous les appels aux constructeurs des produits. Remplacez-les par des appels à la méthode de création appropriée de la fabrique.
    

## Avantages et inconvénients

- Vous êtes assurés que les produits d’une fabrique sont compatibles entre eux.
    
- Vous découplez le code client des produits concrets.
    
- *Principe de responsabilité unique*. Vous pouvez déplacer tout le code de création des produits au même endroit, pour une meilleure maintenabilité.
    
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouvelles variantes de produits sans endommager l’existant.
    
- Le code peut devenir plus complexe que nécessaire, car ce patron de conception impose l’ajout de nouvelles classes et interfaces.
    

## Liens avec les autres patrons

- La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est souvent utilisée dès le début de la conception (moins compliquée et plus personnalisée grâce aux sous-classes) et évolue vers la [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory), le [Prototype](https://refactoring.guru/fr/design-patterns/prototype), ou le [Monteur](https://refactoring.guru/fr/design-patterns/builder) (ce dernier étant plus flexible, mais plus compliqué).
    
- Le [Monteur](https://refactoring.guru/fr/design-patterns/builder) se concentre sur la construction d’objets complexes étape par étape. La [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) se spécialise dans la création de familles d’objets associés. La *fabrique abstraite* retourne le produit immédiatement, alors que le *monteur* vous permet de lancer des étapes supplémentaires avant de récupérer le produit.
    
- Les classes [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) sont souvent basées sur un ensemble de [Fabriques](https://refactoring.guru/fr/design-patterns/factory-method), mais vous pouvez également utiliser le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) pour écrire leurs méthodes.
    
- La [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) peut remplacer la [Façade](https://refactoring.guru/fr/design-patterns/facade) si vous voulez simplement cacher au code client la création des objets du sous-système.
    
- Vous pouvez utiliser la [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) avec le [Pont](https://refactoring.guru/fr/design-patterns/bridge). Ce couple est très utile quand les abstractions définies par le *pont* ne fonctionnent qu’avec certaines implémentations spécifiques. Dans ce cas, la *fabrique abstraite* peut encapsuler ces relations et cacher la complexité au code client.
    
- Les [Fabriques abstraites](https://refactoring.guru/fr/design-patterns/abstract-factory), [Monteurs](https://refactoring.guru/fr/design-patterns/builder) et [Prototypes](https://refactoring.guru/fr/design-patterns/prototype) peuvent tous être implémentés comme des [Singletons](https://refactoring.guru/fr/design-patterns/singleton).
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_6484e190e78d4f02bf2fde813a41c0fc.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/java/example "Patrons de conception : Fabrique abstraite en Java") [<img width="43" height="56" src="../../_resources/csharp_6570c0a1dda74ede82de63f6d81e541f.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/csharp/example "Patrons de conception : Fabrique abstraite en C#")[<img width="43" height="56" src="../../_resources/cpp_020869490e0a4f4a934ca702ef1412b1.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/cpp/example "Patrons de conception : Fabrique abstraite en C++")[<img width="43" height="56" src="../../_resources/php_5b81defad14d4ff8bb0b0951aefc10c8.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/php/example "Patrons de conception : Fabrique abstraite en PHP")[<img width="43" height="56" src="../../_resources/python_e16527be71274dc89282f52e2b7f5764.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/python/example "Patrons de conception : Fabrique abstraite en Python")[<img width="43" height="56" src="../../_resources/ruby_0ea3de41c7af4022bfca5b75c1203aff.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/ruby/example "Patrons de conception : Fabrique abstraite en Ruby")[<img width="43" height="56" src="../../_resources/swift_d90b59204f3a4a7293a77b01fd5569ff.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/swift/example "Patrons de conception : Fabrique abstraite en Swift")[<img width="43" height="56" src="../../_resources/typescript_759668b881a046e7883e80f949b3743b.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/typescript/example "Patrons de conception : Fabrique abstraite en TypeScript")[<img width="43" height="56" src="../../_resources/go_0c0910284e9c46438644811c640cb3c3.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/abstract-factory/go/example "Patrons de conception : Fabrique abstraite en Go")

## Informations supplémentaires

- Lisez notre [Comparaison des fabriques](https://refactoring.guru/fr/design-patterns/factory-comparison) pour en savoir plus sur la différence entre les divers patrons et concepts.

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_9f025e7f78904866ac03dc5e816.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Comparaison des fabriques](https://refactoring.guru/fr/design-patterns/factory-comparison)

#### Retour

[Fabrique](https://refactoring.guru/fr/design-patterns/factory-method)

[<img width="245" height="375" src="../../_resources/web-cover-fr_d607e2ca64e54a30ab7e1e2a93abff3f.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/abstract-factory "English") [Español](https://refactoring.guru/es/design-patterns/abstract-factory "Español") [Français](https://refactoring.guru/fr/design-patterns/abstract-factory "Français") [Polski](https://refactoring.guru/pl/design-patterns/abstract-factory "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/abstract-factory "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/abstract-factory "Русский") [Українська](https://refactoring.guru/uk/design-patterns/abstract-factory "Українська") [中文](https://refactoringguru.cn/design-patterns/abstract-factory "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_c9f9ab409ae24cd19ab33160bea70e0d.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_b3fd1218723f48a68bd81bd9e65cd053.png)

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
![Organization address](../../_resources/fa-building_284223436a504539866b7b608b9330a1.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)