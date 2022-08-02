---
title: Java Versions and Features
updated: 2021-12-26 15:57:37Z
created: 2021-12-23 20:14:45Z
source: https://howtodoinjava.com/java-version-wise-features-history/
tags:
  - multi-paradigme
  - s1
---

| Version | Year | Features added |     |
| --- | --- | --- | --- |
| JDK Beta | 1995 |     |     |
| JDK 1.0 | January 1996 |     |     |
| JDK 1.1 | February 1997 | AWT, JDBC, RMI, JIT |     |
| J2SE 1.2 | December 1998 | Swing, Collections |     |
| J2SE 1.3 | May 2000 | Hotspot JVM, JNDI, JPDA |     |
| J2SE 1.4 | February 2002 | Regular Expressions, Non blocking I/O, JAXP, Exception Handeling |     |
| J2SE 5.0 | September 2004 | Generics, Enumaration, static imports, Varargs, for each, Auto boxing |     |
| Java SE 6 | December 2006 | JAX-WS, JDBC 4, Supports Annotations, JAXB 2.0, compiler level performance |     |
| Java SE 7 | July 2011 | Strings in switch, Concurrency utilities, java.nio packages |     |
| Java SE 8 | March 2014 | lambda expressions, functional interfaces, new Date api, Streams, JavaFX |     |
| Java SE 9 | September 2017 | Modularization, jshell, Reactive Streams |     |
| Java SE 10 | March 2018 | Local-variable type inference, Java-based JIT compiler, Parallel full GC for G1, Thread-local handshakes, Heap allocation on alternative memory devices |     |
| Java SE 11 | September 2018 | Dynamic class-file constants, Epsilon: a no-op garbage collector, Local-variable syntax for lambda parameters, HTTP client |     |
| Java SE 12 | March 2019 | Switch Expressions, Default CDS archives, Microbenchmark, |     |

## Java 17 Features

**Java 17** was released on September 14, 2021. Java 17 is an LTS (Long Term Support) release, like Java 11 and Java 8. [Spring 6 and Spring boot 3](https://www.infoq.com/news/2021/09/spring-6-spring-boot-3-overhaul/) will have first-class support for Java 17. So it is a good idea to plan for upgrading to Java 17.

The below listed 14 JEPs are part of Java 17.

- ([JEP-306](https://openjdk.java.net/jeps/306)) Restore Always-Strict Floating-Point Semantics
- ([JEP-356](https://openjdk.java.net/jeps/356)) Enhanced Pseudo-Random Number Generators
- ([JEP-382](https://openjdk.java.net/jeps/382)) New macOS Rendering Pipeline
- ([JEP-391](https://openjdk.java.net/jeps/391)) macOS/AArch64 Port
- ([JEP-398](https://openjdk.java.net/jeps/398)) Deprecate the Applet API for Removal
- ([JEP-403](https://openjdk.java.net/jeps/403)) Strongly Encapsulate JDK Internals
- ([JEP-406](https://openjdk.java.net/jeps/406)) Pattern Matching for switch (Preview)
- ([JEP-407](https://openjdk.java.net/jeps/407)) Remove RMI Activation
- ([JEP-409](https://openjdk.java.net/jeps/409)) Sealed Classes
- ([JEP-410](https://openjdk.java.net/jeps/410)) Remove the Experimental AOT and JIT Compiler
- ([JEP-411](https://openjdk.java.net/jeps/411)) Deprecate the Security Manager for Removal
- ([JEP-412](https://openjdk.java.net/jeps/412)) Foreign Function & Memory API (Incubator)
- ([JEP-414](https://openjdk.java.net/jeps/414)) Vector API (Second Incubator)
- ([JEP-415](https://openjdk.java.net/jeps/415)) Context-Specific Deserialization Filters

## Java 16 Features

Java 16 was released on 16 March 20121. It was largely a maintenance release, except it made the *Java Records* and *Pattern matching* the standard features of Java language.

- [JEP 338: Vector API (Incubator)](https://mkyong.com/java/what-is-new-in-java-16/#jep-338-vector-api-incubator)
- [JEP 347: Enable C++14 Language Features](https://mkyong.com/java/what-is-new-in-java-16/#jep-347enable-c14-language-features)
- [JEP 357: Migrate from Mercurial to Git](https://mkyong.com/java/what-is-new-in-java-16/#jep-357-migrate-from-mercurial-to-git)
- [JEP 369: Migrate to GitHub](https://mkyong.com/java/what-is-new-in-java-16/#jep-369-migrate-to-github)
- [JEP 376: ZGC: Concurrent Thread-Stack Processing](https://mkyong.com/java/what-is-new-in-java-16/#jep-376-zgc-concurrent-thread-stack-processing)
- [JEP 380: Unix-Domain Socket Channels](https://mkyong.com/java/what-is-new-in-java-16/#jep-380-unix-domain-socket-channels)
- [JEP 386: Alpine Linux Port](https://mkyong.com/java/what-is-new-in-java-16/#jep-386-alpine-linux-port)
- [JEP 387: Elastic Metaspace](https://mkyong.com/java/what-is-new-in-java-16/#jep-387-elastic-metaspace)
- [JEP 388: Windows/AArch64 Port](https://mkyong.com/java/what-is-new-in-java-16/#jep-388-windowsaarch64-port)
- [JEP 389: Foreign Linker API (Incubator)](https://mkyong.com/java/what-is-new-in-java-16/#jep-389-foreign-linker-api-incubator)
- [JEP 390: Warnings for Value-Based Classes](https://mkyong.com/java/what-is-new-in-java-16/#jep-390-warnings-for-value-based-classes)
- [JEP 392: Packaging Tool](https://mkyong.com/java/what-is-new-in-java-16/#jep-392-packaging-tool)
- [JEP 393: Foreign-Memory Access API (Third Incubator)](https://mkyong.com/java/what-is-new-in-java-16/#jep-393-foreign-memory-access-api-third-incubator)
- [JEP 394: Pattern Matching for instanceof](https://mkyong.com/java/what-is-new-in-java-16/#jep-394-pattern-matching-for-instanceof)
- [JEP 395: Records](https://mkyong.com/java/what-is-new-in-java-16/#jep-395-records)
- [JEP 396: Strongly Encapsulate JDK Internals by Default](https://mkyong.com/java/what-is-new-in-java-16/#jep-396-strongly-encapsulate-jdk-internals-by-default)
- [JEP 397: Sealed Classes (Second Preview)](https://mkyong.com/java/what-is-new-in-java-16/#jep-397-sealed-classes-second-preview)

## Java 15 Features

Java 15 was released on 15th Sep’2020. It continues support for various preview features in previous JDK releases; and also introduced some new features.

- [Sealed Classes and Interfaces](https://howtodoinjava.com/java15/sealed-classes-interfaces/) (Preview) (JEP 360)
- [EdDSA Algorithm](https://howtodoinjava.com/java15/java-eddsa-example/) (JEP 339)
- Hidden Classes (JEP 371)
- [Pattern Matching for *instanceof*](https://howtodoinjava.com/java14/pattern-matching-instanceof/) (Second Preview) (JEP 375)
- Removed Nashorn JavaScript Engine (JEP 372)
- Reimplement the Legacy DatagramSocket API (JEP 373)
- Records (Second Preview) (JEP 384)
- Text Blocks become a standard feature. (JEP 378)

## Java 14 Features

[Java 14](https://howtodoinjava.com/java14/java14-new-features/) (released on March 17, 2020) is the latest version available for JDK. Let’s see the new features and improvements, it brings for developers and architects.

- [JEP 305 – Pattern Matching for instanceof (Preview)](https://howtodoinjava.com/java14/pattern-matching-instanceof/)
- [JEP 368 – Text Blocks (Second Preview)](https://howtodoinjava.com/java14/java-text-blocks/)
- [JEP 358 – Helpful NullPointerExceptions](https://howtodoinjava.com/java14/helpful-nullpointerexception/)
- [JEP 359 – Records (Preview)](https://howtodoinjava.com/java14/java-14-record-type/)
- [JEP 361 – Switch Expressions (Standard)](https://howtodoinjava.com/java14/switch-expressions/)
- JEP 343 – Packaging Tool (Incubator)
- JEP 345 – NUMA-Aware Memory Allocation for G1
- JEP 349 – JFR Event Streaming
- JEP 352 – Non-Volatile Mapped Byte Buffers
- JEP 363 – Remove the Concurrent Mark Sweep (CMS) Garbage Collector
- JEP 367 – Remove the Pack200 Tools and API
- JEP 370 – Foreign-Memory Access API (Incubator)

## Java 13 Features

Java 13 (released on September 17, 2019) had fewer developer-specific features. Let’s see the new features and improvements, it brought for developers and architects.

- JEP 355 – Text Blocks (Preview)
- JEP 354 – Switch Expressions Enhancements (Preview)
- JEP 353 – Reimplement the Legacy Socket API
- JEP 350 – Dynamic CDS Archive
- JEP 351 – ZGC: Uncommit Unused Memory
- FileSystems.newFileSystem() Method
- DOM and SAX Factories with Namespace Support

## Java 12 Features

[Java 12](https://howtodoinjava.com/java12/new-features-enhancements/) was released on March 19, 2019. Let’s see the new features and improvements, it brings for developers and architects.

- Collectors.teeing() in Stream API
- String API Changes
- Files.mismatch(Path, Path)
- Compact Number Formatting
- Support for Unicode 11
- Switch Expressions (Preview)

## Java 11 Features

[Java 11](https://howtodoinjava.com/java11/features-enhancements/) (released on September 2018) includes many important and useful updates. Let’s see the new features and improvements, it brings for developers and architects.

- HTTP Client API
- Launch Single-File Programs Without Compilation
- String API Changes
- Collection.toArray(IntFunction)
- Files.readString() and Files.writeString()
- Optional.isEmpty()

## Java 10 Features

After Java 9 release, Java 10 came very quickly. Unlike it’s previous release, Java 10 does not have that many exciting features, still, it has [few important updates](https://howtodoinjava.com/java10/java10-features/) which will change the way you code, and other future Java versions.

- [JEP 286: Local Variable Type Inference](https://howtodoinjava.com/java10/var-local-variable-type-inference/)
- JEP 322: Time-Based Release Versioning
- JEP 304: Garbage-Collector Interface
- JEP 307: Parallel Full GC for G1
- JEP 316: Heap Allocation on Alternative Memory Devices
- JEP 296: Consolidate the JDK Forest into a Single Repository
- JEP 310: Application Class-Data Sharing
- JEP 314: Additional Unicode Language-Tag Extensions
- JEP 319: Root Certificates
- JEP 317: Experimental Java-Based JIT Compiler
- JEP 312: Thread-Local Handshakes
- JEP 313: Remove the Native-Header Generation Tool
- New Added APIs and Options
- Removed APIs and Options

## Java 9 Features

Java 9 was made available on `September, 2017`. The biggest change is the modularization i.e. Java modules.

Some important features/[changes in Java 9](https://howtodoinjava.com/java9/java9-new-features-enhancements/) are:

- [Java platform module system](https://howtodoinjava.com/java9/java-9-modules-tutorial/)
- [Interface Private Methods](https://howtodoinjava.com/java9/java9-private-interface-methods/)
- HTTP 2 Client
- JShell – REPL Tool
- Platform and JVM Logging
- Process API Updates
- Collection API Updates
- [Improvements in Stream API](https://howtodoinjava.com/java9/stream-api-improvements/)
- Multi-Release JAR Files
- @Deprecated Tag Changes
- Stack Walking
- Java Docs Updates
- Miscellaneous Other Features

Please see the updated release info [here](https://openjdk.java.net/projects/jdk9/).

## Java 8 Features

**Release Date** : `March 18, 2014`

Code name culture is dropped. Included features were:

- [Lambda expression](https://howtodoinjava.com/java8/lambda-expressions/) support in APIs
- [Stream API](https://howtodoinjava.com/java8/java-streams-by-examples/)
- [Functional interface](https://howtodoinjava.com/java8/functional-interface-tutorial/) and [default methods](https://howtodoinjava.com/java8/default-methods-in-java-8/)
- [Optionals](https://howtodoinjava.com/java8/java-8-optionals-complete-reference/)
- Nashorn – JavaScript runtime which allows developers to embed JavaScript code within applications
- Annotation on Java Types
- [Unsigned Integer Arithmetic](https://howtodoinjava.com/java8/java-8-exact-airthmetic-operations-supported-in-math-class/)
- Repeating annotations
- [New Date and Time API](https://howtodoinjava.com/java8/date-and-time-api-changes-in-java-8-lambda/)
- Statically-linked JNI libraries
- Launch JavaFX applications from jar files
- Remove the permanent generation from GC

## Java SE 7 Features

**Release Date** : `July 28, 2011`

This release was called “Dolphin”. Included features were:

- JVM support for dynamic languages
- Compressed 64-bit pointers
- [Strings in switch](https://howtodoinjava.com/java7/strings-in-switch-statement/)
- [Automatic resource management in try-statement](https://howtodoinjava.com/java7/try-with-resources/)
- [The diamond operator](https://howtodoinjava.com/java7/improved-type-inference-in-java-7/)
- Simplified varargs method declaration
- Binary integer literals
- [Underscores in numeric literals](https://howtodoinjava.com/java7/improved-formatted-numbers-in-java-7/)
- [Improved exception handling](https://howtodoinjava.com/java7/improved-exception-handling/)
- [ForkJoin Framework](https://howtodoinjava.com/java7/forkjoin-framework-tutorial-forkjoinpool-example/)
- [NIO 2.0](https://howtodoinjava.com/java/nio/nio-read-file/) having support for multiple file systems, file metadata and symbolic links
- [WatchService](https://howtodoinjava.com/java7/auto-reload-of-configuration-when-any-change-happen/)
- Timsort is used to sort collections and arrays of objects instead of merge sort
- APIs for the graphics features
- Support for new network protocols, including SCTP and Sockets Direct Protocol

## Java SE 6 Features

**Release Date** : `December 11, 2006`

This release was called “Mustang”. Sun dropped the “.0” from the version number and version became Java SE 6. Included features were:

- Scripting Language Support
- Performance improvements
- JAX-WS
- JDBC 4.0
- Java Compiler API
- JAXB 2.0 and StAX parser
- Pluggable annotations
- New GC algorithms

## J2SE 5.0 Features

**Release Date** : `September 30, 2004`

This release was called “Tiger”. Most of the features, which are asked in java interviews, were added in this release.

Version was also called 5.0 rather than 1.5. Included features are listed down below:

- [Generics](https://howtodoinjava.com/java/generics/complete-java-generics-tutorial/)
- [Annotations](https://howtodoinjava.com/java/annotations/complete-java-annotations-tutorial/)
- Autoboxing/unboxing
- [Enumerations](https://howtodoinjava.com/java/enum/enum-tutorial/)
- Varargs
- [Enhanced `for each` loop](https://howtodoinjava.com/java/flow-control/enhanced-for-each-loop-in-java/)
- [Static imports](https://howtodoinjava.com/java/basics/static-import-declarations-in-java/)
- New [concurrency utilities](https://howtodoinjava.com/java/multi-threading/executor-framework-tutorial/) in `java.util.concurrent`
- `Scanner` class for parsing data from various input streams and buffers.

## J2SE 1.4 Features

**Release Date** : `February 6, 2002`

This release was called “Merlin”. Included features were:

- `[assert](https://howtodoinjava.com/java/keywords/java-assert/)` keyword
- [Regular expressions](https://howtodoinjava.com/java-regular-expression-tutorials/)
- Exception chaining
- Internet Protocol version 6 (IPv6) support
- [New I/O; NIO](https://howtodoinjava.com/java-nio-tutorials/)
- Logging API
- Image I/O API
- Integrated XML parser and XSLT processor (JAXP)
- Integrated security and cryptography extensions (JCE, JSSE, JAAS)
- Java Web Start
- Preferences API (java.util.prefs)

## J2SE 1.3 Features

**Release Date** : `May 8, 2000`

This release was called “Kestrel”. Included features were:

- HotSpot JVM
- Java Naming and Directory Interface (JNDI)
- Java Platform Debugger Architecture (JPDA)
- JavaSound
- Synthetic proxy classes

## J2SE 1.2 Features

**Release Date** : `December 8, 1998`

This release was called “Playground”. This was a major release in terms of number of classes added (almost trippled the size). “J2SE” term was introduced to distinguish the code platform from J2EE and J2ME. Included features were:

- `strictfp` keyword
- Swing graphical API
- Sun’s JVM was equipped with a JIT compiler for the first time
- Java plug-in
- [Collections framework](https://howtodoinjava.com/java/collections/useful-java-collection-interview-questions/)

## JDK 1 Features

**Release Date** : `January 23, 1996`

This was the [initial release](https://web.archive.org/web/20080205101616/http://www.sun.com/smi/Press/sunflash/1996-01/sunflash.960123.10561.xml) and was originally called **Oak**. This had very unstable APIs and one java web browser named **WebRunner**.

The first stable version, JDK 1.0.2, was called Java 1.

On February 19, 1997, JDK 1.1 was released havind a list of big features such as:

- AWT event model
- Inner classes
- JavaBeans
- JDBC
- RMI
- [Reflection](https://howtodoinjava.com/java/reflection/real-usage-examples-of-reflection-in-java/) which supported Introspection only, no modification at runtime was possible.
- JIT (Just In Time) compiler for Windows

Again, feel free to suggest any **java feature in any java version** which I missed in the above lists.

Happy Learning !!

### Was this post helpful?

Let us know if you liked the post. That’s the only way we can improve.

### Join 7000+ Fellow Programmers

Subscribe to get new post notifications, industry updates, best practices, and much more. Directly into your inbox, for free.

[Subscribe](https://howtodoinjava.com/sendyemails/subscription?f=3qgnusD4fwgMrE1qTMFqStUuFXrFXRMyRQm7C1HVsXueiiFLgewivqQTeeksQPgQ)

### 7 thoughts on “Java Latest Versions and Features”

1.  Ivaylo
    
    [July 19, 2019 at 9:01 pm](https://howtodoinjava.com/java-version-wise-features-history/#comment-56076)
    
    Stream API was introduced in Java 8.
    
    https://www.google.com/search?q=java+stream+api+when+was+it+introduced&rlz=1C1CHBF_enBG856BG856&oq=java+stream+api&aqs=chrome.0.69i59j69i57j0l4.3450j0j7&sourceid=chrome&ie=UTF-8
    
    - [Lokesh Gupta](https://howtodoinjava.com/)
        
        [July 20, 2019 at 11:25 pm](https://howtodoinjava.com/java-version-wise-features-history/#comment-56171)
        
        That’s true. The Java 9 link discusses about improvements added, later.
        
2.  suresh
    
    [October 31, 2017 at 12:01 pm](https://howtodoinjava.com/java-version-wise-features-history/#comment-26637)
    
    Stream API introduced in jdk8 is missed
    
3.  Anil kumar
    
    [July 7, 2017 at 3:32 pm](https://howtodoinjava.com/java-version-wise-features-history/#comment-25763)
    
    Hi Lokesh,
    
    java version 9 release is postponed to 27th September 2017. You wrongly mentioned the year.
    
    - [Lokesh Gupta](https://howtodoinjava.com/)
        
        [July 9, 2017 at 1:05 pm](https://howtodoinjava.com/java-version-wise-features-history/#comment-25785)
        
        Hi Anil, I missed to update the article. In fact, release date has been pushed many times.. so I have added link to release calendar as well, in case there are more changes.
        
4.  Varun T
    
    [June 16, 2017 at 8:51 am](https://howtodoinjava.com/java-version-wise-features-history/#comment-25547)
    
    Java 7 is named as Project Coin
    
    - [Lokesh Gupta](https://howtodoinjava.com/)
        
        [June 16, 2017 at 9:03 am](https://howtodoinjava.com/java-version-wise-features-history/#comment-25548)
        
        Hi Varun, Appreciate your comment. I am referring to [wiki](https://en.wikipedia.org/wiki/Java_version_history#Java_SE_7) which can be wrong, Can you please point to any credible source of information?
        

Comments are closed.

Search for:

#### HowToDoInJava

A blog about Java and its related technologies, the best practices, algorithms, interview questions, scripting languages, and Python.

#### Recommended

[10 Life Lessons](https://howtodoinjava.com/resources/10-life-lessons-i-have-learned-in-last-few-years/)

[Secure Hash Algorithms](https://howtodoinjava.com/java/java-security/how-to-generate-secure-password-hash-md5-sha-pbkdf2-bcrypt-examples/)

[Best Way to Learn Java](https://howtodoinjava.com/resources/best-way-to-learn-java/)

[How to Start New Blog](https://howtodoinjava.com/start-new-blog/)

#### Meta Links

[About Me](https://howtodoinjava.com/about/)

[Contact Us](https://howtodoinjava.com/contact/)

[Privacy policy](https://howtodoinjava.com/privacy-policy/)

[Advertise](https://howtodoinjava.com/advertise/)

[Guest Posts](https://howtodoinjava.com/guest-and-sponsored-posts/)

[Full Archive](https://howtodoinjava.com/full-yearly-archive/)

#### Blogs

[REST API Tutorial](http://restfulapi.net/)

Copyright © 2021 · Hosted on [Bluehost](https://www.bluehost.com/track/howtodp5/) · [Sitemap](https://howtodoinjava.com/sitemap.xml)

## Learn Java

- [Java Date Time](https://howtodoinjava.com/java-date-and-time-apis/)
- [Java Collections](https://howtodoinjava.com/java-collections/)
- [Java Concurrency](https://howtodoinjava.com/java-concurrency-tutorial/)
- [Java New I/O](https://howtodoinjava.com/java-nio-tutorials/)
- [Java OOP](https://howtodoinjava.com/java/oops/object-oriented-programming/)
- [Java Regex](https://howtodoinjava.com/java-regular-expression-tutorials/)
- [Java Puzzles](https://howtodoinjava.com/java-interview-puzzles-answers/)
- [Java Examples](https://howtodoinjava.com/java-examples/examples/)
- [Java Libraries](https://howtodoinjava.com/java/library/readingwriting-excel-files-in-java-poi-tutorial/)
- [Java Resources](https://howtodoinjava.com/resources/)
- [Java 8](https://howtodoinjava.com/java-8-tutorial/)
- [Hibernate](https://howtodoinjava.com/hibernate-tutorials/)
- [JUnit 5](https://howtodoinjava.com/junit-5-tutorial/)
- [Log4j2](https://howtodoinjava.com/log4j2-tutorial/)
- [Maven](https://howtodoinjava.com/maven/)
- [TypeScript](https://howtodoinjava.com/typescript/typescript-tutorial/)