## Design patterns in Object oriented programming

### Strategy pattern
> Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

![strategy](https://github.com/rromank/interview-questions/blob/master/diagrams/stategy.png?raw=true)

Is a behavioral design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.
Strategy lets the algorithm vary independently from clients that use it.

```java
// Client
class Duck {
    private QuackBehavior quackBehavior;
    private FlyBehavior flyBehavior;

    public void quack() {
        quackBehavior.execute();
    }

    public void fly() {
        flyBehavior.execute();
    }
}

interface Behavior {
    execute();
}

class QuackBehavior implements Behavior {
    public void execute() {
        // some quack behavior
    }
}

class FlyBehavior implements Behavior {
    public void execute() {
        // some fly behavior
    }
}
```

To many, the Strategy and State patterns appear similar. It’s true that the structure of both the patterns are similar. It’s the intent that differs – that is, they solve different problems. The State pattern aims to facilitate state transition while the aim of the Strategy pattern is to change the behavior of a class by changing internal algorithm at runtime without modifying the class itself.

There is a lot of debate around the use of the Strategy Pattern with Spring. Often you’ll see the Strategy Pattern used in conjunction with Dependency Injection, where Springs IoC container is making the choice of which strategy to use. Different data sources as a great example. Using a H2 data source for local development is one strategy. Using MySQL for production is another strategy. Which one is to use at runtime is up to the Spring IoC container.

---

### Observer pattern
> Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically. It is mainly used to implement distributed event handling systems, in "event driven" software.

![observer](https://github.com/rromank/interview-questions/blob/master/diagrams/observer.png?raw=true)

The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

##### What problems can the Observer design pattern resolve?
* A one-to-many dependency between objects should be defined without making the objects tightly coupled.
* It should be ensured that when one object changes state an open-ended number of dependent objects are updated automatically.
* It should be possible that one object can notify an open-ended number of other objects.   

Defining a one-to-many dependency between objects by defining one object (subject) that updates the state of dependent objects directly is inflexible because it commits (tightly couples) the subject to particular dependent objects. Tightly coupled objects are hard to implement, change, test, and reuse because they refer to and know about (how to update) many different objects with different interfaces.

```java
interface Observable {
    void add(Observer observer);
    void remove(Observer observer);
    void notify();
}

class WeatherStation implements Observable {
    public void add(Observer observer) {
        observers.add(observer);
    }
    
    public void notify() {
        observers.forEach(Observer::update);
    }
    
    public int getTemperature() {
        return temperature;
    }
}

// =======================================

interface Observer {
    void update();
}

class Display implements Observer {
    private WeatherStation station;
    
    public Display(WeatherStation station) {
        this.station = station;
    }
    
    public void update() {
        this.station.getTemperature();
        // do other staff with updated temperature
    }
}
```

---

### Decorator pattern
> Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

![decorator](https://github.com/rromank/interview-questions/blob/master/diagrams/decorator.png?raw=true)

The decorator pattern is a design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class. The decorator pattern is often useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern. The decorator pattern is structurally nearly identical to the chain of responsibility pattern, the difference being that in a chain of responsibility, exactly one of the classes handles the request, while for the decorator, all classes handle the request.

##### What problems can the Decorator design pattern resolve?
* Responsibilities should be added to (and removed from) an object dynamically at run-time.
* A flexible alternative to subclassing for extending functionality should be provided.

##### What solution does the Decorator design pattern describe?
Define Decorator objects that
* implement the interface of the extended (decorated) object (Component) transparently by forwarding all requests to it and
* perform additional functionality before/after forwarding a request.

```java
interface Beverage {
    int cost();
}

class Espresso implements Beverage {
    public int cost() {
        return 1;
    }
}

/**
* Decorator
*/
interface AddonDecorator extends Beverage {
    int cost();
}

class Caramel implements AddonDecorator {
    private Beverage beverage;
    
    public Caramel(Beverage beverage) {
        this.beverage = beverage;
    }
    
    public int cost() {
        return this beverage.cost() + 2;
    }
}
```

---
