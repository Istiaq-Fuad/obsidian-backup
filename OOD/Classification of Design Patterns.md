
Design patterns are reusable solutions to common problems in object-oriented software design. The **Gang of Four (GoF)** classified design patterns into three main categories based on the type of problem they address: **Creational**, **Structural**, and **Behavioral** patterns.

## 1. Creational Design Patterns

Creational patterns focus on **object creation**. Instead of instantiating objects directly using `new`, these patterns provide flexible ways to create objects, making the system independent of how objects are created and represented.

### Core Intent

- Hide object creation logic
- Controls when and how objects are created
- Reduce tight coupling to concrete classes
- Improve flexibility and reuse

### Common Creational Patterns

- **Singleton** – Ensures only one instance of a class exists
- **Factory Method** – Creates objects through a common interface
- **Abstract Factory** – Creates families of related objects
- **Builder** – Builds complex objects step by step
- **Prototype** – Creates new objects by cloning existing ones

---

## 2. Structural Design Patterns

Structural patterns deal with **how classes and objects are composed** to form larger structures. They help ensure that changes in one part of the system do not affect others.

### Core Intent

- Organize relationships between classes
- Use composition over inheritance
- Simplify complex structures

### Common Structural Patterns

- **Adapter** – Converts one interface into another
- **Decorator** – Adds responsibilities dynamically
- **Facade** – Provides a simplified interface to a complex system
- **Composite** – Treats individual and composite objects uniformly
- **Bridge** – Separates abstraction from implementation
- **Proxy** – Controls access to an object

---

## 3. Behavioral Design Patterns

Behavioral design patterns deal with **communication and responsibility assignment** between objects. They define how objects interact and share responsibilities.

### Core Intent

- Manage object interaction
- Encapsulate behavior
- Reduce complex conditional logic

### Common Behavioral Patterns

- **Observer** – Notifies dependents of state changes
- **Strategy** – Encapsulates interchangeable algorithms
- **Command** – Encapsulates a request as an object
- **State** – Changes behavior based on internal state
- **Iterator** – Traverses elements without exposing structure
- **Mediator** – Centralizes communication between objects

---

## Summary Table

|Category|Main Focus|What It Solves|
|---|---|---|
|Creational|Object creation|How objects are created|
|Structural|Object composition|How objects are connected|
|Behavioral|Object interaction|How objects communicate and behave|

---

## One-Line Intuition

- **Creational** → Create objects flexibly
- **Structural** → Build class/object structures
- **Behavioral** → Manage object behavior and interaction
