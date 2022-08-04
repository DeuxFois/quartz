---
title: Commande / Command
updated: 2021-12-26 15:29:14Z
created: 2021-12-26 15:17:34Z
source: https://refactoring.guru/fr/design-patterns/command
tags:
  - behavioral
  - design_pattern
---

**Commande** prend une action à effectuer et la transforme en un objet autonome qui contient tous les détails de cette action. Cette transformation permet de paramétrer des méthodes avec différentes actions, planifier leur exécution, les mettre dans une file d’attente ou d’annuler des opérations effectuées.

![Patron de conception&nbsp;commande](../../_resources/command-fr_787ffeb0f2814bf1b60a1647af987cf1.png)

## Problème

Imaginez-vous en train de développer un nouvel éditeur de texte. Vous travaillez actuellement sur la création d’une barre d’outils équipée d’un ensemble de boutons qui permettent d’effectuer diverses opérations dans l’éditeur. Vous avez créé une belle classe `Bouton` qui peut être utilisée pour les boutons de la barre d’outils ou pour les boutons génériques des différentes boîtes de dialogue.

![Problème résolu grâce au patron de conception commande](../../_resources/problem1_0a1d8f8507fd4fd38c429d2198aceebd.png)

Tous les boutons de l’application sont dérivés de la même classe.

Ces boutons ont beau se ressembler, ils sont censés effectuer des actions différentes. Où va-t-on bien pouvoir mettre le code des actions que ces différents boutons déclenchent ? Le plus simple est de créer des tonnes de sous-classes où les boutons sont utilisés. Ces sous-classes vont contenir le code exécuté lors d’un clic sur un bouton.

<img width="400" height="0" src="../../_resources/problem2_61c5cc835bf5489aaf6313660e5ab36c.png"/>

De nombreuses sous-classes de boutons. Tout semble aller pour le mieux.

Mais bientôt, vous allez remarquer que cette approche comporte de sérieux défauts. Le premier que nous allons aborder ici, est le fait que vous avez créé énormément de sous-classes. Lors de chaque modification de la classe `Bouton`, vous risquez d’ajouter de nouveaux bugs dans le code. Pour faire simple, votre GUI (interface graphique utilisateur) est devenue un peu trop dépendante du code volatile de la logique métier.

<img width="480" height="0" src="../../_resources/problem3-fr_96602177276743e9919a2525579306f2.png"/>

Plusieurs classes implémentent la même fonctionnalité.

Abordons à présent le défaut majeur. Certaines opérations, comme copier-coller du texte, doivent être appelées depuis différents endroits. Par exemple, un utilisateur peut cliquer sur un petit bouton « copier » de la barre d’outils, copier via le menu contextuel ou encore faire `Ctrl+C` sur son clavier.

À l’époque où notre application n’avait qu’une barre d’outils, cela ne posait pas de problème de mettre l’implémentation des différentes actions dans la sous-classe du bouton. Vous pouviez écrire le code de la copie du texte dans la sous-classe `BoutonCopier` sans problème. Mais lorsque vous implémentez des menus contextuels, des raccourcis ou autres fonctionnalités du même ordre, vous devez dupliquer le code de ces actions dans plusieurs classes ou rendre les menus dépendants des boutons, ce qui est encore pire.

## Solution

Un logiciel bien conçu repose souvent sur le *principe de séparation des préoccupations*, qui est en général obtenu en décomposant l’application en plusieurs couches. Le cas le plus classique consiste à avoir une couche pour la GUI et une autre pour la logique métier. La couche GUI a la responsabilité d’afficher une belle image à l’écran, de détecter les actions de l’utilisateur et de l’application, et de les répercuter à l’écran. Mais en ce qui concerne l’exécution des traitements importants comme calculer la trajectoire vers la lune ou générer les rapports annuels, la couche GUI délègue le travail à la couche métier.

Dans le code, on se retrouve avec un objet GUI qui appelle une méthode de l’objet de la logique métier et lui passe des paramètres. Ce processus est souvent décrit comme un objet envoyant une *demande* à un autre.

<img width="470" height="0" src="../../_resources/solution1-fr_f0bfc13936a646c08478833b5d9cadc2.png"/>

Les objets de la GUI peuvent accéder directement aux objets de la couche métier.

Le patron de conception commande propose de ne pas envoyer ces demandes directement depuis les objets de la GUI. À la place, vous devez extraire le détail de ces demandes (un appel vers l’objet, le nom de la méthode et la liste des paramètres) dans une classe *commande* séparée avec une unique méthode qui déclenche cette demande.

Les objets commande servent de lien entre les diverses GUI et les objets métier. À partir de maintenant, l’objet GUI ne saura plus quel objet métier reçoit la demande et comment il la traite. L’objet GUI déclenche juste la commande, et cette dernière se charge de gérer les détails.

<img width="550" height="0" src="../../_resources/solution2-fr_b398e7a1d8f5438f8204c6a5801123f7.png"/>

Accéder à la couche métier via une commande.

La prochaine étape consiste à implémenter la même interface pour toutes vos commandes. En général, elle ne possède qu’une seule méthode d’exécution et ne prend aucun paramètre. Cette interface vous permet d’utiliser diverses commandes avec le même demandeur (invoker), sans avoir à la coupler avec les classes concrètes des commandes. En bonus, vous pouvez dorénavant échanger les objets de la commande liés au demandeur, ce qui permet de modifier le comportement du demandeur lors de l’exécution.

Vous vous êtes peut-être rendu compte qu’il manquait une pièce du puzzle : les paramètres de la demande. Un objet GUI devrait les fournir à la couche métier. Mais comme la méthode d’exécution de la commande ne possède aucun paramètre, comment allons-nous nous en sortir ? En fait, la commande doit être pré configurée avec ces données ou être capable de les récupérer par elle-même.

<img width="440" height="0" src="../../_resources/solution3-fr_605f9965c7dd444dafdd84bc897d819a.png"/>

Les objets de la GUI délèguent le travail aux commandes.

Revenons à notre éditeur de texte. Après avoir mis en place le patron de conception commande, nous n’avons plus besoin de toutes ces sous-classes pour implémenter le comportement des différents clics. Vous pouvez vous contenter de mettre un seul attribut dans la classe de base `Bouton` qui conserve une référence vers un objet commande et lui permet de lancer cette commande lors d’un clic.

Vous allez implémenter plusieurs classes commande pour chaque opération possible, et allez les relier avec les boutons en fonction du comportement attendu.

Vous pouvez implémenter tous les autres éléments de la GUI comme des menus, raccourcis ou des boîtes de dialogue complètes de la même manière. Ils seront reliés à une commande qui est exécutée lorsqu’un utilisateur interagit avec l’élément de la GUI. Vous l’avez probablement deviné, les éléments qui déclenchent les mêmes actions seront reliés aux mêmes commandes afin d’éviter la duplication de code.

Les commandes forment ainsi une couche intermédiaire très pratique et réduisent le couplage entre la GUI et les couches métier. Et nous n’avons abordé qu’une infime partie des bénéfices que le patron de conception commande peut apporter.

## Analogie

<img width="600" height="0" src="../../_resources/command-comic-1_2ac73732d661451b97ca1fde356f3c24.png"/>

Passer une commande au restaurant.

Après une longue balade en ville, vous trouvez un restaurant sympa et vous vous asseyez à une table près de la fenêtre. Un serveur aimable se rapproche et prend votre commande en l’écrivant sur un bout de papier. Il se rend dans la cuisine et colle la commande sur le mur. Au bout d’un moment, la commande arrive au chef, qui la lit et prépare le plat. Le cuisinier place le repas sur un plateau avec la commande. Le serveur découvre le plateau, vérifie la commande pour s’assurer que tout est conforme, et l’apporte à votre table.

Le bout de papier joue le rôle de la commande. Il reste dans une file d’attente jusqu’à ce que le chef soit prêt à la servir. Il peut commencer à cuisiner directement, car la commande contient toutes les informations permettant au chef de préparer le plat. C’est mieux que de perdre du temps à venir vous demander les détails de la commande.

## Structure

<img width="630" height="0" src="../../_resources/structure_a31bf8616bbd4b5c9f69145b30e62ce7.png"/>

1.  La classe **Demandeur** (*invoker*) a la responsabilité de l’envoi des demandes. Elle doit inclure un attribut qui stocke la référence à un objet commande. Le demandeur déclenche la commande plutôt que de l’envoyer directement au récepteur. Gardez bien en tête que le demandeur n’est pas responsable de la création de l’objet commande. En général, il s’agit d’une commande créée auparavant dans le constructeur du client.
    
2.  L’interface **Commande** déclare habituellement une seule méthode pour exécuter la commande.
    
3.  Les **Commandes Concrètes** implémentent plusieurs types de demandes. Une commande concrète n’est pas censée accomplir la tâche d’elle-même. Elle transmet l’appel aux objets de la logique métier. Mais pour simplifier le code, ces classes peuvent être fusionnées.
    
    Les paramètres nécessaires à l’exécution d’une méthode sur un objet récepteur peuvent être déclarés comme des attributs de la commande concrète. Vous pouvez rendre les objets de la commande non modifiables en autorisant uniquement l’initialisation de ces attributs dans le constructeur.
    
4.  La classe **Récepteur** porte la logique métier. À peu près n’importe quel objet peut prendre le rôle de récepteur. La majorité des commandes se contentent de passer la demande au récepteur et ce dernier se charge du traitement.
    
5.  Le **Client** crée et configure les objets de la commande concrète. Il doit passer tous les paramètres de la demande (dont l’instance du récepteur) dans le constructeur de la commande. Ensuite, la commande qui en résulte peut être associée avec un ou plusieurs demandeurs.
    

## Pseudo-code

Dans cet exemple, le patron de conception **Commande** aide à tracer l’historique des traitements et est en mesure d’annuler les modifications effectuées par un traitement.

<img width="640" height="0" src="../../_resources/example_d52951cee5ac4b8fa02dbd2e96556ce0.png"/>

Actions pouvant être annulées dans un éditeur de texte.

Les commandes qui modifient l’état de l’éditeur (par exemple couper et coller) font une sauvegarde de son état avant de lancer le traitement associé à la commande. Une fois que la commande a été exécutée, elle est placée dans un historique de commandes (une pile d’objets commande) avec une sauvegarde de l’état actuel de l’éditeur. Si l’utilisateur a besoin d’annuler un traitement, l’application peut prendre la commande la plus récente de l’historique, lire la sauvegarde associée à l’état de l’éditeur et la restaurer.

Le code client (éléments de la GUI, historique des commandes, etc.) n’est pas couplé avec les classes de la commande concrète, car il interagit avec les commandes via l’interface commande. Cette approche vous permet d’ajouter de nouvelles commandes dans l’application sans endommager l’existant.

```
// La classe de base commande définit une interface commune pour
// toutes les commandes concrètes.
abstract class Command is
    protected field app: Application
    protected field editor: Editor
    protected field backup: text

    constructor Command(app: Application, editor: Editor) is
        this.app = app
        this.editor = editor

    // Fait une sauvegarde de l’état de l’éditeur.
    method saveBackup() is
        backup = editor.text

    // Rétablit l’état de l’éditeur.
    method undo() is
        editor.text = backup

    // La méthode d’exécution est abstraite pour forcer les
    // commandes concrètes à fournir leurs propres
    // implémentations. La méthode doit retourner un booléen qui
    // indique si la commande a modifié l'état de l’éditeur.
    abstract method execute()

// Les commandes concrètes sont écrites ici.
class CopyCommand extends Command is
    // La commande pour copier n’est pas sauvegardée dans
    // l’historique, car elle ne modifie pas l'état de
    // l’éditeur.
    method execute() is
        app.clipboard = editor.getSelection()
        return false

class CutCommand extends Command is
    // La commande ‘couper’ change l’état de l’éditeur, elle
    // doit donc être sauvegardée dans l’historique. Elle sera
    // sauvegardée tant que la méthode renvoie vrai.
    method execute() is
        saveBackup()
        app.clipboard = editor.getSelection()
        editor.deleteSelection()
        return true

class PasteCommand extends Command is
    method execute() is
        saveBackup()
        editor.replaceSelection(app.clipboard)
        return true

// Le traitement ‘annuler’ est aussi une commande.
class UndoCommand extends Command is
    method execute() is
        app.undo()
        return false

// L’historique principal des commandes est juste une pile.
class CommandHistory is
    private field history: array of Command

    // Dernier entré...
    method push(c: Command) is
        // Pousse la commande à la fin du tableau de
        // l’historique.

    // ...premier sorti.
    method pop():Command is
        // Récupère la commande la plus récente de l’historique.

// La classe éditeur possède des fonctionnalités de traitement
// de texte. Elle joue le rôle du récepteur : toutes les
// commandes finissent par déléguer l’exécution aux méthodes de
// l’éditeur.
class Editor is
    field text: string

    method getSelection() is
        // Retourne le texte sélectionné.

    method deleteSelection() is
        // Supprime le texte sélectionné.

    method replaceSelection(text) is
        // Insère le contenu du presse-papiers à la position
        // actuelle.

// La classe application configure les relations de l’objet.
// Elle agit comme un demandeur : lorsque quelque chose doit
// être fait, elle crée un objet commande et lance son
// traitement.
class Application is
    field clipboard: string
    field editors: array of Editors
    field activeEditor: Editor
    field history: CommandHistory

    // Le code qui assigne les commandes à l’objet UI pourrait
    // ressembler à ceci.
    method createUI() is
        // ...
        copy = function() { executeCommand(
            new CopyCommand(this, activeEditor)) }
        copyButton.setCommand(copy)
        shortcuts.onKeyPress("Ctrl+C", copy)

        cut = function() { executeCommand(
            new CutCommand(this, activeEditor)) }
        cutButton.setCommand(cut)
        shortcuts.onKeyPress("Ctrl+X", cut)

        paste = function() { executeCommand(
            new PasteCommand(this, activeEditor)) }
        pasteButton.setCommand(paste)
        shortcuts.onKeyPress("Ctrl+V", paste)

        undo = function() { executeCommand(
            new UndoCommand(this, activeEditor)) }
        undoButton.setCommand(undo)
        shortcuts.onKeyPress("Ctrl+Z", undo)

    // Lance une commande et vérifie si elle doit être ajoutée à
    // l’historique.
    method executeCommand(command) is
        if (command.execute)
            history.push(command)

    // Prend la commande la plus récente dans l’historique et
    // lance sa méthode ‘annuler’. Vous remarquerez que nous ne
    // connaissons pas la classe de la commande. Nous n’avons
    // pas besoin de la connaître puisque la commande sait
    // comment annuler sa propre action.
    method undo() is
        command = history.pop()
        if (command != null)
            command.undo()

```

## Possibilités d’application

Utilisez le patron de conception commande lorsque vous voulez paramétrer des objets avec des traitements.

Le patron de conception commande peut transformer l’appel d’une méthode en un objet autonome. Cette approche permet des utilisations très intéressantes : vous pouvez passer des commandes dans les paramètres d’une méthode, les stocker à l’intérieur d’objets, modifier les liens des commandes à l’exécution, etc.

Voici un exemple : vous développez un composant de GUI (un menu contextuel par exemple) et vous voulez que vos utilisateurs puissent configurer des éléments de menu qui déclenchent des traitements lorsqu’un utilisateur final clique dessus.

Utilisez ce patron si vous voulez mettre des traitements dans une file d’attente, programmer leur déclenchement, ou les exécuter à distance.

Comme pour tous les objets, une commande peut être sérialisée, ce qui veut dire qu’on la convertit en une chaîne de caractères qui peut facilement être écrite dans un fichier ou dans une base de données. La chaîne de caractère pourra ensuite être restaurée et utilisée par l’objet de la commande originale. Vous pouvez ainsi différer et planifier l’exécution de la commande. Mais ce n’est pas tout ! De la même manière, vous pouvez mettre une commande dans une file d’attente, l’historiser ou l’envoyer par le réseau.

Utilisez le patron de conception commande quand vous voulez implémenter des opérations réversibles.

Parmi les nombreuses techniques qui permettent d’implémenter la fonctionnalité annuler/rétablir, le patron de conception commande est probablement le plus populaire.

Pour pouvoir annuler des traitements, vous devez mettre en place un historique des actions effectuées. L’historique des commandes est une pile qui contient tous les objets des commandes exécutées avec l’état de l’application correspondant.

Cette méthode possède deux défauts. Premièrement, l’état d’une application n’est pas facile à sauvegarder, car une partie peut être privée. Ce problème peut être résolu grâce à l’intervention du patron de conception [Mémento](https://refactoring.guru/fr/design-patterns/memento).

Deuxièmement, la sauvegarde de l’état risque de consommer beaucoup de RAM. Par conséquent, vous pouvez utiliser une autre alternative : plutôt que de rétablir l’état précédent, la commande effectue l’inverse de la modification, ce qui est malheureusement parfois impossible ou difficile à mettre en place.

## Mise en œuvre

1.  Déclarez l’interface commande avec une seule méthode d’exécution.
    
2.  Commencez à récupérer les demandes et à les mettre dans les classes concrètes Commande qui implémentent l’interface commande. Chaque classe doit comporter un ensemble d’attributs qui permettent de stocker les paramètres des demandes, avec une référence à l’objet initial du récepteur. Ces valeurs doivent toutes être initialisées dans le constructeur de la commande.
    
3.  Identifiez les classes qui vont jouer le rôle de *Demandeurs*. Créez les attributs de ces classes qui vont stocker les commandes. Les demandeurs doivent uniquement communiquer avec leurs commandes via l’interface commande. En général, ils ne créent pas les objets commande, c’est le code client qui s’en charge.
    
4.  Plutôt que d’envoyer la demande directement au récepteur, modifiez les demandeurs afin qu’ils exécutent la commande.
    
5.  Le client doit initialiser les objets dans l’ordre suivant :
    
    - Créer les récepteurs.
    - Créer les commandes et les associer avec les récepteurs si besoin.
    - Créer les demandeurs et les associer avec les commandes.

## Avantages et inconvénients

- *Principe de responsabilité unique*. Vous pouvez découpler les classes qui appellent des traitements, de celles qui les exécutent.
- *Principe ouvert/fermé*. Vous pouvez ajouter de nouvelles commandes dans le programme sans endommager le code client existant.
- Vous pouvez mettre en place une fonctionnalité annuler-rétablir.
- Vous pouvez différer l’exécution de vos traitements.
- Vous pouvez assembler plusieurs commandes simples en une seule plus complexe.

- Le code peut devenir plus compliqué, car vous créez une nouvelle couche entre les demandeurs et les récepteurs.

## Liens avec les autres patrons

- La [Chaîne de responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility), la [Commande](https://refactoring.guru/fr/design-patterns/command), le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) et l’[Observateur](https://refactoring.guru/fr/design-patterns/observer) proposent différentes solutions pour associer les demandeurs et les récepteurs.
    
    - La *chaîne de responsabilité* envoie une demande ordonnée qui est passée tout au long d’une chaîne dynamique de récepteurs potentiels, jusqu’à ce que l’un d’entre eux décide de la traiter.
    - La *commande* établit des connexions unidirectionnelles entre les demandeurs et les récepteurs.
    - Le *médiateur* élimine les liens directs entre les demandeurs et les récepteurs, et les force à communiquer indirectement via un objet médiateur.
    - L’*observateur* permet aux récepteurs de s’inscrire et de se désinscrire dynamiquement à la réception des demandes.
- Les handlers dans la [Chaîne de Responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility "Chaîne de responsabilité") peuvent être implémentés comme des [Commandes](https://refactoring.guru/fr/design-patterns/command). Dans ce cas, vous pouvez lancer beaucoup de traitements différents sur le même objet contexte, représenté par une demande.
    
    Vous pouvez cependant utiliser une autre approche dans laquelle la demande en elle-même est un objet *commande*. Vous pouvez ainsi exécuter le même traitement dans une série de différents contextes connectés par une chaîne.
    
- Vous pouvez utiliser la [Commande](https://refactoring.guru/fr/design-patterns/command) et le [Mémento](https://refactoring.guru/fr/design-patterns/memento) ensemble pour implémenter la fonctionnalité « annuler ». Dans ce cas, les commandes ont la responsabilité d’exécuter les divers traitements sur un objet cible. Les mémentos sauvegardent l’état de cet objet juste avant le lancement de la commande.
    
- La [Commande](https://refactoring.guru/fr/design-patterns/command) et la [Stratégie](https://refactoring.guru/fr/design-patterns/strategy) peuvent se ressembler, car vous les utilisez toutes les deux pour paramétrer un objet avec une action. Cependant, ces patrons ont des intentions très différentes.
    
    - Vous pouvez utiliser la *commande* pour convertir un traitement en un objet. Les paramètres du traitement deviennent des attributs de cet objet. La conversion vous permet de différer le lancement du traitement, le mettre dans une file d’attente, stocker l’historique des commandes, envoyer les commandes à des services distants, etc.
        
    - La *stratégie* quant à elle, décrit généralement différentes manières de faire la même chose et vous laisse permuter entre ces algorithmes à l’intérieur d’une unique classe contexte.
        
- Le [Prototype](https://refactoring.guru/fr/design-patterns/prototype) se révèle utile lorsque vous voulez sauvegarder des copies de [Commandes](https://refactoring.guru/fr/design-patterns/command) dans l’historique.
    
- Vous pouvez traiter le [Visiteur](https://refactoring.guru/fr/design-patterns/visitor) comme une version plus puissante du patron de conception [Commande](https://refactoring.guru/fr/design-patterns/command). Ses objets peuvent lancer des traitements sur divers objets dans différentes classes.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_9dde852adff34fa4a10abd21826c9e9a.svg"/>](https://refactoring.guru/fr/design-patterns/command/java/example "Patrons de conception : Commande en Java") [<img width="43" height="56" src="../../_resources/csharp_4a431deef84845d7bb92c28c0e4e8f44.svg"/>](https://refactoring.guru/fr/design-patterns/command/csharp/example "Patrons de conception : Commande en C#")[<img width="43" height="56" src="../../_resources/cpp_23110627b7bc4a1f971ed7921a374376.svg"/>](https://refactoring.guru/fr/design-patterns/command/cpp/example "Patrons de conception : Commande en C++")[<img width="43" height="56" src="../../_resources/php_8f6e35dab074422d90dfca2f3a3649f8.svg"/>](https://refactoring.guru/fr/design-patterns/command/php/example "Patrons de conception : Commande en PHP")[<img width="43" height="56" src="../../_resources/python_7c6111689cc442b2ab75afd599f6bd46.svg"/>](https://refactoring.guru/fr/design-patterns/command/python/example "Patrons de conception : Commande en Python")[<img width="43" height="56" src="../../_resources/ruby_793212824ac74f4397cf08fcf74f28f0.svg"/>](https://refactoring.guru/fr/design-patterns/command/ruby/example "Patrons de conception : Commande en Ruby")[<img width="43" height="56" src="../../_resources/swift_6a6f040c76a542729b27db1390d3c1d5.svg"/>](https://refactoring.guru/fr/design-patterns/command/swift/example "Patrons de conception : Commande en Swift")[<img width="43" height="56" src="../../_resources/typescript_ecb8e29d27aa408e9a018e02fa1f4f54.svg"/>](https://refactoring.guru/fr/design-patterns/command/typescript/example "Patrons de conception : Commande en TypeScript")[<img width="43" height="56" src="../../_resources/go_076a877ae3734eec84f2229d11cf7892.svg"/>](https://refactoring.guru/fr/design-patterns/command/go/example "Patrons de conception : Commande en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_30b19b0f23f74abbafdb4404cb3.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Itérateur](https://refactoring.guru/fr/design-patterns/iterator)

#### Retour

[Chaîne de responsabilité](https://refactoring.guru/fr/design-patterns/chain-of-responsibility)

[<img width="245" height="375" src="../../_resources/web-cover-fr_3f4a8916710b4671a528035f3c803611.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/command "English") [Español](https://refactoring.guru/es/design-patterns/command "Español") [Français](https://refactoring.guru/fr/design-patterns/command "Français") [Polski](https://refactoring.guru/pl/design-patterns/command "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/command "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/command "Русский") [Українська](https://refactoring.guru/uk/design-patterns/command "Українська") [中文](https://refactoringguru.cn/design-patterns/command "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_04757fa9b0e641afad98e62c6cdde892.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_16151f0b6383437e8823a655796eb20e.png)

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
 ![Organization address](../../_resources/fa-building_cf28be56a9eb4af29104ab20662cfc63.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)