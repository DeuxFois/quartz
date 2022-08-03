---
title: Médiateur / Mediator
updated: 2021-12-26 15:29:02Z
created: 2021-12-26 15:17:45Z
source: https://refactoring.guru/fr/design-patterns/mediator
tags:
  - behavioral
  - design_pattern
---

**Médiateur** diminue les dépendances chaotiques entre les objets. Il restreint les communications directes entre les objets et les force à collaborer uniquement via un objet médiateur.

![Patron de conception&nbsp;médiateur](../../_resources/mediator_439dcb260c8b446ab7602c447fc50598.png)

## Problème

Prenons une boîte de dialogue qui crée et modifie des profils client. Elle comporte plusieurs contrôles de formulaires comme des champs de texte, des cases à cocher, des boutons, etc.

![Liens chaotiques entre éléments de l’interface utilisateur](../../_resources/problem1-fr_ca25d340de5d4f0b92ba3a5677f497fa.png)

Les dépendances entre les éléments de l’interface utilisateur peuvent devenir chaotiques avec l’évolution de l’application.

Certains éléments du formulaire peuvent interagir. Par exemple, en cochant la case « J’ai un chien », vous allez révéler un champ de texte caché qui vous permet d’écrire le nom du chien. Vous pourriez également avoir un bouton Envoyer qui doit valider les valeurs de tous les champs avant de sauvegarder les données.

<img width="360" height="0" src="../../_resources/problem2_fb154012ed2c446d86e92ac310e9ab19.png"/>

Les éléments peuvent avoir beaucoup de liens. De plus, la modification de certains éléments peut influer sur d’autres.

En écrivant directement la logique dans le code des éléments du formulaire, vous rendez les classes de ces éléments bien plus difficiles à réutiliser dans l’application. Par exemple, vous ne pourrez pas utiliser la classe de la case à cocher dans un autre formulaire, car elle est couplée avec le champ de texte du chien. Vous êtes par conséquent obligé d’utiliser toutes les classes du formulaire du profil si vous voulez vous servir de l’une d’entre elles.

## Solution

Le patron de conception médiateur vous propose de mettre fin à toutes les communications directes entre les composants et de rendre ces derniers indépendants les uns des autres. À la place, ces composants collaborent indirectement en utilisant un objet spécial médiateur qui redirige les appels vers les composants appropriés. Ainsi, les composants ne reposent que sur une seule classe médiateur plutôt que d’être couplés à de nombreux collègues.

Dans notre exemple qui porte sur l’édition du formulaire d’un profil, la classe dialogue peut prendre le rôle du médiateur. Elle connait déjà probablement tous ses sous-éléments, vous n’avez donc pas besoin d’y ajouter des dépendances.

<img width="600" height="0" src="../../_resources/solution1-fr_e669a3897c6c4bb0b1c7783ed570c7b9.png"/>

Les éléments de l’UI doivent communiquer directement avec l’objet médiateur.

Le changement le plus important intervient sur les éléments du formulaire. Prenons le bouton Envoyer. Avant, chaque fois qu’un utilisateur appuyait sur ce bouton, ce dernier devait valider les valeurs des éléments individuels du formulaire. À présent, il se contente d’informer la classe dialogue lorsqu’un clic a lieu. Après avoir reçu cette notification, la classe dialogue effectue la validation ou délègue la tâche aux différents éléments. Le bouton n’est ainsi couplé qu’avec la classe dialogue, au lieu d’être relié à un paquet d’éléments.

Vous pouvez pousser le vice plus loin et diminuer encore plus cette dépendance en extrayant l’interface commune pour toutes ces boîtes de dialogue. L’interface devrait déclarer la méthode de notification que tous les éléments du formulaire utilisent pour prévenir la classe dialogue des événements qui les affectent. Notre bouton Envoyer devrait dorénavant pouvoir manipuler n’importe quelle boîte de dialogue qui implémente cette interface.

Ainsi, le patron de conception médiateur vous permet d’encapsuler une toile complexe de relations entre divers objets à l’intérieur d’un simple objet médiateur. Moins une classe a de dépendances et plus il est facile de la modifier, de l’étendre ou de la réutiliser.

## Analogie

<img width="370" height="0" src="../../_resources/live-example_9641ac6b666747688150ebec224d7c5b.png"/>

Les pilotes d’avion ne communiquent pas directement ensemble pour déterminer qui sera le prochain à atterrir. Toute communication passe par la tour de contrôle.

Les pilotes qui vont décoller ou atterrir sur la piste ne communiquent pas ensemble directement. Ils s’adressent à un contrôleur aérien qui est assis dans une grande tour près de la piste d’atterrissage. Sans lui, les pilotes seraient obligés de savoir si d’autres avions sont à proximité et décider des priorités d’atterrissage avec un ensemble de pilotes. Les accidents d’avion augmenteraient probablement beaucoup.

La tour n’a pas besoin de contrôler la totalité d’un vol. Elle n’est là que pour faire respecter des contraintes au départ et à l’arrivée, pour faire en sorte que les pilotes n’aient pas trop de paramètres à gérer.

## Structure

<img width="520" height="0" src="../../_resources/structure_01f0779cbc9d4706a80fbad61db44928.png"/>

1.  Les classes **Composant** contiennent de la logique métier. Chaque composant possède une référence vers un médiateur, déclaré avec le type de l’interface médiateur. Le composant ne connait pas la classe du médiateur, vous pouvez ainsi réutiliser les mêmes composants dans d’autres programmes en les liant à un autre médiateur.
    
2.  L’interface **Médiateur** déclare des méthodes pour communiquer avec les composants et n’est généralement dotée que d’une seule méthode de notification. Les composants peuvent passer n’importe quel contexte en paramètre de cette méthode (ce qui inclut leurs propres objets), mais en évitant de provoquer un couplage entre un composant récepteur et la classe du demandeur.
    
3.  Les **Médiateurs Concrets** encapsulent les relations entre les divers composants. Les médiateurs concrets gardent souvent des références vers les composants qu’ils gèrent et s’occupent même parfois de leur cycle de vie.
    
4.  Les composants ne doivent pas avoir de visibilité sur les autres composants. S’il arrive quelque chose d’important à un composant, il doit seulement en prévenir le médiateur. Quand ce dernier reçoit la notification, il doit pouvoir facilement identifier le demandeur, ce qui peut suffire pour déterminer le composant qui doit être déclenché en retour.
    
    De son point de vue, le composant ne voit qu’une boîte noire. Le demandeur ne sait pas qui va se charger de sa demande, et le récepteur ignore qui l’a envoyée.
    

## Pseudo-code

Dans cet exemple, le **Médiateur** vous aide à éliminer les dépendances mutuelles entre différentes classes de l’UI (boutons, cases à cocher et libellés de texte).

<img width="560" height="0" src="../../_resources/example_3ba810927e5d4f9f8964a3839812443b.png"/>

Structure des classes des boîtes de dialogue de l’UI.

Un élément déclenché par un utilisateur ne communique pas directement avec les autres éléments, même si c’est l’impression qui s’en dégage. À la place, l’élément n’a besoin que de prévenir son médiateur qu’un événement a eu lieu, en lui donnant les informations contextuelles avec la notification.

Dans cet exemple, la boîte de dialogue d’identification prend le rôle du médiateur. Elle sait comment les éléments concrets sont censés collaborer et facilite leur communication indirecte. Lorsque la boîte de dialogue reçoit une notification d’événement, elle sait quel élément est concerné et redirige l’appel en conséquence.

```
// L’interface médiateur déclare une méthode qui permet aux
// composants d’avertir le médiateur au sujet de divers
// événements. Le médiateur peut réagir à ces événements et
// passer les appels aux autres composants.
interface Mediator is
    method notify(sender: Component, event: string)

// La classe concrète Médiateur. Les interconnexions entre les
// composants individuels ont été démêlées et transférées dans
// le médiateur.
class AuthenticationDialog implements Mediator is
    private field title: string
    private field loginOrRegisterChkBx: Checkbox
    private field loginUsername, loginPassword: Textbox
    private field registrationUsername, registrationPassword,
                  registrationEmail: Textbox
    private field okBtn, cancelBtn: Button

    constructor AuthenticationDialog() is
        // Crée les composants et passe le médiateur actuel dans
        // leurs constructeurs pour établir les liens.

    // Le médiateur est averti si un composant est affecté par
    // quoi que ce soit. Lorsqu’un médiateur reçoit une
    // notification, il peut lancer un traitement ou envoyer la
    // demande à un autre composant.
    method notify(sender, event) is
        if (sender == loginOrRegisterChkBx and event == "check")
            if (loginOrRegisterChkBx.checked)
                title = "Log  in"
                // 1. Affiche les composants du formulaire
                // d’identification.
                // 2. Cache les composants du formulaire
                // d’inscription.
            else
                title = "Register"
                // 1. Affiche les composants du formulaire
                // d’inscription.
                // 2. Cache les composants du formulaire
                // d’identification.

        if (sender == okBtn && event == "click")
            if (loginOrRegister.checked)
                // Essaye de trouver un utilisateur à l’aide de
                // ses identifiants.
                if (!found)
                    // Montre un message d’erreur au-dessus du
                    // champ identifiant.
            else
                // 1. Crée un compte utilisateur en utilisant
                // les données saisies dans les champs
                // d’inscription.
                // 2. Connecte cet utilisateur.
                // ...

// Les composants communiquent avec un médiateur en utilisant
// son interface. Grâce à cela, vous pouvez utiliser les mêmes
// composants dans d’autres contextes en les associant avec
// d’autres objets médiateur.
class Component is
    field dialog: Mediator

    constructor Component(dialog) is
        this.dialog = dialog

    method click() is
        dialog.notify(this, "click")

    method keypress() is
        dialog.notify(this, "keypress")

// Les composants concrets ne communiquent pas ensemble. Leur
// unique canal de communication leur sert à envoyer des
// notifications au médiateur.
class Button extends Component is
    // ...

class Textbox extends Component is
    // ...

class Checkbox extends Component is
    method check() is
        dialog.notify(this, "check")
    // ...

```

## Possibilités d’application

Utilisez ce patron si vous rencontrez des difficultés pour modifier certaines classes trop fortement couplées avec d’autres.

Le médiateur vous permet d’extraire toutes les relations entre les classes dans une classe séparée, en isolant les modifications appliquées à un composant spécifique du reste des composants.

Utilisez ce patron quand vous ne pouvez pas réutiliser un composant ailleurs, car il est trop dépendant des autres composants.

Après avoir mis en place le médiateur, les composants individuels n’ont plus de visibilité sur les autres composants. Ils peuvent toujours communiquer ensemble mais indirectement, via un objet médiateur. Pour réutiliser un composant dans une application différente, vous n’avez besoin que de lui fournir une classe médiateur.

Utilisez le médiateur lorsque vous créez des tonnes de sous-classes pour les composants, juste pour pouvoir bénéficier de leur comportement de base dans différents contextes.

Toutes les relations entre composants se trouvent à l’intérieur d’un médiateur. De nouvelles classes médiateur peuvent donc facilement définir de nouveaux moyens de collaboration pour ces composants sans avoir à les modifier.

## Mise en œuvre

1.  Identifiez des classes qui sont fortement couplées et qui pourraient bénéficier de plus d’indépendance (pour une maintenance plus aisée ou pour faciliter la réutilisation de ces classes).
    
2.  Déclarez l’interface médiateur et écrivez le protocole de communication voulu entre les médiateurs et les différents composants. Dans la plupart des cas, une seule méthode est suffisante pour recevoir les notifications des composants.
    
    Cette interface est cruciale si vous voulez pouvoir réutiliser les classes des composants dans différents contextes. Tant que le composant communique avec le médiateur en passant par l’interface générique, vous pouvez relier le composant avec une implémentation différente du médiateur.
    
3.  Implémentez la classe concrète du médiateur. Faites en sorte que cette classe garde des références vers tous les composants qu’elle gère. Elle en tirera de gros bénéfices.
    
4.  Vous pouvez encore aller plus loin en rendant le médiateur responsable de la création et de la destruction des objets composant. Le médiateur ressemblera ainsi à une [fabrique](https://refactoring.guru/fr/design-patterns/abstract-factory "Fabrique abstraite") ou à une [façade](https://refactoring.guru/fr/design-patterns/facade).
    
5.  Les composants doivent avoir une référence vers l’objet médiateur. La connexion est souvent établie dans le constructeur du composant, dans lequel un objet médiateur est passé en paramètre.
    
6.  Modifiez le code des composants afin qu’ils appellent la méthode de notification du médiateur plutôt que des méthodes écrites dans d’autres composants. Mettez tout le code qui appelle les autres composants dans la classe médiateur, puis appelez-le lorsque le médiateur reçoit des notifications de ce composant.
    

## Avantages et inconvénients

- *Principe de responsabilité unique*. Vous pouvez mettre les communications entre les différents composants au même endroit, rendant le code plus facile à comprendre et à maintenir.
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouveaux médiateurs sans avoir à modifier les composants déjà en place.
- Vous diminuez le couplage entre les différents composants d’un programme.
- Vous pouvez réutiliser les composants individuels plus facilement.

- Avec le temps, un médiateur peut évoluer en [Objet Omniscient](https://refactoring.guru/fr/antipatterns/god-object).

## Liens avec les autres patrons

- La [Chaîne de responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility), la [Commande](https://refactoring.guru/fr/design-patterns/command), le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) et l’[Observateur](https://refactoring.guru/fr/design-patterns/observer) proposent différentes solutions pour associer les demandeurs et les récepteurs.
    
    - La *chaîne de responsabilité* envoie une demande ordonnée qui est passée tout au long d’une chaîne dynamique de récepteurs potentiels, jusqu’à ce que l’un d’entre eux décide de la traiter.
    - La *commande* établit des connexions unidirectionnelles entre les demandeurs et les récepteurs.
    - Le *médiateur* élimine les liens directs entre les demandeurs et les récepteurs, et les force à communiquer indirectement via un objet médiateur.
    - L’*observateur* permet aux récepteurs de s’inscrire et de se désinscrire dynamiquement à la réception des demandes.
- La [Façade](https://refactoring.guru/fr/design-patterns/facade) et le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) ont des rôles similaires : ils essayent de faire collaborer des classes étroitement liées.
    
    - La *façade* définit une interface simplifiée pour un sous-système d’objets, mais elle n’ajoute pas de nouvelles fonctionnalités. Le sous-système est conscient de l’existence de la façade. Les objets situés à l’intérieur du sous-système peuvent communiquer directement.
    - Le *médiateur* centralise la communication entre les composants du système. Les composants ne voient que l’objet médiateur et ne communiquent pas directement.
- La différence entre le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) et l’[Observateur](https://refactoring.guru/fr/design-patterns/observer) est souvent très fine. Dans la majorité des cas, vous pouvez implémenter l’un ou l’autre, mais parfois vous pouvez les utiliser simultanément. Regardons comment faire.
    
    Le but principal du *médiateur* est d’éliminer les dépendances mutuelles entre un ensemble de composants du système. À la place, ces composants peuvent devenir dépendants d’un unique objet médiateur. Le but de l’*observateur* est d’établir des connexions dynamiques à sens unique entre les objets, où certains objets peuvent être les subordonnés d’autres objets.
    
    Il existe une implémentation populaire du *médiateur* qui repose sur l’*observateur*. L’objet médiateur joue le rôle du diffuseur et les composants agissent comme des souscripteurs qui s’inscrivent et se désinscrivent des événements du médiateur. Lorsque ce type de conception est mis en place, le *médiateur* ressemble de près à l’*observateur*.
    
    Si vous êtes un peu perdu, rappelez-vous qu’il y a plusieurs manières d’implémenter le médiateur. Par exemple, vous pouvez associer de manière permanente tous les composants au même objet médiateur. Cette implémentation ne ressemblera pas à l’*observateur*, mais sera tout de même une instance du patron de conception médiateur.
    
    Maintenant, imaginez un programme dont tous les composants sont devenus des diffuseurs, permettant des connexions dynamiques les uns avec les autres. Nous n’aurons pas d’objet médiateur centralisé, seulement un ensemble d’observateurs distribués.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_48593603ce014b40a9fcce2b1a6a4f19.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/java/example "Patrons de conception : Médiateur en Java") [<img width="43" height="56" src="../../_resources/csharp_de1bfccd8e6f41d8b949760accc460d3.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/csharp/example "Patrons de conception : Médiateur en C#")[<img width="43" height="56" src="../../_resources/cpp_1598bf3540e34d2c87f5f869d57762f7.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/cpp/example "Patrons de conception : Médiateur en C++")[<img width="43" height="56" src="../../_resources/php_b24f90df19c848b991d05559151ef163.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/php/example "Patrons de conception : Médiateur en PHP")[<img width="43" height="56" src="../../_resources/python_c75d74cbe1c64ae1aa8899b5f2906d7d.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/python/example "Patrons de conception : Médiateur en Python")[<img width="43" height="56" src="../../_resources/ruby_976fc074c6f94ec8a742a4498b4dc02d.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/ruby/example "Patrons de conception : Médiateur en Ruby")[<img width="43" height="56" src="../../_resources/swift_e44d8b6c28c046caa7d9a354a78b1d72.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/swift/example "Patrons de conception : Médiateur en Swift")[<img width="43" height="56" src="../../_resources/typescript_864db6cc9e8c401ebae6865d606f1cbb.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/typescript/example "Patrons de conception : Médiateur en TypeScript")[<img width="43" height="56" src="../../_resources/go_062ad75dfd424a52ade66c4af740b5ba.svg"/>](https://refactoring.guru/fr/design-patterns/mediator/go/example "Patrons de conception : Médiateur en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_27d0684a40f6495fbf596613451.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Mémento](https://refactoring.guru/fr/design-patterns/memento)

#### Retour

[Itérateur](https://refactoring.guru/fr/design-patterns/iterator)

[<img width="245" height="375" src="../../_resources/web-cover-fr_9955f58440e541408b66399a05cb111a.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/mediator "English") [Español](https://refactoring.guru/es/design-patterns/mediator "Español") [Français](https://refactoring.guru/fr/design-patterns/mediator "Français") [Polski](https://refactoring.guru/pl/design-patterns/mediator "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/mediator "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/mediator "Русский") [Українська](https://refactoring.guru/uk/design-patterns/mediator "Українська") [中文](https://refactoringguru.cn/design-patterns/mediator "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_d8c8406bb2d24c958a8bad22c8fef870.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_77dbcf600a7b4c408ff655115196342f.png)

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
 ![Organization address](../../_resources/fa-building_fb5da57192134c2ab0bc7d74b7d210df.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)