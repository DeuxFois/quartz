---
title: Poids mouche / Flyweight
updated: 2021-12-26 15:24:26Z
created: 2021-12-26 15:10:25Z
source: https://refactoring.guru/fr/design-patterns/flyweight
tags:
  - design_pattern
  - structurel
---

**Poids mouche** est un patron de conception structurel qui permet de stocker plus d’objets dans la RAM en partageant les états similaires entre de multiples objets, plutôt que de stocker les données dans chaque objet.

![Patron de conception poids&nbsp;mouche](../../_resources/flyweight_c8bc40aa58304950869a45a83a3e9bc0.png)

## Problème

Pour vous détendre après de longues heures de travail, vous décidez de créer un jeu vidéo tout simple dans lequel les joueurs peuvent se déplacer sur une carte et se tirer dessus. Vous choisissez de mettre en place un système réaliste de particules et d’en faire l’une des caractéristiques les plus importantes du jeu. De grandes quantités de balles, missiles et shrapnels projetés par des explosions vont être envoyés dans toutes les directions et offrir au joueur une expérience palpitante.

Une fois la programmation terminée, vous lancez le dernier commit, assemblez le jeu, puis l’envoyez à votre ami afin qu’il le teste. Le jeu fonctionnait parfaitement sur votre machine, mais votre ami n’a pas pu jouer bien longtemps : son ordinateur plante systématiquement au bout de quelques minutes. Après plusieurs heures à écumer les logs de debug, vous découvrez que le plantage survient à cause d’une quantité de RAM insuffisante. Il semblerait que la machine de votre ami soit bien moins puissante que la vôtre, le problème se produit donc rapidement sur sa machine.

Il semblerait que le problème soit causé par votre système de particules. Chaque particule (une balle, un missile ou un morceau de shrapnel) est représentée par un objet séparé qui contient de nombreuses données. Lorsque l’action bat son plein sur l’écran du joueur, les nouvelles particules ne peuvent plus tenir dans la RAM et le programme plante.

<img width="640" height="0" src="../../_resources/problem-fr_0b9ea17f0f8344aabe3d509afc15caa3.png"/>

## Solution

En inspectant la classe `Particule` de plus près, vous pourrez remarquer que les attributs de couleur et de sprite consomment beaucoup plus de mémoire que les autres attributs. Le pire est que ces deux attributs stockent des données quasi identiques dans toutes les particules. Par exemple, toutes les balles ont la même couleur et le même sprite.

<img width="640" height="0" src="../../_resources/solution1-fr_f748516ecfb243a79ec0b94ef327c240.png"/>

Certaines parties de l’état d’une particule, comme les coordonnées, le mouvement vectoriel et la vitesse sont uniques pour chaque particule. Après tout, les valeurs de ces attributs changent constamment. Ces données représentent le contexte évolutif de la particule, alors que la couleur et le sprite restent constants.

Les données constantes d’un objet forment ce qu’on appelle en général l’*état intrinsèque*. Elles vivent à l’intérieur de l’objet ; les autres objets peuvent seulement les lire, mais pas les modifier. Le reste des données de l’objet sont souvent modifiées « depuis l’extérieur » par d’autres objets et constituent l’*état extrinsèque*.

Le patron de conception poids mouche propose de ne plus stocker tous les états extrinsèques à l’intérieur de l’objet. À la place, vous devriez passer cet état à des méthodes spécialisées qui en dépendent. Seul l’état intrinsèque va rester à l’intérieur de l’objet, vous permettant de le réutiliser dans différents contextes. Vous n’aurez besoin que de quelques-uns de ces objets, car ils diffèrent uniquement dans l’état intrinsèque. Ce dernier possède bien moins de variantes que l’état extrinsèque.

<img width="424" height="0" src="../../_resources/solution3-fr_5c42624730354fae914929dc2d7f27a1.png"/>

Revenons à notre jeu. Si nous extrayons l’état extrinsèque de notre classe particule, trois objets seraient suffisants pour représenter toutes les particules du jeu : une balle, un missile et un morceau de shrapnel. Vous l’avez probablement déjà deviné, un objet qui ne stocke que l’état intrinsèque est appelé poids mouche.

#### Stockage d’état extrinsèque

Où allons-nous pouvoir mettre l’état extrinsèque ? Il faut bien qu’une classe l’accueille, non ? En général, il se retrouve dans l’objet du contenant, celui qui agrège les objets avant la mise en place du patron.

Dans notre cas, c’est l’objet principal `Jeu` qui stocke toutes les particules dans l’attribut `particules`. Pour déplacer l’état extrinsèque dans cette classe, vous devez créer plusieurs attributs de type tableau pour stocker les coordonnées, les vecteurs et la vitesse de chaque particule. Mais ce n’est pas tout. Vous allez devoir utiliser un autre tableau pour stocker les références à un poids mouche spécifique qui représente la particule. Ces tableaux doivent être synchronisés afin de pouvoir accéder à toutes les données d’une particule en utilisant le même index.

<img width="640" height="0" src="../../_resources/solution2-fr_97d4b69a189648b0965adfdcd92d7eb0.png"/>

Il existe une solution plus élégante qui consiste à créer une classe contexte séparée, qui va stocker l’état extrinsèque avec une référence à l’objet poids mouche. Cette approche ne nécessite qu’un seul tableau dans la classe du contenant.

Attendez une minute ! Au tout début, il y avait de nombreux objets contextuels, en avons-nous encore besoin ? Techniquement, oui. Mais ces objets sont bien plus petits qu’avant. Les attributs qui demandent le plus de mémoire ont été déplacés dans quelques objets poids mouche. À présent, des milliers de petits objets contextuels vont pouvoir réutiliser un seul gros objet poids mouche, plutôt que de stocker des milliers de copies de ses données.

#### Poids mouche et objet immuable

Étant donné que le même objet poids mouche peut être utilisé dans différents contextes, vous devez vous assurer que son état ne peut pas être modifié. Un poids mouche doit initialiser son état une seule fois grâce aux paramètres de son constructeur. Il ne doit pas laisser les autres objets accéder à quelque setter ou attribut public que ce soit.

#### Fabrique poids mouche

Pour accéder plus facilement aux différents poids mouches, vous pouvez créer une méthode fabrique qui gère une réserve d’objets poids mouche existants. La méthode prend en paramètre l’état intrinsèque du poids mouche demandé par le client, cherche s’il en existe un qui correspond à cet état, et le retourne s’il le trouve. Sinon, il crée un nouveau poids mouche et l’ajoute à la réserve.

Cette méthode peut être placée à différents endroits. Il parait logique de l’écrire dans le contenant du poids mouche, mais vous pouvez aussi bien créer une nouvelle classe fabrique. Vous pouvez également rendre la méthode fabrique statique et la mettre dans une classe poids mouche.

## Structure

<img width="640" height="0" src="../../_resources/structure_1ef8e0366e1342d4b7a672c884e0fed5.png"/>

1.  Le patron de conception poids mouche ne permet que de faire de l’optimisation. Avant de le mettre en place, assurez-vous que votre programme souffre bien d’un problème de RAM insuffisante lié à un nombre trop important d’objets similaires en mémoire, et que d’autres solutions ne sont pas envisageables.
    
2.  Le **Poids mouche** contient la part de l’état de l’objet original qui peut être partagée entre plusieurs objets. Le même objet poids mouche peut être utilisé dans de nombreux contextes. L’état stocké à l’intérieur d’un poids mouche est *intrinsèque*. L’état passé aux méthodes du poids mouche est *extrinsèque*.
    
3.  La classe **Contexte** détient l’état extrinsèque, qui est unique dans chaque objet original. Lorsqu’un contexte est relié avec un objet poids mouche, il est identique à l’objet original.
    
4.  En général, le comportement de l’objet original reste dans la classe poids mouche. Dans ce cas, tout appel à une méthode du poids mouche doit également passer en paramètre les parties de l’état extrinsèque appropriées. Vous pouvez déplacer le comportement dans la classe contexte, ce qui permet ensuite de manipuler le poids mouche associé comme n’importe quelles données d’un objet.
    
5.  Le **Client** calcule et stocke l’état extrinsèque des poids mouches. Du point de vue du client, un poids mouche est un objet modèle qui peut être configuré à l’exécution en passant des données contextuelles en paramètre de ses méthodes.
    
6.  La **Fabrique du Poids mouche** gère une réserve de poids mouche existants. Si une fabrique est présente, les clients ne créent pas de poids mouche directement. À la place, ils appellent la fabrique et lui passent les morceaux de l’état intrinsèque du poids mouche désiré. La fabrique parcourt les poids mouches existants et en retourne un s’il correspond aux critères de recherche. Elle en crée un nouveau si elle n’en a pas trouvé.
    

## Pseudo-code

Dans l’exemple suivant, le **Poids mouche** diminue l’utilisation de la mémoire lorsqu’il affiche les millions d’arbres d’un canevas.

<img width="540" height="0" src="../../_resources/example_53b08473d2854a5aac2e6ae5655a2331.png"/>

Le patron récupère l’état intrinsèque qui se répète dans une classe principale `Arbre` et la stocke dans une classe `TypeArbre`.

À présent, on ne stocke plus les mêmes données dans différents objets. On les met dans quelques poids mouche et on les associe avec les `Arbres` correspondants qui jouent le rôle du contexte. Le code client crée de nouveaux objets arbre en utilisant la fabrique du poids mouche. Cette dernière encapsule la complexité de la recherche du bon objet et le réutilise si possible.

```
// La classe poids mouche contient une partie de l'état d’un
// arbre. Ces attributs stockent des valeurs uniques pour chaque
// arbre. Par exemple, vous n’y trouverez pas les coordonnées de
// l’arbre, mais plutôt les textures et les couleurs partagées
// entre plusieurs arbres. Comme ces données sont souvent très
// GROSSES, vous gâcheriez beaucoup de mémoire en stockant
// chacune d’entre elles dans chaque arbre. À la place, nous
// pouvons extraire la texture, la couleur et d’autres données
// redondantes dans un objet séparé que de nombreux arbres vont
// référencer.
class TreeType is
    field name
    field color
    field texture
    constructor TreeType(name, color, texture) { ... }
    method draw(canvas, x, y) is
        // 1. Crée un bitmap d’une couleur, structure et d’un
        // type donnés.
        // 2. Dessine le bitmap sur le canevas aux coordonnées X
        // et Y.

// La fabrique de poids mouche décide si elle veut réutiliser un
// poids mouche existant ou si elle veut en créer un nouveau.
class TreeFactory is
    static field treeTypes: collection of tree types
    static method getTreeType(name, color, texture) is
        type = treeTypes.find(name, color, texture)
        if (type == null)
            type = new TreeType(name, color, texture)
            treeTypes.add(type)
        return type

// L’objet contextuel contient la partie extrinsèque de l’état
// de l’arbre. Une application peut en créer des milliards, car
// ils sont petits : ils sont seulement constitués de deux
// coordonnées entières et d’un attribut pour une référence.
class Tree is
    field x,y
    field type: TreeType
    constructor Tree(x, y, type) { ... }
    method draw(canvas) is
        type.draw(canvas, this.x, this.y)

// Les classes arbre et Forêt sont les clients du poids mouche.
// Vous pouvez les fusionner si vous n’avez pas l’intention
// d’ajouter d’autres évolutions à la classe arbre.
class Forest is
    field trees: collection of Trees

    method plantTree(x, y, name, color, texture) is
        type = TreeFactory.getTreeType(name, color, texture)
        tree = new Tree(x, y, type)
        trees.add(tree)

    method draw(canvas) is
        foreach (tree in trees) do
            tree.draw(canvas)

```

## Possibilités d’application

Utilisez le poids mouche uniquement si votre programme génère de nombreux objets et consomme trop de RAM.

Les bénéfices apportés par ce patron varient énormément en fonction de son utilisation et de son contexte. Il est surtout utile dans les cas suivants :

- Votre application demande un grand nombre d’objets similaires.
- La RAM de l’appareil ciblé est saturée.
- Les objets contiennent des états identiques qui peuvent être extraits et partagés entre plusieurs objets.

## Mise en œuvre

1.  Divisez les attributs d’une classe en deux parties pour les transformer en poids mouche :
    
    - L’état intrinsèque : les attributs qui contiennent des données invariables, dupliquées dans de nombreux objets.
    - L’état extrinsèque : les attributs qui contiennent des données contextuelles uniques à chaque objet.
2.  Laissez les attributs de l’état intrinsèque dans la classe, mais empêchez leur modification. Leur valeur initiale ne doit être modifiable qu’à l’intérieur de leur constructeur.
    
3.  Parcourez toutes les méthodes qui utilisent les attributs de l’état extrinsèque. Pour chacun de ces attributs, introduisez un nouveau paramètre que vous utiliserez à la place.
    
4.  Il est envisageable de créer une fabrique pour gérer une réserve de poids mouches. Elle devra vérifier l’existence du poids mouche avant d’en créer un nouveau. Une fois que la fabrique est en place, les clients doivent obligatoirement passer par elle pour récupérer des poids mouches. Ils doivent donner une description du poids mouche désiré en passant leur état intrinsèque à la fabrique.
    
5.  Le client doit stocker ou calculer les valeurs de l’état extrinsèque (contexte) pour appeler des méthodes du poids mouche. Il est parfois plus pratique de déplacer l’état extrinsèque et l’attribut qui référence le poids mouche dans une classe contexte séparée.
    

## Avantages et inconvénients

- Vous économisez beaucoup de RAM si votre programme est noyé par des tonnes d’objets similaires.

- Vous allez gagner en RAM mais perdre en cycles microprocesseur (CPU) si les données du contexte sont recalculées lors de chaque appel d’une méthode poids mouche.
- Le code devient beaucoup plus compliqué. Les développeurs qui rejoignent l’équipe vont se demander pourquoi les états d’une entité ont été séparés de cette manière.

## Liens avec les autres patrons

- Vous pouvez transformer des nœuds de feuilles de l’arbre [Composite](https://refactoring.guru/fr/design-patterns/composite) en [Poids mouches](https://refactoring.guru/fr/design-patterns/flyweight) et les partager pour économiser de la RAM.
    
- Le [Poids mouche](https://refactoring.guru/fr/design-patterns/flyweight) vous montre comment créer plein de petits objets alors que la [Façade](https://refactoring.guru/fr/design-patterns/facade) permet de créer un seul objet qui représente un sous-système complet.
    
- Le [Poids mouche](https://refactoring.guru/fr/design-patterns/flyweight) ressemble au [Singleton](https://refactoring.guru/fr/design-patterns/singleton) si vous parvenez à compiler tous les états partagés des objets en un seul objet poids mouche. Mais ces patrons de conception ont deux différences fondamentales :
    
    1.  Il ne devrait y avoir qu’une seule instance de singleton, mais une classe *poids mouche* peut avoir plusieurs instances avec différents états intrinsèques.
    2.  L’objet *singleton* peut être modifiable alors que les objets poids mouche ne sont pas modifiables.

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_e9d858a84b194c2a9a83a8995e761d29.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/java/example "Patrons de conception : Poids mouche en Java") [<img width="43" height="56" src="../../_resources/csharp_d3af850393eb4de3897ef5aad8b08946.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/csharp/example "Patrons de conception : Poids mouche en C#")[<img width="43" height="56" src="../../_resources/cpp_b3a300d3955a4bbe8b3636ecdf3a6416.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/cpp/example "Patrons de conception : Poids mouche en C++")[<img width="43" height="56" src="../../_resources/php_ca79b324daa84ed0b1e30067d7a13a61.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/php/example "Patrons de conception : Poids mouche en PHP")[<img width="43" height="56" src="../../_resources/python_3fce858f863d4569be2413296b7e0ecc.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/python/example "Patrons de conception : Poids mouche en Python")[<img width="43" height="56" src="../../_resources/ruby_31dce1f90c8e4fad86f9f1a7b7f37cb6.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/ruby/example "Patrons de conception : Poids mouche en Ruby")[<img width="43" height="56" src="../../_resources/swift_4594ce3315dc43ceb971e607b9678246.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/swift/example "Patrons de conception : Poids mouche en Swift")[<img width="43" height="56" src="../../_resources/typescript_244579f79b394543ba3a3893c4ee3f2b.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/typescript/example "Patrons de conception : Poids mouche en TypeScript")[<img width="43" height="56" src="../../_resources/go_614216b9b393425c939bdd24b565ad11.svg"/>](https://refactoring.guru/fr/design-patterns/flyweight/go/example "Patrons de conception : Poids mouche en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_b5944dfa764a447ba0c5ae013d4.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Procuration](https://refactoring.guru/fr/design-patterns/proxy)

#### Retour

[Façade](https://refactoring.guru/fr/design-patterns/facade)

[<img width="245" height="375" src="../../_resources/web-cover-fr_1dbea5775c2d4136859b71d03e0475a8.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/flyweight "English") [Español](https://refactoring.guru/es/design-patterns/flyweight "Español") [Français](https://refactoring.guru/fr/design-patterns/flyweight "Français") [Polski](https://refactoring.guru/pl/design-patterns/flyweight "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/flyweight "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/flyweight "Русский") [Українська](https://refactoring.guru/uk/design-patterns/flyweight "Українська") [中文](https://refactoringguru.cn/design-patterns/flyweight "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_3c1a9fd9bb684a378f2ea3f39441e90b.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_43d568f378c24f2ea6adbb5483ce8b91.png)

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

2014-2021 [Refactoring.Guru](https://refactoring.guru/fr). Tous droits réservés.
 ![Organization address](../../_resources/fa-building_75107e158d754e5488a09c5cf96a2c13.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)