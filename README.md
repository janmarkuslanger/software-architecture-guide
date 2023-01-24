<h1 align="center">Software Architecture Guide</h1>

<p align="center"><img src="logo.svg" alt="Logo" width="200" /></p>

---


# Abbreviations

- Software Architektur -> SA
- Software engineering -> SE
- Test driven development -> TDD
- Domain driven design -> DDD

# Content


- [Definitions](#definitions)
- [Terms](#terms)
- [Communication](#communication)
  - [Adjusted Language](#adjusted-language)
  - [Explicit vs Implicit](#explicit-vs-implicit)
- [Tasks of a Software Architect](#tasks-of-a-software-architect)
- [Stakeholder analysis](#stakeholder-analysis)
- [Risk analysis](#risk-analysis)
- [Design software architecture](#design-software-architecture)
  - [Approaches](#approaches)
    - [Top-down approach](#top-down-approach)
    - [Bottom-up approach](#bottom-up-approach)
    - [View-based architecture](#view-based-architecture)
  - [Black box](#black-box)
  - [White box](#white-box)
- [Software Architecture Pattern](#software-architecture-pattern)
  - [Layers](#layers)
  - [Microservice architecture](#microservice-architecture)
  - [Event-Driven architecture](#event-driven-architecture)
  - [Pipeline architecture](#pipeline-architecture)
  - [Service-oriented architecture](#service-oriented-architecture)
- [Software quality](#software-quality)
- [Evaluate software architecture](#evaluate-software-architecture)
- [Relations](#relations)
- [Design patterns](#design-patterns)
  - [Creational patterns](#creational-patterns)
    - [Factory method](#factory-method)
    - [Singleton](#singleton)
    - [Builder](#builder)
    - [Prototype](#prototype)
  - [Structural patterns](#structural-patterns)
    - [Adapter](#adapter)
    - [Bridge](#bridge)
    - [Composite](#composite)
    - [Decorator](#decorator)
    - [Fassade](#fassade)
    - [Proxy](#proxy)
    - [Flyweight](#flyweight)
  - [Behavioral patterns](#behavioral-patterns)
    - [Observer](#observer)
    - [Iterator](#iterator)
    - [State](#state)
    - [Mediator](#mediator)
    - [Template method](#template-method)
    - [Memento](#memento)
    - [Strategy](#strategy)
    - [Visitor](#visitor)
    - [Chain of Responsibility](#chain-of-responsibility)
- [SOLID](#solid)
  - [Single-responsiblity Principle](#single-responsiblity-principle)
  - [Open-closed Principle](#open-closed-principle)
  - [Liskov Substitution Principle](#liskov-substitution-principle)
  - [Interface Segregation Principle](#interface-segregation-principle)
  - [Dependency Inversion Principle](#dependency-inversion-principle)
- [Design principles](#design-principles)
  - [Dependency injection](#dependency-injection)
  - [Interface vs Implementation](#interface-vs-implementation)
- [Clean coding](#clean-coding)
- [Resources](#resources)
- [Literature](#literature)


---


# Definitions

There is not one final definition for software archtecture. 
Several authors/organizations will be cited at this point. 

"Architecture is about the important stuff. Whatever that is." - Ralph Johnson

"The goal of software architecture is to minimize the human resources required to build and maintain the required system" - Robert C. Martin

"Fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution." - ISO/IEC/IEEE 42010 


# Terms

**Building block**: A building block is a unit of a system. A building block could be a component, module, class, configuration, system, or subsystem.

**System**: A system is an association of building blocks. A system has a specific goal. 

**Artifact**: An artifact is a "piece" of a product. It can be tests, code, documentation, or anything similar. 

**Coupling**: Degree of dependency between two building blocks. Types of dependencies are temporal, data, structure, or hardware.
Example: Imagine A class car that uses many different classes in its methods like an Engine class or Tires class has a high coupling. 

**Cohesion**: Degree of cohesion of a unit in a system. Functionality, values, and properties with the same scope should belong together. 
Example: Imagine two methods for a Car.


# Communication

## Adjusted Language

The way an architect talks to stakeholders should be different.
It should depend on the stakeholders and their technical knowledge. 


## Explicit vs Implicit

Assumptions and decisions should always be explicitly communicated and documented.
Implicit assumptions such as "It was clear to me" lead to misunderstandings and problems. 


# Tasks of a Software Architect

- Analyze, clarify and, if necessary, refine requirements 
- Make architectural decisions
- Continuously analyze architecture
- Build knowledge in the business area
- Capture current trends 
- Communicate & document design decisions & gather feedback 


# Stakeholder analysis

The stakeholder analysis is about identifying the people relevant to the project. These stakeholders can vary significantly from one organization to another and from one project to another, be it the development manager, the budget manager, another department, or, in the case of a service company, the customer. Each of these stakeholders contributes to the success of the project. Once the stakeholders have been identified, they are prioritized, i.e., do they only need to be informed, or are they active participants in the project.


# Risk analysis

The risk analysis includes the following steps:

- Identify the risk
- Reducing the consequences of a risk
- Reducing the probability of a risk occurring
- Risk monitoring

It is essential to consider the different types of risks:

- Known risks: risks that are already known and can/will occur.
- Known risks from other projects: This means chances known from experience, but it is still unknown whether they have any relevance for the project.
- Unknown risks

Within the software architecture, we can consider the following processes:

- Risk Identification
- Risk Analysis
  - Qualitative risk analysis
  - Quantitative Risk Analysis
- Risk planning
- Risk monitoring


# Design software architecture


## Approaches

- Software architecture should be developed iteratively if possible
- Obtain early feedback


### Top-down approach

A system/component/subsystem is designed from "top" to "bottom".


### Bottom-up approach

A system/component/subsystem is designed from the "bottom" (detail level) to the "top".


### View-based architecture

- Components View
- Runtime View
- Hardware View
- Context View


## Black box

- A "black box" hides its interior. 
- Inside: dependencies / processes / data structure / data
- Focus on the external behavior 
- Tasks of the building block to the outside
- Offered interfaces 
- Required interfaces 


## White box 

- a "white box" shows its inner behaviour and attributes 
- Inner: Inner structure / dependencies / data structure 


# Software Architecture Pattern


## Layers

- Abstraction layers: higher layers access lower layers via interface
- Layers to separate functionality and areas of responsibility
- Calls only take place top-down, i.e., only the upper layers access lower layers, not vice versa

## Microservice architecture

Microservice architecture descripes a system that consists of multiple services. 
Each service must be loosley couped, maintainable and testable and independently deployable.

## Event-driven architecture

## Pipeline architecture

## Service-oriented architecture

- The system consists of one or several services
- Services can depend on other services
- Service communicate mostly over HTTP like REST 
- Most systems have a single database instance 
- Every service has its function 


# Software quality

There is no universal definition of software quality. The ISO organization is constantly releasing new standards to describe software quality. These standards consist of many properties and sub-properties, such as maintainability. Conflicts of interest can quickly arise in the case of quality properties. For example, increased security can lead to poorer performance. It is often necessary to weigh up priorities.


# Evaluate software architecture

Two things can be evaluated in the context of software architecture:
- Processes
- Artifacts (code, requirements, documents)

For this purpose there is also the 

- qualitative assessment 
- quantitative evaluation 


# Relations

**Inheritance**: This relation exists when a class inherits from another. 
E.g., When Class `Employee` (Subclass) inherits from `Human` (Superclass) . 

**Association**: An arcitect should use Association when there is no dependency between two classes, but those classes can communicate.
E.g., Modeling a company with `Employee` you could add `Car` as an association because the Employee can use it. 

**Aggregation**: An arcitect should use Aggregation if one class consists of several other classes. But these classes can also exist on their own. 
E.g., the class `BusDriver` consists of Class `Bus` and `Driver`. `Bus` and `Driver` can exist on their own. 

**Composition**: An arcitect should use Composition if one class consists of several other classes. But these classes cannot exist on their own. 
E.g., the class `Human` consists of Class `Hand` and `Leg`. Both classes cannot exist on their own.


# Design principles

Design principles are helpful solutions for problems in software development. Nevertheless, design patterns can increase complexity without an advantage. It is possible to adjust those patterns if that will fit better to a problem. 


## Creational patterns 


## Factory method

**Problem / Requirements:**<br />
As a developer I want to create classes dynamicly during the runtime escpecially when I do not know which class I will need. 

**Solution:**<br />
Create a function (method) that has the responsiblity to create the objects of the classes. 


## Singleton

**Problem / Requirements:**<br />
There should be only one instance from a specific class. 

**Solution:**<br />
The constructor of the Singleton-Class should be private, and the instance stored in a private attribute. 
Additionally, one method (getInstance()) returns that instance. 
The method returns the instance if an instance is already there and creates one before if there is no instance.


## Builder

**Problem / Requirements:**<br />
Complex objects (multiple steps, nested objects, many fields) should be created. 
  
**Solution:**<br />
The creation of an object is done by multiple methods in a seperated class.


## Prototype
  
**Problem / Requirements:**<br />
As a developer I want to create a clone of a specifiy object. 
The class of the object might have private attributes, so it is not possible to access it from outside. 
  
**Solution:**<br />
The class should implement a method called `clone`. This method create and returns a new object with the same config. 


## Structural patterns


## Adapter

**Problem / Requirements:**<br />
As a developer I want to archieve compatibility of two classes. 

**Solution:**<br />
One class will be used as an adapter class. In this class we use the methods from the other class and makes them compatible. So the developer just need to use the adapter class. 


## Bridge 

**Problem / Requirements:**<br />
There are lots of different inheritances, including abstraction and implementation. This creates a confusing and difficult to extend class hierarchy.

**Solution:**<br />
Abstraction and implementation are separated and outsourced in separate classes. The abstraction then contains a field as a reference. 

**Example:**<br />
For the abstraction, one could imagine a form. Concrete abstractions would be, for example, a sphere or a square. 
For the implementation, one could imagine material. Concrete implementations would be iron and plastic.
Thus, one could create an instance of a sphere and the instance of iron. The sphere, i.e. the abstraction, is then given the iron, the implementation.


## Composition
  
**Problem / Requirements:**<br />
Various connected and individual objects are to be treated in the same way.

**Solution:**<br />
Implementation of a tree structure consisting of a component (interface or abstract class), a leaf (single object) and a composite (related object).


## Decorator

**Problem / Requirements:**<br />
An extension of an (abstract) base class would lead to many classes.

**Solution:**<br />
A concrete component is "decorated" with variants.
Thus a decorating class is created which has a reference to a base class.
The decoration class then "decorates" the base class. 

 
## Facade
  
**Problem / Requirements:**<br />
Clients need to access complex and confusing systems.

**Solution:**<br />
Unification/bundling of several systems/components/... in one façade.
Clients only access this façade and can use it as a simplified interface. 
The systems behind it are hidden. 


## Proxy

**Problem / Requirements:**<br />
An object / class is not to be accessed directly.
  
**Solution:**<br />
A proxy is placed in front of the target object. Both implement the same interface. The proxy then accesses the target object. 


## Flyweight

**Problem / Requirements:**<br />
Many objects in the system consume many resources.

**Solution:**<br />
Detach reusable objects. 


## Behavioral patterns


## Observer

**Problem / Requirements:**<br />
Objects in a system want to be informed about certain events.

**Solution:**<br />
Implementation of a publisher-subscriber pattern.
The subscriber can "register" with the publisher to "listen" for events.
The publisher collects these. As soon as an event is published, all subscribers are informed. 


## Iterator

**Problem / Requirements:**<br />
It should be possible to iterate over a complex group of objects. Tree structure / list.
  
**Solution:**<br />
Structures are to be traversed with an iterator that always has a reference to the next element.
There is the iterator that holds the methods ready to get the next element. 
The iterator collection is responsible for creating the collection. 


## State

**Problem / Requirements:**<br />
The condition is handled directly in the classes and objects. This makes for many if-else conditions in the classes. 
  
**Solution:**<br />
For states, classes are implemented that store these states. 
 
 
## Mediator

**Problem / Requirements:**<br />
Many classes act together (e.g. UI elements of a form) and are thus closely coupled, as they all depend on each other.
  
**Solution:**<br />
Promotion of loose coupling, in which a central class serves as an intermediary.
Thus, all the complexity lies in this mediator class.
  

## Template method

**Problem / Requirements:**<br />
Several classes have many of the same patterns (or code). 
  
**Solution:**<br />
An abstract class holds a templateMethod and the methods of the individual steps. The steps are coordinated in the templateMethod method. The concrete class, which inherits from the abstract class, can then overwrite individual steps if they are needed.
  

## Memento

**Problem / Requirements:**<br />
The states of an object are to be saved so that they can be accessed or restored at a later time. 

**Solution:**<br />
A memento is made available to a clock lifter (any class).
This ensures that the state (private inner attributes) of the clock lifter is saved.
Objects of the memento are then stored in a so-called keeper. 


## Strategy

**Problem / Requirements:**<br />
For one context (e.g. class) there should be several implementations/algorithms 

**Solution:**<br />
A context has an attribute that points to a strategy.
This strategy contains the different implementations (one strategy - one implementation).
The type of implementation is therefore not controlled by the context itself, but depends on which strategy the client assigns.


## Visitor

**Problem / Requirements:**<br />
A class (element) should use different methods (PDF generation / XML generation) (with different contexts).
This would lead to a violation of the single-responsibility principle. 
  
**Solution:**<br />
Separation of the operation and class hierarchy. An object (visitor) is created which is responsible for the operations.
The element gets contains a method, which gets the visitor passed and then performs the corresponding operation. 


## Chain of Responsibility
  
**Problem / Requirements:**<br />
An (e.g.) object must go through multiple tasks. These tasks must perform sequentially. 

**Solution:**<br />
The object gets sent to a "Chain of Responsibility". This chain is a collection of handlers. The object goes through this chain until a handler answers the request.  


# SOLID

## Single-responsiblity Principle
  
There should be only one reason (actor) to change a class. 
Single responsibility does not mean that a block should only do one thing. 


## Open-closed Principle
  
“Modules should be both open (for extension) and closed (for modification).” - Bertrand Meyer

The open-closed principle means that if you need to change a module it should be possible without modifying the module itself. 


## Liskov Substitution Principle

The Liskov Substitution Principle states that objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program.


## Interface Segregation Principle

The Interface Segregation Principle means that no code should depend on methods it doesn´t use.

For example if there is an interface called `Car` with the methods `refuel`. 
Then you want to create a class `ElectricCar` that implements the `Car` interface. 
Then it must implement the `refuel` method, which doesn´t make sense.


## Dependency Inversion Principle

The Dependency Inversion Principle consists of the following things: 

- Higher modules/classes should not depend on lower modules/classes
- Classes/modules should be abstracted by interfaces
- Interfaces should not depend on details
- Details should depend on interfaces


# Design principle

## Dependency injection
- Dependencies of a class or module should not be constructed inside.
- Dependencies should be passed from outside (e.g Arg in a constructor)

```
// bad
class MyClass {
  new Dependency()
}

// good
class MyClass(Dependency myDependency) {
  this.dependency = myDependency
}
```


## Interface vs implementation

Develop against an interface, not against an implementation.
Because implementations can change quickly, the implementation can vary independently if you develop against an interface.

# Clean coding


# Resources

- https://refactoring.guru/ 
- https://www.udacity.com/course/software-architecture-design--ud821


# Literature 

EN:
- Fundamentals of Software Architecture (Mark Richards & Neal Ford)
- Clean Architecture (Robert C. Martin)
- The Clean Coder (Robert C. Martin)

DE:
- Entwurfsmuster (Matthias Geirhos)
- Einführung in die Softwaretechnik (Manfred Broy & Marco Kuhrmann)


