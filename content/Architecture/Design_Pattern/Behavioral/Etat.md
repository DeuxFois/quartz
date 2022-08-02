---
title: État / State
updated: 2021-12-26 15:29:10Z
created: 2021-12-26 15:17:56Z
source: https://refactoring.guru/fr/design-patterns/state
tags:
  - behavioral
  - design_pattern
---

**État** permet de modifier le comportement d’un objet lorsque son état interne change. L’objet donne l’impression qu’il change de classe.

![Patron de conception&nbsp;état](../../_resources/state-fr_7368c6b587874b7e830f220d8295c19a.png)

## Problème

Le patron de conception est très proche du concept de l’[Automate fini](https://fr.wikipedia.org/wiki/Automate_fini).

![Automate fini](../../_resources/problem1_e0c985d3f29c40609bb5ab30b1284951.png)

Automate fini.

Le principe repose sur le fait qu’un programme possède un nombre *fini* d'*états*. Le programme se comporte différemment selon son état et peut en changer instantanément. En revanche, selon l’état dans lequel il se trouve, certains états ne lui sont pas accessibles. Ces règles de changement d’état sont appelées *transitions*. Elles sont également finies et prédéterminées.

Vous pouvez appliquer cette approche aux objets. Imaginons une classe `Document`. Un document peut être dans l’un des trois états suivants : `Brouillon` (draft), `Modération` et `Publié`. La méthode `publier` du document fonctionne un peu différemment en fonction de son état :

- Dans `Brouillon`, elle passe le document en modération.
- Dans `Modération`, elle rend le document public si l’utilisateur actuel est un administrateur.
- Dans `Publié`, elle ne fait rien du tout.

<img width="560" height="0" src="../../_resources/problem2-fr_ce199476de484ec080252139b51e6a21.png"/>

Les états et transitions possibles d’un objet document.

Les automates sont généralement implémentés avec beaucoup d’opérateurs conditionnels (`if` ou `switch`) qui choisissent le comportement approprié en fonction de l’état actuel de l’objet. Cet « état » se limite souvent à un ensemble de valeurs dans les attributs de l’objet. Même si vous n’avez jamais entendu parler des automates finis, vous avez probablement déjà implémenté un état au moins une fois. La structure du code suivant vous dit-elle quelque chose ?

```
class Document is
    field state: string
    // ...
    method publish() is
        switch (state)
            "draft":
                state = "moderation"
                break
            "moderation":
                if (currentUser.role == 'admin')
                    state = "published"
                break
            "published":
                // Do nothing.
                break
    // ...

```

La plus grosse faiblesse de l’automate fini devient visible lorsque l’on commence à ajouter de plus en plus d’états et de comportements qui en sont dépendants à la classe `Document`. La majorité des méthodes va contenir d’énormes blocs de conditions qui vont choisir le comportement d’une méthode en fonction de l’état actuel. Ce genre de code est très difficile à maintenir, car tout changement dans la logique de transition demande de modifier les états conditionnels dans chaque méthode.

Plus le projet évolue et plus cette faiblesse s’aggrave. Il est très difficile de prédire tous les états et transitions possibles lors de la phase de conception. Un automate fini doté d’un nombre limité de conditions peut se transformer en un bazar pas possible au bout d’un certain temps.

## Solution

Le patron de conception état propose de créer de nouvelles classes pour tous les états possibles d’un objet et d’extraire les comportements liés aux états dans ces classes.

Plutôt que d’implémenter tous les comportements de lui-même, l’objet original que l’on nomme *contexte*, stocke une référence vers un des objets état qui représente son état actuel. Il délègue tout ce qui concerne la manipulation des états à cet objet.

<img width="490" height="0" src="../../_resources/solution-fr_185ff9d6455c44a29778798580e63d51.png"/>

Document délègue la tâche à un objet état.

Pour faire passer le contexte dans un autre état, remplacez l’objet état par un autre qui représente son nouvel état. Vous ne pourrez le faire que si toutes les classes suivent la même interface et si le contexte utilise cette dernière pour manipuler ces objets.

Cette structure ressemble de près au patron de conception [Stratégie](https://refactoring.guru/fr/design-patterns/strategy), mais il y a une différence majeure. Dans le patron de conception état, les états ont de la visibilité entre eux et peuvent lancer les transitions d’un état à l’autre, alors que les stratégies ne peuvent pas se voir.

## Analogie

Les boutons de votre smartphone fonctionnent différemment selon l’état de l’appareil :

- Si le téléphone est déverrouillé, appuyer sur des boutons lance différentes fonctionnalités.
- Si le téléphone est verrouillé, appuyer sur n’importe quel bouton envoie sur l’écran de déverrouillage.
- Si la batterie du téléphone est faible, appuyer sur n’importe quel bouton montre l’écran de charge.

## Structure

<img width="540" height="0" src="../../_resources/structure-fr_212cf9cf716e49088232c0d41657b24d.png"/>

1.  Le **Contexte** stocke une référence vers un des objets concrets État et lui délègue toutes les tâches concernant les états. Il utilise l’interface état pour communiquer avec l’objet état. Il expose un setter pour lui passer un nouvel état.
    
2.  L’interface **État** déclare les méthodes spécifiques aux états. Ces méthodes doivent fonctionner avec tous les états concrets : des méthodes inutiles qui ne sont jamais appelées à l’intérieur de vos états sont à proscrire.
    
3.  Les **États Concrets** fournissent leurs propres implémentations aux méthodes qui agissent sur les états. Pour éviter d’écrire le même code dans les différents états, vous pouvez créer des classes abstraites intermédiaires qui encapsulent les comportements identiques.
    
    Les états peuvent garder une référence vers le contexte. Grâce à cette référence, l’état peut récupérer des informations depuis le contexte et lancer des transitions.
    
4.  Le contexte et les états concrets peuvent modifier le prochain état du contexte et lancer une transition en remplaçant l’état lié au contexte.
    

## Pseudo-code

Dans cet exemple, le patron de conception **État** permet aux touches du lecteur multimédia d’avoir un comportement relatif à l’état actuel de la lecture.

<img width="590" height="0" src="../../_resources/example_7f3ee819c1654a1da41ad9c13a1844f0.png"/>

Un exemple de modification du comportement de l’objet effectué à l’aide d’objets état.

L’objet principal du lecteur est toujours associé à un objet état qui effectue la majeure partie du travail pour le lecteur. Certaines actions remplacent l’état actuel du lecteur par un autre, modifiant sa manière de réagir aux interactions de l’utilisateur.

```
// La classe lecteurAudio prend le rôle du contexte. Elle
// maintient également une référence vers l’instance de l’une
// des classes état qui représente l’état actuel du lecteur
// audio.
class AudioPlayer is
    field state: State
    field UI, volume, playlist, currentSong

    constructor AudioPlayer() is
        this.state = new ReadyState(this)

        // Le contexte délègue la gestion des interventions de
        // l’utilisateur à un objet état. Le résultat va
        // évidemment dépendre de l'état actuel, puisque chaque
        // état réagit différemment aux manipulations des
        // utilisateurs.
        UI = new UserInterface()
        UI.lockButton.onClick(this.clickLock)
        UI.playButton.onClick(this.clickPlay)
        UI.nextButton.onClick(this.clickNext)
        UI.prevButton.onClick(this.clickPrevious)

    // Les autres objets doivent pouvoir changer l'état du
    // lecteur audio.
    method changeState(state: State) is
        this.state = state

    // Les méthodes de l’UI délèguent l’exécution à l'état
    // actuel.
    method clickLock() is
        state.clickLock()
    method clickPlay() is
        state.clickPlay()
    method clickNext() is
        state.clickNext()
    method clickPrevious() is
        state.clickPrevious()

    // Un état peut appeler les méthodes d’un service sur le
    // contexte.
    method startPlayback() is
        // ...
    method stopPlayback() is
        // ...
    method nextSong() is
        // ...
    method previousSong() is
        // ...
    method fastForward(time) is
        // ...
    method rewind(time) is
        // ...

// La classe de base état déclare des méthodes que tous les
// états concrets doivent obligatoirement implémenter et fournit
// aussi une référence arrière vers l’objet du contexte associé
// à l’état. Les états peuvent utiliser cette référence arrière
// pour permuter l’état du contexte.
abstract class State is
    protected field player: AudioPlayer

    // Le contexte s’envoie lui-même au constructeur de l’état,
    // permettant de donner un coup de pouce à l'état pour
    // récupérer des données contextuelles si nécessaire.
    constructor State(player) is
        this.player = player

    abstract method clickLock()
    abstract method clickPlay()
    abstract method clickNext()
    abstract method clickPrevious()

// Les états concrets implémentent différents comportements
// associés à un état du contexte.
class LockedState extends State is

    // Lorsque vous déverrouillez un lecteur verrouillé, il peut
    // prendre l’un des deux états.
    method clickLock() is
        if (player.playing)
            player.changeState(new PlayingState(player))
        else
            player.changeState(new ReadyState(player))

    method clickPlay() is
        // Verrouillé, ne rien faire.

    method clickNext() is
        // Verrouillé, ne rien faire.

    method clickPrevious() is
        // Verrouillé, ne rien faire.

// Ils peuvent également déclencher les transitions de l’état
// dans le contexte.
class ReadyState extends State is
    method clickLock() is
        player.changeState(new LockedState(player))

    method clickPlay() is
        player.startPlayback()
        player.changeState(new PlayingState(player))

    method clickNext() is
        player.nextSong()

    method clickPrevious() is
        player.previousSong()

class PlayingState extends State is
    method clickLock() is
        player.changeState(new LockedState(player))

    method clickPlay() is
        player.stopPlayback()
        player.changeState(new ReadyState(player))

    method clickNext() is
        if (event.doubleclick)
            player.nextSong()
        else
            player.fastForward(5)

    method clickPrevious() is
        if (event.doubleclick)
            player.previous()
        else
            player.rewind(5)

```

## Possibilités d’application

Utilisez le patron de conception état lorsque le comportement de l’un de vos objets varie en fonction de son état, qu’il y a beaucoup d’états différents et que ce code change souvent.

Ce patron vous propose d’extraire tout le code lié aux états et de le mettre dans des classes distinctes. Ceci vous permet d’ajouter de nouveaux états ou de modifier ceux qui existent indépendamment des autres, et de réduire les coûts de maintenance.

Utilisez ce patron si l’une de vos classes est polluée par d’énormes blocs conditionnels qui modifient le comportement de la classe en fonction de la valeur de ses attributs.

Le patron de conception état vous permet d’extraire des branches de ces conditions et de les transformer en méthodes dans les classes état. Tout en faisant vos modifications, vous pouvez retirer les attributs temporaires et les méthodes qui gèrent les changements d’état du code de votre classe principale.

Utilisez ce patron de conception si vous avez trop de code dupliqué dans des états et transitions similaires de votre automate.

Le patron de conception état vous permet d’assembler des hiérarchies de classes état et de réduire la duplication de code en regroupant le code commun dans des classes de base abstraites.

## Mise en œuvre

1.  Choisissez la classe qui va prendre le rôle du contexte. Cette classe peut déjà exister et posséder du code qui gère les états, mais vous pouvez en créer une nouvelle si ce code est réparti dans plusieurs classes.
    
2.  Déclarez l’interface état. Vous pourriez très bien vous contenter de recopier toutes les méthodes déclarées dans le contexte, mais ne reprenez que celles qui concernent les états.
    
3.  Pour chaque état, créez une classe qui dérive de l’interface état. Parcourez ensuite les méthodes du contexte pour identifier le code qui concerne cet état et recopiez-le dans votre nouvelle classe.
    
    En effectuant cette manipulation, vous pourriez tomber sur des membres privés dans le contexte. Il y a plusieurs moyens de contournement :
    
    - Rendez ces attributs ou ces méthodes publics.
    - Transformez le comportement que vous extrayez en méthode publique que vous mettez dans le contexte, puis appelez-la depuis la classe état. Ce n’est pas le plus esthétique, mais vous pourrez revenir dessus plus tard.
    - Imbriquez les classes état dans la classe contexte si votre langage de programmation le permet.
4.  Dans votre classe contexte, ajoutez un attribut qui référence le type de l’interface état et un setter public qui permet de redéfinir la valeur de cet attribut.
    
5.  Parcourez à nouveau les méthodes du contexte et remplacez les conditions concernant les états par des appels aux méthodes correspondantes de l’objet état.
    
6.  Pour changer l’état du contexte, créez une instance de l’une des classes état et passez-la au contexte. Ceci peut être fait à l’intérieur du contexte, dans les différents états ou dans le client. Où qu’elle soit, cette classe devient dépendante de la classe concrète État qu’elle instancie.
    

## Avantages et inconvénients

- *Principe de responsabilité unique*. Organisez le code lié aux différents états dans des classes séparées.
- *Principe ouvert/fermé*. Ajoutez de nouveaux états sans modifier les classes état ou le contexte existants.
- Simplifiez le code du contexte en éliminant les gros blocs conditionnels de l’automate.

- L’utilisation de ce patron est un peu exagérée si votre automate n’a que quelques états ou qu’il y a peu de transitions.

## Liens avec les autres patrons

- Le [Pont](https://refactoring.guru/fr/design-patterns/bridge), l’[État](https://refactoring.guru/fr/design-patterns/state), la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) (et dans une certaine mesure l’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter)) ont des structures très similaires. En effet, ces patrons sont basés sur la composition, qui délègue les tâches aux autres objets. Cependant, ils résolvent différents problèmes. Un patron n’est pas juste une recette qui vous aide à structurer votre code d’une certaine manière. C’est aussi une façon de communiquer aux autres développeurs le problème qu’il résout.
    
- L’[État](https://refactoring.guru/fr/design-patterns/state) peut être considéré comme une extension de la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy). Ces deux patrons de conception sont basés sur la composition : ils changent le comportement du contexte en déléguant certaines tâches aux objets assistant. La *stratégie* rend ces objets complètement indépendants sans aucune visibilité l’un sur l’autre. Cependant, l’*état* n’impose pas de restrictions sur les dépendances entre les états concrets, et leur laisse modifier l’état du contexte à volonté.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_38e38a44146a41f085d0963bf27055be.svg"/>](https://refactoring.guru/fr/design-patterns/state/java/example "Patrons de conception : État en Java") [<img width="43" height="56" src="../../_resources/csharp_dcb24a4ad5c341f5b3d1305ad5379aca.svg"/>](https://refactoring.guru/fr/design-patterns/state/csharp/example "Patrons de conception : État en C#")[<img width="43" height="56" src="../../_resources/cpp_24eb3e52cf52406c89fc6ec8740adf4b.svg"/>](https://refactoring.guru/fr/design-patterns/state/cpp/example "Patrons de conception : État en C++")[<img width="43" height="56" src="../../_resources/php_57b0630edcdb438d8a4d674e2e20819f.svg"/>](https://refactoring.guru/fr/design-patterns/state/php/example "Patrons de conception : État en PHP")[<img width="43" height="56" src="../../_resources/python_04f176c1ead64aadbd2a238fb21ce06d.svg"/>](https://refactoring.guru/fr/design-patterns/state/python/example "Patrons de conception : État en Python")[<img width="43" height="56" src="../../_resources/ruby_21c68026b9194c1d8d47445829541392.svg"/>](https://refactoring.guru/fr/design-patterns/state/ruby/example "Patrons de conception : État en Ruby")[<img width="43" height="56" src="../../_resources/swift_0a5e43e4853747eda8130a799801c41c.svg"/>](https://refactoring.guru/fr/design-patterns/state/swift/example "Patrons de conception : État en Swift")[<img width="43" height="56" src="../../_resources/typescript_83004feed206441db270c9295c520a87.svg"/>](https://refactoring.guru/fr/design-patterns/state/typescript/example "Patrons de conception : État en TypeScript")[<img width="43" height="56" src="../../_resources/go_1afbe281ebeb42c59c3e6b49f6a9a082.svg"/>](https://refactoring.guru/fr/design-patterns/state/go/example "Patrons de conception : État en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_75b3edbae34c403987d97b67482.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Stratégie](https://refactoring.guru/fr/design-patterns/strategy)

#### Retour

[Observateur](https://refactoring.guru/fr/design-patterns/observer)

[<img width="245" height="375" src="../../_resources/web-cover-fr_17f812cb8d664ee5953e06b049846025.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/state "English") [Español](https://refactoring.guru/es/design-patterns/state "Español") [Français](https://refactoring.guru/fr/design-patterns/state "Français") [Polski](https://refactoring.guru/pl/design-patterns/state "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/state "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/state "Русский") [Українська](https://refactoring.guru/uk/design-patterns/state "Українська") [中文](https://refactoringguru.cn/design-patterns/state "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_99a2bc13f2944cb2a3b459ecbb639f0a.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_0830521447a542b0b0614e2dd748b019.png)

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
 ![Organization address](../../_resources/fa-building_4c438f64160147cfbe29175337a1960c.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)