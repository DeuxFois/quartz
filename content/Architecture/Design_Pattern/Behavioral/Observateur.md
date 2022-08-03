---
title: Observateur / Observer
updated: 2021-12-26 15:28:54Z
created: 2021-12-26 15:17:53Z
source: https://refactoring.guru/fr/design-patterns/observer
tags:
  - behavioral
  - design_pattern
---

L’**Observateur** permet de mettre en place un mécanisme de souscription pour envoyer des notifications à plusieurs objets, au sujet d’événements concernant les objets qu’ils observent.

![Patron de conception&nbsp;observateur](../../_resources/observer_cd44810e58424fcf996dc6dd8ea71b07.png)

## Problème

Imaginez que vous avez deux types d’objets : un `Client` et un `Magasin`. Le client s’intéresse à une marque spécifique d’un produit (disons que c’est un nouveau modèle d’iPhone) qui sera bientôt disponible dans la boutique.

Le client pourrait se rendre sur place tous les jours et vérifier la disponibilité du produit. Mais comme le produit n’est pas encore prêt, ses allées et venues seraient inutiles.

![Se rendre au magasin ou envoyer du spam](../../_resources/observer-comic-1-fr_80107edbebe24cc98e769565366b00.png)

Se rendre au magasin ou envoyer du spam.

À la place, le magasin pourrait envoyer des tonnes d’e-mails (ce qui peut être vu comme du spam) à leurs clients chaque fois qu’un nouveau produit est disponible. Cette solution économiserait bien des voyages à leurs clients. En contrepartie, le magasin risque de se mettre à dos ceux qui ne sont pas intéressés par les nouveaux produits.

Nous nous retrouvons dans une situation conflictuelle. Soit les clients perdent leur temps à venir vérifier la disponibilité des produits, soit le magasin gâche des ressources pour prévenir des clients qui ne sont pas concernés.

## Solution

L’objet que l’on veut suivre est en général appelé *sujet*, mais comme il va envoyer des notifications pour prévenir les autres objets dès qu’il est modifié, nous l’appellerons *diffuseur* (publisher). Tous les objets qui veulent suivre les modifications apportées au diffuseur sont appelés des *souscripteurs* (subscribers).

Le patron de conception Observateur vous propose d’ajouter un mécanisme de souscription à la classe diffuseur pour permettre aux objets individuels de s’inscrire ou se désinscrire de ce diffuseur. Pas d’inquiétude ! Ce n’est pas si compliqué que cela en a l’air. En réalité, ce mécanisme est composé 1) d’un tableau d’attributs qui stocke une liste de références vers les objets souscripteur et 2) de plusieurs méthodes publiques qui permettent d’ajouter ou de supprimer des souscripteurs de cette liste.

<img width="470" height="0" src="../../_resources/solution1-fr_f7e5474286de4c84bb5d0c1fd5a26b0e.png"/>

Un mécanisme de souscription qui permet aux objets individuels de s’inscrire aux notifications des événements.

Quand un événement important arrive au diffuseur, il fait le tour de ses souscripteurs et appelle la méthode de notification sur leurs objets.

Les applications peuvent comporter des dizaines de classes souscripteur différentes qui veulent être tenues au courant des événements qui affectent une même classe diffuseur. Vous n’avez sûrement pas envie de coupler le diffuseur à toutes ces classes. De plus, certaines ne seront peut-être pas connues à l’avance, dans les cas où votre classe diffuseur est censée pouvoir être utilisée par d’autres personnes.

C’est pourquoi il est crucial que tous les souscripteurs implémentent la même interface et qu’elle soit le seul moyen utilisé par le diffuseur pour communiquer avec eux. Elle doit déclarer la méthode de notification avec un ensemble de paramètres que le diffuseur peut utiliser pour envoyer des données contextuelles avec la notification.

<img width="460" height="0" src="../../_resources/solution2-fr_3d8497b4c27a4b318d29a0ab457aadcb.png"/>

Le diffuseur envoie des notifications aux souscripteurs en appelant la méthode de notification spécifique sur leurs objets.

De plus, les diffuseurs doivent tous suivre la même interface si votre application en comporte plusieurs types et que vous voulez que vos souscripteurs soient tous compatibles avec eux. Cette interface doit contenir quelques méthodes de souscription et elle doit permettre aux souscripteurs d’observer les états du diffuseur sans le coupler avec leurs classes concrètes.

## Analogie

<img width="600" height="0" src="../../_resources/observer-comic-2-fr_c30250ceed22477488c7a7460e0abd.png"/>

Abonnement aux magazines et aux journaux.

Lorsque vous vous inscrivez à un journal ou à un magazine, vous n’avez plus besoin de vous rendre en magasin pour vérifier si le dernier numéro est sorti. À la place, le diffuseur vous envoie directement les nouveaux numéros dans votre boîte aux lettres dès qu’ils le publient, ou même parfois en avance.

Le diffuseur garde une liste de souscripteurs et connait les magazines qui les intéressent. S’ils ne souhaitent plus recevoir les nouveaux numéros, les souscripteurs peuvent quitter la liste à n’importe quel moment.

## Structure

<img width="610" height="0" src="../../_resources/structure_5a495d48b0c448cba69f66acb7002f9b.png"/>

1.  Le **Diffuseur** envoie des événements intéressants à d’autres objets. Ces événements se produisent quand le diffuseur change d’état ou exécute certains comportements. Le diffuseur possède une infrastructure d’inscription qui permet aux nouveaux souscripteurs de rejoindre la liste et aux souscripteurs actuels de la quitter.
    
2.  Quand un nouvel événement survient, le diffuseur parcourt la liste d’inscriptions et appelle la méthode de notification déclarée dans l’interface des souscripteurs sur chaque objet souscripteur.
    
3.  L’interface **Souscripteur** déclare les méthodes de notification. Dans la majorité des cas, il n’y a qu’une seule méthode `update`. Elle peut prendre plusieurs paramètres pour que le diffuseur leur envoie plus de détails concernant la modification.
    
4.  Les **Souscripteurs Concrets** exécutent certaines actions en réponse aux notifications envoyées par le diffuseur. Toutes ces classes doivent implémenter la même interface pour ne pas coupler le diffuseur avec leurs classes concrètes.
    
5.  En général, les souscripteurs ont besoin de détails à propos du contexte afin d’exécuter correctement la mise à jour. C’est pour cela que les diffuseurs passent souvent des données du contexte en paramètre de la méthode de notification. Le diffuseur peut même s’envoyer lui-même en paramètre et laisser les souscripteurs récupérer directement les données nécessaires.
    
6.  Le **Client** crée des objets diffuseur et Souscripteur séparément et inscrit les souscripteurs aux mises à jour du diffuseur.
    

## Pseudo-code

Dans cet exemple, le patron de conception **Observateur** permet à l’objet éditeur de texte d’avertir d’autres objets de service des changements de son état.

<img width="510" height="0" src="../../_resources/example_3551f760a6ad43d593e1b20ae3c0f9eb.png"/>

Envoyer des notifications aux objets au sujet d’événements qui affectent d’autres objets.

La liste des souscripteurs est compilée dynamiquement : les objets peuvent commencer ou arrêter de suivre les notifications lors du lancement de l’application, en fonction du comportement voulu.

Dans cette implémentation, la classe éditeur ne gère pas la liste des souscripteurs toute seule. Elle délègue la tâche à l’objet spécial assistant dont c’est la seule tâche. Vous pouvez moduler cet objet afin qu’il serve de centrale de distribution, transformant n’importe quel objet en diffuseur.

Tant que les classes diffuseur passent toutes par la même interface pour communiquer avec les souscripteurs, l’ajout de nouveaux souscripteurs ne nécessite aucun changement dans les classes diffuseur.

```
// La classe de base diffuseur contient le code pour
// l’inscription et les méthodes de notification.
class EventManager is
    private field listeners: hash map of event types and listeners

    method subscribe(eventType, listener) is
        listeners.add(eventType, listener)

    method unsubscribe(eventType, listener) is
        listeners.remove(eventType, listener)

    method notify(eventType, data) is
        foreach (listener in listeners.of(eventType)) do
            listener.update(data)

// Le diffuseur concret abrite de la logique métier dédiée à
// certains souscripteurs. Nous pourrions dériver cette classe
// depuis le diffuseur de base, mais ce n’est pas toujours
// possible, car le diffuseur concret pourrait déjà être une
// sous-classe. Dans ce cas, vous pouvez utiliser la composition
// pour effectuer le lien avec la logique de souscription, comme
// effectué ci-dessous :
class Editor is
    public field events: EventManager
    private field file: File

    constructor Editor() is
        events = new EventManager()

    // Les méthodes de la logique métier peuvent prévenir les
    // souscripteurs de toute modification.
    method openFile(path) is
        this.file = new File(path)
        events.notify("open", file.name)

    method saveFile() is
        file.write()
        events.notify("save", file.name)

    // ...

// Voici l’interface des souscripteurs. Si votre langage de
// programmation prend en charge les types fonctionnels, vous
// pouvez remplacer toute la hiérarchie des souscripteurs par un
// ensemble de fonctions.
interface EventListener is
    method update(filename)

// Les souscripteurs concrets réagissent aux mises à jour de
// leur diffuseur.
class LoggingListener implements EventListener is
    private field log: File
    private field message: string

    constructor LoggingListener(log_filename, message) is
        this.log = new File(log_filename)
        this.message = message

    method update(filename) is
        log.write(replace('%s',filename,message))

class EmailAlertsListener implements EventListener is
    private field email: string
    private field message: string

    constructor EmailAlertsListener(email, message) is
        this.email = email
        this.message = message

    method update(filename) is
        system.email(email, replace('%s',filename,message))

// Une application peut configurer des diffuseurs et des
// souscripteurs à l’exécution.
class Application is
    method config() is
        editor = new Editor()

        logger = new LoggingListener(
            "/path/to/log.txt",
            "Someone  has  opened  the  file:  %s")
        editor.events.subscribe("open", logger)

        emailAlerts = new EmailAlertsListener(
            "admin@example.com",
            "Someone  has  changed  the  file:  %s")
        editor.events.subscribe("save", emailAlerts)

```

## Possibilités d’application

Utilisez le patron de conception Observateur quand des modifications de l’état d’un objet peuvent en impacter d’autres, et que l’ensemble des objets n’est pas connu à l’avance ou qu’il change dynamiquement.

Ce problème est souvent rencontré lorsque l’on travaille sur des classes d’une interface utilisateur graphique. Par exemple, si vous créez des classes bouton personnalisées et que vous voulez que les clients puissent y ajouter du code déclenché par le clic d’un utilisateur.

L’observateur permet à tous les objets qui suivent l’interface souscripteur de s’inscrire aux notifications des événements des objets diffuseur. Vous pouvez ajouter le mécanisme de souscription à tous vos boutons et laisser les clients mettre leur code personnalisé dans des classes souscripteur personnalisées.

Utilisez ce patron quand certains objets de votre application doivent en suivre d’autres, mais seulement pendant un certain temps ou dans des cas spécifiques.

La liste d’inscription est dynamique, les souscripteurs peuvent donc rejoindre ou quitter la liste quand ils le désirent.

## Mise en œuvre

1.  Passez en revue votre logique métier et découpez-la en deux parties : la fonctionnalité principale (indépendante du reste du code) prendra le rôle du diffuseur ; le reste va être transformé en classes souscripteur.
    
2.  Déclarez l’interface souscripteur. Elle doit au moins contenir une méthode `update`.
    
3.  Déclarez l’interface diffuseur et écrivez des méthodes qui permettent d’ajouter et de retirer des objets souscripteur de la liste. Rappelez-vous que les diffuseurs doivent manipuler les souscripteurs uniquement en passant par l’interface des souscripteurs.
    
4.  Décidez où vous allez mettre la liste de souscription ainsi que l’implémentation des méthodes de souscription. En général, ce code est presque identique pour tous les types de diffuseurs. Le mieux est donc de le mettre dans une classe abstraite directement dérivée de l’interface diffuseur. Les diffuseurs concrets étendent cette classe et héritent du comportement de la souscription.
    
    Si vous implémentez ce patron dans une hiérarchie de classes existante, pensez à la composition : mettez la logique de souscription dans un objet séparé et faites en sorte que les diffuseurs l’utilisent.
    
5.  Créez les classes concrètes Diffuseur. Chaque fois que quelque chose d’important se produit chez un diffuseur, il doit prévenir ses souscripteurs.
    
6.  Implémentez les méthodes de notifications (`update`) dans les classes concrètes Souscripteur. La majorité des souscripteurs va vouloir des données du contexte qui concernent l’événement en question. Vous pouvez les envoyer en paramètre de la méthode de notification.
    
    Mais il y a une autre possibilité. Lors de la réception d’une notification, le souscripteur peut aller chercher les données directement dans la notification. Dans ce cas, le diffuseur doit s’envoyer lui-même en paramètre de la méthode `update`. On peut également lier un diffuseur à un souscripteur de manière permanente dans le constructeur, mais cette possibilité est moins flexible.
    
7.  Le client doit créer tous les souscripteurs nécessaires et les enregistrer auprès des diffuseurs.
    

## Avantages et inconvénients

- *Principe ouvert/fermé*. Vous pouvez ajouter de nouvelles classes souscripteur sans avoir à modifier le code du diffuseur (et inversement si vous avez une interface diffuseur).
- Vous pouvez établir des relations entre les objets lors du lancement de l’application.

- Les souscripteurs sont avertis dans un ordre aléatoire.

## Liens avec les autres patrons

- La [Chaîne de responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility), la [Commande](https://refactoring.guru/fr/design-patterns/command), le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) et l’[Observateur](https://refactoring.guru/fr/design-patterns/observer) proposent différentes solutions pour associer les demandeurs et les récepteurs.
    
    - La *chaîne de responsabilité* envoie une demande ordonnée qui est passée tout au long d’une chaîne dynamique de récepteurs potentiels, jusqu’à ce que l’un d’entre eux décide de la traiter.
    - La *commande* établit des connexions unidirectionnelles entre les demandeurs et les récepteurs.
    - Le *médiateur* élimine les liens directs entre les demandeurs et les récepteurs, et les force à communiquer indirectement via un objet médiateur.
    - L’*observateur* permet aux récepteurs de s’inscrire et de se désinscrire dynamiquement à la réception des demandes.
- La différence entre le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) et l’[Observateur](https://refactoring.guru/fr/design-patterns/observer) est souvent très fine. Dans la majorité des cas, vous pouvez implémenter l’un ou l’autre, mais parfois vous pouvez les utiliser simultanément. Regardons comment faire.
    
    Le but principal du *médiateur* est d’éliminer les dépendances mutuelles entre un ensemble de composants du système. À la place, ces composants peuvent devenir dépendants d’un unique objet médiateur. Le but de l’*observateur* est d’établir des connexions dynamiques à sens unique entre les objets, où certains objets peuvent être les subordonnés d’autres objets.
    
    Il existe une implémentation populaire du *médiateur* qui repose sur l’*observateur*. L’objet médiateur joue le rôle du diffuseur et les composants agissent comme des souscripteurs qui s’inscrivent et se désinscrivent des événements du médiateur. Lorsque ce type de conception est mis en place, le *médiateur* ressemble de près à l’*observateur*.
    
    Si vous êtes un peu perdu, rappelez-vous qu’il y a plusieurs manières d’implémenter le médiateur. Par exemple, vous pouvez associer de manière permanente tous les composants au même objet médiateur. Cette implémentation ne ressemblera pas à l’*observateur*, mais sera tout de même une instance du patron de conception médiateur.
    
    Maintenant, imaginez un programme dont tous les composants sont devenus des diffuseurs, permettant des connexions dynamiques les uns avec les autres. Nous n’aurons pas d’objet médiateur centralisé, seulement un ensemble d’observateurs distribués.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_6151b7cbd3cf48b1926f235607565ad2.svg"/>](https://refactoring.guru/fr/design-patterns/observer/java/example "Patrons de conception : Observateur en Java") [<img width="43" height="56" src="../../_resources/csharp_292ebba4cc7a4d7aa8e46cee18176793.svg"/>](https://refactoring.guru/fr/design-patterns/observer/csharp/example "Patrons de conception : Observateur en C#")[<img width="43" height="56" src="../../_resources/cpp_be87849b3a3148ab8b9a89dca2b42673.svg"/>](https://refactoring.guru/fr/design-patterns/observer/cpp/example "Patrons de conception : Observateur en C++")[<img width="43" height="56" src="../../_resources/php_e9bc4d4fb943486f988f530ccf3abfc0.svg"/>](https://refactoring.guru/fr/design-patterns/observer/php/example "Patrons de conception : Observateur en PHP")[<img width="43" height="56" src="../../_resources/python_3514e806444a4260b07127e40da1cbbd.svg"/>](https://refactoring.guru/fr/design-patterns/observer/python/example "Patrons de conception : Observateur en Python")[<img width="43" height="56" src="../../_resources/ruby_a8be9dad7b1941139d347ab5df4c1185.svg"/>](https://refactoring.guru/fr/design-patterns/observer/ruby/example "Patrons de conception : Observateur en Ruby")[<img width="43" height="56" src="../../_resources/swift_297f0b7036684941948abe0a0044f0dc.svg"/>](https://refactoring.guru/fr/design-patterns/observer/swift/example "Patrons de conception : Observateur en Swift")[<img width="43" height="56" src="../../_resources/typescript_b9e0a01ecc83421981227f7d219bb7bf.svg"/>](https://refactoring.guru/fr/design-patterns/observer/typescript/example "Patrons de conception : Observateur en TypeScript")[<img width="43" height="56" src="../../_resources/go_65198590a21e4eb08bfcebe5757456bc.svg"/>](https://refactoring.guru/fr/design-patterns/observer/go/example "Patrons de conception : Observateur en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_2e1e682d15924113a7ff33b00aa.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[État](https://refactoring.guru/fr/design-patterns/state)

#### Retour

[Mémento](https://refactoring.guru/fr/design-patterns/memento)

[<img width="245" height="375" src="../../_resources/web-cover-fr_468b488e20324e2595fde02d2bd27096.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/observer "English") [Español](https://refactoring.guru/es/design-patterns/observer "Español") [Français](https://refactoring.guru/fr/design-patterns/observer "Français") [Polski](https://refactoring.guru/pl/design-patterns/observer "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/observer "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/observer "Русский") [Українська](https://refactoring.guru/uk/design-patterns/observer "Українська") [中文](https://refactoringguru.cn/design-patterns/observer "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_483c527d4b144ecd81a3f5d8ebf4e9f8.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_bc4768ed65be455e9ff3b3eb8d3fb61b.png)

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
 ![Organization address](../../_resources/fa-building_f037e27a79aa411697579406054ac05b.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)