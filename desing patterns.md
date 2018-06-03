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
