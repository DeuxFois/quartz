---
title: Fabrique / Factory Method
updated: 2021-12-26 16:00:26Z
created: 2021-12-26 14:01:09Z
source: https://refactoring.guru/fr/design-patterns/factory-method
tags:
  - création
  - design_pattern
---

**Fabrique** définit une interface pour créer des objets dans une classe mère, mais délègue le choix des types d’objets à créer aux sous-classes.

![Patron de conception fabrique](../../_resources/factory-method-fr_9c56a2cdc750461b87b42d9376b693b6.png)

## Problème

Imaginez que vous êtes en train de créer une application de gestion logistique. La première version de votre application ne propose que le transport par camion, la majeure partie de votre code est donc située dans la classe `Camion`.

Au bout d’un certain temps, votre application devient populaire et de nombreuses entreprises de transport maritime vous demandent tous les jours d’ajouter la gestion de la logistique maritime dans l’application.

![Ajouter un nouveau moyen de transport au programme peut créer des problèmes](../../_resources/problem1-fr_a873b3fd38d5498d8740634f75bb5234.png)

L’ajout d’une nouvelle classe au programme ne s’avère pas si simple que cela si le reste du code est déjà couplé aux classes existantes.

C’est super, n’est-ce pas ? Mais qu’en est-il du code ? La majeure partie est actuellement couplée à la classe `Camion`. Pour pouvoir ajouter des `Bateaux` dans l’application, il faudrait revoir la base du code. De plus, si vous décidez plus tard d’ajouter un autre type de transport dans l’application, il faudra effectuer à nouveau ces changements.

Par conséquent, vous allez vous retrouver avec du code pas très propre, rempli de conditions qui modifient le comportement du programme en fonction de la classe des objets de transport.

## Solution

Le patron de conception fabrique vous propose de remplacer les appels directs au constructeur de l’objet (à l’aide de l’opérateur `new`) en appelant une méthode *fabrique* spéciale. Pas d’inquiétude, les objets sont toujours créés avec l’opérateur `new`, mais l’appel se fait à l’intérieur de la méthode fabrique. Les objets qu’elle retourne sont souvent appelés *produits*.

<img width="620" height="0" src="../../_resources/solution1_44f9615333de43fc97f87e11a9f1a8cd.png" class="jop-noMdConv">

Les sous-classes peuvent modifier les classes des objets retournés par la méthode fabrique.

À première vue, cette modification peut sembler inutile : nous avons juste déplacé l’appel du constructeur dans une autre partie du programme. Mais maintenant, vous pouvez redéfinir la méthode fabrique dans la sous-classe et changer la classe des produits créés par la méthode.

Il y a tout de même une petite limitation : les sous-classes peuvent retourner des produits différents seulement si les produits ont une classe de base ou une interface commune. De plus, cette interface doit être le type retourné par la méthode fabrique de la classe de base.

<img width="490" height="0" src="../../_resources/solution2-fr_9a42a67615c444a2965ec7d6a7e81bae.png" class="jop-noMdConv">

Les produits doivent tous implémenter la même interface.

Par exemple, les classes `Camion` et `Bateau` doivent toutes les deux implémenter l’interface `Transport`, qui déclare une méthode `livrer`. Chaque classe implémente cette méthode à sa façon : les camions livrent par la route et les bateaux livrent par la mer. La méthode fabrique de la classe `LogistiqueRoute` retourne des camions, alors que celle de la classe `LogistiqueMer` retourne des bateaux.

<img width="640" height="0" src="../../_resources/solution3-fr_3300314049144fdb8f4dc77ef85e2372.png" class="jop-noMdConv">

Tant que les classes produit implémentent une interface commune, vous pouvez passer leurs objets au code client sans tout faire planter.

Le code qui appelle la méthode fabrique (souvent appelé le code *client*) ne fait pas la distinction entre les différents produits concrets retournés par les sous-classes, il les considère tous comme des `Transports` abstraits. Le client sait que tous les objets transportés sont censés avoir une méthode `livrer`, mais son fonctionnement lui importe peu.

## Structure

<img width="660" height="0" src="../../_resources/structure_ea59814269984a5bbd1765404b5273ef.png" class="jop-noMdConv">

1.  L’interface est déclarée par le **Produit** et est commune à tous les objets qui peuvent être conçus par le créateur et ses sous-classes.
    
2.  Les **Produits Concrets** sont différentes implémentations de l’interface produit.
    
3.  La méthode fabrique est déclarée par la classe **Créateur** et retourne les nouveaux produits. Il est important que son type de retour concorde avec l’interface produit.
    
    Vous pouvez rendre la méthode fabrique abstraite afin d’obliger ses sous-classes à implémenter leur propre version de la méthode ou vous pouvez modifier la méthode fabrique de la classe de base afin qu’elle retourne un type de produit par défaut.
    
    Il faut bien comprendre que malgré son nom, la création de produits n’est **pas** la responsabilité principale du créateur. La classe créateur a en général déjà un fonctionnement propre lié à la nature de ses produits. La fabrique aide à découpler cette logique des produits concrets. C’est un peu comme une grande entreprise de développement de logiciels : elle peut posséder un département spécialisé dans la formation des développeurs, mais son activité principale reste d’écrire du code, pas de produire des développeurs.
    
4.  Les **Créateurs Concrets** redéfinissent la méthode fabrique de la classe de base afin de pouvoir retourner les différents types de produits.
    
    Notez toutefois que la méthode fabrique n’est pas obligée de **créer** tout le temps de nouvelles instances. Elle peut retourner des objets depuis un cache, un réservoir d’objets ou une autre source.
    

## Pseudo-code

Cet exemple montre comment la **fabrique** peut être utilisée pour créer des éléments d’une UI (interface utilisateur) multiplateforme sans coupler le code client aux classes concrètes de l’UI.

<img width="640" height="0" src="../../_resources/example_c9ed934b5f524674b583512946494426.png" class="jop-noMdConv">

Exemple multiplateforme pour une boîte de dialogue.

La classe de base dialogue utilise différents éléments d’UI pour le rendu de ses fenêtres. Ces éléments peuvent un peu varier en fonction du système d’exploitation, mais ils doivent garder le même comportement. Un bouton sous Windows est aussi un bouton sous Linux.

Lorsque la fabrique entre dans l’équation, il est inutile de réécrire la logique de la boîte de dialogue pour chaque système d’exploitation. Si nous déclarons une méthode fabrique qui crée des boutons à l’intérieur de la classe de base dialogue, nous pourrons par la suite sous-classer cette dernière afin que la méthode fabrique retourne des boutons dotés du style Windows. La sous-classe hérite alors du code de la classe de base dialogue, mais grâce à la fabrique, elle est capable d’afficher à l’écran des boutons à l’allure Windows.

Pour le bon fonctionnement de ce patron, la classe de base dialogue doit utiliser des boutons abstraits représentés par une classe de base ou une interface dont tous les boutons concrets hériteront. Ainsi, quel que soit le type de boutons, le code de la classe dialogue reste fonctionnel.

Bien sûr, cette approche fonctionne également avec les autres éléments de l’UI. Cependant, chaque nouvelle méthode fabrique ajoutée à Dialogue nous rapproche du patron de conception [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory). Pas de panique, nous aborderons ce patron plus tard !

```
// La classe créateur déclare la méthode fabrique qui doit
// renvoyer un objet de la classe produit. Les sous-classes du
// créateur fournissent en général une implémentation de cette
// méthode.
class Dialog is
    // Le créateur peut également fournir des implémentations
    // par défaut de la méthode fabrique.
    abstract method createButton():Button

    // Ne vous laissez pas berner par son nom, la responsabilité
    // principale du créateur n’est pas de fabriquer des
    // produits. Il héberge en général de la logique métier qui
    // concerne les produits retournés par la méthode fabrique.
    // Les sous-classes peuvent modifier indirectement cette
    // logique métier en redéfinissant la méthode fabrique et en
    // lui faisant retourner un type de produit différent.
    method render() is
        // Appelle la méthode fabrique pour créer un objet
        // Produit.
        Button okButton = createButton()
        // Utilise le produit.
        okButton.onClick(closeDialog)
        okButton.render()

// Les créateurs concrets redéfinissent la méthode fabrique pour
// changer le type du produit qui en résulte.
class WindowsDialog extends Dialog is
    method createButton():Button is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton():Button is
        return new HTMLButton()

// L’interface du produit déclare les traitements que tous les
// produits concrets doivent implémenter.
interface Button is
    method render()
    method onClick(f)

// Les produits concrets fournissent diverses implémentations de
// l’interface du produit.
class WindowsButton implements Button is
    method render(a, b) is
        // Affiche un bouton avec le style Windows.
    method onClick(f) is
        // Attribue un événement sur un clic natif dans un
        // système d’exploitation.

class HTMLButton implements Button is
    method render(a, b) is
        // Retourne une représentation HTML d’un bouton.
    method onClick(f) is
        // Attribue un événement sur un clic dans un navigateur
        // Internet.

class Application is
    field dialog: Dialog

    // L’application choisit un type de créateur en fonction de
    // la configuration actuelle ou des paramètres
    // d’environnement.
    method initialize() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error!  Unknown  operating  system.")

    // Le code client manipule une instance d’un créateur
    // concret, mais uniquement au moyen de son interface de
    // base. Tant que le client continue de passer par cette
    // interface pour manipuler le créateur, vous pouvez passer
    // n’importe quelle sous-classe du créateur.
    method main() is
        this.initialize()
        dialog.render()

```

## Possibilités d’application

Utilisez la fabrique si vous ne connaissez pas à l’avance les types et dépendances précis des objets que vous allez utiliser dans votre code.

La fabrique effectue une séparation entre le code du constructeur et le code qui utilise réellement le produit. Le code du constructeur devient ainsi plus évolutif et indépendant du reste du code.

Par exemple, si vous voulez ajouter un nouveau produit dans l’application, il vous suffit d’ajouter une sous-classe de création et d’y redéfinir la méthode fabrique.

Utilisez la fabrique si vous voulez mettre à disposition une librairie ou un framework pour vos utilisateurs avec un moyen d’étendre ses composants internes.

L’héritage est probablement le moyen le plus simple pour étendre le comportement par défaut d’une librairie ou d’un framework. Mais comment le framework peut-il savoir qu’il doit utiliser votre sous-classe plutôt qu’un composant standard ?

La solution est de réunir dans une seule méthode fabrique le code qui construit les composants dans le framework, et non seulement d’étendre ceux-ci, mais de laisser la possibilité de redéfinir la méthode fabrique.

Voyons un exemple d’utilisation. Imaginez la conception d’une application qui utilise un framework d’UI open source. Vous désirez utiliser des boutons ronds, mais le framework ne fournit que des boutons carrés. Vous étendez le `Bouton` standard avec une magnifique sous-classe `BoutonRond`. Mais vous devez à présent expliquer à la classe principale `UIFramework` qu’elle doit utiliser la sous-classe du nouveau bouton plutôt que celle par défaut.

Pour ce faire, vous créez une sous-classe `UIAvecBoutonsRonds` depuis une classe de base du framework et redéfinissez sa méthode `créerBouton`. Même si la méthode de la classe de base retourne des `Boutons`, votre sous-classe renvoie des `BoutonsRonds`. Vous pouvez dorénavant utiliser `UIAvecBoutonsRonds` à la place de `UIFramework`. Et c’est à peu près tout !

Utilisez la fabrique lorsque vous voulez économiser des ressources système en réutilisant des objets au lieu d’en construire de nouveaux.

Le besoin se présente souvent lorsque l’on utilise des objets qui prennent beaucoup de ressources tels que des bases de données, des systèmes de fichiers ou des ressources réseau.

Que faut-il pour réutiliser un objet existant ?

1.  Tout d’abord, vous devez créer un moyen de stockage afin de garder la trace de tous les objets créés.
2.  Lorsqu’un nouvel objet est demandé, le programme doit chercher un objet libre dans cette réserve.
3.  … et le renvoyer au code client.
4.  Si aucun objet n’est disponible, le programme en crée un nouveau (et l’ajoute à la réserve).

Cela représente un paquet de code ! De plus, il faut tout mettre au même endroit afin de ne pas polluer le code avec des doublons.

Il serait probablement plus pratique de l’écrire dans le constructeur de la classe de l’objet que l’on veut réutiliser, mais par définition, un constructeur doit toujours renvoyer de **nouveaux objets**. Il ne peut pas retourner des instances existantes.

C’est pourquoi vous devez disposer d’une méthode non seulement capable de créer de nouveaux objets, mais aussi de réutiliser ceux qui existent déjà. Cela ressemble énormément à un patron de conception fabrique.

## Mise en œuvre

1.  Implémentez la même interface pour tous les produits. Cette interface doit déclarer des méthodes que tous les produits peuvent avoir en commun.
    
2.  Ajoutez une méthode fabrique vide à l’intérieur de la classe créateur. Le type de retour de la méthode doit correspondre à l’interface commune des produits.
    
3.  Localisez toutes les références aux constructeurs des produits dans le code du créateur. Remplacez-les une par une par des appels à la méthode fabrique et déplacez le code de la création de produits dans la méthode fabrique.
    
    Vous allez peut-être devoir ajouter un paramètre temporaire à la méthode fabrique pour vérifier le type du produit retourné.
    
    À ce stade, le code de la méthode fabrique peut paraître désordonné. Il pourrait même contenir un gros `switch` qui choisit la classe à instancier. Ne vous inquiétez pas, tout va bientôt rentrer dans l’ordre.
    
4.  Pour chaque type de produit listé dans la méthode fabrique, créez une sous-classe de Créateur. Redéfinissez la méthode fabrique dans les sous-classes et récupérez les morceaux de code appropriés de la méthode de base.
    
5.  S’il y a trop de types de produits et peu d’intérêt de créer des sous-classes pour tous, vous pouvez réutiliser le paramètre de contrôle de la classe de base dans les sous-classes.
    
    Imaginons la hiérarchie de classes suivante : la classe de base `Courrier` avec les sous-classes `CourrierAérien` et `CourrierTerrestre` ; les classes de `Transport` sont `Avion`, `Camion` et `Train`. La classe `CourrierAérien` n’utilise que des `Avions` et la classe `CourrierTerrestre` peut utiliser à la fois des `Camions` et des `Trains`. Vous pouvez créer une nouvelle sous-classe (`CourrierFerroviaire` par exemple) pour gérer les deux cas, mais il y a une autre possibilité. Le code client peut passer un argument à la méthode fabrique du `CourrierTerrestre` pour désigner le type de produit qu’elle veut recevoir.
    
6.  Si après tous ces changements la méthode fabrique de base est devenue complètement vide, vous pouvez la rendre abstraite. S’il reste encore quelques lignes, vous pouvez y laisser un comportement par défaut.
    

## Avantages et inconvénients

- Vous désolidarisez le Créateur des produits concrets.
    
- *Principe de responsabilité unique*. Vous pouvez déplacer tout le code de création des produits au même endroit, permettant ainsi une meilleure maintenabilité.
    
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouveaux types de produits dans le programme sans endommager l’existant.
    
- Le code peut devenir plus complexe puisque vous devez introduire de nombreuses sous-classes pour la mise en place du patron. La condition optimale d’intégration du patron dans du code existant se présente lorsque vous avez déjà une hiérarchie existante de classes de création.
    

## Liens avec les autres patrons

- La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est souvent utilisée dès le début de la conception (moins compliquée et plus personnalisée grâce aux sous-classes) et évolue vers la [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory), le [Prototype](https://refactoring.guru/fr/design-patterns/prototype), ou le [Monteur](https://refactoring.guru/fr/design-patterns/builder) (ce dernier étant plus flexible, mais plus compliqué).
    
- Les classes [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) sont souvent basées sur un ensemble de [Fabriques](https://refactoring.guru/fr/design-patterns/factory-method), mais vous pouvez également utiliser le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) pour écrire leurs méthodes.
    
- Vous pouvez utiliser la [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) avec l’[Itérateur](https://refactoring.guru/fr/design-patterns/iterator) pour permettre aux sous-classes des collections de renvoyer différents types d’itérateurs compatibles avec les collections.
    
- Le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) n’est pas basé sur l’héritage, il n’a donc pas ses désavantages. Mais le *prototype* requiert une initialisation compliquée pour l’objet cloné. La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est basée sur l’héritage, mais n’a pas besoin d’une étape d’initialisation.
    
- La [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) est une spécialisation du [Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method). Une *fabrique* peut aussi faire office d’étape dans un grand *patron de méthode*.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_1666917d9f7b4795bbd0828f9a8922d5.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/java/example "Patrons de conception : Fabrique en Java") [<img width="43" height="56" src="../../_resources/csharp_3589127d5ded41e2b3d2f042272d3475.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/csharp/example "Patrons de conception : Fabrique en C#")[<img width="43" height="56" src="../../_resources/cpp_c933f58693144fa2b796247f044dd55c.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/cpp/example "Patrons de conception : Fabrique en C++")[<img width="43" height="56" src="../../_resources/php_ff2e08f607734ec7905a6dac659da652.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/php/example "Patrons de conception : Fabrique en PHP")[<img width="43" height="56" src="../../_resources/python_996d21f6666a412dbd5faeacdcbfbb91.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/python/example "Patrons de conception : Fabrique en Python")[<img width="43" height="56" src="../../_resources/ruby_8fa68619e414414f98c396d6b55ffd1c.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/ruby/example "Patrons de conception : Fabrique en Ruby")[<img width="43" height="56" src="../../_resources/swift_3b34a780eb184f3ab4067d01702cb2db.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/swift/example "Patrons de conception : Fabrique en Swift")[<img width="43" height="56" src="../../_resources/typescript_4fd75f2958254c28a762da282531cfa0.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/typescript/example "Patrons de conception : Fabrique en TypeScript")[<img width="43" height="56" src="../../_resources/go_028710c23fb7445bac4202cd4de706c2.svg" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/factory-method/go/example "Patrons de conception : Fabrique en Go")

## Informations supplémentaires

- Lisez notre [Comparaison des fabriques](https://refactoring.guru/fr/design-patterns/factory-comparison) si vous peinez à saisir la nuance entre les différents concepts et patrons.

[Votre navigateur ne peut pas lire les vidéos HTML.](https://refactoring.guru/fr/design-patterns/book)

### Pourquoi acheter le meilleur ebook sur les patrons de conception ?

- Admettons-le : lire un site Internet n’est pas très amusant.
- Soyez productifs dans les transports en commun.
- Apprenez de nouvelles choses tout en vous reposant sur votre canapé.
- Rempli d’exemples de code très utiles en POO.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory)

#### Retour

[Patrons de création](https://refactoring.guru/fr/design-patterns/creational-patterns)

[<img width="150" height="205" src="../../_resources/web-cover-en_0c1927f6fda54c9087484914aa3182d3.png" class="jop-noMdConv">](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/factory-method "English") [Español](https://refactoring.guru/es/design-patterns/factory-method "Español") [Français](https://refactoring.guru/fr/design-patterns/factory-method "Français") [Polski](https://refactoring.guru/pl/design-patterns/factory-method "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/factory-method "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/factory-method "Русский") [Українська](https://refactoring.guru/uk/design-patterns/factory-method "Українська") [中文](https://refactoringguru.cn/design-patterns/factory-method "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_37ae71ff2d074d38a224e52bdc8ffdb1.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_0b8697bcb474419982bb4e8fb046e6c7.png)

- [Contenu premium](https://refactoring.guru/fr/store)
- [Patrons de conception](https://refactoring.guru/fr/design-patterns)
    - [Introduction](https://refactoring.guru/fr/design-patterns/what-is-pattern)
    - [Catalogue](https://refactoring.guru/fr/design-patterns/catalog)
    - [Patrons de création](https://refactoring.guru/fr/design-patterns/creational-patterns)
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
![Organization address](../../_resources/fa-building_a7c0361655b04fc0812158866baea027.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)