# Class relationships

### ==Important to understand==
You cannot always determine the type of relationship between a parent class and its child classes just by looking at how they are passed to the parent class as parameters. The relationship between the parent class and its child classes is determined by the nature of the relationship between the objects themselves, which is typically based on the semantics of the problem being modeled.

Nice video about class relationships:
https://www.youtube.com/watch?v=sN2_CoB_kbw

## Association

Association in OOP represents a relationship between two or more classes where the objects of one class are connected to the objects of another class based on some criteria, but there is no ownership or containment relationship between them.

## Composition
Can be defined using the term `"has a"`.

Special type of aggregation (restricted aggregation).

The composite object (parent) has sole "responsibility for the existence and storage of the composed objects (children)", the composed object can be part of at most one composite (composite aggregation), and "If a composite object is deleted, all of its part instances that are objects are deleted with it".

Parent object **has a** child object.

The lifetime of the child object is tightly coupled with the lifetime of the parent object, meaning that the child object cannot exist without the parent object.

### Example
In the case of a car composed of an engine, if you remove the car, then you also remove the engine, as the engine is a critical part of the car and its relationship with the car is an example of composition. The engine may still physically exist, but it cannot perform its intended function without the car.

If you remove the car, you remove its engine as well because an engine is a critical part of a car which makes a car being a car.

## Aggregation
Can be defined using the term `"has a"`.

Aggregation differs from ordinary composition in that it does not imply ownership. In composition, when the owning object is destroyed, so are the contained objects. In aggregation, this is not necessarily true. For example, a university owns various departments (e.g., chemistry), and each department has a number of professors. If the university closes, the departments will no longer exist, but the professors in those departments will continue to exist. Therefore, a university can be seen as a composition of departments, whereas departments have an aggregation of professors. In addition, a professor could work in more than one department, but a department could not be part of more than one university.

Parent object **has a** child object and they both can exit without each other.

In aggregation, the object may only contain a reference or pointer to the object (and not have lifetime responsibility for it).

Sometimes aggregation is referred to as composition when the distinction between ordinary composition and aggregation is unimportant.


## Dependency
A dependency relationship in object-oriented programming occurs when one object, such as a class or module, relies on another object to function properly. In other words, if the dependent object is changed or removed, it will impact the functionality of the object that depends on it.

### Good to know
The dependency relationship is a broad term that can be used for any type of association if it is unclear what type of association is exactly used.

When the nature of the relationship between two classes is not clear, or when the relationship does not fit neatly into one of the more specific types of associations (such as association, aggregation, or composition), the dependency relationship can be used as a catch-all term to indicate that one class depends on another.

A dependency relationship indicates that one class relies on another, but it does not necessarily imply ownership, containment, or a strong relationship between the classes.

## Inheritance
Can be defined using the term `"is a"`.

Child object **is a** type of parent object.

For example a sedan **is a** car.

Inheritance is a relationship where one object derives characteristics or behaviors from another object. For example, a sports car can inherit characteristics from a general car object, such as having four wheels and an engine, but also have additional features specific to sports cars, such as a spoiler and a high-performance engine.

## Realization
The Realization principle is a design principle in object-oriented programming that suggests using interfaces to define a contract between classes. The idea is that a class that implements an interface promises to provide certain methods or properties defined by that interface.

By using the Realization principle and defining a contract between classes with interfaces, we can create more flexible and modular code that can be easily extended or modified in the future.

It also often introduces the Inversion of Dependencies principle which is important to build reliable and flexible system.

## Composition vs Inheritance

Some say inheritance introduces the strongest coupling between two classes than the other types of relationship.

The inheritance is when you design your classes around what they are, on the other hand the composition is when you design your classes around what they do.

### "Favor composition over inheritance"
The principle "favor composition over inheritance" is a guideline in object-oriented programming that suggests using composition instead of inheritance when possible to achieve code reuse.

However, it's worth noting that inheritance and composition are not mutually exclusive, and they can often be used together to create more complex and flexible systems.
