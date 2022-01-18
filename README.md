# Software Architektur Guide [DE]

> ⚠️ Der Guide wird laufend ergänzt, erweitert und angepasst. 

# Abkürzungen

Diese Abkürzungen werden hier genutzt. 

- Software Architektur -> SA
- Software engineering -> SE
- Test driven development -> TDD
- Domain driven development -> Domain driven development

# Inhalte 

- [Definition](#definition)
- [Begriffe](#begriffe)
- [Kommunikation](#kommunikation)
  - [Angepasste Sprache](#angepasste-sprache)
  - [Explizit vs Implizit](#explizit-vs-implizit)
- [Stakeholderanalyse](#stakeholderanalyse)
- [Risikoanalyse](#risikoanalyse)
- [Software Architektur entwerfen](#software-architektur-entwerfen)
  - [Top-down Ansatz](#top-down-ansatz)
  - [Bottom-up Ansatz](#bottom-up-ansatz)
  - [Ansichtbasierte Architektur](#ansichtbasierte-architektur)
- [Software Architektur Muster](#software-architektur-muster)
- [Entwurfsmuster](#entwurfsmuster)
  - [Erzeugungsmuster](#erzeugungsmuster)
    - [Fabrik](#fabrik)
    - [Singleton](#singleton)
    - [Erbauer](#erbauer)
  - [Strukturmuster](#strukturmuster)
    - [Adapter](#adapter)
    - [Bridge](#bridge)
    - [Fassade](#fassade)
    - [Proxy](#proxy)
  - [Verhaltensmuster](#verhaltensmuster)
    - [Beobachter](#beobachter)
    - [Iterator](#iterator)
- [Software Architektur bewerten](#software-architektur-bewerten)
- [Clean coding](#clean-coding)
- [Literatur](#literatur)


---


## Definition

Für SA gibt es keine einheitliche Definition. 
An dieser Stelle sollen mehrere Autoren/Organisationen zitiert werden. 

"Architecture is about the important stuff. Whatever that is." - Ralph Johnson

"The goal of software architecture is to minimize the human resources required to build and maintain the required system" - Robert C. Martin

"Fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution." - ISO/IEC/IEEE 42010 


## Begriffe

Komponente - Einheit eines Systems (Funktion, Klasse, Modul)

System - Zusammenschluss von Einheiten / Komponenten, die einem bestimmten Zweck dienen

## Kommunikation

### Angepasste Sprache

Je nach Art des Stakeholder sollte die Sprache entsprechend ausfallen. 
z.B. sollte man mit einem Business analysten eher auf der fachlichen Ebene sprechen und technische Details vermeiden. 

### Explizit vs Implizit

Es sollte vermieden werden implizite Annahmen zu machen. 
Das kann schnell zu unterschiedlichen Interpretationen führen. 
Explizit ansprechen und dokumentieren als impliziert annehmen. 


## Aufgaben eines Software Architekten

- Anforderungen analysieren, klären und gegebenfalls verfeinern 
- Architekturentscheidungen treffen
- Kontinuierliche Analyse der Architekur
- Wissen im Geschäftsfeld aufbauen
- Aktuelle Trends erfassen 
- Designentscheidungen kommunizieren & dokumentieren & Feedback einholen 

## Stakeholderanalyse

## Risikoanalyse

## Software Architektur entwerfen

- SA sollte wenn möglich iterativ entwickelt werden
- Frühes Feedback einholen

### Top-down Ansatz

Ein System/Komponente/Subsystem wird von "oben" nach "unten" aufgebaut.

### Bottom-up Ansatz

Ein System/Komponente/Subsystem wird von "unten" (detail level) nach "oben" aufgebaut.

### Ansichtbasierte Architektur

- Komponenten Ansicht
- Laufzeit Ansicht
- Hardware Ansicht
- Kontext Ansicht

## Software Architektur Muster

## Entwurfsmuster

### Erzeugungsmuster 

#### Fabrik

#### Singleton

#### Erbauer

### Strukturmuster

#### Adapter

#### Bridge 

#### Fassade

### Proxy

### Verhaltensmuster 

#### Beobachter

#### Iterator

## Software Qualität

Nach ISO 25010:

- Angemessenheit (Functional Suitability)
- Sicherheit (Security)
- Benutzbarkeit (Usability)
- Wartbarkeit (Maintainability)
- Portierbarkeit ()
- Kompatibilität ()
- Leistungsfähigkeit ()
- Wiederverwendbarkeit (Reusability)

> Bei Qualitätseigenschaften können schnell Interessenskonflikte enstehen. So kann die erhöhte Sicherheit für ein schlechtere Performance sorgen. Häufig ist nach Priorität abzuwägen.

## Software Architektur bewerten

Im Rahmen der SA lassen sich zwei Dinge bewerten:
- Prozesse
- Artefakte (Code, Anforderungen, Dokumente)

Dazu gibt es auch die 

- qualitativer Bewertung 
- quantitative Bewertung 

## Clean coding

## Literatur 

- Fundamentals of Software Architecture (Mark Richards & Neal Ford)
- Entwurfsmuster (Matthias Geirhos)
- Clean Architecture (Robert C. Martin)
- The Clean Coder (Robert C. Martin)
- Einführung in die Softwaretechnik (Manfred Broy & Marco Kuhrmann)
