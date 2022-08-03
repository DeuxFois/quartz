---
title: Chaîne de responsabilité / Chain of Responsibility
updated: 2021-12-26 15:29:18Z
created: 2021-12-26 15:17:30Z
source: https://refactoring.guru/fr/design-patterns/chain-of-responsibility
tags:
  - behavioral
  - design_pattern
---

**Chaîne de responsabilité** permet de faire circuler des demandes dans une chaîne de handlers. Lorsqu’un handler reçoit une demande, il décide de la traiter ou de l’envoyer au handler suivant de la chaîne.

![Patron de conception chaîne de&nbsp;responsabilité](../../_resources/chain-of-responsibility_e3977af6b53c43cb91ac291e68.png)

## Problème

Imaginez que vous travaillez sur un système de commandes en ligne. Vous voulez restreindre l’accès au système pour que seuls les utilisateurs authentifiés puissent créer des commandes. Les utilisateurs qui ont les autorisations administratives doivent avoir un accès total aux commandes.

Après un travail de préparation, vous vous rendez compte que ces étapes doivent être exécutées dans un ordre précis. L’application peut essayer d’authentifier un utilisateur auprès du système lorsqu’il reçoit une demande qui contient ses identifiants. Mais si ces derniers ne sont pas corrects et que l’authentification échoue, ce n’est pas la peine de lancer d’autres vérifications.

<img width="600" height="0" src="../../_resources/problem1-fr_2b6f3e03a837497ab045090fadd23865.png"/>

La demande doit d’abord passer par une série de vérifications avant que le système de commandes ne prenne le relais.

Pendant les mois qui ont suivi, vous avez mis en place plusieurs vérifications supplémentaires.

- Un de vos collègues vous a fait remarquer qu’il n’était pas très prudent envoyer des données brutes directement dans le système de commandes. Vous avez donc ajouté une étape de validation supplémentaire pour purger les données de la demande.
    
- Plus tard, quelqu’un a découvert que le système de mot de passe était vulnérable aux attaques par force brute. Vous avez ajouté une étape qui filtre les échecs répétés provenant de la même adresse IP pour y remédier.
    
- Quelqu’un d’autre vous a suggéré d’améliorer la vitesse du système en envoyant les résultats directement depuis le cache, lorsque des demandes répétées retournent les mêmes résultats. Vous avez donc implémenté une autre étape qui laisse la demande passer si le système ne trouve pas la réponse correspondante dans le cache.
    

<img width="610" height="0" src="../../_resources/problem2-fr_ab53831df60d48efb5c09c2e86ea36e6.png"/>

Plus le code grossit et plus il devient moche et désordonné.

Le code des vérifications — qui n’était déjà pas très ordonné au départ — a grossi au fur et à mesure de l’ajout de nouvelles fonctionnalités. La modification d’une vérification affecte parfois les autres. Le pire dans tout cela, c’est qu’en voulant réutiliser les vérifications existantes pour protéger les autres composants du système, vous avez été obligé de dupliquer du code, car ces composants n’avaient pas besoin de toutes les étapes.

Le système est devenu très difficile à comprendre et cher à maintenir. Vous vous êtes débattu avec le code pendant un moment, jusqu’au jour où vous avez décidé de tout refaire.

## Solution

Tout comme plusieurs patrons de conception comportementaux, la **Chaîne de Responsabilité** repose sur la transformation de comportements particuliers en objets autonomes que l’on appelle *handlers*. Dans notre cas, chaque étape doit être extraite de sa propre classe avec une seule méthode qui effectue la vérification. La demande est passée en paramètre de la méthode avec toutes ses données.

Le patron vous propose de relier ces handlers par une chaîne. Chaque handler stocke une référence vers le prochain handler de la chaîne dans l’un de ses attributs. En plus de traiter la demande, les handlers la font passer plus loin dans la chaîne. La demande fait le tour de la chaîne jusqu’à ce que tous les handlers aient eu l’occasion de la traiter.

Le mieux dans tout cela, c’est qu’un handler peut décider de ne pas envoyer la demande plus loin dans la chaîne et de mettre fin à son traitement.

Dans notre exemple du système de commandes, un handler effectue le traitement et décide s’il doit envoyer la demande plus loin dans la chaîne. Si la commande contient les bonnes données, les handlers peuvent exécuter leur traitement, qu’il s’agisse de l’authentification ou de la mise en cache.

<img width="640" height="0" src="../../_resources/solution1-fr_2c09f38631634b428e610049835d75e5.png"/>

Les handlers forment une chaîne.

Il existe une approche légèrement différente (un peu plus canonique) dans laquelle un handler décide s’il traite la demande dès sa réception. S’il peut la traiter, la demande n’ira pas plus loin. Dans ce cas de figure, un seul handler s’occupera de traiter la demande (ou aucun). C’est une approche classique utilisée dans les piles d’éléments d’une interface graphique (GUI).

Par exemple, lorsqu’un utilisateur clique sur un bouton, l’événement est propagé à travers la chaîne des éléments de la GUI. Cette chaîne débute par le bouton, continue avec ses conteneurs (les formulaires ou les panneaux) et se termine avec la fenêtre principale de l’application. L’événement est traité par le premier élément de la chaîne qui est en mesure de s’en occuper. Cet exemple est particulièrement intéressant, car il démontre qu’une chaîne peut toujours être extraite depuis une arborescence.

<img width="520" height="0" src="../../_resources/solution2-fr_d10a18c7dd9b479aad998e00f22bce3f.png"/>

Une chaîne peut être construite à partir de la branche d’une arborescence.

Les classes handler doivent toutes implémenter la même interface. Chaque handler concret ne se préoccupe que de l’existence d’une méthode `traiter` chez le handler suivant. Ainsi, vous pouvez créer vos chaînes à l’exécution et utiliser divers handlers sans coupler votre code à leurs classes concrètes.

## Analogie

<img width="600" height="0" src="../../_resources/chain-of-responsibility-comic-1-_c5e8e9e472674469a.png"/>

Un appel au support technique passe par plusieurs opérateurs.

Vous venez juste d’installer un nouveau composant sur votre ordinateur. Comme vous êtes un geek, vous avez installé plusieurs systèmes d’exploitation. Vous essayez de tous les démarrer pour voir si votre matériel est bien pris en compte. Windows détecte votre nouveau composant et l’active automatiquement. En revanche, votre petit chouchou Linux refuse de le faire fonctionner. Avec un soupçon d’espoir, vous appelez le support technique dont le numéro est indiqué sur la boîte.

Votre premier interlocuteur n’est autre que la voix robotique du répondeur. Il vous propose neuf solutions à des problèmes classiques, mais aucun ne vous concerne. Au bout d’un moment, le robot vous redirige vers un opérateur humain.

Malheureusement, ce dernier ne vous répond rien de bien intéressant. Il ne cesse de répéter des extraits de son manuel et ignore vos commentaires. Après avoir entendu « avez-vous essayé de redémarrer votre ordinateur ? » pour la dixième fois, vous demandez à parler avec un vrai technicien.

Finalement, l’opérateur vous envoie vers un de leurs techniciens qui était probablement assis tout seul depuis des heures dans la salle des serveurs, située quelque part dans la cave sombre des bureaux de leur entreprise, et attendait avec impatience de pouvoir parler à quelqu’un. Le technicien vous indique un lien de téléchargement pour récupérer les bons pilotes et la marche à suivre pour les installer sur Linux. Enfin, la solution ! Vous mettez fin à l’appel, fou de joie !

## Structure

<img width="380" height="0" src="../../_resources/structure_882c6e30b4c74739a1396127e9473c35.png"/>

1.  Le **Handler** déclare une interface commune pour tous les handlers concrets. En général, il ne comporte qu’une seule méthode pour gérer les demandes, mais il peut parfois en contenir une autre pour désigner le prochain handler de la chaîne.
    
2.  Le **Handler de Base** est une classe facultative dans laquelle le code commun à tous les handlers peut être écrit.
    
    En général, cette classe définit un attribut qui pointe vers le prochain handler. Les clients peuvent assembler une chaîne en passant un handler au constructeur ou au setter du handler précédent. La classe peut également implémenter le comportement par défaut d’un handler : il s’assure de l’existence du prochain handler, puis lui délègue le travail.
    
3.  Les **Handlers Concrets** contiennent le code qui traite les demandes. Lors de la réception d’une demande, chaque handler décide s’il doit la traiter et s’il doit l’envoyer plus loin dans la chaîne.
    
    Les handlers sont généralement autonomes et non modifiables, et n’accepteront qu’une seule fois les données nécessaires par le biais du constructeur.
    
4.  Le **Client** peut créer les chaînes juste une fois ou les assembler dynamiquement en fonction de la logique métier. Notez bien que la demande initiale n’est pas obligatoirement envoyée au premier handler de la chaîne.
    

## Pseudo-code

Dans cet exemple, la **Chaîne de responsabilité** est chargée d’afficher l’aide contextuelle pour les éléments actifs de la GUI.

<img width="610" height="0" src="../../_resources/example-fr_bf148181693642cda57a1fca3f25c67b.png"/>

Les classes de la GUI sont construites à l’aide du patron composite. Chaque élément est relié à son conteneur. À n’importe quel moment, vous pouvez bâtir une chaîne d’éléments qui commence par l’élément lui-même et parcourt tous ses conteneurs.

La GUI de l’application prend généralement la forme d’une arborescence. Par exemple, la classe `Dialogue` qui s’occupe du rendu de la fenêtre principale de l’application, est la racine de l’arbre. La boîte de dialogue contient des `Panneaux`, qui peuvent eux-mêmes être composés d’autres panneaux ou d’éléments simples de plus bas niveau comme des `Boutons` et des `ChampsTexte`.

Un composant simple peut afficher brièvement des infobulles contextuelles si son texte d’aide a été configuré. Les composants plus complexes ont leur propre manière d’afficher l’aide contextuelle. Ils peuvent par exemple consulter l’aperçu du manuel ou ouvrir une page dans un navigateur.

<img width="250" height="0" src="../../_resources/example2-fr_63388b8f09a34098a59dcffe55eeca9b.png"/>

Voici comment une demande d’aide parcourt les objets de la GUI.

Si un utilisateur positionne le pointeur de sa souris sur un élément et appuie sur la touche `F1`, l’application détecte le composant situé sous le pointeur et lui envoie une demande d’aide. La demande remonte vers la surface en parcourant tous les conteneurs jusqu’à ce qu’elle atteigne un élément qui peut afficher les informations de l’aide.

```
// L’interface du handler déclare une méthode pour construire
// une chaîne de handlers. Elle déclare également une méthode
// pour exécuter la demande.
interface ComponentWithContextualHelp is
    method showHelp()

// La classe de base des composants simples.
abstract class Component implements ComponentWithContextualHelp is
    field tooltipText: string

    // Le conteneur du composant agit comme le prochain maillon
    // de la chaîne des handlers.
    protected field container: Container

    // Le composant affiche une infobulle si un texte d’aide y
    // est associé.  Sinon, il envoie l’appel vers le conteneur
    // (s’il existe).
    method showHelp() is
        if (tooltipText != null)
            // Affiche l’infobulle.
        else
            container.showHelp()

// Les conteneurs peuvent avoir des composants simples et
// d’autres conteneurs enfants. Les liens de la chaîne sont
// établis ici. La classe hérite du comportement de la méthode
// montrerAide (showHelp) de son parent.
abstract class Container extends Component is
    protected field children: array of Component

    method add(child) is
        children.add(child)
        child.container = this

// Les composants primitifs peuvent se contenter de l’aide par
// défaut...
class Button extends Component is
    // ...

// Mais les composants complexes peuvent redéfinir
// l’implémentation par défaut. Si le texte d’aide ne peut être
// fourni d’une autre manière, le composant peut toujours
// appeler l’implémentation de base (se référer à la classe
// Composant).
class Panel extends Container is
    field modalHelpText: string

    method showHelp() is
        if (modalHelpText != null)
            // Affiche une fenêtre modale avec le texte d’aide.
        else
            super.showHelp()

// ...idem qu’au-dessus...
class Dialog extends Container is
    field wikiPageURL: string

    method showHelp() is
        if (wikiPageURL != null)
            // Ouvre la page wiki d’aide.
        else
            super.showHelp()

// Code client.
class Application is
    // Chaque application configure la chaîne différemment.
    method createUI() is
        dialog = new Dialog("Budget  Reports")
        dialog.wikiPageURL = "http://..."
        panel = new Panel(0, 0, 400, 800)
        panel.modalHelpText = "This  panel  does..."
        ok = new Button(250, 760, 50, 20, "OK")
        ok.tooltipText = "This  is  an  OK  button  that..."
        cancel = new Button(320, 760, 50, 20, "Cancel")
        // ...
        panel.add(ok)
        panel.add(cancel)
        dialog.add(panel)

    // Imaginez ce qui se passe ici.
    method onF1KeyPress() is
        component = this.getComponentAtMouseCoords()
        component.showHelp()

```

## Possibilités d’application

Utilisez la chaîne de responsabilité quand votre programme doit traiter des types de demandes variées de différentes manières, mais que leur type exact et leur ordre dans la chaîne ne sont pas connus à l’avance.

Ce patron vous permet de former une chaîne avec les handlers et d’interroger chacun d’entre eux lors de la réception de la demande afin de savoir s’ils peuvent la traiter. Chaque handler a ainsi l’opportunité de traiter la demande.

Utilisez ce patron si vos handlers doivent absolument respecter un ordre donné.

Comme vous pouvez définir les liens entre les handlers de la chaîne, les demandes la parcourront selon l’ordonnancement que vous avez configuré.

Utilisez la chaîne de responsabilité si l’ensemble des handlers et leur ordre dans la chaîne peuvent changer lors de l’exécution.

Si vous fournissez des setters à un attribut à l’intérieur d’une classe handler, vous serez en mesure d’ajouter, de retirer ou d’ordonner dynamiquement les handlers.

## Mise en œuvre

1.  Déclarez l’interface du handler et la méthode qui gère les demandes.
    
    Déterminez la manière dont le client passera les données de la demande dans la méthode. La manière la plus flexible consiste à convertir la demande en objet et à le passer en paramètre de la méthode.
    
2.  Pour éviter la duplication du code de base dans les handlers concrets, il peut être utile de créer une classe abstraite de base pour le handler, dérivée de l’interface handler.
    
    Cette classe doit posséder un attribut qui est une référence vers le prochain handler de la chaîne et vous devriez envisager de la rendre non modifiable. Mais si vous prévoyez de modifier les chaînes lors de l’exécution, vous devez définir des setters pour changer la valeur de l’attribut qui stocke les références.
    
    Vous pouvez également mettre en place le comportement par défaut des méthodes des handlers qui consiste à envoyer la demande au prochain objet (s’il en reste). Les handlers concrets peuvent utiliser ce comportement en faisant appel à la méthode de leur parent.
    
3.  Créez les sous-classes des handlers concrets une par une et mettez en place leurs traitements. Chaque handler doit prendre deux décisions lors de la réception d’une demande :
    
    - Traiter ou non la demande.
    - Passer ou non la demande au handler suivant de la chaîne.
4.  Le client doit assembler lui-même les chaînes ou recevoir des chaînes pré construites via d’autres objets. Dans le dernier cas, vous devez mettre en place des fabriques pour assembler des chaînes selon la configuration ou selon les paramètres d’environnement.
    
5.  Le client peut déclencher n’importe quel élément de la chaîne, pas forcément le premier. La demande continuera de parcourir la chaîne jusqu’à ce qu’un handler refuse de la laisser continuer, ou jusqu’à ce que l’on atteigne la fin de la chaîne.
    
6.  La chaîne étant dynamique, le client doit être capable de gérer les scénarios suivants :
    
    - La chaîne peut être composée d’un unique lien.
    - Certaines demandes n’iront pas jusqu’au bout de la chaîne.
    - Certaines demandes atteindront la fin de la chaîne sans être traitées.

## Avantages et inconvénients

- Vous pouvez contrôler l’ordre des traitements de la demande.
- *Principe de responsabilité unique*. Vous pouvez découpler les classes qui appellent des traitements, de celles qui les exécutent.
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouveaux handlers dans le programme sans toucher au code client existant.

- Il se peut que certaines demandes ne soient pas traitées.

## Liens avec les autres patrons

- La [Chaîne de responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility), la [Commande](https://refactoring.guru/fr/design-patterns/command), le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) et l’[Observateur](https://refactoring.guru/fr/design-patterns/observer) proposent différentes solutions pour associer les demandeurs et les récepteurs.
    - La *chaîne de responsabilité* envoie une demande ordonnée qui est passée tout au long d’une chaîne dynamique de récepteurs potentiels, jusqu’à ce que l’un d’entre eux décide de la traiter.
    - La *commande* établit des connexions unidirectionnelles entre les demandeurs et les récepteurs.
    - Le *médiateur* élimine les liens directs entre les demandeurs et les récepteurs, et les force à communiquer indirectement via un objet médiateur.
    - L’*observateur* permet aux récepteurs de s’inscrire et de se désinscrire dynamiquement à la réception des demandes.

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_140f00c0bd894be0a807878862a4cda5.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/java/example "Patrons de conception : Chaîne de responsabilité en Java") [<img width="43" height="56" src="../../_resources/csharp_043fb932596e4b9a857b3ee31b90ec8d.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/csharp/example "Patrons de conception : Chaîne de responsabilité en C#")[<img width="43" height="56" src="../../_resources/cpp_18bdf4c85a104dcb805b401f4ec66102.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/cpp/example "Patrons de conception : Chaîne de responsabilité en C++")[<img width="43" height="56" src="../../_resources/php_616b603d17d54eed9f32fa09b7874a80.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/php/example "Patrons de conception : Chaîne de responsabilité en PHP")[<img width="43" height="56" src="../../_resources/python_245ab1ea89a44136936ecb95f29b1753.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/python/example "Patrons de conception : Chaîne de responsabilité en Python")[<img width="43" height="56" src="../../_resources/ruby_fcdc0b9a13424c4bba99c78d232fffe0.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/ruby/example "Patrons de conception : Chaîne de responsabilité en Ruby")[<img width="43" height="56" src="../../_resources/swift_0a34cb28cc3f46fabcdec1c1c4ebe7a7.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/swift/example "Patrons de conception : Chaîne de responsabilité en Swift")[<img width="43" height="56" src="../../_resources/typescript_a3e666b7d8494892b5375c1d8aa85dd2.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/typescript/example "Patrons de conception : Chaîne de responsabilité en TypeScript")[<img width="43" height="56" src="../../_resources/go_983af88674f74cddbd88d71cdd43442e.svg"/>](https://refactoring.guru/fr/design-patterns/chain-of-responsibility/go/example "Patrons de conception : Chaîne de responsabilité en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_40863b5ecae4401fb5f16b9e923.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Commande](https://refactoring.guru/fr/design-patterns/command)

#### Retour

[Patrons comportementaux](https://refactoring.guru/fr/design-patterns/behavioral-patterns)

[<img width="245" height="375" src="../../_resources/web-cover-fr_c852e245edb840aaaf28320736719d52.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/chain-of-responsibility "English") [Español](https://refactoring.guru/es/design-patterns/chain-of-responsibility "Español") [Français](https://refactoring.guru/fr/design-patterns/chain-of-responsibility "Français") [Polski](https://refactoring.guru/pl/design-patterns/chain-of-responsibility "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/chain-of-responsibility "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/chain-of-responsibility "Русский") [Українська](https://refactoring.guru/uk/design-patterns/chain-of-responsibility "Українська") [中文](https://refactoringguru.cn/design-patterns/chain-of-responsibility "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_90a63c86904e4667974e8c2607737c5d.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_81a8004335464afba8b37207a9b8cc89.png)

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
 ![Organization address](../../_resources/fa-building_d1487c45b2994584a7b23849aa9fa577.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)