---
title: Mémento / Memento
updated: 2021-12-26 15:28:58Z
created: 2021-12-26 15:17:49Z
source: https://refactoring.guru/fr/design-patterns/memento
tags:
  - behavioral
  - design_pattern
---

**Mémento** permet de sauvegarder et de rétablir l’état précédent d’un objet sans révéler les détails de son implémentation.

![Patron de conception&nbsp;mémento](../../_resources/memento-fr_66e03e2a2fea4a8282a85380144a01fa.png)

## Problème

Imaginez que vous êtes en train de créer un éditeur de texte. En plus de pouvoir écrire du texte, votre éditeur permet de le formater, d’insérer des images alignées, etc.

Au bout d’un moment, vous décidez d’ajouter une fonctionnalité qui permet aux utilisateurs d’annuler une action sur le texte. Cette fonctionnalité est devenue si classique au fil des années, que les utilisateurs s’attendent systématiquement à en disposer. Vous choisissez une approche directe pour la mettre en place. Avant d’effectuer une action, l’application enregistre l’état de tous les objets et les sauvegarde. Plus tard, lorsqu’un utilisateur décide d’annuler une action, l’application récupère la dernière sauvegarde de l’historique et s’en sert pour restaurer l’état de tous les objets.

<img width="470" height="0" src="../../_resources/problem1-fr_fa7f3dd064484673adf792002729e3af.png"/>

Avant d’effectuer une action, l’application prend un instantané (snapshot) du dernier état des objets. Il peut être utilisé plus tard pour rétablir les objets dans leur ancien état.

Prenons un moment pour réfléchir à ces photos. Comment va-t-on s’y prendre pour les créer ? Vous allez probablement devoir parcourir tous les attributs d’un objet et copier leurs valeurs quelque part. Cependant, cela ne fonctionnera que si le contenu de l’objet ne possède pas trop de restrictions. Malheureusement, la plupart des objets ne se laissent pas approcher si facilement et cachent les données importantes dans des attributs privés.

Laissons de côté ce problème pour le moment et considérons que nos objets se comportent en bons hippies : ils préfèrent avoir des relations ouvertes et garder leur état public. Même si cette approche nous permet de produire des instantanés de l’état des objets à volonté, de sérieux problèmes demeurent. Dans le futur, vous pourriez décider de refactoriser certaines classes de l’éditeur, ou d’ajouter ou de supprimer certains attributs. Cela semble facile à première vue, mais vous allez devoir également modifier les classes qui ont la responsabilité de créer la copie de l’état des objets concernés.

<img width="300" height="0" src="../../_resources/problem2-fr_80975a310d0f4d7bb8a8ab114078b84f.png"/>

Comment faire une copie de l’état privé d’un objet ?

Mais ce n’est pas tout ! Regardons un peu du côté des « photos » de l’état de l’éditeur. Quel genre de données contiennent-elles ? On doit au moins avoir accès aux coordonnées du curseur de la souris, à la position de la barre de défilement, etc. Pour prendre une photo, vous devez récupérer ces valeurs et les mettre dans un conteneur.

Vous allez très certainement stocker un paquet de ces conteneurs dans une liste qui représente l’historique. Ces conteneurs seront probablement les objets d’une classe. Cette classe possèdera très peu de méthodes, mais aura beaucoup d’attributs qui répliquent l’état de l’éditeur. Pour permettre à d’autres objets de lire et d’écrire les données d’une photo, vous allez devoir rendre ses attributs publics, ce qui exposerait les états de l’éditeur, qu’ils soient privés ou non. Les autres classes deviendraient dépendantes au moindre changement apporté à la classe photo. Si nous rendons ses attributs et méthodes privés, ces changements ne seraient de toute façon pas répercutés sur les autres classes.

Il semblerait que nous sommes dans une impasse : soit on expose tous les détails d’une classe, ce qui les rend trop fragiles, soit on restreint l’accès à leur état, ce qui empêche de prendre des instantanés. Dispose-t-on d’une autre technique pour implémenter un « annuler » ?

## Solution

Tous les problèmes que nous rencontrons sont provoqués par une mauvaise encapsulation. Certains objets essayent d’en faire plus que ce qu’ils sont supposés faire. Pour récupérer les données dont ils ont besoin pour effectuer une action, ils envahissent l’espace personnel des autres objets, plutôt que de leur demander d’exécuter l’action eux-mêmes.

Le mémento délègue la création des états des photos à leur propriétaire, l’objet *créateur* (originator). Plutôt que d’essayer de copier l’état de l’éditeur depuis « l’extérieur », la classe de l’éditeur de texte peut prendre la photo elle-même, car elle a accès à son propre état.

Ce patron propose de stocker la copie de l’état de l’objet dans un objet spécial appelé *mémento*. Son contenu n’est accessible que pour l’objet qui l’a créé. Les autres objets peuvent communiquer avec les mémentos via une interface limitée qui leur permet de récupérer certaines métadonnées de la photo (date de création, nom de l’action effectuée, etc.), mais pas l’état de l’objet original contenu dans la photo.

<img width="610" height="0" src="../../_resources/solution-fr_e5c079381dc546f4b3d5f0f7d4f60733.png"/>

Le créateur a un accès total au mémento, alors que le gardien ne peut que consulter les métadonnées.

Une telle stratégie vous permet de stocker des mémentos à l’intérieur d’autres objets que l’on appelle *gardiens* (caretakers). Le gardien ne manipule le mémento qu’en passant par l’interface limitée. Il ne peut donc pas modifier les états qui y sont stockés. De son côté, le créateur rétablit les états qu’il désire, car il a accès à tous les attributs du mémento.

Dans l’exemple de notre éditeur de texte, nous pouvons créer une classe historique séparée qui prend le rôle du gardien. La pile de mémentos stockée dans le gardien va grandir chaque fois que l’éditeur va effectuer une action. Vous pouvez même afficher cette pile dans l’interface utilisateur, afin de montrer les dernières opérations effectuées par un utilisateur.

Lorsqu’un utilisateur veut annuler une action, l’historique prend le mémento le plus récent de la pile et l’envoie à l’éditeur en demandant un rollback. Comme l’éditeur possède un accès total au mémento, il change lui-même son état en copiant les valeurs du mémento.

## Structure

#### Implémentation basée sur des classes imbriquées

L’implémentation classique du patron repose sur le principe des classes imbriquées, disponibles dans de nombreux langages de programmation populaires (comme le C++, C# et Java).

<img width="580" height="0" src="../../_resources/structure1_34c2ae97af17429ebb3694f5c21bf5eb.png"/>

1.  La classe **Créateur** (Originator) peut prendre des instantanés de son propre état, et le restaurer à volonté.
    
2.  Le **Mémento** est un objet de valeur qui joue le rôle d’un instantané d’un état du créateur. En général, le mémento n’est pas modifiable et écrit ses données une seule fois dans son constructeur.
    
3.  Le **Gardien** sait « quand » et « pourquoi » il doit photographier l’état du créateur, mais il sait également quand l’état doit être restauré.
    
    Le gardien peut conserver la trace de l’historique du créateur en enregistrant une pile de mémentos. Lorsque le créateur veut revenir dans le passé, le gardien récupère l’élément du haut de la pile et l’envoie à la méthode de restauration du créateur.
    
4.  Dans cette implémentation, la classe mémento est imbriquée à l’intérieur du créateur. Ceci permet au créateur d’accéder aux attributs et méthodes du mémento, même s’ils sont privés. Le gardien quant à lui n’a qu’un accès limité aux attributs et méthodes du mémento : on le laisse entasser les mémentos dans la pile, mais il ne peut pas les modifier.
    

#### Implémentation basée sur une interface intermédiaire

Voici une autre possibilité très pratique pour les langages qui ne gèrent pas les classes imbriquées (oui PHP, je te regarde).

<img width="560" height="0" src="../../_resources/structure2_f623ccee869148db8ac94dba7b451b89.png"/>

1.  En l’absence de classes imbriquées, vous pouvez restreindre l’accès aux attributs du mémento en établissant une convention : les gardiens ne vont pouvoir manipuler un mémento qu’à travers une interface intermédiaire déclarée explicitement. Cette interface ne déclare que des méthodes liées aux métadonnées du mémento.
    
2.  De leur côté, les créateurs peuvent manipuler directement les objets mémento, accéder à leurs attributs et méthodes déclarés dans la classe mémento. L’inconvénient de cette technique est que vous devez déclarer tous les membres du mémento en public.
    

#### Implémentation avec une encapsulation encore plus stricte

Si vous ne voulez vraiment pas que les autres classes accèdent au créateur en passant par le mémento, vous pouvez vous y prendre différemment.

<img width="590" height="0" src="../../_resources/structure3_82aecfba5f5b4323bafec0affac38f07.png"/>

1.  Cette implémentation permet de gérer plusieurs créateurs et plusieurs mémentos. Chaque créateur manipule sa propre classe mémento. Les créateurs et les mémentos n’exposent pas leur état aux autres.
    
2.  Il est désormais explicité que les gardiens ne peuvent pas modifier l’état stocké dans le mémento. De plus, la classe gardien n’est plus couplée avec le créateur, car la méthode de restauration est maintenant définie dans la classe mémento.
    
3.  Chaque mémento devient lié au créateur qui l’a produite. Le créateur s’envoie lui-même et toutes les valeurs de son état au constructeur du mémento. Grâce à l’étroite proximité de ces deux classes, un mémento peut restaurer l’état de son créateur, tant que ce dernier a bien défini les setters appropriés.
    

## Pseudo-code

Cet exemple utilise le patron [Commande](https://refactoring.guru/fr/design-patterns/command) en plus du mémento pour stocker les photos de l’état de l’éditeur de texte complexe, et rétablit un état précédent lorsqu’on le lui demande.

<img width="600" height="0" src="../../_resources/example_5e4ab6e60ec64528baac79ac3f05dd8b.png"/>

Sauvegarder des photos de l’état de l’éditeur de texte.

Les objets commande prennent le rôle de gardiens. Ils vont chercher le mémento de l’éditeur avant de lancer les actions déclenchées par les commandes. Lorsqu’un utilisateur veut annuler l’action la plus récente, l’éditeur peut se servir du mémento stocké dans cette commande pour revenir à son état précédent.

La classe mémento ne déclare aucun attribut public, ni de getters et de setters. Aucun objet ne peut modifier son contenu. Les mémentos sont reliés à l’objet éditeur qui les a créés. Le mémento peut aussi restaurer l’état de l’éditeur qui lui est associé, en passant les données dans les setters de l’objet éditeur. Comme les mémentos sont liés à des objets éditeur spécifiques, votre application peut gérer plusieurs fenêtres avec des éditeurs indépendants et une pile centralisée d’états à rétablir.

```
// Le créateur conserve des données importantes qui varient
// parfois avec le temps. Il définit également une méthode pour
// sauvegarder son état dans un mémento, et une autre méthode
// pour rétablir cet état.
class Editor is
    private field text, curX, curY, selectionWidth

    method setText(text) is
        this.text = text

    method setCursor(x, y) is
        this.curX = x
        this.curY = y

    method setSelectionWidth(width) is
        this.selectionWidth = width

    // Sauvegarde l’état actuel dans un mémento.
    method createSnapshot():Snapshot is
        // Le mémento est un objet non modifiable. C’est pour
        // cela que le créateur passe son état dans les
        // paramètres du constructeur du mémento.
        return new Snapshot(this, text, curX, curY, selectionWidth)

// La classe mémento conserve un ancien état de l’éditeur.
class Snapshot is
    private field editor: Editor
    private field text, curX, curY, selectionWidth

    constructor Snapshot(editor, text, curX, curY, selectionWidth) is
        this.editor = editor
        this.text = text
        this.curX = x
        this.curY = y
        this.selectionWidth = selectionWidth

    // Un objet mémento peut être utilisé pour rétablir un
    // ancien état de l’éditeur.
    method restore() is
        editor.setText(text)
        editor.setCursor(curX, curY)
        editor.setSelectionWidth(selectionWidth)

// Un objet commande peut prendre le rôle du gardien. Dans ce
// cas, un mémento est affecté à la commande juste avant que
// l'état du créateur ne change. Lorsqu’une demande de
// restauration arrive, il rétablit l’état du créateur à partir
// du mémento.
class Command is
    private field backup: Snapshot

    method makeBackup() is
        backup = editor.createSnapshot()

    method undo() is
        if (backup != null)
            backup.restore()
    // ...

```

## Possibilités d’application

Utilisez le mémento si vous voulez prendre des photos de l’état d’un objet afin de pouvoir rétablir un de ses états précédents.

Le mémento vous permet de faire des copies complètes de l’état d’un objet (attributs privés inclus), et les stocke en dehors de l’objet. Ce patron est surtout connu pour l’utilisation de la fonctionnalité annuler, mais est également indispensable pour les transactions (par exemple, pour permettre le rollback d’une opération en erreur).

Utilisez ce patron lorsque vous ne pouvez pas accéder directement aux attributs/getters/setters d’un objet sans enfreindre les principes de l’encapsulation.

Le mémento rend l’objet responsable de lui-même, ce qui sécurise les données de l’état de l’objet. Aucun autre objet ne peut lire les données de la photo, l’objet original est donc complètement sécurisé.

## Mise en œuvre

1.  Déterminez la classe qui jouera le rôle du créateur. Il est important de savoir si le programme va utiliser un seul objet central ou plusieurs petits objets.
    
2.  Créez la classe mémento. Déclarez un à un l’ensemble des attributs qui recopient les attributs de la classe créateur.
    
3.  Rendez la classe mémento non modifiable. Un mémento ne doit écrire ses données qu’une seule fois via son constructeur. Sa classe ne doit pas avoir de setters.
    
4.  Si votre langage de programmation accepte les classes imbriquées, mettez le mémento à l’intérieur du créateur. Sinon, extrayez une interface vide depuis la classe du mémento et obligez les autres objets à utiliser cette interface pour y accéder. Vous pouvez ajouter des opérations sur les métadonnées dans l’interface, mais rien qui ne puisse exposer l’état du créateur.
    
5.  Ajoutez une méthode qui crée des mémentos à la classe créateur. Le créateur doit passer son état dans un ou plusieurs paramètres du constructeur du mémento.
    
    Le type de la valeur de retour de la méthode doit être l’interface extraite dans l’étape précédente (si vous avez suivi cette étape). La méthode qui crée le mémento doit directement manipuler la classe mémento.
    
6.  Écrivez la méthode qui permet de rétablir l’état du créateur dans sa propre classe. Il doit prendre un objet mémento en paramètre. Si vous avez extrait une interface lors de l’une des étapes précédentes, c’est le type de la valeur de retour que cette méthode doit accepter. Dans ce cas, vous devez caster cet objet dans la classe mémento, car le créateur a besoin d’un accès total à cet objet.
    
7.  Le gardien, que ce soit une commande, un historique ou n’importe quoi d’autre, doit savoir quand demander de nouveaux mémentos au créateur, comment les stocker et quand restaurer le créateur à l’aide d’un mémento donné.
    
8.  Le lien entre les gardiens et les créateurs peut être déplacé dans la classe mémento. Dans ce cas, chaque mémento doit être associé à son propre créateur. La méthode de restauration doit également être déplacée dans la classe mémento. Mais faites-le uniquement si la classe mémento est imbriquée dans son créateur, ou si la classe du créateur fournit assez de setters pour redéfinir son état.
    

## Avantages et inconvénients

- Vous pouvez prendre des instantanés de l’état d’un objet tout en respectant son encapsulation.
- Vous pouvez simplifier le code du créateur en laissant le gardien gérer l’historique de l’état du créateur.

- L’application pourrait consommer beaucoup de RAM si les clients créent des mémentos trop souvent.
- Les gardiens doivent garder la trace du cycle de vie des créateurs pour pouvoir détruire les mémentos obsolètes.
- La majorité des langages de programmation dynamiques comme le PHP, Python ou le JavaScript ne peuvent pas garantir que l’état sauvegardé dans le mémento ne sera pas modifié.

## Liens avec les autres patrons

- Vous pouvez utiliser la [Commande](https://refactoring.guru/fr/design-patterns/command) et le [Mémento](https://refactoring.guru/fr/design-patterns/memento) ensemble pour implémenter la fonctionnalité « annuler ». Dans ce cas, les commandes ont la responsabilité d’exécuter les divers traitements sur un objet cible. Les mémentos sauvegardent l’état de cet objet juste avant le lancement de la commande.
    
- Vous pouvez utiliser le [Mémento](https://refactoring.guru/fr/design-patterns/memento) avec l’[Itérateur](https://refactoring.guru/fr/design-patterns/iterator) pour récupérer l’état actuel de l’itération et rétablir sa valeur plus tard si besoin.
    
- Parfois le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) peut venir remplacer le [Mémento](https://refactoring.guru/fr/design-patterns/memento) et proposer une solution plus simple. Cela n’est possible que si l’objet (l’état que vous voulez stocker dans l’historique) est assez direct et ne possède pas de liens vers des ressources externes, ou que les liens sont faciles à recréer.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_bb24a0ec83eb45659ad0ca626065e81a.svg"/>](https://refactoring.guru/fr/design-patterns/memento/java/example "Patrons de conception : Mémento en Java") [<img width="43" height="56" src="../../_resources/csharp_acaaf63c1dc342faaaca6a691718109d.svg"/>](https://refactoring.guru/fr/design-patterns/memento/csharp/example "Patrons de conception : Mémento en C#")[<img width="43" height="56" src="../../_resources/cpp_dbe89c1423f94fc18dfa833ff90c966d.svg"/>](https://refactoring.guru/fr/design-patterns/memento/cpp/example "Patrons de conception : Mémento en C++")[<img width="43" height="56" src="../../_resources/php_36f9f74901434ccfad22ee18aaa66e6c.svg"/>](https://refactoring.guru/fr/design-patterns/memento/php/example "Patrons de conception : Mémento en PHP")[<img width="43" height="56" src="../../_resources/python_d4f6bc1d3e7948f589cec749d357e508.svg"/>](https://refactoring.guru/fr/design-patterns/memento/python/example "Patrons de conception : Mémento en Python")[<img width="43" height="56" src="../../_resources/ruby_c9324f1b778841b3850b5c5f3fa1fd1a.svg"/>](https://refactoring.guru/fr/design-patterns/memento/ruby/example "Patrons de conception : Mémento en Ruby")[<img width="43" height="56" src="../../_resources/swift_f71621aeb3f442ad90fa8e259d0d8082.svg"/>](https://refactoring.guru/fr/design-patterns/memento/swift/example "Patrons de conception : Mémento en Swift")[<img width="43" height="56" src="../../_resources/typescript_354a1917fe32401b8c3f7614927b900e.svg"/>](https://refactoring.guru/fr/design-patterns/memento/typescript/example "Patrons de conception : Mémento en TypeScript")[<img width="43" height="56" src="../../_resources/go_5e5cce54e2034d98909afff545fadb29.svg"/>](https://refactoring.guru/fr/design-patterns/memento/go/example "Patrons de conception : Mémento en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_89ed79455d9440fd824034deb5f.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Observateur](https://refactoring.guru/fr/design-patterns/observer)

#### Retour

[Médiateur](https://refactoring.guru/fr/design-patterns/mediator)

[<img width="245" height="375" src="../../_resources/web-cover-fr_182eb7a47d2a4b7f8e875095be2a48df.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/memento "English") [Español](https://refactoring.guru/es/design-patterns/memento "Español") [Français](https://refactoring.guru/fr/design-patterns/memento "Français") [Polski](https://refactoring.guru/pl/design-patterns/memento "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/memento "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/memento "Русский") [Українська](https://refactoring.guru/uk/design-patterns/memento "Українська") [中文](https://refactoringguru.cn/design-patterns/memento "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_233a9b0746fa4eb5aa4fe979ce2218a7.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_7110bf5a5ba24131870dc90154a62986.png)

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
 ![Organization address](../../_resources/fa-building_92749a2b41304699a390922775e15f9e.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)