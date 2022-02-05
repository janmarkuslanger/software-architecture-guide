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
  - [Ansätze](#ansätze)
    - [Top-down Ansatz](#top-down-ansatz)
    - [Bottom-up Ansatz](#bottom-up-ansatz)
    - [Ansichtbasierte Architektur](#ansichtbasierte-architektur)
  - [Black box](#black-box)
  - [White box](#white-box)  
- [Software Architektur Muster](#software-architektur-muster)
  - [Schichten](#schichten)
  - [Microservice-Architektur](#microservice-architektur)
  - [Event-Driven-Architektur](#event-driven-architektur)
  - [Pipeline-Architektur](#pipeline-architektur)
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


# Definition

Für SA gibt es keine einheitliche Definition. 
An dieser Stelle sollen mehrere Autoren/Organisationen zitiert werden. 

"Architecture is about the important stuff. Whatever that is." - Ralph Johnson

"The goal of software architecture is to minimize the human resources required to build and maintain the required system" - Robert C. Martin

"Fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution." - ISO/IEC/IEEE 42010 


# Begriffe

Baustein - Einheit eines Systems (Funktion, Klasse, Modul, Komponente)

System - Zusammenschluss von Bausteinen, die einem bestimmten Zweck dienen

# Kommunikation

## Angepasste Sprache

Je nach Art des Stakeholder sollte die Sprache entsprechend ausfallen. 
z.B. sollte man mit einem Business analysten eher auf der fachlichen Ebene sprechen und technische Details vermeiden. 

## Explizit vs Implizit

Es sollte vermieden werden implizite Annahmen zu machen. 
Das kann schnell zu unterschiedlichen Interpretationen führen. 
Explizit ansprechen und dokumentieren als impliziert annehmen. 


# Aufgaben eines Software Architekten

- Anforderungen analysieren, klären und gegebenfalls verfeinern 
- Architekturentscheidungen treffen
- Kontinuierliche Analyse der Architekur
- Wissen im Geschäftsfeld aufbauen
- Aktuelle Trends erfassen 
- Designentscheidungen kommunizieren & dokumentieren & Feedback einholen 

# Stakeholderanalyse

# Risikoanalyse

# Software Architektur entwerfen

## Ansätze

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

## Black box

- eine "Black box" versteckt sein inneres. 
- Inneres: Abhängigkeiten / Prozesse / Datenstruktur / Daten
- Fokus auf das Verhalten nach Außen 
- Aufgaben des Bausteins nach Außen
- angebotene Schnittstellen 
- benötigte Schnittsellen 

## White box 

- eine "White box" zeigt sein inneres 
- Inneres: Innere Struktur / Abhängikeiten / Datenstruktur 

# Software Architektur Muster

## Schichten 

- Abstraktionschichten: höhere Schichten greifen auf untere Schichten via Schnittstelle zu
- Schichten um Funktionalität und Verantwortungsbereiche zu trennen

## Microservice-Architektur

## Event-Driven-Architektur

## Pipeline-Architektur

# Entwurfsmuster

## Erzeugungsmuster 

### Fabrik

### Singleton

### Erbauer

## Strukturmuster

### Adapter

Problem: 
Ein bestehender Client will über eine bestehende eigene Schnittstelle auf eine Klasse/Objekt zugreifen.

Lösung:
Ein Adapter, der die Target Schnittstelle implementiert und die externe Klasse oder Objekt integriert. Auf diesen Adapter kann der Client nun zugreifen.



Beispiel:
Der Client kann über die Schnittstelle "Target" auf den "Adapter" zugreifen. Der Adapter sorgt dann für einen Aufruf der Methode von der Klasse "AdaptierteKlasse".


### Bridge 

Problem: 
Durch diverse Vererbungen, welche Abstraktion und Implementierung beinhalten, entsteht eine unübersichtliche und schwer erweiterbare Klassenherachie. 

Lösung: 
- Trennung von Abstraktion und Implementierung. 
- Es gibt eine Abstraktionsklasse, von der spezifische Abstraktionen entstehen können. z.B. Dokument -> Rechnung / Angebot
- Es eine Implementierungklasse/Schnittstelle, von der spezifische Implementierungen entstehen können. -> Drucker -> HTMLDrucker / TextDrucker 
- Es können mehrere Implementierung assoziert werden so Könnter der Abstraktion: Form -> Kugel; die Implementierung können zb Farbe -> Rot/Schwarz und/oder Größe -> klein/groß 


<img src="assets/bridge.drawio.png" alt="Bridge Pattern" />

### Fassade

## Proxy

## Verhaltensmuster 

### Beobachter

### Iterator

# Software Qualität

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

# Software Architektur bewerten

Im Rahmen der SA lassen sich zwei Dinge bewerten:
- Prozesse
- Artefakte (Code, Anforderungen, Dokumente)

Dazu gibt es auch die 

- qualitativer Bewertung 
- quantitative Bewertung 

# Clean coding

# Literatur 

- Fundamentals of Software Architecture (Mark Richards & Neal Ford)
- Entwurfsmuster (Matthias Geirhos)
- Clean Architecture (Robert C. Martin)
- The Clean Coder (Robert C. Martin)
- Einführung in die Softwaretechnik (Manfred Broy & Marco Kuhrmann)
