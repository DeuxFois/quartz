---
title: Pont / Bridge
updated: 2021-12-26 15:28:23Z
created: 2021-12-26 15:10:13Z
source: https://refactoring.guru/fr/design-patterns/bridge
tags:
  - design_pattern
  - structurel
---

Le **Pont** permet de séparer une grosse classe ou un ensemble de classes connexes en deux hiérarchies — abstraction et implémentation — qui peuvent évoluer indépendamment l’une de l’autre.

![Patron de conception&nbsp;pont](../../_resources/bridge_6b7231fedc7a44ee8b33d9caa578d0dc.png)

## Problème

*Abstraction ?* *Implémentation ?* Ces termes vous donnent des frissons ? Rassurez-vous, nous allons prendre un exemple simple.

Prenons une classe de `Forme` géométrique avec les sous-classes suivantes : `Cercle` et `Carré`. Vous voulez étendre cette hiérarchie de classes pour incorporer des couleurs, vous créez donc des sous-classes de formes : `Rouge` et `Bleu`. Mais vous avez déjà deux sous-classes, vous devez donc créer quatre combinaisons comme par exemple `CercleBleu` et `CarréRouge`.

![Problème du patron de conception pont](../../_resources/problem-fr_356d24c91a8547fc8b7290e78051ca75.png)

Le nombre de combinaisons augmente exponentiellement.

Ajouter de nouvelles formes et couleurs va augmenter la taille de la hiérarchie exponentiellement. Par exemple, pour ajouter une forme triangle, vous devez créer deux nouvelles sous-classes : une par couleur. Ensuite, ajouter une couleur demandera trois nouvelles sous-classes : une pour chaque forme. Si l’on continue ainsi, la situation ne fait qu’empirer.

## Solution

Nous rencontrons ce problème, car nous essayons d’étendre les classes forme dans deux dimensions indépendantes : la forme et la couleur. C’est un problème classique causé par l’héritage.

Le pont tente de résoudre ce problème en utilisant la composition à la place de l’héritage. Pour ce faire, vous insérez une des dimensions dans une hiérarchie de classes séparée afin que la classe originale puisse référencer un objet de cette nouvelle hiérarchie, plutôt que de réunir tous les états et comportements à l’intérieur d’une même classe.

<img width="460" height="0" src="../../_resources/solution-fr_2dd2bd307d7a49238a90cc8a5c3f5b94.png"/>

Vous évitez l’explosion de la hiérarchie de classes en la transformant en plusieurs hiérarchies connexes.

Nous allons appliquer ce procédé et récupérer le code concernant les couleurs dans sa propre classe avec deux sous-classes : `Rouge` et `Bleu`. La classe `Forme` est ensuite dotée d’un attribut qui référence l’un des objets couleur. La forme peut maintenant déléguer tous les traitements concernant la couleur de l’objet. Cette référence agira comme un pont entre les classes `Forme` et `Couleur`. Dorénavant, l’ajout de nouvelles couleurs ne bouleversera plus la hiérarchie des formes et inversement.

#### Abstraction et implémentation

Le livre du GoF  ajoute les termes *abstraction* et *implémentation* à la définition du pont. Trop académiques, ces termes rendent le patron plus compliqué qu’il ne l’est en réalité. En gardant en tête l’exemple avec les formes et couleurs, déchiffrons la signification de ces termes barbares.

L’*abstraction* (aussi appelée *interface*) est une couche de contrôle de haut niveau pour une entité. Cette couche n’est pas censée effectuer de traitements toute seule. Elle doit déléguer le travail à la couche *implémentation* (appelée également *plateforme*).

Notez bien qu’il ne s’agit pas des *interfaces* ou des *classes abstraites* d’un langage de programmation.

Si l’on prend un logiciel comme exemple concret, l’interface utilisateur graphique (GUI) prend le rôle de l’abstraction et le code du système d’exploitation (API) prend le rôle de l’implémentation que la couche GUI appelle en réponse aux interactions de l’utilisateur.

D’une manière générale, vous pouvez développer un tel programme en deux parties indépendantes :

- Disposer de plusieurs GUI différentes (on lance celle qui est adaptée à l’utilisateur, client ou admin).
- Gérer plusieurs API différentes (pouvoir exécuter l’application sous Windows, Linux et macOS).

Dans le pire des cas, cette application pourrait ressembler à un énorme plat de spaghettis avec des centaines de conditions connectant différents types de GUI et d’API, réparties un peu partout à travers le code.

<img width="600" height="0" src="../../_resources/bridge-3-fr_e243489ee97244ad80548abf97cad453.png"/>

La moindre modification dans une architecture monolithique peut se révéler compliquée, car vous devez comprendre *la totalité du code*. Vous pouvez facilement effectuer des modifications dans de plus petits modules bien définis.

Vous pouvez mettre de l’ordre dans ce chaos en rangeant le code concernant les combinaisons spécifiques de l’interface/plateforme dans des classes séparées, mais vous découvrirez rapidement que ces classes sont *nombreuses*. La hiérarchie des classes croît rapidement, car l’ajout d’une nouvelle GUI ou l’intégration d’une nouvelle API requièrent la création de classes supplémentaires.

Tentons de résoudre ce problème avec le pont. Il nous propose de mettre les classes dans deux hiérarchies :

- Abstraction : la couche GUI de l’application.
- Implémentation : les API des systèmes d’exploitation.

<img width="640" height="0" src="../../_resources/bridge-2-fr_21db5a43d2d944e5ba4615554cc2aa7b.png"/>

Une des techniques pour organiser une application multiplateforme.

L’objet abstraction contrôle l’apparence de l’application et délègue la partie métier à l’objet d’implémentation correspondant. Les implémentations sont interchangeables tant qu’elles implémentent la même interface, ce qui permet à la même GUI de fonctionner aussi bien sous Windows que sous Linux.

Grâce à cela, vous pouvez modifier les classes de la GUI sans toucher aux classes des API. De plus, adapter le code pour gérer un autre système d’exploitation ne requiert que l’ajout d’une sous-classe dans la hiérarchie de l’implémentation.

## Structure

<img width="560" height="0" src="../../_resources/structure-fr_dd8cf234254848ce8a937ca78c264f1d.png"/>

1.  L’**Abstraction** offre une logique de contrôle de haut niveau. Elle compte sur l’objet de l’implémentation pour s’occuper des tâches de bas niveau.
    
2.  L’**Implémentation** déclare une interface commune pour toutes les implémentations concrètes. L’abstraction ne peut communiquer avec les objets de l’implémentation que grâce aux méthodes qui y sont déclarées.
    
    L’abstraction peut contenir les mêmes méthodes que l’implémentation, mais en général l’abstraction déclare des comportements complexes qui reposent sur une grande variété d’opérations primitives déclarées par l’implémentation.
    
3.  Les **Implémentations Concrètes** contiennent du code spécialisé pour les plateformes.
    
4.  L’**Abstraction Fine** procure des variantes pour la logique de contrôle. Tout comme leur parent, elles travaillent avec différentes implémentations en passant par l’interface d’implémentation principale.
    
5.  En général, le **Client** ne veut interagir qu’avec l’abstraction, mais c’est son rôle de faire correspondre l’objet d’abstraction avec un des objets d’implémentation.
    

## Pseudo-code

Cet exemple montre comment le **Pont** aide à diviser le code monolithique d’une application qui gère les appareils et leurs télécommandes. Les `Appareils` prennent le rôle de l’implémentation et les `Télécommandes` font office d’abstraction.

<img width="640" height="0" src="../../_resources/example-fr_146a07f1bccb4967bcedf85e633dd453.png"/>

La hiérarchie de la classe originale est divisée en deux parties : appareils et télécommandes.

La classe de base télécommande déclare un attribut de référence qui la lie avec un objet appareil. Toutes les télécommandes utilisent l’interface principale des appareils, ce qui leur permet de fonctionner avec tous les types d’appareils.

Vous pouvez faire évoluer les télécommandes indépendamment des appareils, vous devez juste créer une nouvelle sous-classe de télécommande. Par exemple, une télécommande basique pourrait juste avoir deux boutons, mais vous pouvez lui rajouter des fonctionnalités comme une batterie supplémentaire ou un écran tactile.

Le code client établit le lien entre le type de télécommande désiré et un appareil spécifique en passant par le constructeur de la télécommande.

```
// L’« abstraction » définit l’interface pour la partie
// « télécommande » des deux hiérarchies de classes. Elle garde
// une référence sur un objet de la hiérarchie de
// l’« implémentation » et lui délègue les tâches.
class RemoteControl is
    protected field device: Device
    constructor RemoteControl(device: Device) is
        this.device = device
    method togglePower() is
        if (device.isEnabled()) then
            device.disable()
        else
            device.enable()
    method volumeDown() is
        device.setVolume(device.getVolume() - 10)
    method volumeUp() is
        device.setVolume(device.getVolume() + 10)
    method channelDown() is
        device.setChannel(device.getChannel() - 1)
    method channelUp() is
        device.setChannel(device.getChannel() + 1)

// Vous pouvez étendre les classes de la hiérarchie de
// l’abstraction indépendamment des classes appareil.
class AdvancedRemoteControl extends RemoteControl is
    method mute() is
        device.setVolume(0)

// L’interface de l’implémentation déclare les méthodes communes
// à toutes les classes concrètes de l’implémentation. Elle n’a
// pas besoin de correspondre à l’interface de l’abstraction. En
// fait, les deux interfaces peuvent être complètement
// différentes. En général, l’interface de l’implémentation ne
// fournit que des opérations primitives, alors que
// l’abstraction définit des opérations de plus haut niveau
// basées sur ces primitives.
interface Device is
    method isEnabled()
    method enable()
    method disable()
    method getVolume()
    method setVolume(percent)
    method getChannel()
    method setChannel(channel)

// Tous les appareils suivent la même interface.
class Tv implements Device is
    // ...

class Radio implements Device is
    // ...

// Quelque part dans le code client.
tv = new Tv()
remote = new RemoteControl(tv)
remote.togglePower()

radio = new Radio()
remote = new AdvancedRemoteControl(radio)

```

## Possibilités d’application

Utilisez le pont dans les situations où vous souhaitez diviser et organiser une classe monolithique composée de plusieurs variantes d’une fonctionnalité (par exemple, si la classe fonctionne avec différents serveurs de base de données).

Plus une classe grandit, plus il est difficile de comprendre son fonctionnement et plus les modifications prennent du temps. Les modifications apportées à l’une des variantes de la fonctionnalité vont demander des changements dans toute la classe, ce qui risque de créer des erreurs ou de provoquer des effets de bord critiques.

Le pont vous permet de diviser la classe monolithique en plusieurs hiérarchies de classes. Ensuite, les classes d’une hiérarchie peuvent être modifiées indépendamment des classes des autres hiérarchies. La maintenance du code devient ainsi plus simple et minimise les risques de bugs.

Utilisez le pont si vous voulez étendre une classe dans plusieurs dimensions orthogonales (indépendantes).

Le pont vous propose de construire une hiérarchie de classes séparée pour chaque dimension. La classe d’origine délègue la tâche aux objets de ces hiérarchies plutôt que de tout faire par elle-même.

Utilisez ce patron si vous voulez être en mesure de changer d’implémentation dès le lancement de l’application.

Grâce à ce patron, l’objet de l’implémentation peut être déplacé à l’intérieur de l’abstraction. Cette manipulation n’est pas obligatoire, mais elle est aussi simple à mettre en place que de donner une valeur à un attribut.

*J’en profite pour vous informer que ce dernier point pousse souvent les développeurs à confondre le pont et la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy). Rappelez-vous bien qu’un patron n’est pas seulement une manière de structurer vos classes, c’est aussi un moyen de communiquer votre intention ou de répondre à un problème.*

## Mise en œuvre

1.  Identifiez les dimensions orthogonales de vos classes. Ces concepts indépendants peuvent représenter les couples suivants : abstraction/plateforme, domaine/infrastructure, front-end/back-end, interface/implémentation.
    
2.  Déterminez les opérations que le client veut utiliser et définissez-les dans la classe d’abstraction de base.
    
3.  Établissez la liste des opérations disponibles sur toutes les plateformes. L’abstraction a besoin de certaines de ces opérations : déclarez-les dans l’interface de l’implémentation principale.
    
4.  Créez des classes d’implémentations concrètes pour chaque plateforme de votre domaine, et assurez-vous qu’elles implémentent l’interface de l’implémentation.
    
5.  Ajoutez un attribut de référence pour le type de l’implémentation à l’intérieur de la classe abstraction. Cette dernière délègue la majorité des tâches à l’objet implémentation référencé dans cet attribut.
    
6.  Si vous avez plusieurs variantes de logique de haut niveau, créez des abstractions fines pour chacune d’entre elles en étendant la classe de base abstraction.
    
7.  Le code client doit en principe passer un objet d’implémentation au constructeur de l’abstraction afin d’associer les deux. Ensuite, le client n’a plus besoin de s’occuper de l’implémentation et peut se contenter de travailler uniquement avec l’abstraction.
    

## Avantages et inconvénients

- Vous pouvez créer des classes et des applications multiplateformes.
- Le code client manipule des abstractions de haut niveau. Il n’est pas dépendant des détails de la plateforme.
- *Principe ouvert/fermé*. Vous pouvez introduire de nouvelles abstractions et implémentations indépendamment les unes des autres.
- *Principe de responsabilité unique*. Vous pouvez vous concentrer sur la logique de haut niveau dans l’abstraction, et sur les détails de la plateforme dans l’implémentation.

- Le code va devenir plus compliqué si vous introduisez ce patron dans une classe très cohésive.

## Liens avec les autres patrons

- Le [Pont](https://refactoring.guru/fr/design-patterns/bridge) est habituellement mis en place durant la conception, ce qui vous permet de développer les différentes parties de l’application indépendamment. L’[adaptateur](https://refactoring.guru/fr/design-patterns/adapter "Adaptateur") quant à lui est plus souvent utilisé dans une application existante pour permettre à des classes normalement incompatibles de fonctionner ensemble.
    
- Le [Pont](https://refactoring.guru/fr/design-patterns/bridge), l’[État](https://refactoring.guru/fr/design-patterns/state), la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) (et dans une certaine mesure l’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter)) ont des structures très similaires. En effet, ces patrons sont basés sur la composition, qui délègue les tâches aux autres objets. Cependant, ils résolvent différents problèmes. Un patron n’est pas juste une recette qui vous aide à structurer votre code d’une certaine manière. C’est aussi une façon de communiquer aux autres développeurs le problème qu’il résout.
    
- Vous pouvez utiliser la [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) avec le [Pont](https://refactoring.guru/fr/design-patterns/bridge). Ce couple est très utile quand les abstractions définies par le *pont* ne fonctionnent qu’avec certaines implémentations spécifiques. Dans ce cas, la *fabrique abstraite* peut encapsuler ces relations et cacher la complexité au code client.
    
- Vous pouvez combiner le [Monteur](https://refactoring.guru/fr/design-patterns/builder) avec le [Pont](https://refactoring.guru/fr/design-patterns/bridge) : la classe directeur joue le rôle de l’abstraction, et les différents *monteurs* prennent le rôle des implémentations.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_88284675f6634983b71c9628038ef7d0.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/java/example "Patrons de conception : Pont en Java") [<img width="43" height="56" src="../../_resources/csharp_75fec7f33720407f866a0f567be81fbe.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/csharp/example "Patrons de conception : Pont en C#")[<img width="43" height="56" src="../../_resources/cpp_9ad5e3b4218f462883a58ec3b839c79a.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/cpp/example "Patrons de conception : Pont en C++")[<img width="43" height="56" src="../../_resources/php_1c82a291f8734842a7dc11981fbe9fe2.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/php/example "Patrons de conception : Pont en PHP")[<img width="43" height="56" src="../../_resources/python_41dcd88a6c774309b0fd54b8e92908cc.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/python/example "Patrons de conception : Pont en Python")[<img width="43" height="56" src="../../_resources/ruby_21ff54f4b971456391669609a4314bda.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/ruby/example "Patrons de conception : Pont en Ruby")[<img width="43" height="56" src="../../_resources/swift_a648cd40c45946bab53c25afc2f37b11.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/swift/example "Patrons de conception : Pont en Swift")[<img width="43" height="56" src="../../_resources/typescript_6f99c29af76046a0b7a2a9d99e9906f9.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/typescript/example "Patrons de conception : Pont en TypeScript")[<img width="43" height="56" src="../../_resources/go_d411a44255c44bc18d460e491f50f8ae.svg"/>](https://refactoring.guru/fr/design-patterns/bridge/go/example "Patrons de conception : Pont en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_d73bc764a22842d190ea5abe8a7.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Composite](https://refactoring.guru/fr/design-patterns/composite)

#### Retour

[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter)

[<img width="245" height="375" src="../../_resources/web-cover-fr_3a62d04615d94f1b818d3b62c3c84ed2.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/bridge "English") [Español](https://refactoring.guru/es/design-patterns/bridge "Español") [Français](https://refactoring.guru/fr/design-patterns/bridge "Français") [Polski](https://refactoring.guru/pl/design-patterns/bridge "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/bridge "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/bridge "Русский") [Українська](https://refactoring.guru/uk/design-patterns/bridge "Українська") [中文](https://refactoringguru.cn/design-patterns/bridge "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_37fdb4217d414c6e832ce290b6e755f8.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_66a2aac616d0459d86ae9e809c23f5a4.png)

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
 ![Organization address](../../_resources/fa-building_d1ba702091d945298b65695c602e6d2d.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)