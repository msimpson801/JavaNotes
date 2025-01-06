## **What is  loose coupling?**

Many modern technologies like microservices, containers, and API use loose coupling. It means the services are designed to be reusable and interchangeable without breaking existing business rules or inter-connections.

Think of loose coupling like choosing clothes from your wardrobe:

#### Tight Coupling:

Imagine wearing a jumpsuit where the top and bottom are stitched together. If you want to change the top, you have to replace the entire outfit.
Here, the jumpsuit is tightly coupled. Altering one part affects the whole.

#### Loose Coupling:

Now consider separate pieces of clothing: a shirt and trousers. You can swap the shirt without changing the pants.
Loose coupling allows flexibility. Just like clothes, components can be replaced or modified independently.
Summary: Loose coupling is like mixing and matching clothes—each piece stands alone, yet they work together seamlessly. 👚👖

### What is dependency Injection?

In object-oriented programming, a class may rely on objects of other classes. Dependency injection is a software design pattern that helps you create loosely coupled, testable, and maintainable code. It achieves this by allowing you to inject objects (also called dependencies), into the class that needs them from an external source, rather than creating them internally within the class

### Why should I use dependency injection?

Imagine we have a Kart class in Mario Kart that needs an Engine to function. Imagine we have different engine types
Here the kart class is responsible for creating its engine dependency by creating an engine object when we instantiate our Kart class

```Java
interface Engine {
    public void start();
}

class StandardEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Standard engine starting...");
    }
}

class TurboEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Turbo engine starting brrm brrm...");
    }
}

class ElectricEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Electric engine starting beep borp...");
    }
}


class Kart {
    /// We have a concrete Type for the Engine
    /// This kart - for now - can only easily facilitate a single engine type
    private StandardEngine engine;

    public Kart() {
        // Here, the constructor is responsible for
        // instantiating - "making" - its dependency
        this.engine = new StandardEngine();
    }

    public void startKart() {
        engine.start();
    }
}
```

Now, what if we decide to upgrade our engine from a standard engine to a turbo engine ?
We will need to modify the kart class with a new engine dependency like so.

```Java
class Kart {
//    now using Turbo
    private TurboEngine engine;

    public Kart() {
        this.engine = new TurboEngine();
    }

    public void startKart() {
        engine.start();
    }
}
```

## **Changing our code to use dependency injection**

The most common form of dependency injection is to inject dependencies into a class through its constructor as seen below 

Changing the constructor to receive an Engine object. This makes the Kart class independent of the specific engine type.
```Java
public class Kart {
    private Engine engine;

    // Constructor injection
    public Kart(Engine engine) {
        this.engine = engine;
    }

    public void startEngine() {
        engine.start();
    }
}

```

The below code demonstrates how we can easily inject different types of engines into the Kart class and start the engines.

```Java
public class MarioKartGame {
    public static void main(String[] args) {
        // Injecting different types of engines
        Engine standardEngine = new StandardEngine();
        Engine turboEngine = new TurboEngine();
        Engine electricEngine = new ElectricEngine();

        Kart standardKart = new Kart(standardEngine);
        Kart turboKart = new Kart(turboEngine);
        Kart electricKart = new Kart(electricEngine);

        standardKart.startEngine(); // Outputs: Standard engine starting...
        turboKart.startEngine();    // Outputs: Turbo engine starting...
        electricKart.startEngine(); // Outputs: Electric engine starting...
    }
}

```

Explanation:
The key here is that the Kart class doesn’t create its own Engine object; instead, it relies on an external Engine instance passed during construction.
Here the dependency (in this case, the Engine) is injected into the class rather than being created internally.
By using dependency injection, the Kart class remains flexible and can easily work with different types of engines without tightly coupling them.



