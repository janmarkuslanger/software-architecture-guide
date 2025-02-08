<h1 align="center">A software architecture guide</h1>

<p align="center"><img src="head.webp" alt="Dog assembling building blocks" width="300" /></p>

---

# Introduction 👋

This guide aims to provide a concise overview of various aspects of software architecture. It covers key concepts, architectural styles, and essential principles to help people understand the foundational elements of building software systems.

## What is Software Architecture?

There is no clear and universally accepted definition of software architecture, as it heavily depends on the context and objectives of a system. Generally, software architecture describes the entirety of decisions that shape the design, structure, and behavior of a software system. These decisions can be made consciously or unconsciously.
Regardless of whether these decisions are deliberate or accidental, every software project inevitably develops a software architecture. The quality and clarity of this architecture significantly impact the maintainability, extensibility, and scalability of the system.

---

# Key concepts 🔑

## Coupling

Coupling describes the degree of dependency between two components in a system.
In general, we aim for a system with as **loose coupling** between components as possible. 
A component can represent various entities, such as a module, a service, or a database. 

Dependencies can manifest in many forms and affect the flexibility and maintainability of the system.

### Types of coupling 

#### Temporal Coupling

Building blocks are dependent on the timing of their execution. 

#### Data Coupling

Building blocks are connected by exchanging data.

#### Content Coupling

A building block directly accesses or manipulates internal data of another building block. 

#### Control Coupling

A building block controls the behavior of another by passing control data. 

#### Contextual Coupling

Building blocks depend on a shared context (for example configuration).

#### Common Coupling

Building blocks share the same global resources.

#### Sequential Coupling

The output of one building block serves as the input for another building block.

#### External Coupling

A building block depends on external systems, services, or interfaces.

Examples of Coupling:

- Component A imports another component B: This creates a direct dependency where A relies on the functionality or structure of B. If B changes, A might also need to be updated.
- A component depends on receiving data from a REST API: The component must wait for the API's response before it can process or proceed. This introduces a dependency on external communication and response times.

## Cohesion

Cohesion describes how strongly components within a module or system are related. In general, we aim to achieve high cohesion.
High cohesion ensures that components are focused on a single responsibility, making the system easier to understand, maintain, and extend.

---

# Architecture Styles 🏭

## Monolithic Architecture

Monolithic Architecture is an architectural style where the entire system is built as a single, unified block. 
All components, such as the user interface, business logic, and data access layer, are tightly integrated and operate as a single application.

## Microservices

A microservice is an architectural style where a system is composed of multiple independent services.
Each service is designed to perform a specific function and operates autonomously, communicating with other services through well-defined APIs.

## Event-Driven Architecture

Event-Driven Architecture is a software architectural pattern where the system state is determined by events. 
An event represents a change in the system. Building blocks can listen to those events.

## Service-Oriented Architecture (SOA)

Service-Oriented Architecture (SOA) is a software architectural style where applications are built by services.
Each service represents a discrete functionality and communicates with other services through well-defined interfaces.

SOA and Microservices share many similarities. 
However, in Microservices, the services are significantly smaller, whereas in SOA, the services are much larger and provide multiple functionalities within a specific domain.

## Serverless Architecture

Serverless architecture is a cloud computing model where the cloud provider dynamically manages the allocation of resources.
Developers focus on writing and deploying code without worrying about managing or provisioning servers.

## Cloud Architecture

Cloud architecture refers to the design and deployment of applications and services that run on cloud infrastructure provided by cloud providers.

---

# Design Patterns 🏁

Design Patterns are reusable solutions to common problems in software development. 
They serve as blueprints that help developers create software that is more efficient, flexible, and maintainable.


## Structural Patterns

**Structural Patterns are solutions to organize classes and their objects**

---

### Adapter 

The adapter patterns enables two incompatible interfaces to work together.
In the Adapter Pattern, an incompatible interface is wrapped by an adapter, and the corresponding methods and more are adapted to the target interface.

<details>
  <summary>Example</summary>
  
Imagine you have a workout application that operates using kilograms as the unit of measurement. In this application, there's a class called `Workout` with a method named `addExercise`, which takes the weight in kilograms as input.
Now, you want to extend the app to also support weights in pounds. However, the existing client method, `buildWorkout`, is designed to work exclusively with kilograms. To address this, you introduce a new class called `PoundWorkout`, which also has a method named `addExercise` but expects the weight in pounds.
To bridge the gap between the `PoundWorkout` class and the existing client logic, you create an adapter class called `Adapter`. This adapter takes an instance of the incompatible `PoundWorkout` class as an argument. It provides its own implementation of the `addExercise` method, which internally calls the `addExercise` method of the `PoundWorkout` class after converting the weight from kilograms to pounds.
This way, the client can seamlessly use the adapter without any need to modify or restructure the existing codebase. The adapter handles the conversion and ensures compatibility between the two systems.
</details>

<details>
  <summary>Code Example</summary>

  ```python
  class Workout:
    def add_exercise(self, name, weight_kg):
      print(f"Added exercise: {name}, Weight: {weight_kg} kg")

  class PoundWorkout:
    def add_exercise(self, name, weight_lb):
      print(f"Added exercise: {name}, Weight: {weight_lb} lbs")

  class Adapter:
    def __init__(self, pound_workout):
      self.pound_workout = pound_workout
  
    def add_exercise(self, name, weight_kg):
      weight_lb = weight_kg * 2.20462
      self.pound_workout.add_exercise(name, weight_lb)
  
  def build_workout(workout):
    workout.add_exercise("Bench Press", 100) 
    workout.add_exercise("Deadlift", 130) 
  
  print("using regular kg")
  workout = Workout()
  calculate_volume(workout)
  
  print("using pounds")
  pound_workout = PoundWorkout()
  adapter = Adapter(pound_workout)
  calculate_volume(adapter)
```
</details>

---

### Bridge 

The Bridge Pattern provides a solution to decouple abstraction from implementation by using object composition, allowing both to evolve independently for related classes.

<details>

  <summary>Example</summary>

Imagine you have a car that you want to build. For the car there might be different engines available. 
You could create multiple subclasses like `ElectricCar` or `PetrolCar`. But now we want to add gear-shift to the cars. 
This would lead into more subclasses like `ElectricManuelGearCar`, `ElectricAutomaticGearCar`, `PetrolManuelGearCar` and `PetrolAutomaticGearCar`.
We can fix this by switching from inheritance to composition. Our class `Car(engine: Engine, gearshift: Gearshift)` expects the abstractions `Engine` and `Gearshift`. 
Then we create our implementations `AutomaticGear`, `ManuelGear`, `PetrolEngine` and `ElectricEngine`. 
Now we can create our cars like Car(new AutomaticGear(), new Petrol()).

</details>

<details>
  <summary>Code Example</summary>

  ```python

  class Gearshift:
    def start(self):
        raise NotImplementedError

  class ManuelGear(Gearshift)
    def start(self):
      print("Start manuel")

  class AutomaticGear(Gearshift)
    def start(self):
      print("Start automatic")

  class Engine:
    def start():
      raise NotImplementedError

  class PetrolEngine(Engine):
    def start(self):
      print("Start petrol engine"

  class ElectricEngine():
    def start(self):
      print("Start electric engine"

  class Car():
    def __init__(self, engine, gearshift):
      self.engine = engine
      self.gearshift

    def drive(self):
      self.engine.start()
      self.gearshift.start()


    petrol = PetrolEngine()
    manuel = ManuelGear()
    car = Car(petrol, manuel)
    car.start()

  ```
</details>

---

## Behavioral Patterns

**Behavioral Patterns are solutions to define interaction between objects**

## Creational Patterns

**Behavioral Patterns are solutions to create objects**

---

### Factory method

The Factory Method encapsulates object creation in a method that subclasses override to decide which concrete object to instantiate.

# Contribution 

If you'd like to contribute, feel free to create a pull request. 
If you think a topic is missing, please open an issue.
