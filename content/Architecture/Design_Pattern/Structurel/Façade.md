---
title: Façade / Facade
updated: 2021-12-26 15:28:26Z
created: 2021-12-26 15:10:29Z
source: https://refactoring.guru/fr/design-patterns/facade
tags:
  - design_pattern
  - structurel
---

**Façade** procure une interface offrant un accès simplifié à une librairie, un framework ou à n’importe quel ensemble complexe de classes.

![Patron de conception&nbsp;façade](../../_resources/facade_143c03590f2741a487e911c4f5b00333.png)

## Problème

Imaginez que vous devez adapter votre code pour manipuler un ensemble d’objets qui appartiennent à une librairie ou à un framework assez sophistiqué. D’ordinaire, vous initialisez tous ces objets en premier, gardez la trace des dépendances et appelez les méthodes dans le bon ordre, etc.

Par conséquent, la logique métier de vos classes devient fortement couplée avec les détails de l’implémentation des classes externes, rendant cette logique difficile à comprendre et à maintenir.

## Solution

Une façade est une classe qui procure une interface simple vers un sous-système complexe de parties mobiles. Les fonctionnalités proposées par la façade seront plus limitées que si vous interagissiez directement avec le sous-système, mais vous pouvez vous contenter de n’inclure que les fonctionnalités qui intéressent votre client.

Une façade se révèle très pratique si votre application n’a besoin que d’une partie des fonctionnalités d’une librairie sophistiquée parmi les nombreuses qu’elle propose.

Par exemple, une application qui envoie des petites vidéos de chats comiques sur des réseaux sociaux peut potentiellement utiliser une librairie de conversion vidéo professionnelle. Mais la seule chose dont vous ayez réellement besoin, c’est d’une classe dotée d’une méthode `encoder(fichier, format)`. Après avoir créé cette classe et intégré la librairie de conversion vidéo, votre façade est opérationnelle.

## Analogie

<img width="490" height="0" src="../../_resources/live-example-fr_a6d5d827c38a4dce96246b71fb5cd7dd.png"/>

Passer une commande au téléphone.

Lorsque vous téléphonez à un magasin pour commander, un opérateur joue le rôle de la façade pour tous ses services. L’opérateur vous sert d’interface vocale avec le système de commandes, les moyens de paiement et les différents services de livraison.

## Structure

<img width="560" height="0" src="../../_resources/structure_36eb5b45f1d24866b7b0dca2920bebf6.png"/>

1.  La **Façade** procure un accès pratique aux différentes parties des fonctionnalités du sous-système. Elle sait où diriger les requêtes du client et comment utiliser les différentes parties mobiles.
    
2.  Une classe **Façade Supplémentaire** peut être créée pour éviter de polluer une autre façade avec des fonctionnalités qui pourraient la rendre trop complexe. Les façades supplémentaires peuvent être utilisées à la fois par le client et par les autres façades.
    
3.  Le **Sous-système Complexe** est composé de dizaines d’objets variés. Pour leur donner une réelle utilité, vous devez plonger au cœur des détails de l’implémentation du sous-système, comme initialiser les objets dans le bon ordre et leur fournir les données dans le bon format.
    
    Les classes du sous-système ne sont pas conscientes de l’existence de la façade. Elles opèrent et interagissent directement à l’intérieur de leur propre système.
    
4.  Le **Client** passe par la façade plutôt que d’appeler les objets du sous-système directement.
    

## Pseudo-code

Dans cet exemple, la **Façade** simplifie l’interaction avec un framework complexe de conversion vidéo.

<img width="570" height="0" src="../../_resources/example_881a40ce195e497ca1e3ce386048540e.png"/>

Utilisation d’une classe façade dans un exemple d’isolation de dépendances multiples.

Plutôt que d’adapter directement la base de votre code à des dizaines de classes, vous créez une façade qui permet d’encapsuler cette fonctionnalité et de la cacher du reste du code. Cette structure permet de minimiser le travail nécessaire aux futures évolutions du framework, ou au remplacement de ce dernier par un autre. Vous pouvez vous contenter de modifier l’implémentation des méthodes de la façade.

```
// Voici quelques classes d’un framework complexe de conversion
// vidéo externe. Nous n’avons pas la main sur le code, nous ne
// pouvons donc pas le simplifier.
class VideoFile
// ...

class OggCompressionCodec
// ...

class MPEG4CompressionCodec
// ...

class CodecFactory
// ...

class BitrateReader
// ...

class AudioMixer
// ...

// Nous créons une classe façade pour cacher la complexité du
// framework derrière une interface simple. C’est un compromis
// entre fonctionnalité et simplicité.
class VideoConverter is
    method convert(filename, format):File is
        file = new VideoFile(filename)
        sourceCodec = new CodecFactory.extract(file)
        if (format == "mp4")
            destinationCodec = new MPEG4CompressionCodec()
        else
            destinationCodec = new OggCompressionCodec()
        buffer = BitrateReader.read(filename, sourceCodec)
        result = BitrateReader.convert(buffer, destinationCodec)
        result = (new AudioMixer()).fix(result)
        return new File(result)

// Les classes application ne dépendent pas d’un milliard de
// classes fournies par un framework complexe. Si vous décidez
// de changer de framework, vous avez seulement besoin de
// réécrire la classe façade.
class Application is
    method main() is
        convertor = new VideoConverter()
        mp4 = convertor.convert("funny-cats-video.ogg", "mp4")
        mp4.save()

```

## Possibilités d’application

Utilisez la façade si vous avez besoin d’une interface limitée mais directe à un sous-système complexe.

Souvent, les sous-systèmes gagnent en complexité avec le temps et mettre en place un patron de conception implique de créer de nouvelles classes. Dans de nombreux contextes, un sous-système peut se révéler plus souple et facile à réutiliser, mais la quantité de travail pour configurer et écrire le code de base ne fait que croitre. La façade tente de remédier à ce problème en fournissant un raccourci aux fonctionnalités du sous-système qui sont adaptées au besoin du client.

Utilisez la façade si vous voulez structurer un sous-système en plusieurs couches.

Créez des façades pour définir des points d’entrée à chaque niveau d’un sous-système. Vous pouvez réduire le couplage entre plusieurs sous-systèmes en les obligeant à communiquer au travers de façades.

Reprenons notre exemple de framework de conversion vidéo. Il peut être désassemblé en deux couches : tout ce qui concerne la vidéo d’un côté et l’audio de l’autre. Créez une façade pour chaque couche. Vous pouvez utiliser ce genre de façades pour faire communiquer les classes de ces couches ensemble. Cette approche peut fortement ressembler au patron de conception [Médiateur](https://refactoring.guru/fr/design-patterns/mediator).

## Mise en œuvre

1.  Vérifiez s’il est possible de fournir une interface plus simple que ce qu’un sous-système vous propose. Si cette interface peut découpler le code client des nombreuses classes du sous-système, vous êtes sur la bonne voie !
    
2.  Déclarez et implémentez cette interface en tant que nouvelle façade. Elle doit rediriger les appels du code client aux objets appropriés du sous-système. Elle doit également être responsable de l’initialisation du sous-système et gérer son cycle de vie, sauf si le code client s’en occupe déjà.
    
3.  Pour exploiter au maximum les avantages de la façade, toute communication du code client avec le sous-système doit passer par elle. Cette manipulation protège le code client de tout effet de bord que des modifications apportées au sous-système pourraient provoquer. Si un sous-système est mis à jour, vous n’aurez besoin que de modifier le code de la façade.
    
4.  Si la façade devient [trop grande](https://refactoring.guru/fr/smells/large-class "Large Class"), vous devriez envisager de transférer une partie de ses comportements vers une nouvelle façade plus spécialisée.
    

## Avantages et inconvénients

- Vous pouvez isoler votre code de la complexité d’un sous-système.

- Une façade peut devenir [un objet omniscient](https://refactoring.guru/fr/antipatterns/god-object) couplé à toutes les classes d’une application.

## Liens avec les autres patrons

- La [Façade](https://refactoring.guru/fr/design-patterns/facade) définit une nouvelle interface pour les objets existants, alors que l’[Adaptateur](https://refactoring.guru/fr/design-patterns/adapter) essaye de rendre l’interface existante utilisable. L’*adaptateur* emballe généralement un seul objet alors que la *façade* s’utilise pour un sous-système complet d’objets.
    
- La [Fabrique abstraite](https://refactoring.guru/fr/design-patterns/abstract-factory) peut remplacer la [Façade](https://refactoring.guru/fr/design-patterns/facade) si vous voulez simplement cacher au code client la création des objets du sous-système.
    
- Le [Poids mouche](https://refactoring.guru/fr/design-patterns/flyweight) vous montre comment créer plein de petits objets alors que la [Façade](https://refactoring.guru/fr/design-patterns/facade) permet de créer un seul objet qui représente un sous-système complet.
    
- La [Façade](https://refactoring.guru/fr/design-patterns/facade) et le [Médiateur](https://refactoring.guru/fr/design-patterns/mediator) ont des rôles similaires : ils essayent de faire collaborer des classes étroitement liées.
    
    - La *façade* définit une interface simplifiée pour un sous-système d’objets, mais elle n’ajoute pas de nouvelles fonctionnalités. Le sous-système est conscient de l’existence de la façade. Les objets situés à l’intérieur du sous-système peuvent communiquer directement.
    - Le *médiateur* centralise la communication entre les composants du système. Les composants ne voient que l’objet médiateur et ne communiquent pas directement.
- Une classe [Façade](https://refactoring.guru/fr/design-patterns/facade) peut souvent être transformée en [Singleton](https://refactoring.guru/fr/design-patterns/singleton), car un seul objet façade est en général suffisant.
    
- La [Façade](https://refactoring.guru/fr/design-patterns/facade) et la [Procuration](https://refactoring.guru/fr/design-patterns/proxy) ont une similarité : ils mettent en tampon une entité complexe et l’initialisent individuellement. Contrairement à la *façade*, la *procuration* implémente la même interface que son objet service, ce qui les rend interchangeables.
    

## Exemples de code

[<img width="43" height="56" src="../../_resources/java_588fa6b18de0492b858fb924dd14f344.svg"/>](https://refactoring.guru/fr/design-patterns/facade/java/example "Patrons de conception : Façade en Java") [<img width="43" height="56" src="../../_resources/csharp_26ffae48663d4e7499096da576bdf4ac.svg"/>](https://refactoring.guru/fr/design-patterns/facade/csharp/example "Patrons de conception : Façade en C#")[<img width="43" height="56" src="../../_resources/cpp_1c7e286ed2b245f685d1ffd9e675c842.svg"/>](https://refactoring.guru/fr/design-patterns/facade/cpp/example "Patrons de conception : Façade en C++")[<img width="43" height="56" src="../../_resources/php_b3dfd9d7530c4af8944636fe8439ae17.svg"/>](https://refactoring.guru/fr/design-patterns/facade/php/example "Patrons de conception : Façade en PHP")[<img width="43" height="56" src="../../_resources/python_5ac324b5665741d1b486505c63adfa93.svg"/>](https://refactoring.guru/fr/design-patterns/facade/python/example "Patrons de conception : Façade en Python")[<img width="43" height="56" src="../../_resources/ruby_b8eec9bbdb4d4dc7a6a6d03adc107337.svg"/>](https://refactoring.guru/fr/design-patterns/facade/ruby/example "Patrons de conception : Façade en Ruby")[<img width="43" height="56" src="../../_resources/swift_9d31aa04ffad47c0ba69b61ea9aa9950.svg"/>](https://refactoring.guru/fr/design-patterns/facade/swift/example "Patrons de conception : Façade en Swift")[<img width="43" height="56" src="../../_resources/typescript_2db4468132b34304ab5ba714fe6907f2.svg"/>](https://refactoring.guru/fr/design-patterns/facade/typescript/example "Patrons de conception : Façade en TypeScript")[<img width="43" height="56" src="../../_resources/go_7fbbdde1f7624969b85989461ae358d6.svg"/>](https://refactoring.guru/fr/design-patterns/facade/go/example "Patrons de conception : Façade en Go")

[<img width="200" height="200" src="../../_resources/patterns-book-banner-3_c86c2d3e8fc3498bad87237369a.png"/>](https://refactoring.guru/fr/design-patterns/book)

### Soutenez notre site gratuit et obtenez l’ebook !

- 22 patrons de conception et 8 principes expliqués en profondeur.
- 444 pages de vulgarisation, bien structurées et faciles à lire.
- 225 illustrations et diagrammes clairs pour aider à la compréhension.
- Une archive avec des exemples de code dans 9 langages.
- Tous les appareils sont pris en charge : formats PDF/EPUB/MOBI/KFX.

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

#### Suivant

[Poids mouche](https://refactoring.guru/fr/design-patterns/flyweight)

#### Retour

[Décorateur](https://refactoring.guru/fr/design-patterns/decorator)

[<img width="245" height="375" src="../../_resources/web-cover-fr_54258c924f3748efbd645dffafc5121f.png"/>](https://refactoring.guru/fr/design-patterns/book)

Cet article est tiré de notre ebook
**Plongée au cœur des
PATRONS DE CONCEPTION**

[En savoir plus…](https://refactoring.guru/fr/design-patterns/book)

Facebook

Twitter

- [Nous contacter](https://feedback.refactoring.guru/?lang=en&show_feedback_form_private=true "Nous contacter")
- [Connexion](https://refactoring.guru/fr/login "Connexion")

[English](https://refactoring.guru/design-patterns/facade "English") [Español](https://refactoring.guru/es/design-patterns/facade "Español") [Français](https://refactoring.guru/fr/design-patterns/facade "Français") [Polski](https://refactoring.guru/pl/design-patterns/facade "Polski") [Português-Br](https://refactoring.guru/pt-br/design-patterns/facade "Português Brasileiro") [Русский](https://refactoring.guru/ru/design-patterns/facade "Русский") [Українська](https://refactoring.guru/uk/design-patterns/facade "Українська") [中文](https://refactoringguru.cn/design-patterns/facade "中文")

[![Refactoring.Guru](../../_resources/logo-covid-winter_687ab0139d9d489db6d1695ff8e9ec45.png)](https://refactoring.guru/fr)

![](../../_resources/winter-hat_ddbf9237283d4d4f800b0db1f36c5cd3.png)

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
 ![Organization address](../../_resources/fa-building_b2849318c4f140f086e879eab5533745.svg) Khmelnitske shosse 19 / 27, Kamianets-Podilskyi, Ukraine, 32305
Email: support@refactoring.guru

Illustrations par [Dmitry Zhart](http://zhart.us/)

- [Termes et conditions](https://refactoring.guru/fr/terms)
- [Politique de protection de la vie privée](https://refactoring.guru/fr/privacy-policy)
- [Politique de l’utilisation des données](https://refactoring.guru/fr/content-usage-policy)