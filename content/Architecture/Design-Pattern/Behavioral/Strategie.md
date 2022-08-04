---
title: Stratégie / Strategy
updated: 2021-12-26 15:28:43Z
created: 2021-12-26 15:18:00Z
source: https://refactoring.guru/fr/design-patterns/strategy
tags:
  - behavioral
  - design_pattern
---

**Stratégie** permet de définir une famille d’algorithmes, de les mettre dans des classes séparées et de rendre leurs objets interchangeables.

![Patron de conception&nbsp;stratégie](../../_resources/strategy_063f6e651d104b90b9d27810a0040bec.png)

## Problème

Un beau jour, vous avez décidé de créer une application de navigation pour les voyageurs occasionnels. Vous l’avez développé avec une superbe carte comme fonctionnalité principale, qui aide les utilisateurs à s’orienter rapidement dans n’importe quelle ville.

La fonctionnalité la plus demandée était la planification d’itinéraire. Un utilisateur devrait pouvoir entrer une adresse et le chemin le plus rapide pour arriver à destination s’afficherait sur la carte.

La première version de l’application ne pouvait tracer des itinéraires que sur les routes. Les automobilistes étaient comblés. Mais apparemment, certaines personnes préfèrent utiliser d’autres moyens de locomotion pendant leurs vacances. Vous avez ajouté la possibilité de créer des trajets à pied dans la version suivante. Juste après cela, vous avez ajouté la possibilité d’utiliser les transports en commun dans les itinéraires.

Mais tout ceci n’était que le début. Vous avez continué en adaptant l’application pour les cyclistes, et plus tard, ajouté la possibilité de construire les itinéraires en passant par les attractions touristiques de la ville.

<img width="330" height="0" src="../../_resources/problem_8cb26d9af91d40a4a8b9f62545d91fa0.png"/>

Le code du navigateur grossit à vue d’œil.

L’application a beau avoir très bien marché d’un point de vue financier, vous vous êtes arraché les cheveux sur le côté technique. Chaque fois que vous ajoutiez un nouvel algorithme pour tracer les itinéraires, la classe principale Navigateur doublait de taille. À un moment donné, la bête n’était plus possible à maintenir.

Que ce soit pour corriger un petit problème ou pour ajuster les scores des rues, la moindre touche apportée aux algorithmes impactait la totalité de la classe, augmentant les chances de créer des bugs dans du code qui fonctionnait très bien.

De plus, travailler en équipe n’était plus efficace. Les membres de votre équipe embauchés juste après la sortie et le succès de votre application se plaignaient de passer trop de temps à résoudre des problèmes de fusion. Ajouter une nouvelle fonctionnalité vous demandait de modifier une classe énorme, créant des conflits dans le code produit par les autres développeurs.

## Solution

Le patron de conception stratégie vous propose de prendre une classe dotée d’un comportement spécifique mais qui l’exécute de différentes façons, et de décomposer ses algorithmes en classes séparées appelées *stratégies*.

La classe originale (le *contexte*) doit avoir un attribut qui garde une référence vers une des stratégies. Plutôt que de s’occuper de la tâche, le contexte la délègue à l’objet stratégie associé.

Le contexte n’a pas la responsabilité de la sélection de l’algorithme adapté, c’est le client qui lui envoie la stratégie. En fait, le contexte n’y connait pas grand-chose en stratégies, c’est l’interface générique qui lui permet de les utiliser. Elle n’expose qu’une seule méthode pour déclencher l’algorithme encapsulé à l’intérieur de la stratégie sélectionnée.

Le contexte devient indépendant des stratégies concrètes. Vous pouvez ainsi modifier des algorithmes ou en ajouter de nouveaux sans toucher au code du contexte ou aux autres stratégies.

<img width="570" height="0" src="../../_resources/solution_3aa3e1b8b89e4b4894ee5e7bd58f6dae.png"/>

Stratégies de planification d’itinéraire.

Dans notre application de navigation, chaque algorithme d’itinéraire peut être extrait de sa propre classe avec une seule méthode `tracerItinéraire`. La méthode accepte une origine et une destination, puis retourne une liste de points de passage.

Quand bien même les différentes classes itinéraire ne donneraient pas un résultat identique avec les mêmes paramètres, la classe navigateur principale ne se préoccupe pas de l’algorithme sélectionné, car sa fonction première est d’afficher les points de passage sur la carte. La classe navigateur possède une méthode pour changer la stratégie d’itinéraire active afin que ses clients (les boutons de l’interface utilisateur par exemple) puissent remplacer le comportement sélectionné par un autre.

## Analogie

<img width="640" height="0" src="../../_resources/strategy-comic-1-fr_974d0462c8b64b53bd59f917cb01c9.png"/>

Différentes stratégies pour se rendre à l’aéroport.

Imaginez que vous devez vous rendre à l’aéroport. Vous pouvez prendre le bus, appeler un taxi ou enfourcher votre vélo. Ce sont vos stratégies de transport. Vous pouvez sélectionner une de ces stratégies en fonction de certains facteurs, comme le budget ou les contraintes de temps.

## Structure

<img width="440" height="0" src="../../_resources/structure_d77fa9851eec4b0c88c2830f4a77a8a5.png"/>

1.  Le **Contexte** garde une référence vers une des stratégies concrètes et communique avec cet objet uniquement au travers de l’interface stratégie.
    
2.  L’interface **Stratégie** est commune à toutes les stratégies concrètes. Elle déclare une méthode que le contexte utilise pour exécuter une stratégie.
    
3.  Les **Stratégies Concrètes** implémentent différentes variantes d’algorithmes utilisées par le contexte.
    
4.  Chaque fois qu’il veut lancer un algorithme, le contexte appelle la méthode d’exécution de l’objet stratégie associé. Le contexte ne sait pas comment la stratégie fonctionne ni comment l’algorithme est lancé.
    
5.  Le **Client** crée un objet spécifique Stratégie et le passe au contexte. Le contexte expose un setter qui permet aux clients de remplacer la stratégie associée au contexte lors de l’exécution.
    

## Pseudo-code

Dans cet exemple, le contexte utilise plusieurs **Stratégies** pour lancer diverses opérations arithmétiques.

```
// L’interface stratégie déclare les traitements communs à
// toutes les versions supportées de l’algorithme. Le contexte
// utilise cette interface pour appeler l’algorithme défini par
// les stratégies concrètes.
interface Strategy is
    method execute(a, b)

// Les stratégies concrètes implémentent l’interface de base
// Stratégie et hébergent l’algorithme. L’interface les rend
// interchangeables dans le contexte.
class ConcreteStrategyAdd implements Strategy is
    method execute(a, b) is
        return a + b

class ConcreteStrategySubtract implements Strategy is
    method execute(a, b) is
        return a - b

class ConcreteStrategyMultiply implements Strategy is
    method execute(a, b) is
        return a * b

// Le contexte définit l’interface dont les clients ont besoin.
class Context is
    // Le contexte maintient une référence à l’un des objets
    // Stratégie. Le contexte ne connait pas la classe concrète
    // de la stratégie. Il doit manipuler toutes les stratégies
    // via l’interface stratégie.
    private strategy: Strategy

    // En général, le contexte accepte une stratégie en
    // paramètre du constructeur et fournit un setter pour
    // permettre de changer de stratégie lors de l’exécution.
    method setStrategy(Strategy strategy) is
        this.strategy = strategy

    // Le contexte délègue certaines tâches à l’objet stratégie,
    // plutôt que d’implémenter plusieurs versions de
    // l’algorithme.
    method executeStrategy(int a, int b) is
        return strategy.execute(a, b)

// Le code client choisit une stratégie concrète et la passe au
// contexte. Le client doit connaitre les différences entre les
// stratégies afin de faire le bon choix.
class ExampleApplication is
    method main() is
        Create context object.

        Read first number.
        Read last number.
        Read the desired action from user input.

        if (action == addition) then
            context.setStrategy(new ConcreteStrategyAdd())

        if (action == subtraction) then
            context.setStrategy(new ConcreteStrategySubtract())

        if (action == multiplication) then
            context.setStrategy(new ConcreteStrategyMultiply())

        result = context.executeStrategy(First number, Second number)

        Print result.

```

## Possibilités d’application

Utilisez le patron de conception stratégie si vous voulez avoir différentes variantes d’un algorithme à l’intérieur d’un objet à disposition, et pouvoir passer d’un algorithme à l’autre lors de l’exécution.

Ce patron vous permet de modifier indirectement le comportement de l’objet lors de l’exécution, en l’associant avec différents sous-objets qui peuvent accomplir des sous-tâches spécifiques de différentes manières.

Utilisez la stratégie si vous avez beaucoup de classes dont la seule différence est leur façon d’exécuter un comportement.

Ce patron vous permet d’extraire des variantes d’un comportement dans une hiérarchie de classes séparées et de combiner les classes originales dans une seule, évitant de dupliquer du code.

Utilisez la stratégie pour isoler la logique métier d’une classe, de l’implémentation des algorithmes dont les détails ne sont pas forcément importants pour le contexte.

Ce patron vous permet de séparer le code, les données internes et les dépendances des divers algorithmes du reste du code. Une interface toute simple permet aux clients d’exécuter les algorithmes et d’en changer lors de l’exécution.

Utilisez ce patron si votre classe possède un gros bloc conditionnel qui choisit entre différentes variantes du même algorithme.

La stratégie vous débarrasse de toutes ces conditions en extrayant tous les algorithmes dans des classes séparées, et ces dernières implémentent toutes la même interface. L’objet original délègue l’exécution à l’un de ces objets, au lieu d’implémenter toutes les variantes de l’algorithme.

## Mise en œuvre

1.  Dans la classe contexte, identifiez un algorithme qui varie souvent. Il peut s’agir d’un gros bloc conditionnel qui sélectionne une variante du même algorithme lors de l’exécution.
    
2.  Déclarez l’interface stratégie commune à toutes les variantes de l’algorithme.
    
3.  Extrayez tous les algorithmes un par un et mettez-les dans leurs propres classes. Elles doivent toutes implémenter l’interface stratégie.
    
4.  Ajoutez un attribut pour garder une référence vers un objet stratégie dans la classe contexte. Créez un setter pour modifier le contenu de cet attribut. Le contexte ne doit manipuler l’objet stratégie qu’au travers de l’interface stratégie. Le contexte peut définir une interface qui laisse la stratégie accéder à ses données.
    
5.  Les clients d’un contexte doivent l’associer avec une stratégie adaptée au comportement attendu.
    

## Avantages et inconvénients

- Vous pouvez permuter l’algorithme utilisé à l’intérieur d’un objet à l’exécution.
- Vous pouvez séparer les détails de l’implémentation d’un algorithme et le code qui l’utilise.
- Vous pouvez remplacer l’héritage par la composition.
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouvelles stratégies sans avoir à modifier le contexte.

- Si vous n’avez que quelques algorithmes qui ne varient pas beaucoup, nul besoin de rendre votre programme plus compliqué avec les nouvelles classes et interfaces qui accompagnent la mise en place du patron.
- Les clients doivent pouvoir comparer les différentes stratégies et choisir la bonne.
- De nombreux langages de programmation modernes gèrent les types fonctionnels et vous permettent d’implémenter différentes versions d’un algorithme à l’intérieur d’un ensemble de fonctions anonymes. Vous pouvez ensuite utiliser ces fonctions exactement comme vous le feriez pour des objets stratégie, sans encombrer votre code avec des classes et interfaces supplémentaires.

## Liens avec les autres patrons

- Le [Pont](https://refactoring.guru/fr/design-patterns/bridge), l’[État](https://refactoring.guru/fr/design-patterns/state), la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) (et dans une certaine mesure l’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter)) ont des structures très similaires. En effet, ces patrons sont basés sur la composition, qui délègue les tâches aux autres objets. Cependant, ils résolvent différents problèmes. Un patron n’est pas juste une recette qui vous aide à structurer votre code d’une certaine manière. C’est aussi une façon de communiquer aux autres développeurs le problème qu’il résout.
    
- La [Commande](https://refactoring.guru/fr/design-patterns/command) et la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) peuvent se ressembler, car vous les utilisez toutes les deux pour paramétrer un objet avec une action. Cependant, ces patrons ont des intentions très différentes.
    
    - Vous pouvez utiliser la *commande* pour convertir un traitement en un objet. Les paramètres du traitement deviennent des attributs de cet objet. La conversion vous permet de différer le lancement du traitement, le mettre dans une file d’attente, stocker l’historique des commandes, envoyer les commandes à des services distants, etc.
        
    - La *stratégie* quant à elle, décrit généralement différentes manières de faire la même chose et vous laisse permuter entre ces algorithmes à l’intérieur d’une unique classe contexte.
        
- Le [Décorateur](https://refactoring.guru/fr/design-patterns/decorator) vous permet de changer la peau d’un objet, alors que la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) vous permet de changer ses tripes.
    
- Le [Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method) est basé sur l’héritage : il vous laisse modifier certaines parties d’un algorithme en les étendant dans les sous-classes. La [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) est basée sur la composition : vous pouvez modifier certaines parties du comportement de l’objet en lui fournissant différentes stratégies qui correspondent à ce comportement. Le *patron de méthode* agit au niveau de la classe, il est donc statique. La *stratégie* agit au niveau de l’objet et vous laisse permuter les comportements à l’exécution.
    
- L’[État](https://refactoring.guru/fr/design-patterns/state) peut être considéré comme une extension de la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy). Ces deux patrons de conception sont basés sur la composition : ils changent le comportement du contexte en déléguant certaines tâches aux objets assistant. La *stratégie* rend ces objets complètement indépendants sans aucune visibilité l’un sur l’autre. Cependant, l’*état* n’impose pas de restrictions sur les dépendances entre les états concrets, et leur laisse modifier l’état du contexte à volonté.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_0671d268bc8a4cc99bc7b0b2f076ab1c.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/java/example "Patrons de conception : Stratégie en Java") [<img width="43" height="56" src="../../_resources/csharp_790603965dbc483c9ab9260e0298db80.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/csharp/example "Patrons de conception : Stratégie en C#")[<img width="43" height="56" src="../../_resources/cpp_7164a480463f45bb854d0b1aec87dde7.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/cpp/example "Patrons de conception : Stratégie en C++")[<img width="43" height="56" src="../../_resources/php_ed3212e7f8a940de85214e70a0d67191.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/php/example "Patrons de conception : Stratégie en PHP")[<img width="43" height="56" src="../../_resources/python_b42f680cd332455ea19ce3cba166e844.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/python/example "Patrons de conception : Stratégie en Python")[<img width="43" height="56" src="../../_resources/ruby_4177d1e45286482ba117f1a36822cbf9.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/ruby/example "Patrons de conception : Stratégie en Ruby")[<img width="43" height="56" src="../../_resources/swift_c5b0805e91694c3396c39a5c552b3549.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/swift/example "Patrons de conception : Stratégie en Swift")[<img width="43" height="56" src="../../_resources/typescript_6ea16e3698d2481983189128154bb271.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/typescript/example "Patrons de conception : Stratégie en TypeScript")[<img width="43" height="56" src="../../_resources/go_a4f0f582a8974effbc2209fca785f1e1.svg"/>](https://refactoring.guru/fr/design-patterns/strategy/go/example "Patrons de conception : Stratégie en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_5a5661196eca4534af8c3961393.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Patron de méthode](https://refactoring.guru/fr/design-patterns/template-method)

#### Retour

[État](https://refactoring.guru/fr/design-patterns/state)

[<img width="245" height="375" src="../../_resources/web-cover-fr_648c1ff9963943f19e01b47331b15b61.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/strategy "English") [Español](https://refactoring.guru/es/design-patterns/strategy "Español") [Français](https://refactoring.guru/fr/design-patterns/strategy "Français") [Polski](https://refactoring.guru/pl/design-patterns/strategy "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/strategy "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/strategy "Русский") [Українська](https://refactoring.guru/uk/design-patterns/strategy "Українська") [中文](https://refactoringguru.cn/design-patterns/strategy "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_13ee3659b4264ddba651dbcc1ecd6f09.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_eb703a88f5bc4d61b6fbaf6141d546ce.png)

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
 ![Organization address](../../_resources/fa-building_d7094997382b40b99b196a1809c1dd14.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)