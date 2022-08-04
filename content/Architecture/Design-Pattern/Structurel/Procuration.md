---
title: Procuration / Proxy
updated: 2021-12-26 15:28:15Z
created: 2021-12-26 15:10:33Z
source: https://refactoring.guru/fr/design-patterns/proxy
tags:
  - design_pattern
  - structurel
---

La **Procuration** qui vous permet d’utiliser un substitut pour un objet. Elle donne le contrôle sur l’objet original, vous permettant d’effectuer des manipulations avant ou après que la demande ne lui parvienne.

![Patron de conception&nbsp;procuration](../../_resources/proxy_8bec69d3da76480da2d3e7595a0dd136.png)

## Problème

Pourquoi vouloir maîtriser l’accès à un objet ? Voici un exemple : vous avez un énorme objet qui consomme beaucoup de ressources, mais vous n’en avez besoin que de temps en temps.

![Problème résolu grâce au patron de conception procuration](../../_resources/problem-fr_4516c5a9474b418cb15ebc4ea43160bb.png)

Les requêtes sur la base de données sont parfois très lentes.

Vous pouvez recourir à l’instanciation paresseuse : créer l’objet uniquement lorsque vous en avez besoin. Vous devrez implémenter une initialisation différée dans tous les clients pour l’objet concerné. Malheureusement, vous allez vous retrouver avec beaucoup de code dupliqué.

Dans le meilleur des mondes, nous voudrions mettre ce code directement dans la classe de l’objet, mais ce n’est pas toujours possible. La classe pourrait par exemple faire partie d’une application externe non modifiable.

## Solution

Ce patron de conception vous propose de créer une classe procuration qui a la même interface que l’objet du service original. Vous passez ensuite l’objet procuration à tous les clients de l’objet original. Lors de la réception d’une demande d’un client, la procuration crée l’objet du service original et lui délègue la tâche.

<img width="510" height="0" src="../../_resources/solution-fr_d108a79cf5f0490d9ebed8f7e2bfa2d3.png"/>

La procuration se déguise en objet base de données. Elle peut gérer l’instanciation paresseuse et mettre en cache le résultat sans que le client ou que l’objet de la base de données ne le remarque.

Mais quel est son intérêt ? Si vous voulez lancer un traitement avant ou après avoir appliqué la logique principale de la classe, la procuration peut intervenir sans modifier cette dernière. Elle implémente la même interface que la classe originale, et peut donc être passée en paramètre à n’importe quel client qui attend l’objet du service original.

## Analogie

<img width="540" height="0" src="../../_resources/live-example_9e4ab5179b8f478297bf488e9c063341.png"/>

Une carte de crédit peut être utilisée au même titre que de l’argent liquide.

Une carte de crédit est une procuration pour un compte bancaire, qui est une procuration pour une liasse de billets. Ils implémentent la même interface : tous deux peuvent être utilisés pour effectuer un paiement. Le consommateur apprécie grandement, car il n’a pas besoin de garder une grosse somme en liquide sur lui. Le commerçant est également très heureux, car les transactions sont versées électroniquement vers son compte en banque, sans courir le risque d’égarer son dépôt ou de se le faire voler sur le chemin de la banque.

## Structure

<img width="370" height="0" src="../../_resources/structure_7ec857c0c80941b8af769751a190a5dc.png"/>

1.  L’**Interface Service** déclare l’interface du service. La procuration doit implémenter cette interface afin de pouvoir se déguiser en objet du service.
    
2.  Le **Service** est une classe qui fournit la logique métier dont vous voulez vous servir.
    
3.  La **Procuration** est une classe dotée d’un attribut qui pointe vers un objet service. Une fois que la procuration a lancé tous ses traitements (instanciation paresseuse, historisation des logs, vérification des droits, mise en cache, etc.), elle envoie la demande à l’objet du service.
    
    En général, les procurations gèrent le cycle de vie de leurs objets service.
    
4.  Le **Client** passe par la même interface pour travailler avec les services et les procurations. Il est ainsi possible de passer une procuration à n’importe quel code qui attend un objet service.
    

## Pseudo-code

L’exemple suivant montre comment le patron de conception **Procuration** nous aide à utiliser l’instanciation paresseuse et la mise en cache pour intégrer une librairie YouTube externe.

<img width="490" height="0" src="../../_resources/example_d6dd498295c04089bc59f6341952feb2.png"/>

Résultats obtenus pour la mise en cache d’un service à l’aide d’une procuration.

La librairie nous fournit une classe pour télécharger des vidéos. Malheureusement, elle n’est pas très efficace. Si l’application client demande une même vidéo à plusieurs reprises, la librairie va la télécharger encore et encore, plutôt que de la mettre en cache après la première utilisation.

La classe procuration implémente la même interface que l’outil de téléchargement original et délègue tout le travail à ce dernier. En revanche, elle garde la trace de tous les fichiers téléchargés et retourne les données du cache lorsque l’application fait plusieurs fois appel à la même vidéo.

```
// L’interface d’un service distant
interface ThirdPartyYouTubeLib is
    method listVideos()
    method getVideoInfo(id)
    method downloadVideo(id)

// La méthode d’implémentation concrète d’un connecteur de
// service. Les méthodes de cette classe peuvent demander des
// informations à YouTube. La vitesse de la demande dépend de la
// connexion Internet de l’utilisateur, ainsi que de celle de
// YouTube. L’application sera plus lente si plusieurs demandes
// sont envoyées en même temps, même si elles recherchent les
// mêmes informations.
class ThirdPartyYouTubeClass implements ThirdPartyYouTubeLib is
    method listVideos() is
        // Envoie une demande via une API à YouTube.

    method getVideoInfo(id) is
        // Récupère les métadonnées d’une vidéo.

    method downloadVideo(id) is
        // Télécharge un fichier vidéo sur YouTube.

// Pour économiser de la bande passante, nous pouvons mettre en
// cache les résultats des demandes et les mettre de côté
// temporairement. Mais vous n’allez pas forcément pouvoir
// écrire ce code directement dans la classe du service. Elle
// pourrait provenir d’une librairie externe ou avoir été
// définie comme `final`. Pour cette raison, nous écrivons le
// code de la mise en cache dans une nouvelle classe procuration
// qui implémente la même interface que la classe du service.
// Elle délègue les tâches à l’objet du service lorsque les
// demandes doivent vraiment être envoyées.
class CachedYouTubeClass implements ThirdPartyYouTubeLib is
    private field service: ThirdPartyYouTubeLib
    private field listCache, videoCache
    field needReset

    constructor CachedYouTubeClass(service: ThirdPartyYouTubeLib) is
        this.service = service

    method listVideos() is
        if (listCache == null || needReset)
            listCache = service.listVideos()
        return listCache

    method getVideoInfo(id) is
        if (videoCache == null || needReset)
            videoCache = service.getVideoInfo(id)
        return videoCache

    method downloadVideo(id) is
        if (!downloadExists(id) || needReset)
            service.downloadVideo(id)

// La classe GUI qui fonctionnait auparavant avec un objet
// Service n’a pas besoin d’être modifiée tant qu’elle interagit
// avec lui par le biais d’une interface. Nous pouvons passer en
// toute sécurité un objet procuration à la place d’un objet du
// service, car ils implémentent la même interface.
class YouTubeManager is
    protected field service: ThirdPartyYouTubeLib

    constructor YouTubeManager(service: ThirdPartyYouTubeLib) is
        this.service = service

    method renderVideoPage(id) is
        info = service.getVideoInfo(id)
        // Affiche la page de la vidéo.

    method renderListPanel() is
        list = service.listVideos()
        // Affiche la liste des miniatures des vidéos.

    method reactOnUserInput() is
        renderVideoPage()
        renderListPanel()

// L’application peut configurer les procurations à la volée.
class Application is
    method init() is
        aYouTubeService = new ThirdPartyYouTubeClass()
        aYouTubeProxy = new CachedYouTubeClass(aYouTubeService)
        manager = new YouTubeManager(aYouTubeProxy)
        manager.reactOnUserInput()

```

## Possibilités d’application

La procuration possède de nombreux usages. Passons en revue les plus populaires.

Instanciation paresseuse (procuration virtuelle). À utiliser lorsque l’objet du service est très consommateur en ressources système car il est actif en permanence, mais vous n’en avez pas tout le temps besoin.

Vous pouvez différer l’initialisation de l’objet plutôt que de le créer au lancement de l’application.

Vérification des droits (procuration de protection). Si vous avez besoin de limiter l’accès de vos clients à l’objet du service, par exemple si vos objets sont des parties cruciales d’un système d’exploitation et que les clients sont différentes applications lancées (dont certaines sont malveillantes).

La procuration peut ne transmettre une demande à l’objet du service que si les identifiants du client remplissent certains critères.

Lancement local d’un service distant (procuration à distance). L’objet du service se situe sur un serveur distant.

Dans ce cas, la procuration envoie la demande par le réseau en s’occupant de gérer tous les détails compliqués de la gestion du réseau.

Demande de logs (procuration de logs). Pour garder un historique des demandes passées auprès de l’objet du service.

La procuration peut garder la trace de toutes les demandes passées au service.

Mettre en cache les résultats des demandes (procuration de cache). Si vous voulez mettre en cache les résultats des demandes faites au client et gérer le cycle de vie de ce cache, principalement lorsque les résultats sont imposants.

La procuration peut gérer la mise en cache des demandes récurrentes qui retournent toujours le même résultat. Les paramètres des demandes peuvent servir de clé.

Référencement intelligent. Si vous voulez vous débarrasser d’un objet très consommateur en ressources lorsqu’aucun client ne l’utilise.

La procuration peut garder la trace des clients qui ont récupéré une référence vers l’objet du service ou vers ses résultats. De temps en temps, la procuration peut faire le tour des clients et vérifier s’ils sont toujours actifs. Si la liste des clients est vide, la procuration peut supprimer l’objet du service afin de libérer des ressources.

Elle peut également savoir si le client avait modifié l’objet du service. Les objets non modifiés peuvent ensuite être réutilisés par les autres clients.

## Mise en œuvre

1.  Si le service n’a pas encore d’interface, créez-en une pour rendre la procuration et les objets du service interchangeables. Il n’est pas toujours possible d’extraire l’interface de la classe du service, car vous devez effectuer des modifications pour que tous les clients du service utilisent cette interface. Heureusement, nous avons un plan B. Transformez la procuration en sous-classe de la classe service, afin qu’elle hérite de l’interface du service.
    
2.  Créez la classe procuration. Elle doit inclure un attribut qui stocke la référence au service. En général, les procurations créent et gèrent le cycle de vie de leurs services. En de rares occasions, un service est passé à la procuration par le biais d’un constructeur du client.
    
3.  Mettez en place les méthodes de la procuration et leur fonctionnement. Dans la plupart des cas, la procuration doit déléguer le travail à l’objet du service après avoir lancé ses traitements.
    
4.  Réfléchissez à l’implémentation d’une méthode de création qui décide si votre client doit utiliser directement le service ou passer par la procuration. Il peut s’agir d’une méthode statique toute simple ou d’une classe procuration avec une méthode fabrique complète.
    
5.  Envisagez également l’implémentation d’une instanciation paresseuse pour l’objet du service.
    

## Avantages et inconvénients

- Vous pouvez contrôler l’objet du service sans que le client ne s’en aperçoive.
- Vous pouvez gérer le cycle de vie de l’objet du service si les clients ne s’en occupent pas.
- La procuration fonctionne même si l’objet du service n’est pas prêt ou pas disponible.
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouvelles procurations sans toucher au service ou aux clients.

- Le code peut devenir plus complexe puisque vous devez y introduire de nombreuses classes.
- La réponse du service peut mettre plus de temps à arriver.

## Liens avec les autres patrons

- L’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter) procure une interface différente à l’objet emballé, la [Procuration](https://refactoring.guru/fr/design-patterns/proxy) lui fournit la même interface et le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) lui met à disposition une interface améliorée.
    
- La [Façade](https://refactoring.guru/fr/design-patterns/facade) et la [Procuration](https://refactoring.guru/fr/design-patterns/proxy) ont une similarité : ils mettent en tampon une entité complexe et l’initialisent individuellement. Contrairement à la *façade*, la *procuration* implémente la même interface que son objet service, ce qui les rend interchangeables.
    
- Le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) et la [Procuration](https://refactoring.guru/fr/design-patterns/proxy) ont des structures similaires, mais des intentions différentes. Ces deux patrons sont bâtis sur le principe de la composition, où un objet est censé déléguer certains traitements à un autre. La différence est qu’en principe, la *procuration* gère elle-même le cycle de vie de son objet service, alors que la composition des *décorateurs* est toujours contrôlée par le client.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_a0eb42f5aebc48258309a769bd3f2ca8.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/java/example "Patrons de conception : Procuration en Java") [<img width="43" height="56" src="../../_resources/csharp_f77b5e322a7a4c45bad3ce31c72ce495.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/csharp/example "Patrons de conception : Procuration en C#")[<img width="43" height="56" src="../../_resources/cpp_fb7eaa54ec944f68b19b6f01db34d95f.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/cpp/example "Patrons de conception : Procuration en C++")[<img width="43" height="56" src="../../_resources/php_66efd986b0f74c009b4526e2dd8e97e2.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/php/example "Patrons de conception : Procuration en PHP")[<img width="43" height="56" src="../../_resources/python_b5ac4f08505047139db5727df5ad4b41.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/python/example "Patrons de conception : Procuration en Python")[<img width="43" height="56" src="../../_resources/ruby_efbb5e27b6f44d13a093639471f4fa94.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/ruby/example "Patrons de conception : Procuration en Ruby")[<img width="43" height="56" src="../../_resources/swift_b7df4c4b325f4b8da153de520e4f1fc2.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/swift/example "Patrons de conception : Procuration en Swift")[<img width="43" height="56" src="../../_resources/typescript_931dfe2231594fe099522d266bbe80c7.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/typescript/example "Patrons de conception : Procuration en TypeScript")[<img width="43" height="56" src="../../_resources/go_6ac91ff300584d2b910e5f723d5eb211.svg"/>](https://refactoring.guru/fr/design-patterns/proxy/go/example "Patrons de conception : Procuration en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_6aaf8bfa7dfc4b79989512b18a2.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Patrons comportementaux](https://refactoring.guru/fr/design-patterns/behavioral-patterns)

#### Retour

[Poids mouche](https://refactoring.guru/fr/design-patterns/flyweight)

[<img width="245" height="375" src="../../_resources/web-cover-fr_8732287de86e489a9a66f92c20e1acbe.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/proxy "English") [Español](https://refactoring.guru/es/design-patterns/proxy "Español") [Français](https://refactoring.guru/fr/design-patterns/proxy "Français") [Polski](https://refactoring.guru/pl/design-patterns/proxy "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/proxy "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/proxy "Русский") [Українська](https://refactoring.guru/uk/design-patterns/proxy "Українська") [中文](https://refactoringguru.cn/design-patterns/proxy "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_859d0ab064004adeafcbffedc354ade3.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_a1e60a23c42c4a59be2f8b6a407293ff.png)

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
 ![Organization address](../../_resources/fa-building_87bf62543bf64cdc9482b81a7c692725.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)