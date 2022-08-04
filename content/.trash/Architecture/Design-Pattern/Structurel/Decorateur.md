---
title: Décorateur / Decorator
updated: 2021-12-26 15:28:29Z
created: 2021-12-26 15:10:17Z
source: https://refactoring.guru/fr/design-patterns/decorator
tags:
  - design_pattern
  - structurel
---

**Décorateur** permet d’affecter dynamiquement de nouveaux comportements à des objets en les plaçant dans des emballeurs qui implémentent ces comportements.

![Patron de conception&nbsp;décorateur](../../_resources/decorator_c5817e96250547319197fdb249dc05f0.png)

## Problème

Imaginez que vous travaillez sur une librairie qui permet aux programmes d’envoyer des notifications à leurs utilisateurs lorsque des événements importants se produisent.

La version initiale de la librairie était basée sur la classe `Notificateur` qui n’avait que quelques attributs, un constructeur et une unique méthode `envoyer`. La méthode pouvait prendre un message en paramètre et l’envoyait à une liste d’e-mails à l’aide du constructeur du notificateur. Une application externe qui jouait le rôle du client devait créer et configurer l’objet notificateur une première fois, puis l’utiliser lorsqu’un événement important se produisait.

<img width="540" height="0" src="../../_resources/problem1-fr_e2b56b5e3e5e48fca80dd93c589daf38.png"/>

Un programme peut utiliser la classe notificateur afin d’envoyer des notifications au sujet d’événements importants à une liste prédéfinie d’adresses e-mail.

Au bout d’un certain temps, vous vous rendez compte que les utilisateurs de la librairie veulent plus que les notifications qu’ils reçoivent sur leur boîte mail. Ils sont nombreux à vouloir recevoir des SMS lorsque leurs applications rencontrent des problèmes critiques. Certains voudraient être prévenus sur Facebook et les employés de certaines entreprises adoreraient recevoir des notifications Slack.

<img width="440" height="0" src="../../_resources/problem2_2c28e16ffe874ed3bee4895a0eff7706.png"/>

Chaque type de notification est implémenté dans une sous-classe du notificateur.

Cela n’a pas l’air bien difficile ! Vous avez étendu la classe `Notificateur` et ajouté des méthodes de notification supplémentaires dans de nouvelles sous-classes. Le client devait instancier la classe de notification désirée et utiliser cette instance pour toutes les autres notifications, mais quelqu’un a remis en question ce fonctionnement en vous affirmant qu’il aimerait bien utiliser plusieurs types de notifications simultanément. Si votre maison prend feu, vous avez très certainement envie d’en être informé par tous les moyens possibles.

Vous avez tenté de résoudre ce problème en créant des sous-classes spéciales qui combinent plusieurs méthodes de notification dans une seule classe. Mais il s’avère que cette approche va non seulement gonfler le code de la librairie, mais aussi celui du client.

<img width="630" height="0" src="../../_resources/problem3_3c2bd6bf9ffc4432ac07aedd4ba223d2.png"/>

Explosion combinatoire des sous-classes.

Vous devez structurer vos classes de notification différemment pour ne pas trop les multiplier, sinon vous allez vous retrouver dans le livre Guinness des records.

## Solution

La première chose qui nous vient à l’esprit lorsque l’on veut modifier le comportement d’un objet, c’est d’étendre sa classe. Cependant, voici quelques mises en garde concernant l’héritage :

- L’héritage est statique. Vous ne pouvez pas modifier le comportement d’un objet au moment de l’exécution. Vous ne pouvez que remplacer la totalité de l’objet par un autre, généré grâce à une sous-classe différente.
- Les sous-classes ne peuvent avoir qu’un seul parent. Dans la majorité des langages de programmation, vous ne pouvez hériter que d’une seule classe à la fois.

Une des solutions pour contourner ce problème consiste à utiliser l’*aggrégation* ou la *composition*  à la place de l’*héritage*. Ces alternatives fonctionnent à peu près de la même façon : un objet *possède* une référence à un autre objet et lui délègue une partie de son travail. Avec l’héritage, l’objet *est* capable de faire le travail tout seul en héritant du comportement de sa classe mère.

Grâce à cette nouvelle approche, il est facile de remplacer l’objet référencé par un autre, ce qui modifie le comportement du conteneur au moment de l’exécution. Un objet peut utiliser le comportement de diverses classes, posséder des références à de nombreux objets et leur déléguer toutes sortes de tâches. L’agrégation et la composition sont des principes clés dans le décorateur, tout comme dans de nombreux autres patrons. Sur ces belles paroles, revenons à nos moutons.

<img width="550" height="0" src="../../_resources/solution1-fr_de236bc0b3cd46cc9231ee786533539d.png"/>

Héritage contre Agrégation.

Le décorateur est également appelé « emballeur » ou « empaqueteur ». Ces surnoms révèlent l’idée générale derrière le concept. Un *emballeur* est un objet qui peut être lié par un objet *cible*. L’emballeur possède le même ensemble de méthodes que la cible et lui délègue toutes les demandes qu’il reçoit. Il peut exécuter un traitement et modifier le résultat avant ou après avoir envoyé sa demande à la cible.

À quel moment un emballeur devient-il réellement un décorateur ? Comme je l’ai déjà dit, un emballeur implémente la même interface que l’objet emballé. Du point de vue du client, ces objets sont identiques. L’attribut de la référence de l’emballeur doit pouvoir accueillir n’importe quel objet qui implémente cette interface. Vous pouvez ainsi utiliser plusieurs emballeurs sur un seul objet et lui attribuer les comportements de plusieurs emballeurs en même temps.

Reprenons notre exemple et mettons-y une notification par e-mail dans la classe de base `Notificateur`, mais transformons toutes les autres méthodes de notification en décorateurs.

<img width="640" height="0" src="../../_resources/solution2_280d6d29f9ce4dc792d56d9f2f896ab8.png"/>

Des méthodes de notification deviennent des décorateurs.

Le code client doit emballer un objet basique Notificateur pour le transformer en un ensemble de décorateurs adapté aux préférences du client. Les objets qui en résultent sont empilés.

<img width="300" height="0" src="../../_resources/solution3-fr_80fb3de812754d34915e7b091f9ccb6d.png"/>

Les applications peuvent configurer des piles complexes de décorateurs pour les notifications.

Le client traite le dernier objet de la pile. Les décorateurs implémentent tous la même interface (le notificateur de base). Le reste du code client manipule indifféremment l’objet notificateur original et l’objet décoré.

Nous pouvons utiliser cette technique pour tous les autres comportements, comme la mise en page des messages ou la création de la liste des destinataires. Le client peut décorer l’objet avec des décorateurs personnalisés tant qu’ils suivent la même interface que les autres.

## Analogie

<img width="600" height="0" src="../../_resources/decorator-comic-1_cf35dd3e862545359b7ef71b080748ac.png"/>

Les effets se cumulent si vous portez plusieurs couches de vêtements.

Porter des vêtements est un bon exemple d’utilisation. Si vous avez froid, vous vous enroulez dans un pull. Si vous avez encore froid, vous pouvez porter un blouson par-dessus. S’il pleut, vous enfilez un imperméable. Tous ces vêtements « étendent » votre comportement de base mais ne font pas partie de vous, et vous pouvez facilement enlever un vêtement lorsque vous n’en avez plus besoin.

## Structure

<img width="480" height="0" src="../../_resources/structure_3eafb1b77d834dbda293ae65dc11e350.png"/>

1.  Le **Composant** déclare l’interface commune pour les décorateurs et les objets décorés.
    
2.  Le **Composant Concret** est une classe contenant des objets qui vont être emballés. Il définit le comportement par défaut qui peut être modifié par les décorateurs.
    
3.  Le **Décorateur de Base** possède un attribut pour référencer un objet emballé. L’attribut doit être déclaré avec le type de l’interface du composant afin de contenir à la fois les composants concrets et les décorateurs. Le décorateur de base délègue toutes les opérations à l’objet emballé.
    
4.  Les **Décorateurs Concrets** définissent des comportements supplémentaires qui peuvent être ajoutés dynamiquement aux composants. Les décorateurs concrets redéfinissent les méthodes du décorateur de base et exécutent leur traitement avant ou après l’appel à la méthode du parent.
    
5.  Le **Client** peut emballer les composants dans plusieurs couches de décorateurs, tant qu’il manipule les objets à l’aide de l’interface du composant.
    

## Pseudo-code

Dans cet exemple, le **Décorateur** permet la compression et le chiffrage des données indépendamment du code qui les utilise.

<img width="540" height="0" src="../../_resources/example_a160323bbb944584b0933bebcc01ba89.png"/>

L’exemple de chiffrage et de compression utilisé.

L’application emballe l’objet de la source de données à l’aide de deux décorateurs. Ils modifient la manière dont les données sont lues et écrites sur le disque :

- Les décorateurs chiffrent et compressent les données juste avant qu’elles soient **écrites sur le disque**. La classe d’origine écrit les données chiffrées et protégées dans le fichier sans savoir qu’il y a eu une modification.
    
- Une fois que les données sont **lues depuis le disque**, elles repassent dans ces mêmes décorateurs qui les décompressent et les déchiffrent.
    

Les décorateurs et la classe de la source de données implémentent la même interface, ce qui les rend interchangeables aux yeux du code client.

```
// L’interface du composant définit les traitements qui peuvent
// être modifiés par les décorateurs.
interface DataSource is
    method writeData(data)
    method readData():data

// Les composants concrets fournissent des implémentations par
// défaut pour les traitements. Plusieurs variantes de ces
// classes peuvent se trouver dans un programme.
class FileDataSource implements DataSource is
    constructor FileDataSource(filename) { ... }

    method writeData(data) is
        // Écrit les données dans le fichier.

    method readData():data is
        // Lit les données du fichier.

// La classe de base du décorateur implémente la même interface
// que les autres composants. Le but principal de cette classe
// est de définir l’interface d’emballage pour tous les
// décorateurs concrets. L’implémentation par défaut du code
// d’emballage peut comprendre un attribut pour stocker un
// composant emballé et tout le nécessaire pour l’initialiser.
class DataSourceDecorator implements DataSource is
    protected field wrappee: DataSource

    constructor DataSourceDecorator(source: DataSource) is
        wrappee = source

    // Le décorateur de base délègue tout le travail à l’objet
    // emballé. Les comportements supplémentaires peuvent être
    // ajoutés aux décorateurs concrets.
    method writeData(data) is
        wrappee.writeData(data)

    // Les décorateurs concrets peuvent appeler l’implémentation
    // parent du traitement plutôt que d’appeler l’objet emballé
    // directement. Cette approche simplifie l’extension des
    // classes décorateur.
    method readData():data is
        return wrappee.readData()

// Les décorateurs concrets doivent appeler les méthodes de
// l’objet emballé, mais ils peuvent ajouter quelque chose au
// résultat. Les décorateurs peuvent exécuter ce comportement
// supplémentaire avant ou après l’appel à l’objet emballé.
class EncryptionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Chiffre les données passées.
        // 2. Envoie les données chiffrées à la méthode emballée
        // écrireDonnées

    method readData():data is
        // 1. Récupère les données de la méthode emballée
        // lireDonnées.
        // 2. La déchiffre si besoin.
        // 3. Retourne le résultat.

// Vous pouvez emballer les objets dans plusieurs couches de
// décorateurs.
class CompressionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Compresse les données passées.
        // 2. Envoie les données compressées à la méthode
        // emballée écrireDonnées

    method readData():data is
        // 1. Récupère les données de la méthode emballée
        // lireDonnées.
        // 2. La décompresse si besoin.
        // 3. Retourne le résultat.

// Option 1 : un exemple simple de l’assemblage d’un décorateur.
class Application is
    method dumbUsageExample() is
        source = new FileDataSource("somefile.dat")
        source.writeData(salaryRecords)
        // Le fichier cible a été écrit avec des données brutes.

        source = new CompressionDecorator(source)
        source.writeData(salaryRecords)
        // Le fichier cible a été écrit avec des données
        // compressées.

        source = new EncryptionDecorator(source)
        // La variable source contient à présent :
        // Chiffrage > Compression > FileDataSource.
        source.writeData(salaryRecords)
        // Le fichier cible a été écrit avec des données
        // compressées et chiffrées.

// Option 2 : un code client qui utilise une source de données
// externe. Les objets responsablePaye (SalaryManager) ne
// s’encombrent pas des détails concernant le stockage des
// données. Ils utilisent une source de données pré configurée
// reçue par le configurateur de l’application.
class SalaryManager is
    field source: DataSource

    constructor SalaryManager(source: DataSource) { ... }

    method load() is
        return source.readData()

    method save() is
        source.writeData(salaryRecords)
    // ...d’autres méthodes utiles...

// L’application peut assembler différentes piles de décorateurs
// à l’exécution, en fonction de la configuration ou de
// l’environnement.
class ApplicationConfigurator is
    method configurationExample() is
        source = new FileDataSource("salary.dat")
        if (enabledEncryption)
            source = new EncryptionDecorator(source)
        if (enabledCompression)
            source = new CompressionDecorator(source)

        logger = new SalaryManager(source)
        salary = logger.load()
    // ...

```

## Possibilités d’application

Utilisez le décorateur si vous avez besoin d’ajouter des comportements supplémentaires au moment de l’exécution sans avoir à altérer le code source de ces objets.

Le décorateur vous permet de structurer votre logique métier en couches, de créer un décorateur pour chacune de ces couches et de décorer les objets avec différentes combinaisons au moment de l’exécution. Le code client peut traiter les objets uniformément puisqu’ils implémentent la même interface.

Utilisez ce patron si l’héritage est impossible ou peu logique pour étendre le comportement d’un objet.

De nombreux langages de programmation permettent l’utilisation du mot clé `final` pour interdire l’héritage une classe. Le seul moyen d’étendre le comportement d’une telle classe est de l’emballer en utilisant un décorateur.

## Mise en œuvre

1.  Assurez-vous que votre domaine peut être représenté sous la forme d’un composant principal recouvert par plusieurs couches facultatives.
    
2.  Déterminez les méthodes communes entre le composant principal et les couches facultatives. Créez l’interface du composant et déclarez-y ces méthodes.
    
3.  Créez une classe concrète pour le composant et définissez son comportement de base.
    
4.  Créez une classe de base décorateur. Elle doit inclure un attribut qui va permettre de stocker la référence à un objet emballé. Cet attribut doit être déclaré avec le type de l’interface du composant, afin de le relier aux composants concrets et aux décorateurs. Le décorateur de base doit déléguer tout le travail à l’objet emballé.
    
5.  Assurez-vous que les classes implémentent l’interface du composant.
    
6.  Créez des décorateurs concrets en les implémentant à partir du décorateur de base. Un décorateur concret doit exécuter son comportement avant ou après l’appel à la méthode de son parent (qui délègue toujours la tâche à l’objet emballé).
    
7.  Le code client doit être responsable de la création des décorateurs et de leur agencement en fonction des besoins du client.
    

## Avantages et inconvénients

- Vous pouvez étendre le comportement d’un objet sans avoir recours à la création d’une nouvelle sous-classe.
- Vous pouvez ajouter ou retirer dynamiquement des responsabilités à un objet au moment de l’exécution.
- Vous pouvez combiner plusieurs comportements en emballant un objet dans plusieurs décorateurs.
- *Principe de responsabilité unique*. Vous pouvez découper une classe monolithique qui implémente plusieurs comportements différents en plusieurs petits morceaux.

- Retirer un emballeur spécifique de la pile n’est pas chose aisée.
- Il n’est pas non plus aisé de mettre en place un décorateur dont le comportement ne varie pas en fonction de sa position dans la pile.
- Le code de configuration initial des couches peut avoir l’air assez moche.

## Liens avec les autres patrons

- L’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter) modifie l’interface d’un objet existant, alors que le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) améliore un objet sans toucher à son interface. De plus, le *décorateur* prend en charge la composition récursive, ce qui n’est pas possible avec l’*adaptateur*.
    
- L’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter) procure une interface différente à l’objet emballé, la [Procuration](https://refactoring.guru/fr/design-patterns/proxy) lui fournit la même interface et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) lui met à disposition une interface améliorée.
    
- La [Chaîne de Responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility "Chaîne de responsabilité") et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) ont des structures de classes très similaires. Ils reposent tous deux sur la composition récursive pour passer les appels à une suite d’objets. Ils possèdent cependant plusieurs différences majeures.
    
    Les handlers de la *chaîne de responsabilité* peuvent lancer des opérations arbitraires indépendantes l’une de l’autre. Ils peuvent également décider de ne pas propager la demande plus loin dans la chaîne. Les *décorateurs* quant à eux peuvent étendre le comportement de l’objet sans perturber leur fonctionnement avec l’interface de base. De plus, les décorateurs n’ont pas le droit d’interrompre la demande.
    
- Le [Composite](https://refactoring.guru/fr/design-patterns/composite) et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) ont des diagrammes de structure similaires puisqu’ils reposent sur la composition récursive pour organiser un nombre variable d’objets.
    
    Un *décorateur* est comme un *composite*, mais avec un seul composant enfant. Il y a une autre différence importante : Le *décorateur* ajoute des responsabilités supplémentaires à l’objet emballé, alors que le *composite* se contente d’« additionner » les résultats de ses enfants.
    
    Mais ces patrons de conception peuvent également coopérer : vous pouvez utiliser le *décorateur* pour étendre le comportement d’un objet spécifique d’un arbre *Composite*.
    
- Les conceptions qui reposent énormément sur le [Composite](https://refactoring.guru/fr/design-patterns/composite) et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) tirent des avantages à utiliser le [Prototype](https://refactoring.guru/fr/design-patterns/prototype). Il vous permet de cloner les structures complexes plutôt que de les reconstruire à partir de rien.
    
- Le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) vous permet de changer la peau d’un objet, alors que la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) vous permet de changer ses tripes.
    
- Le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) et la [Procuration](https://refactoring.guru/fr/design-patterns/proxy) ont des structures similaires, mais des intentions différentes. Ces deux patrons sont bâtis sur le principe de la composition, où un objet est censé déléguer certains traitements à un autre. La différence est qu’en principe, la *procuration* gère elle-même le cycle de vie de son objet service, alors que la composition des *décorateurs* est toujours contrôlée par le client.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_47a338e0c5584f0d92ef3b1f91ac57f5.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/java/example "Patrons de conception : Décorateur en Java") [<img width="43" height="56" src="../../_resources/csharp_95db31896cd64afe9fcbcaf277b96b22.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/csharp/example "Patrons de conception : Décorateur en C#")[<img width="43" height="56" src="../../_resources/cpp_9a2aa855c1a945b5ae581b4c29bef6d9.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/cpp/example "Patrons de conception : Décorateur en C++")[<img width="43" height="56" src="../../_resources/php_71c737b109b345dc91c334179b50d316.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/php/example "Patrons de conception : Décorateur en PHP")[<img width="43" height="56" src="../../_resources/python_074b598eb7334282a5502b00820880d0.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/python/example "Patrons de conception : Décorateur en Python")[<img width="43" height="56" src="../../_resources/ruby_1c1d03cb8f9e4133975fa053f9fc8f93.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/ruby/example "Patrons de conception : Décorateur en Ruby")[<img width="43" height="56" src="../../_resources/swift_c14ed9eb94024cc3ba08b56cffa5702f.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/swift/example "Patrons de conception : Décorateur en Swift")[<img width="43" height="56" src="../../_resources/typescript_073c75bda2f849a3ad22836fb2e9f26d.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/typescript/example "Patrons de conception : Décorateur en TypeScript")[<img width="43" height="56" src="../../_resources/go_92ab165b2cb74e578a06fff0b8d29abc.svg"/>](https://refactoring.guru/fr/design-patterns/decorator/go/example "Patrons de conception : Décorateur en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_dbad23c6d11548f9a4eab63fd65.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Façade](https://refactoring.guru/fr/design-patterns/facade)

#### Retour

[Composite](https://refactoring.guru/fr/design-patterns/composite)

[<img width="245" height="375" src="../../_resources/web-cover-fr_671dfe2ed7ad47daa093d29ef004f257.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/decorator "English") [Español](https://refactoring.guru/es/design-patterns/decorator "Español") [Français](https://refactoring.guru/fr/design-patterns/decorator "Français") [Polski](https://refactoring.guru/pl/design-patterns/decorator "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/decorator "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/decorator "Русский") [Українська](https://refactoring.guru/uk/design-patterns/decorator "Українська") [中文](https://refactoringguru.cn/design-patterns/decorator "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_8b36ef8101444dd1aa9a0510768c2f2d.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_456ca67ec62948a79d470aab8418c0d3.png)

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
 ![Organization address](../../_resources/fa-building_417e1698716e4636bc1cd54c0297190b.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)