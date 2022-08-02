---
title: Itérateur / Iterator
updated: 2021-12-26 15:29:06Z
created: 2021-12-26 15:17:39Z
source: https://refactoring.guru/fr/design-patterns/iterator
tags:
  - behavioral
  - design_pattern
---

**Itérateur** permet de parcourir les éléments d’une collection sans révéler sa représentation interne (liste, pile, arbre, etc.).

![Patron de conception&nbsp;itérateur](../../_resources/iterator-en_501f7a11e44141d492b0baad75db40a4.png)

## Problème

Les collections ne servent que de conteneur pour un groupe d’objets, mais elles demeurent l’un des types de données les plus utilisés en programmation.

![Différents types de collections](../../_resources/problem1_2fc25640bca040988e4287daebb51b84.png)

Différents types de collections.

La majorité des collections stockent leurs éléments dans de simples listes, mais certaines d’entre elles sont basées sur les piles, les arbres, les graphes ou d’autres structures complexes de données.

Quelle que soit sa structure, une collection doit fournir un moyen d’accéder à ses éléments pour permettre au code de les utiliser. Elle doit donner la possibilité de parcourir tous ses éléments sans passer plusieurs fois par les mêmes.

À première vue, cela semble simple pour les collections qui ressemblent à une liste. Vous lancez juste une boucle sur tous les éléments. Mais comment faites-vous pour parcourir séquentiellement les éléments d’une structure complexe de données comme un arbre ? Un jour donné, vous allez peut-être vous en sortir avec un parcours en profondeur. Mais le lendemain, vous allez avoir besoin d’un parcours en largeur. La semaine d’après, vous allez avoir besoin d’autre chose pour établir un parcours aléatoire des éléments de l’arbre.

<img width="600" height="0" src="../../_resources/problem2_0ecb79889e184ac98023693dac74e812.png"/>

Il y a plusieurs manières de parcourir une même collection.

Plus vous ajoutez d’algorithmes différents pour parcourir votre collection et plus vous masquez sa responsabilité principale : stocker efficacement les données. De plus, certains algorithmes sont dédiés à un usage spécifique. Il serait donc bizarre de les ajouter au comportement d’une collection générique.

Le code client qui va se servir de ces collections se fiche probablement de la manière dont elles stockent leurs éléments. Cependant, ces collections fournissent différentes manières d’accéder à leurs éléments, vous couplez donc forcément votre code aux classes de ces collections.

## Solution

Le but du patron de conception itérateur est d’extraire le comportement qui permet de parcourir une collection et de le mettre dans un objet que l’on nomme *itérateur*.

<img width="400" height="0" src="../../_resources/solution1_21256d25a80e4bcf81bc90fdc0941349.png"/>

Les itérateurs implémentent différents algorithmes de parcours. Plusieurs itérateurs peuvent parcourir une même collection simultanément.

En plus d’implémenter l’algorithme de parcours, un objet itérateur encapsule tous les détails comme la position actuelle et le nombre d’éléments restants avant d’atteindre la fin. Grâce à cela, plusieurs itérateurs peuvent parcourir une même collection simultanément et indépendamment les uns des autres.

En général, les itérateurs fournissent une méthode principale pour récupérer les éléments d’une collection. Le client peut appeler la méthode en continu jusqu’à ce qu’elle ne retourne plus rien, ce qui signifie que l’itérateur a parcouru tous les éléments.

Les itérateurs doivent tous implémenter la même interface. Le code client est ainsi compatible avec tous les types de collections et tous les algorithmes de parcours, tant que l’itérateur adéquat existe. Si vous voulez effectuer un parcours un peu spécial dans une collection, il vous suffit de créer une nouvelle classe itérateur, sans toucher à la collection ni au client.

## Analogie

<img width="640" height="0" src="../../_resources/iterator-comic-1-fr_fcc0876115e6408691fe2cd5a9e8bc.png"/>

Différentes manières de parcourir Rome.

Vous projetez de visiter Rome pendant quelques jours et voulez voir ses principaux sites et attractions. Mais une fois sur place, vous risquez de perdre beaucoup de temps à tourner en rond et même de ne pas réussir à trouver le Colisée.

Vous pourriez acheter une application de guide virtuel sur votre smartphone qui vous permettrait de trouver votre chemin. C’est pratique et peu onéreux, et vous pourriez visiter des sites intéressants à volonté.

Vous pourriez également dépenser une partie de votre budget pour engager un guide local qui connait la ville comme sa poche. Il adapterait votre séjour en fonction de vos goûts et de vos attentes, vous ferait visiter toutes les attractions et vous raconterait des histoires passionnantes. Ce serait vraiment génial, mais malheureusement, beaucoup plus cher.

Toutes ces possibilités — des directions aléatoires que vous prenez, l’application sur smartphone ou le guide humain — sont des itérateurs sur une vaste collection de sites et d’attractions dans Rome.

## Structure

<img width="480" height="0" src="../../_resources/structure_1f99bf777bea49bd89ea0878050acb3d.png"/>

1.  L’interface **Itérateur** déclare les opérations nécessaires au parcours d’une collection : récupérer le prochain élément, donner la position actuelle, recommencer la boucle depuis le début, etc.
    
2.  Les **Itérateurs Concrets** implémentent les algorithmes qui servent au parcours d’une collection. L’objet itérateur doit garder la trace du parcours actuel. Grâce à cela, plusieurs itérateurs peuvent parcourir la même collection de manière indépendante.
    
3.  L’interface **Collection** déclare une ou plusieurs méthodes pour récupérer des itérateurs compatibles avec la collection. Le type de retour de la méthode doit être l’interface de l’itérateur afin de permettre aux collections concrètes de renvoyer tous types d’itérateurs.
    
4.  Les **Collections Concrètes** renvoient les nouvelles instances de la classe concrète de l’itérateur lorsque le client en demande une. Vous vous demandez peut-être où l’on doit mettre le reste du code de la collection. Ne vous inquiétez pas, il devrait être dans la même classe. Ces détails ne sont pas très importants pour ce patron de conception, je me permets donc de les omettre.
    
5.  Le **Client** manipule les collections et les itérateurs grâce à leurs interfaces. Ceci permet de ne pas coupler le client avec les classes concrètes et d’utiliser différents itérateurs et collections avec le même code client.
    
    En général, les clients ne créent pas les itérateurs, ils les récupèrent auprès des collections. Mais dans certains cas, le client peut en créer un directement. Le client peut par exemple définir son propre itérateur spécial.
    

## Pseudo-code

Dans cet exemple, le patron de conception **Itérateur** va servir à parcourir un type spécial de collection qui encapsule l’accès au graphe social de Facebook. Cette collection procure plusieurs itérateurs qui parcourent les profils de différentes manières.

<img width="600" height="0" src="../../_resources/example_2ddab3334fe4409a838558aa329de764.png"/>

Un exemple d’itération sur les profils sociaux.

L’itérateur des ‘amis’ peut être utilisé pour parcourir les amis d’un profil donné. L’itérateur des ‘collègues’ fait la même chose, sauf qu’il ignore les amis qui ne travaillent pas dans la même entreprise que la personne sélectionnée. Ces deux itérateurs implémentent une interface commune qui permet aux clients d’aller chercher les profils sans tenir compte des détails de l’implémentation, comme l’identification ou l’envoi de requêtes REST.

Le code client n’est pas couplé aux classes concrètes, car il manipule les collections et les itérateurs en passant par leurs interfaces. Si vous décidez de connecter votre application à un réseau social, vous devez simplement fournir de nouvelles classes de collections et d’itérateurs sans toucher au code existant.

```
// L’interface collection doit déclarer une fabrique pour créer
// des itérateurs. Si vous avez besoin de plusieurs types
// d’itérations dans votre programme, vous pouvez déclarer
// plusieurs méthodes.
interface SocialNetwork is
    method createFriendsIterator(profileId):ProfileIterator
    method createCoworkersIterator(profileId):ProfileIterator

// Chaque collection concrète est couplée à un ensemble de
// classes concrètes Itérateur qu’elle retourne. Mais le client
// en est indépendant, car la signature de ces méthodes renvoie
// des interfaces itérateur.
class Facebook implements SocialNetwork is
    // ...Le code principal de la collection devrait être écrit
    // ici...

    // Code de création de l’itérateur.
    method createFriendsIterator(profileId) is
        return new FacebookIterator(this, profileId, "friends")
    method createCoworkersIterator(profileId) is
        return new FacebookIterator(this, profileId, "coworkers")

// L’interface commune à tous les itérateurs.
interface ProfileIterator is
    method getNext():Profile
    method hasMore():bool

// La classe concrète Itérateur.
class FacebookIterator implements ProfileIterator is
    // L’itérateur a besoin d’une référence à la collection
    // qu’il parcourt.
    private field facebook: Facebook
    private field profileId, type: string

    // Un objet itérateur parcourt la collection indépendamment
    // des autres itérateurs. Il doit donc stocker l’état de
    // l’itération.
    private field currentPosition
    private field cache: array of Profile

    constructor FacebookIterator(facebook, profileId, type) is
        this.facebook = facebook
        this.profileId = profileId
        this.type = type

    private method lazyInit() is
        if (cache == null)
            cache = facebook.socialGraphRequest(profileId, type)

    // Chaque classe concrète Itérateur a sa propre
    // implémentation de l’interface commune Itérateur.
    method getNext() is
        if (hasMore())
            currentPosition++
            return cache[currentPosition]

    method hasMore() is
        lazyInit()
        return currentPosition < cache.length

// Voici une astuce très pratique : vous pouvez passer un
// itérateur à une classe du client, plutôt que de lui donner
// accès à une collection complète. Cette manipulation vous
// permet de ne pas exposer la collection au client.
//
// Elle vous permet également de modifier la façon dont le
// client fonctionne avec les collections lors de son exécution,
// en lui passant un itérateur différent. Cette manipulation
// serait impossible si le code client était couplé aux classes
// concrètes de l’itérateur.
class SocialSpammer is
    method send(iterator: ProfileIterator, message: string) is
        while (iterator.hasMore())
            profile = iterator.getNext()
            System.sendEmail(profile.getEmail(), message)

// La classe application configure les collections et les
// itérateurs, puis les envoie au code client.
class Application is
    field network: SocialNetwork
    field spammer: SocialSpammer

    method config() is
        if working with Facebook
            this.network = new Facebook()
        if working with LinkedIn
            this.network = new LinkedIn()
        this.spammer = new SocialSpammer()

    method sendSpamToFriends(profile) is
        iterator = network.createFriendsIterator(profile.getId())
        spammer.send(iterator, "Very  important  message")

    method sendSpamToCoworkers(profile) is
        iterator = network.createCoworkersIterator(profile.getId())
        spammer.send(iterator, "Very  important  message")

```

## Possibilités d’application

Utilisez le patron de conception itérateur quand la structure de données de votre collection est trop complexe et que vous voulez cacher ces détails aux clients (parce que c’est pratique ou pour des raisons de sécurité).

L’itérateur encapsule le détail du fonctionnement de la structure complexe de données et passe plusieurs méthodes simples au client qui lui permettent d’accéder aux éléments de la collection. En plus d’être très pratique pour le client, ce fonctionnement permet de protéger la collection d’actions maladroites ou malicieuses que le client pourrait entreprendre s’il travaillait directement avec la collection.

Utilisez l’itérateur pour que le code du parcours soit le moins dupliqué possible dans votre application.

Un algorithme qui permet une itération complexe sur la collection a tendance à être très volumineux. Si on le place à l’intérieur de la logique de l’application, cela va masquer la responsabilité du code original et le rendre moins maintenable. Pour rendre le code encore plus simple et propre, vous pouvez écrire l’algorithme de parcours dans leur itérateur.

Utilisez ce patron pour permettre à votre code de naviguer dans des structures de données complexes ou inconnues.

Ce patron procure des interfaces génériques pour les collections et les itérateurs. Si votre code utilise bien ces interfaces, il va pouvoir manipuler toutes les collections (et leurs itérateurs) qui les implémentent.

## Mise en œuvre

1.  Déclarez l’interface itérateur. Elle doit comporter au moins une méthode qui récupère le prochain élément de la collection. Mais pour qu’elle soit vraiment utile, vous devriez en ajouter d’autres, par exemple récupérer l’élément précédent, connaitre la position actuelle ou vérifier la fin de l’itération.
    
2.  Déclarez l’interface de la collection et écrivez une méthode pour récupérer les itérateurs. Le type de la valeur de retour doit être l’interface de l’itérateur. Vous pouvez déclarer des itérateurs supplémentaires si vous pensez en utiliser d’autres.
    
3.  Mettez en place les classes concrètes des itérateurs pour les collections que vous allez parcourir. Un objet itérateur ne peut être lié qu’à une seule instance d’une collection. En général, ce lien est établi dans le constructeur de l’itérateur.
    
4.  Implémentez l’interface de la collection dans les classes de collection. Son but est de fournir un raccourci pour le client qui crée les itérateurs spécifiques à chaque classe de la collection. L’objet collection doit s’envoyer lui-même au constructeur de l’itérateur pour établir un lien entre eux.
    
5.  Écumez le code et remplacez toutes les occurrences de parcours de collection par vos itérateurs. Le client va chercher un nouvel itérateur chaque fois qu’il parcourt les éléments d’une collection.
    

## Avantages et inconvénients

- *Principe de responsabilité unique*. Vous allez nettoyer le code client et les collections en déplaçant les algorithmes de parcours — souvent très lourds — dans des classes séparées.
- *Principe ouvert/fermé*. Vous pouvez implémenter de nouveaux types de collections et d’itérateurs et les utiliser avec le code existant sans rien endommager.
- Vous pouvez parcourir une même collection avec plusieurs itérateurs simultanément, car chacun possède son propre état d’itération.
- Pour cette même raison, vous pouvez arrêter une itération et la reprendre quand vous le souhaitez.

- L’utilisation de ce patron est exagérée si votre application ne se sert que de collections simples.
- Les itérateurs sont parfois moins efficaces que certaines collections spécialisées.

## Liens avec les autres patrons

- Vous pouvez utiliser les [Itérateurs](https://refactoring.guru/fr/design-patterns/iterator) pour parcourir des arbres [Composites](https://refactoring.guru/fr/design-patterns/composite).
    
- Vous pouvez utiliser la [Fabrique](https://refactoring.guru/fr/design-patterns/factory-method) avec l’[Itérateur](https://refactoring.guru/fr/design-patterns/iterator) pour permettre aux sous-classes des collections de renvoyer différents types d’itérateurs compatibles avec les collections.
    
- Vous pouvez utiliser le [Mémento](https://refactoring.guru/fr/design-patterns/memento) avec l’[Itérateur](https://refactoring.guru/fr/design-patterns/iterator) pour récupérer l’état actuel de l’itération et rétablir sa valeur plus tard si besoin.
    
- Vous pouvez utiliser le [Visiteur](https://refactoring.guru/fr/design-patterns/visitor) avec l’[Itérateur](https://refactoring.guru/fr/design-patterns/iterator) pour parcourir une structure de données complexe et lancer un traitement sur ses éléments, même s’ils ont des classes différentes.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_80295644eeeb4e4a972ef25b49bc2f84.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/java/example "Patrons de conception : Itérateur en Java") [<img width="43" height="56" src="../../_resources/csharp_a3bb14453ba248209a05c5cf757e853d.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/csharp/example "Patrons de conception : Itérateur en C#")[<img width="43" height="56" src="../../_resources/cpp_e20bc7135f8f4cbcab34e445aab47a45.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/cpp/example "Patrons de conception : Itérateur en C++")[<img width="43" height="56" src="../../_resources/php_f3b217e7d71441829db577cbd79a775f.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/php/example "Patrons de conception : Itérateur en PHP")[<img width="43" height="56" src="../../_resources/python_331652b8107d4be69f6cb734dfd670d8.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/python/example "Patrons de conception : Itérateur en Python")[<img width="43" height="56" src="../../_resources/ruby_b5f3b8ad6111427db96cb188aeedb21d.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/ruby/example "Patrons de conception : Itérateur en Ruby")[<img width="43" height="56" src="../../_resources/swift_2dca1076918a4873b4081dc406724da9.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/swift/example "Patrons de conception : Itérateur en Swift")[<img width="43" height="56" src="../../_resources/typescript_fafb290b652e4a6cace0e60aeebe87a8.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/typescript/example "Patrons de conception : Itérateur en TypeScript")[<img width="43" height="56" src="../../_resources/go_fbdf882ed399416899d18c0995afa795.svg"/>](https://refactoring.guru/fr/design-patterns/iterator/go/example "Patrons de conception : Itérateur en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_b247475588e24e328a07540bc39.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Médiateur](https://refactoring.guru/fr/design-patterns/mediator)

#### Retour

[Commande](https://refactoring.guru/fr/design-patterns/command)

[<img width="245" height="375" src="../../_resources/web-cover-fr_265d416a363d448ca9c0b2d2d4d267cc.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/iterator "English") [Español](https://refactoring.guru/es/design-patterns/iterator "Español") [Français](https://refactoring.guru/fr/design-patterns/iterator "Français") [Polski](https://refactoring.guru/pl/design-patterns/iterator "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/iterator "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/iterator "Русский") [Українська](https://refactoring.guru/uk/design-patterns/iterator "Українська") [中文](https://refactoringguru.cn/design-patterns/iterator "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_d4a3549cf80e41cb8d56f93adbf43b33.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_32416e0a3bfd423e8b557478b85bede0.png)

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
 ![Organization address](../../_resources/fa-building_ea399000da7f42d690350c9008b03211.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)