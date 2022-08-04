---
title: Visiteur / Visitor
updated: 2021-12-26 15:28:38Z
created: 2021-12-26 15:18:06Z
source: https://refactoring.guru/fr/design-patterns/visitor
tags:
  - behavioral
  - design_pattern
---

**Visiteur** vous permet de séparer les algorithmes et les objets sur lesquels ils opèrent.

![Patron de conception&nbsp;visiteur](../../_resources/visitor_df679eb372cc4b0f8e1e9d6c78e50bf2.png)

## Problème

Imaginez que votre équipe développe une application avec des informations géographiques qui prennent la forme d’un graphe géant. Chaque nœud du graphe peut représenter une entité complexe comme une ville, mais aussi des choses plus spécifiques comme des usines, des sites touristiques, etc. Les nœuds sont interconnectés s’il est possible de les relier. Dans le code, chaque type de nœud est représenté par sa propre classe et chaque nœud spécifique est un objet.

![Export du graphe en XML](../../_resources/problem1_eccc2e36b63e4ef3b40a70a9f6af4d61.png)

Export du graphe en XML.

Le jour vient où vous devez vous attaquer à la mise en place de l’export du graphe au format XML. À première vue, cela semble assez simple. Vous avez prévu de mettre en place une méthode d’export pour chaque classe nœud, puis vous exécutez la méthode sur chaque nœud du graphe en le parcourant récursivement. La solution était simple mais élégante : grâce au polymorphisme, vous n’avez pas couplé le code qui appelait la méthode d’exportation aux classes concrètes des nœuds.

Malheureusement, l’architecte du système vous a interdit de modifier les classes nœud existantes. Sa décision était basée sur le fait que le code était déjà en production et que vos modifications pourraient causer des bugs.

<img width="500" height="0" src="../../_resources/problem2-fr_89e9ccd442f240638b2a5e39c23487c5.png"/>

La méthode d’export XML devait être ajoutée dans toutes les classes nœud, ce qui risquait de compromettre l’intégrité du code de l’application.

De plus, il a remis en question la pertinence de placer le code d’export XML à l’intérieur des nœuds. Le rôle principal de ces classes est de manipuler les données géographiques, ce code serait perçu comme un intrus.

Il a avancé un autre argument contre votre modification. Il semblait très probable qu’une fois cette fonctionnalité en place, une personne du service marketing demande un export dans un format différent ou d’autres trucs bizarres. Cela vous obligerait à modifier une fois de plus ces petites classes fragiles.

## Solution

Le patron de conception visiteur vous propose de placer ce nouveau comportement dans une classe séparée que l’on appelle *visiteur*, plutôt que de l’intégrer dans des classes existantes. L’objet qui devait lancer ce traitement à l’origine est maintenant passé en paramètre des méthodes du visiteur, ce qui permet à la méthode d’avoir accès à toutes les données nécessaires qui se trouvent à l’intérieur de l’objet.

Comment fait-on pour que ce comportement puisse être exécuté sur des objets de différentes classes ? Par exemple, dans le cas de notre export XML, l’implémentation sera probablement légèrement différente pour chaque nœud. La classe visiteur va donc avoir besoin d’un ensemble de méthodes et chacune d’entre elles pourra prendre des paramètres de différents types, comme ce qui suit :

```
class ExportVisitor implements Visitor is
    method doForCity(City c) { ... }
    method doForIndustry(Industry f) { ... }
    method doForSightSeeing(SightSeeing ss) { ... }
    // ...

```

Mais comment allons-nous appeler ces méthodes, surtout celles qui gèrent le graphe complet ? Nous ne pouvons pas utiliser le polymorphisme, car ces méthodes ont différentes signatures. Pour sélectionner une méthode qui peut traiter un objet donné, nous devons vérifier sa classe. On se croirait dans un cauchemar !

```
foreach (Node node in graph)
    if (node instanceof City)
        exportVisitor.doForCity((City) node)
    if (node instanceof Industry)
        exportVisitor.doForIndustry((Industry) node)
    // ...
}

```

Il vous est peut-être venu à l’esprit d’utiliser la surcharge. La surcharge, c’est une technique qui permet de donner le même nom à toutes les méthodes, même si elles n’ont pas des paramètres identiques. Malheureusement, même si l’on suppose que notre langage de programmation nous le permet (le Java et le C# par exemple), cela ne nous sera d’aucune aide. Comme nous ne connaissons pas la classe d’un nœud à l’avance, le mécanisme de la surcharge ne sera pas capable de déterminer la bonne méthode à exécuter. Il ira systématiquement chercher la méthode qui prend un objet de la classe de base `Nœud`.

Heureusement, le patron de conception visiteur résout ce problème. Il utilise une technique appelée [double répartition](https://refactoring.guru/fr/design-patterns/visitor-double-dispatch "Visiteur et double répartition") (double dispatch), qui aide à lancer la bonne méthode sans s’encombrer avec des blocs conditionnels. Plutôt que de laisser le client choisir la version de la méthode à appeler, pourquoi ne déléguons-nous pas la décision aux objets que nous passons en paramètre au visiteur ? Comme les objets connaissent leur propre classe, ils seront plus à même de choisir la méthode adaptée au visiteur. Ils « acceptent » un visiteur et lui indiquent la méthode à exécuter.

```
// Code client
foreach (Node node in graph)
    node.accept(exportVisitor)

// City
class City is
    method accept(Visitor v) is
        v.doForCity(this)
    // ...

// Industry
class Industry is
    method accept(Visitor v) is
        v.doForIndustry(this)
    // ...

```

J’avoue. Nous avons finalement été obligés de toucher aux classes nœud. Mais cette modification est mineure et elle nous permet d’ajouter de nouveaux comportements sans avoir à retoucher le code.

À présent, si nous extrayons une interface pour tous les visiteurs, les nœuds existants vont accepter tous les visiteurs ajoutés dans l’application. Pour ajouter un nouveau comportement aux nœuds, il vous suffit d’implémenter une nouvelle classe visiteur.

## Analogie

<img width="600" height="0" src="../../_resources/visitor-comic-1_b1d8e2d074b04ab1ab50e9bbeed82412.png"/>

Un bon agent d’assurance est toujours prêt à proposer différents contrats pour différentes organisations.

Imaginez un agent d’assurance expérimenté qui veut absolument acquérir de nouveaux clients. Il peut faire du porte-à-porte dans tous les bâtiments d’un quartier et essayer de vendre des assurances à tous ceux qu’il rencontre. En fonction du type d’entreprise qui occupe le bâtiment, il peut proposer des contrats d’assurance adaptés :

- Si c’est un bâtiment résidentiel, il vend des assurances maladie.
- Si c’est une banque, il vend des assurances contre le vol.
- Si c’est un café, il vend des assurances contre les incendies et les inondations.

## Structure

<img width="520" height="0" src="../../_resources/structure-fr_015ddc89ef9a41748b0cc4d127f68aa6.png"/>

1.  L’interface **Visiteur** déclare un ensemble de méthodes de parcours qui peuvent prendre les éléments concrets d’une structure d’objets en paramètre. Ces méthodes peuvent avoir le même nom si le programme est écrit dans un langage qui gère la surcharge, mais le type de ses paramètres sera différent.
    
2.  Chaque **Visiteur Concret** implémente plusieurs versions des mêmes comportements, en fonction des classes des éléments concrets.
    
3.  L’interface **Élément** déclare une méthode qui « accepte » les visiteurs. Cette méthode déclare un paramètre du type de l’interface visiteur.
    
4.  Chaque **Élément Concret** doit implémenter une méthode d’acceptation. Le but de cette méthode est de rediriger l’appel vers la méthode appropriée du visiteur en fonction de la classe de l’élément actuel. Soyez conscient que même si la classe d’un élément de base implémente cette méthode, toutes les sous-classes doivent tout de même la redéfinir et appeler la méthode appropriée sur l’objet visiteur.
    
5.  Le **Client** représente en général une collection ou tout autre objet complexe (par exemple un arbre [Composite](https://refactoring.guru/fr/design-patterns/composite)). En général, les clients n’ont pas de visibilité sur les classes des éléments concrets, car ils manipulent les objets de cette collection via une interface abstraite.
    

## Pseudo-code

Dans cet exemple, le **Visiteur** implémente l’export XML dans la hiérarchie de classes des formes géométriques.

<img width="560" height="0" src="../../_resources/example_bcaf73dcdeb54b8b8320f858d9b4d5aa.png"/>

Exporter différents types d’objets vers le format XML à l’aide d’un objet visiteur.

```
// L’interface élément déclare une méthode `accepter` qui prend
// l’interface de base visiteur en argument.
interface Shape is
    method move(x, y)
    method draw()
    method accept(v: Visitor)

// Chaque classe concrète Élément doit implémenter la méthode
// `accepter` et la faire appeler la méthode du visiteur qui
// correspond à la classe de l’élément.
class Dot implements Shape is
    // ...

    // Vous remarquerez que nous appelons `visiterPoint`, ce qui
    // correspond au nom de la classe actuelle. Ainsi, nous
    // fournissons le nom de la classe de l’élément au visiteur
    // qui le manipule.
    method accept(v: Visitor) is
        v.visitDot(this)

class Circle implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitCircle(this)

class Rectangle implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitRectangle(this)

class CompoundShape implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitCompoundShape(this)


// L’interface visiteur déclare un ensemble de méthodes visiter
// qui correspondent aux classes élément. La signature de la
// méthode visiter permet au visiteur d’identifier la classe
// exacte de l’élément qu’il manipule.
interface Visitor is
    method visitDot(d: Dot)
    method visitCircle(c: Circle)
    method visitRectangle(r: Rectangle)
    method visitCompoundShape(cs: CompoundShape)

// Les visiteurs concrets implémentent plusieurs versions du
// même algorithme. Ce dernier fonctionne avec toutes les
// classes concrètes Élément.
//
// Vous tirez tous les bénéfices du patron de conception
// visiteur lorsque vous l’utilisez avec une structure complexe
// d’objets, comme une arborescence. Dans ce cas, il peut être
// pratique de stocker certains états intermédiaires de
// l’algorithme tout en exécutant les méthodes du visiteur sur
// les différents objets de la structure.
class XMLExportVisitor implements Visitor is
    method visitDot(d: Dot) is
        // Exporte l’ID du point et ses coordonnées.

    method visitCircle(c: Circle) is
        // Exporte l’ID du cercle, les coordonnées de son centre
        // et son rayon.

    method visitRectangle(r: Rectangle) is
        // Exporte l’ID du rectangle, les coordonnées du point
        // supérieur gauche, ainsi que sa largeur et sa
        // longueur.

    method visitCompoundShape(cs: CompoundShape) is
        // Exporte l’ID de la forme, ainsi qu’une liste des ID
        // de ses enfants.


// Le code client peut lancer des traitements du visiteur sur
// n’importe quel ensemble d’éléments sans connaître leurs
// classes concrètes. La méthode accepter envoie un appel au
// traitement approprié de l’objet visiteur.
class Application is
    field allShapes: array of Shapes

    method export() is
        exportVisitor = new XMLExportVisitor()

        foreach (shape in allShapes) do
            shape.accept(exportVisitor)

```

Si vous voulez savoir pourquoi nous avons besoin d’une méthode `accepter` dans cet exemple, vous pouvez consulter mon article [Visiteur et Double répartition](https://refactoring.guru/fr/design-patterns/visitor-double-dispatch "Visiteur et double répartition") qui décrit le sujet en détail.

## Possibilités d’application

Utilisez le visiteur lorsque vous voulez lancer des traitements sur les éléments d’un objet ayant une structure complexe (une arborescence par exemple).

Le patron de conception visiteur vous permet de lancer des traitements sur un ensemble d’objets de différentes classes à l’aide d’un objet visiteur qui implémente une variante d’un même traitement pour chaque classe visée.

Utilisez le visiteur pour nettoyer la logique métier de tous les comportements secondaires.

Ce patron vous permet de spécialiser encore plus les classes principales de votre application, en transférant les autres comportements dans des classes visiteur.

Utilisez le visiteur si un comportement n’est adapté que pour certaines classes d’une hiérarchie de classes, mais pas pour les autres.

Vous pouvez envoyer ce comportement dans une classe visiteur séparée, implémenter seulement les méthodes de visite qui acceptent les objets des classes concernées et laisser le reste vide.

## Mise en œuvre

1.  Déclarez l’interface visiteur avec un ensemble de méthodes « visiter » ; une pour chaque classe d’élément concret qui existe dans le programme.
    
2.  Déclarez l’interface élément. Si vous avez déjà une hiérarchie de classes élément, ajoutez la méthode abstraite « accepter » à la classe de base de la hiérarchie. Cette méthode doit prendre un Visiteur en paramètre.
    
3.  Implémentez les méthodes d’acceptation dans toutes les classes des éléments concrets. Ces méthodes doivent simplement rediriger l’appel vers la méthode de visite de l’objet visiteur qui correspond à la classe de l’élément actuel.
    
4.  Les classes élément doivent uniquement interagir avec les visiteurs via l’interface visiteur. En revanche, les visiteurs doivent avoir la visibilité sur toutes les classes des éléments concrets, qui sont référencés comme les types des paramètres des méthodes de visite.
    
5.  Pour chaque comportement qui ne peut être écrit à l’intérieur de la hiérarchie des éléments, créez une nouvelle classe concrète Visiteur et implémentez toutes les méthodes de visite.
    
    Vous pourriez vous retrouver dans une situation où le visiteur aura besoin d’un accès aux membres privés d’une classe élément. Dans ce cas, vous pouvez rendre ces attributs ou méthodes publics (ce qui ne respecte pas l’encapsulation de l’élément) ou imbriquer la classe visiteur dans la classe de l’élément. Cette dernière possibilité n’est envisageable que si votre langage de programmation gère les classes imbriquées.
    
6.  Le client doit créer des objets visiteur et les passer dans des éléments via des méthodes « accepter ».
    

## Avantages et inconvénients

- *Principe ouvert/fermé*. Vous pouvez ajouter un nouveau comportement qui acceptera les objets de différentes classes sans les modifier.
- *Principe de responsabilité unique*. Vous pouvez déplacer plusieurs versions du même comportement dans une seule classe.
- Un objet visiteur peut accumuler des informations utiles en manipulant différents objets. Cela peut se révéler pratique si vous voulez parcourir une structure complexe d’objets comme un arbre, et lancer le traitement du visiteur sur chaque objet de cette structure.

- Vous devez mettre à jour les visiteurs chaque fois qu’une classe est ajoutée ou retirée de la hiérarchie des éléments.
- Les visiteurs n’ont parfois pas les accès nécessaires aux attributs ou méthodes privés des éléments qu’ils sont censés manipuler.

## Liens avec les autres patrons

- Vous pouvez traiter le [Visiteur](https://refactoring.guru/fr/design-patterns/visitor) comme une version plus puissante du patron de conception [Commande](https://refactoring.guru/fr/design-patterns/command). Ses objets peuvent lancer des traitements sur divers objets dans différentes classes.
    
- Vous pouvez utiliser le [Visiteur](https://refactoring.guru/fr/design-patterns/visitor) pour lancer une opération sur un arbre [Composite](https://refactoring.guru/fr/design-patterns/composite) entier.
    
- Vous pouvez utiliser le [Visiteur](https://refactoring.guru/fr/design-patterns/visitor) avec l’[Itérateur](https://refactoring.guru/fr/design-patterns/iterator) pour parcourir une structure de données complexe et lancer un traitement sur ses éléments, même s’ils ont des classes différentes.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_e0525d7ad18143148b216dffe26863e2.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/java/example "Patrons de conception : Visiteur en Java") [<img width="43" height="56" src="../../_resources/csharp_5048387a95804fa08476c1bdd3e5d4be.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/csharp/example "Patrons de conception : Visiteur en C#")[<img width="43" height="56" src="../../_resources/cpp_d9a26fa3c8ae4f058832f7fbcbcfd2c4.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/cpp/example "Patrons de conception : Visiteur en C++")[<img width="43" height="56" src="../../_resources/php_56d76c08ed054db094717a24792e5f9e.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/php/example "Patrons de conception : Visiteur en PHP")[<img width="43" height="56" src="../../_resources/python_85e37f96fa184bf0a65f64d15251e846.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/python/example "Patrons de conception : Visiteur en Python")[<img width="43" height="56" src="../../_resources/ruby_7932415689b6460599fa9bd24656184e.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/ruby/example "Patrons de conception : Visiteur en Ruby")[<img width="43" height="56" src="../../_resources/swift_141ec61410b942ffb3484e5270248cba.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/swift/example "Patrons de conception : Visiteur en Swift")[<img width="43" height="56" src="../../_resources/typescript_3503798b09f54132b8a53d7e7316c0c2.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/typescript/example "Patrons de conception : Visiteur en TypeScript")[<img width="43" height="56" src="../../_resources/go_fd16a9b0507446f5acdd021abda97fc6.svg"/>](https://refactoring.guru/fr/design-patterns/visitor/go/example "Patrons de conception : Visiteur en Go")

## Informations supplémentaires

- Vous demandez-vous toujours pourquoi vous ne pouvez pas remplacer le patron de conception visiteur par la surcharge ? Lisez mon article [Visiteur et double répartition](https://refactoring.guru/fr/design-patterns/visitor-double-dispatch) pour mieux comprendre ces vilains détails.

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_067889e7a692473bb183585cf31.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Visiteur et double répartition](https://refactoring.guru/fr/design-patterns/visitor-double-dispatch)

#### Retour

[Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method)

[<img width="245" height="375" src="../../_resources/web-cover-fr_f421adc19d7e4ad1bce6e534c3d98019.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/visitor "English") [Español](https://refactoring.guru/es/design-patterns/visitor "Español") [Français](https://refactoring.guru/fr/design-patterns/visitor "Français") [Polski](https://refactoring.guru/pl/design-patterns/visitor "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/visitor "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/visitor "Русский") [Українська](https://refactoring.guru/uk/design-patterns/visitor "Українська") [中文](https://refactoringguru.cn/design-patterns/visitor "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_dc7f44a6e8504378973c79837cbbcd98.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_3ddd85238ad34d2e92fe5a16e7e29b33.png)

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
 ![Organization address](../../_resources/fa-building_4b20b337243d479eb4cd2b39d7f3591a.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)