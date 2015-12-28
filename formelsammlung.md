# Objektorientiertes Design

## SOLID

### Single Responsibility

Jede Klasse sollte nur eine Verantwortung haben (Unix-Philosophie)

Grund: steigende Änderungswahrscheinlichkeit führt zu steigender
Fehlerwahrscheinlichkeit

### Open-Closed-Prinzip

Klassen sollten für Erweiterungen offen sein, gleichzeitig aber ihr
Verhalten nicht ändern. (open for extension, closed for modification)

Beispiel: Vererbung ändert Basisverhalten nicht, erweitert aber um
zusätzliche Funktionen oder Daten.

### Liskovsches Substitutionsprinzip

Instanzen von abgeleiteten Klassen müssen sich so verhalten, dass sie
das Verhalten der Basisklasse 1:1 abbilden können. Überschriebene
Methoden dürfen ihr Basisverhalten nicht ändern.

Ziel: Vermeidung von Überraschungen beim Anwender.

### Interface Segregation

Clients sollten nicht dazu gezwungen werden, von Interfaces abzuhängen,
die sie nicht verwenden. Clients agieren nur mit Interfaces, die das und
*nur* das implementieren, was die Clients benötigen.

Ziel: Aufteilung großer Interfaces und Reduktion unnötiger
Abhängigkeiten.

### Dependency Inversion

Abhängigkeiten sollten von kronkreteren Modulen niedrigerer Ebenen zu
abstrakten Modulen höherer Ebenen gerichtet sein.

Ziel: Reduktion von Abhängigkeiten zwischen Modulen.

#### Dependency Lookup

Objekt sucht nach benötigtem Objekt, beispielsweise in einem Register.
Damit hängt es nicht von jedem benötigten Objekt ab, sondern nur vom
Register. Das Objekt stellt zur Laufzeit seine Verknüpfung mit einem
anderen Objekt her, indem es nach einem logischen Namen sucht. (Aktiv)

Beispiel: Person ruft Escort-Service an und fragt gezielt nach "Scarlett
O'Hara".

#### Dependency Injection

Erzeugung von Objekten und Zuordnungen von Abhängigkeiten werden an eine
dritte Partei delegiert, damit sind die Objekte voneinander nicht
abhängig. Abhängigkeiten werden von außen injiziert. (Passiv)

Beispiel: Der Escort-Service weiß, was seine Kunden brauchen und teilt
diesen ohne Nachfragen oder Auftrag eine Escort-Dame zu.

## Kopplung

### Strong cohesion

Eine Klasse mit "strong cohesion" hat wenige, wohldefinierte Aufgaben,
die eng miteinander verwandt sind.

Beispiel: High Cohesion: Escort-Dame. Low Cohesion: Partnerin.

### Loose coupling

Komponenten einer Software kommunizieren nur über wenige Schnittstellen
mit anderen Komponenten, dadurch ist die Abhängigkeit sehr gering.
Globale Variablen, öffentliche Attribute, Singletons, Zustände in
Datenbanken: Schlecht, da sie Schnittstellen vergrößern. // TODO: Bild
strong cohesion/loose coupling

# Architekturmuster

## Unterschied Architektur- zu Entwurfsmuster

Architekturmuster beziehen sich auf das Gesamtsystem (grundlegendes
Problem), während sich Entwurfsmuster auf die Teilgebiete und die
Umsetzung beziehen (lokales Problem)

Beispiel: Einkommensgenerierung. Architektur: Gründung Escort-Service.
Entwurf: Standort, Personal, Verwaltung.

## Layer

Software ist in logischen Schichten organisiert. Problem:

-   Unterteilung komplexer Systeme (Abhängigkeitsminimierung)
-   Horizontale Strukturierung (Obere Schicht = Client, Untere Schicht =
    Server)
-   Jede Schicht repräsentiert eine Abstraktion

Lösung:

-   Jede Schicht verfolgt einen bestimmten Zweck
-   Client-Server Modell

// TODO: Bild Layers

### strict layering / closed architecture

Schichten dürfen nicht übersprungen werden und dürfen nur die direkt
darunter liegende Schicht aufrufen.

Vorteil: leichter test- und wartbar.

### loose layering / open architecture

Schichten dürfen übersprungen werden, höchste Schicht kann direkt mit
tiefster Schicht kommunizieren.

Vorteil: höhere Performanz.

## Pipes and Filters

Filter modifizieren Daten, Pipes transportieren Daten. Problem:

-   Unterteilung Datenstrom in Einzelschritte
-   Einfach erweiterbar
-   Parallelverarbeitung gut möglich

Lösung:

-   Zerlegung in Einzelschritte
-   Jeder Verarbeitungsschritt ist ein Filter
-   Jeder Transportweg ist eine Pipe
-   Filter lesen, verarbeiten und geben Daten aus
-   Pipes transportieren und puffern Daten
-   Pipes entkoppeln Entitäten
-   Asynchrone Schreib- und Lesevorgänge möglich

// TODO: Bild Pipes / Filters

### Aktiver Filter

Eigenständig laufende Prozesse oder Threads, werden nach Instanziierung
von übergeordnetem Programmteil aufgerufen. Laufen kontinuierlich.

### Passiver Filter

Aufruf durch benachbartes Filterelement.

#### Push

Vorgänger hat Ausgabedaten erzeugt und ruft zu aktivierenden Filter auf.
(Daten werden zur Verfügung gestellt)

#### Pull

Nachfolger fordert Daten an. (Daten werden benötigt)

## Plug-In

Interface beschreibt Funktion, Plug-Ins sind die konkreten Umsetzungen
dieser Interfaces. Der Plugin-Manager sorgt für die korrekte Zuordnung
der konkreten Umsetzung zum Interface bei Aufruf.

Problem:

-   Erweiterbarkeit ohne Modifikation
-   Neue Funktionalität durch Dritte
-   Core-System funktioniert ohne Zusätze, bietet mit ihnen aber mehr
    Funktionalität

Lösung:

-   Interfaces für Erweiterungen
-   Plug-Ins implementieren diese
-   Plug-Ins ersetzen zur Laufzeit die Interfaces durch konkrete
    Implementierungen.

Beispiel: Interface: Escort-Dame, Konkrete Umsetzungen: Cindy, Mandy,
Scarlett, Plugin-Manager: Telefonistin der Agentur.

// TODO: Bild Plug-In

## Broker

Broker ist verantwortlich für die Koordination von Kommunikation
(Weitergabe von Anfragen und Antworten)

Problem:

-   Skalierbarkeit
-   Verteilbarkeit
-   Lose Kopplung
-   Gegenseitige Abhängigkeit nicht sinnvoll

Lösung:

-   Komponenten klassifiziert durch deren Rolle
-   Serverkomponenten bieten n\>=1 Services
-   Clients nutzen m\>=1 Services
-   Rollen können sich ändern
-   Broker bietet Vermittlungs- und Zwischenschicht

### marshalling

Serialisierung von Methodenaufrufen für Netzwerktransmission

### unmarshalling

Rückkonvertierung in Standard-Methodenaufrufe auf Empfängerseite

Beispiel: Pornokino mit Gloryhole. Die Wand mit dem Loch ist der Broker,
die Kundschaft sind die Clients, die Dienstleisterinnen sind die für den
Client unzugänglichen Systeme und unterscheiden und/oder ergänzen sich
im Funktionsumfang.

// TODO: Bild Broker

## Service-Oriented Architecture (SOA)

// TODO: Bild SOA

## Model-View-Controller (MVC)

Problem: Lösung: // TODO: Bild MVC

### Model

Speichert Daten.

### View

Verantwortlich für jegliche Art der Darstellung, kann lesend auf Model
zugreifen.

#### Push

Model sendet aktiv Daten an die View, welche dann die Darstellung
aktualisiert.

#### Pull

View wird durch Model informiert, dass neue Daten vorhanden sind. Wird
die View *immer* informiert ist es aktiver Pull, sonst passiver. Der
Controller wertet Benutzereingaben aus und aktualisiert ggf. das Model
oder die View. Die View zieht sich die neuen Daten nach Information
durch das Model.

### Controller

Verarbeitet Nutzeranfragen, kann lesend und schreibend auf Model
zugreifen.

# Entwurfsmuster

## Zweck

## Abstraktion

### Abstrakte Klasse

Wenn Methoden auf eine bestimmte Art implementiert werden müssen oder
wenn non-public Daten oder Methoden enthalten sein sollen. Dies
beeinträchtigt nicht die Möglichkeit, reine Methodenköpfe zu definieren,
die überschrieben werden müssen.

### Interface

Reine Schnittstellenbeschreibung.

## Erzeugungsmuster

### Factory

Problem: Lösung: // TODO: Bild

### Abstract Factory

Problem: Lösung: // TODO: Bild

### Singleton

Problem: Lösung: // TODO: Bild

### Object Pool

Problem: Lösung: // TODO: Bild

## Strukturmuster

### Adapter

Problem: Lösung: // TODO: Bild

### Bridge

Problem: Lösung: // TODO: Bild

### Decorator

Problem: Lösung: // TODO: Bild

### Facade

Problem: Lösung: // TODO: Bild

### Composition

Problem: Lösung: // TODO: Bild

### Proxy

Problem: Lösung: // TODO: Bild

## Verhaltensmuster

### Command

Problem: Lösung: // TODO: Bild

### Observer

Problem: Lösung: // TODO: Bild

### Strategy

Problem: Lösung: // TODO: Bild

### Mediator

Problem: Lösung: // TODO: Bild

### State

Problem: Lösung: // TODO: Bild

### Role

Problem: Lösung: // TODO: Bild

### Visitor

Problem: Lösung: // TODO: Bild

### Iterator

Problem: Lösung: // TODO: Bild
