## Q. What is Object-Oriented Programming (OOP)?
Object-Oriented Programming (OOP) is a programming paradigm that uses objects and classes, which allows for more robust, flexible and manageable code.

## Q. Can you explain the four basic principles of OOP?
The four basic principles of OOP are encapsulation, inheritance, polymorphism, and abstraction.

1. **Encapsulation**: Encapsulation is often referred to as "data hiding". It's a mechanism where the data and the code that manipulates the data are bound together and are kept safe from outside interference and misuse. In simple terms, it's about hiding complex details and only exposing the necessary parts. For example, when using a coffee machine, you don't need to know the internal workings of every part (like how water is heated), you just need to know how to use its interface (buttons and levers).

2. **Inheritance**: Inheritance is a feature that allows a class (child class or derived class) to inherit properties from another class (parent class or base class). These properties can be methods, data, or attributes. For instance, if we have a class 'Vehicle' with properties like 'speed' and 'size', and we have another class 'Car', the 'Car' class can inherit the properties of the 'Vehicle' class. This facilitates code reusability and reducing redundancy.

3. **Polymorphism**: Polymorphism, derived from Greek words "poly" (many) and "morph" (forms), allows us to perform a single action in different ways. Consider an example of a 'Shape' class with a method 'draw()'. We might have multiple child classes like 'Rectangle', 'Circle', etc. These child classes will inherit the 'Shape' class and possibly override the 'draw()' method to provide their own implementation. So, the 'draw()' method will behave differently for each child class. That's polymorphism.

4. **Abstraction**: Abstraction is the principle of simplifying complex reality by modeling classes appropriate to the problem, and working at the most appropriate level of inheritance for a given aspect of the problem. In other words, it's about creating a simple interface that works at a high level of complexity. This can be achieved in many ways including patterns, conventions, and a simple-to-understand UI. For instance, when driving a car, you don't need to understand the complexities of the internal combustion engine. You just need to know how to interact with the car's controls (steering wheel, pedals, and gauges). That's abstraction.

## Q. What is the difference between overloading and overriding?
Overloading is when two or more methods in the same class have the same name but different parameters. Overriding is when a child class has the same method as a parent class.

## Q. What is an abstract class, and how is it different from an interface?
An abstract class is a class that cannot be instantiated and is typically used as a base class for other classes. Interfaces, on the other hand, are completely abstract and cannot have any implementation.

## Q. What is a class and what is an object in OOP?
A class is a blueprint for creating objects. An object is an instance of a class.

## Q. What is a constructor in OOP?
A constructor in OOP is a special type of subroutine called to create an object. It prepares the new object for use, often accepting arguments that the constructor uses to set any member variables required when the object is first created.

## Q.   What is the SOLID principle in OOP? Explain each of the SOLID principles?
SOLID is an acronym for the first five object-oriented design principles by Robert C. Martin. 
These principles make it easier to develop software that is easy to maintain, understand, and extend.
The SOLID principles include: 

   - **Single Responsibility Principle (SRP)**: It states that a class should only have one reason to change. 
        For example, if you have a class to handle database operations and printing reports, you might want to split this into two classes. 
        One class handles the database operations and another handles the printing of reports. 
        This way, if you need to change something about the report printing, it doesn't affect your database operations.    


   - **Open-Closed Principle (OCP)**: It is about entities (classes, modules, functions, etc.) being open for extension but closed for modification. 
        For example, if you have a Shape class and you want to add a new type of Shape, you can create a new class that inherits from Shape and adds new behavior, without changing the existing Shape class.


   - **Liskov Substitution Principle (LSP)**: This principle extends the Open/Closed Principle by focusing on the behavior of a superclass and its subclasses. 
        According to the Liskov Substitution Principle, "Subtypes must be substitutable for their base types", meaning that, methods or functions that use pointers or references to the base class must be able to use objects of the derived class without knowing it.
        Let's use an example to illustrate this. 
        Consider a class `Bird` with a method `fly()`. 
        This works fine for birds like `Eagle` or `Sparrow`, which are subclasses of `Bird`. 
        But what if you have a `Penguin` class, which is also a subclass of `Bird`, but penguins can't fly? 
        If you create a `Penguin` object and call `fly()`, you'll have a problem because penguins can't fly.
        If your code assumes that all `Bird` classes can `fly()`, but some subclasses can't support that behavior, then you're violating the Liskov Substitution Principle. 
        In other words, you shouldn't derive `Penguin` from `Bird` if it's going to invalidate an assumption of the base class, `Bird`, that all birds can fly.
        Instead, to adhere to LSP, you could create different subclasses for `FlyingBird` and `NonFlyingBird` from `Bird`. 
        The `fly()` method would be in the `FlyingBird` class. `Eagle` and `Sparrow` would be subclasses of `FlyingBird`, and `Penguin` would be a subclass of `NonFlyingBird`. 
        This way, you're not making any invalid assumptions about the behavior of your classes, and any class that uses `Bird` objects can still use any of its subclasses without knowing it, which is what Liskov Substitution Principle is all about.
        In essence, LSP is aimed at ensuring semantic consistency across a class hierarchy. Semantic consistency, in this case, means that derived classes should only add or override behavior in a way that doesn’t create problems when they're used in place of the base class.


   - **Interface Segregation Principle (ISP)**:  It states that no client should be forced to depend on interfaces they do not use. 
         This principle deals with the disadvantages of implementing big interfaces.
         ISP is intended to keep a system decoupled and thus easier to refactor, change, and redeploy. 
         The principle helps in reducing the side effects and frequency of required changes.
         Let's take an example to understand this principle:
         Suppose you have an interface, `IMachine`, that has methods like `print()`, `fax()`, `scan()`, and `copy()`. 
         Now, if a class `AllInOnePrinter` implements this interface, it's fine because an all-in-one printer can perform all these operations. 
         However, what if we have a class `SimplePrinter` that only needs to implement `print()` and doesn't need `fax()`, `scan()`, or `copy()`? If `SimplePrinter` implements `IMachine`, it will have to provide some sort of dummy implementation for `fax()`, `scan()`, and `copy()`, which it actually doesn't need. 
         This is where ISP comes in. 
         According to ISP, we should not force `SimplePrinter` to depend on methods it does not need. Instead, we should break down `IMachine` into smaller interfaces like `IPrinter`, `IScanner`, `IFax`, and `ICopier`. Then `AllInOnePrinter` can implement all these interfaces, while `SimplePrinter` can just implement `IPrinter`.
         So, the Interface Segregation Principle promotes the use of several, specific interfaces rather than a general purpose, "fat" interface, which can lead to high coupling and violate the high cohesion principle.
        

   - **Dependency Inversion Principle (DIP)**: It helps to decouple software modules. 
        It states that:
        1. High-level modules should not depend on low-level modules. 
           Both should depend on abstractions.
        2. Abstractions should not depend upon details. Details should depend upon abstractions.
           This principle is all about reducing dependencies amongst the code. 
           The main purpose of the Dependency Inversion Principle is to decouple the software modules. 
           This principle provides flexibility, reusability, and a way to easily change the functionality of our program without causing a ripple effect of changes in the dependent classes.
           Let's illustrate this with an example:
     
       Suppose we have a high-level class `NotificationService` that depends on a low-level class `EmailService`. 
       If we need to add another way to send notifications, like `SmsService`, we would have to modify the `NotificationService` class, which is not good as per the Open/Closed Principle.
       According to the Dependency Inversion Principle, we should instead create an abstraction (interface) `IMessageService`, and have `EmailService` and `SmsService` implement this interface. 
       Then `NotificationService` should depend upon `IMessageService` and not directly on the `EmailService`. 
       Now if we need to add another way to send notifications, we can just create a new class that implements `IMessageService` and `NotificationService` remains unmodified.
       So, the Dependency Inversion Principle encourages us to use interfaces to abstract the implementation details and construct systems through the interfaces, making high-level modules independent of the low-level module implementation details. 
       This results in a more flexible, maintainable, and most importantly, a more testable application.

## Q. What is Aggregation in OOP? Can you provide an example?
Aggregation is a specialized form of Association where all objects have their own lifecycle but there is ownership. 
This represents a "whole-part or a-part-of" relationship.
For example, consider a `Department` and `Teacher` class. 
A teacher belongs to a department. If a department is deleted, the teacher will not be deleted. So, it's an aggregation. 
Here, `Department` is a whole and `Teacher` is a part.
```python
from typing import List
class Teacher:
    def __init__(self, name : str):
        self.name = name

class Department:
    def __init__(self, teachers : List[Teacher]):
        self.teachers = teachers

john = Teacher('John Doe')
jane = Teacher('Jane Doe')

dept = Department([john, jane])

for teacher in dept.teachers:
    print(teacher.name)  # John Doe, Jane Doe
```
## Q. What is Composition in OOP? Can you provide an example?
Composition is a stricter type of aggregation that implies ownership. 
It's still a part-of relationship but the composed object can't exist independently of the main object. 
For example, a Room class might have a Wall class. If the Room is destroyed, the Wall can't exist on its own.
```python
from typing import List
class Room:
    def __init__(self, name):
        self.name = name

class House:
    def __init__(self, rooms : List[Room]):
        self.rooms = rooms
    
    def print_rooms(self):
        for room in self.rooms:
            print(f'Room: {room.name}')

kitchen = Room('Kitchen')
living_room = Room('Living Room')

house = House([kitchen, living_room])
house.print_rooms() # Room: Kitchen, Room: Living Room
```
## Q. What is Association / Double Association in OOP? Can you provide an example?
Association is a relationship where all objects have their own lifecycle and there is no owner. 
It's a "uses-a" relationship. 
For example, a Teacher class might use a Student class. 
They can each exist independently and there's no ownership implied.

```python
class Teacher:
    def __init__(self, name):
        self.name = name
        
class Student:
    def __init__(self, name, teacher : Teacher):
        self.name = name
        self.teacher = teacher

teacher = Teacher('Mr. Smith')
student = Student('John Doe', teacher)

print(student.teacher.name)  # Mr. Smith
```
Double Association is when two classes are associated with each other.
```python
class Customer:
    def __init__(self, name, product : Product):
        self.name = name
        self.product = product

class Product:
    def __init__(self, name, customer : Customer):
        self.name = name
        self.customer = customer

product = Product('Apple', 'John Doe')
customer = Customer('John Doe', 'Apple')

print(customer.product)  # Apple
print(product.customer)  # John Doe
```

