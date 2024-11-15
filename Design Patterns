
Here's a detailed explanation of each of the mentioned design patterns along with example code in Java:

### 1. **Factory Method Pattern**

The Factory Method Pattern provides an interface for creating objects but lets subclasses decide which class to instantiate. It allows a class to defer instantiation to subclasses.

**Example Code**:

```
// Product Interface
interface Animal {
    void speak();
}

// Concrete Products
class Dog implements Animal {
    @Override
    public void speak() {
        System.out.println("Woof!");
    }
}

class Cat implements Animal {
    @Override
    public void speak() {
        System.out.println("Meow!");
    }
}

// Factory Method
class AnimalFactory {
    public static Animal getAnimal(String type) {
        if ("Dog".equalsIgnoreCase(type)) {
            return new Dog();
        } else if ("Cat".equalsIgnoreCase(type)) {
            return new Cat();
        }
        throw new IllegalArgumentException("Unknown animal type");
    }
}

// Usage
public class FactoryMethodExample {
    public static void main(String[] args) {
        Animal dog = AnimalFactory.getAnimal("Dog");
        dog.speak(); // Output: Woof!

        Animal cat = AnimalFactory.getAnimal("Cat");
        cat.speak(); // Output: Meow!
    }
}
``` 

### 2. **Singleton Pattern**

The Singleton Pattern ensures that a class has only one instance and provides a global point of access to it.

**Example Code**:


```// Singleton Class
public class Singleton {
    // Volatile keyword ensures visibility of changes to variables across threads
    private static volatile Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {}

    // Double-checked locking for thread-safe singleton
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("Hello from Singleton!");
    }
}

// Usage
public class SingletonExample {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        singleton.showMessage(); // Output: Hello from Singleton!
    }
}
```
### 3. **Builder Pattern**

The Builder Pattern is used to construct a complex object step by step. It separates the construction of a complex object from its representation.

**Example Code**:

```
// Product Class
public class House {
    private final int windows;
    private final int doors;
    private final String roofType;

    // Private constructor
    private House(Builder builder) {
        this.windows = builder.windows;
        this.doors = builder.doors;
        this.roofType = builder.roofType;
    }

    @Override
    public String toString() {
        return "House with " + windows + " windows, " + doors + " doors, roof type: " + roofType;
    }

    // Builder Class
    public static class Builder {
        private int windows;
        private int doors;
        private String roofType;

        public Builder setWindows(int windows) {
            this.windows = windows;
            return this;
        }

        public Builder setDoors(int doors) {
            this.doors = doors;
            return this;
        }

        public Builder setRoofType(String roofType) {
            this.roofType = roofType;
            return this;
        }

        public House build() {
            return new House(this);
        }
    }
}

// Usage
public class BuilderPatternExample {
    public static void main(String[] args) {
        House house = new House.Builder()
                .setWindows(4)
                .setDoors(2)
                .setRoofType("Gabled")
                .build();
        System.out.println(house); // Output: House with 4 windows, 2 doors, roof type: Gabled
    }
}
``` 

### 4. **Prototype Pattern**

The Prototype Pattern is used to create a duplicate object or clone of the current object to avoid creating instances using the `new` keyword repeatedly.

**Example Code**:

```// Prototype Interface
interface Prototype extends Cloneable {
    Prototype clone();
}

// Concrete Prototype Class
class Book implements Prototype {
    private String title;

    public Book(String title) {
        this.title = title;
    }

    @Override
    public Prototype clone() {
        return new Book(this.title);
    }

    @Override
    public String toString() {
        return "Book Title: " + title;
    }
}

// Usage
public class PrototypePatternExample {
    public static void main(String[] args) {
        Book originalBook = new Book("Design Patterns");
        Book clonedBook = (Book) originalBook.clone();

        System.out.println(originalBook); // Output: Book Title: Design Patterns
        System.out.println(clonedBook);   // Output: Book Title: Design Patterns
    }
}
```

### 5. **Adapter Pattern**

The Adapter Pattern allows incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces.

**Example Code**:

```
// Target Interface
interface MediaPlayer {
    void play(String audioType, String fileName);
}

// Adaptee Class
class AdvancedMediaPlayer {
    public void playMp3(String fileName) {
        System.out.println("Playing mp3 file: " + fileName);
    }
}

// Adapter Class
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedMediaPlayer;

    public MediaAdapter() {
        this.advancedMediaPlayer = new AdvancedMediaPlayer();
    }

    @Override
    public void play(String audioType, String fileName) {
        if ("mp3".equalsIgnoreCase(audioType)) {
            advancedMediaPlayer.playMp3(fileName);
        } else {
            System.out.println("Unsupported audio type");
        }
    }
}

// Usage
public class AdapterPatternExample {
    public static void main(String[] args) {
        MediaPlayer player = new MediaAdapter();
        player.play("mp3", "song.mp3"); // Output: Playing mp3 file: song.mp3
    }
}
```

### 6. **Decorator Pattern**

The Decorator Pattern allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class.

**Example Code**:

```
// Component Interface
interface Coffee {
    String getDescription();
    double getCost();
}

// Concrete Component
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}

// Decorator
class MilkDecorator implements Coffee {
    private final Coffee decoratedCoffee;

    public MilkDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 1.5;
    }
}

// Usage
public class DecoratorPatternExample {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " $" + coffee.getCost()); // Simple Coffee $5.0

        // Adding milk
        coffee = new MilkDecorator(coffee);
        System.out.println(coffee.getDescription() + " $" + coffee.getCost()); // Simple Coffee, Milk $6.5
    }
}
``` 

### 7. **Facade Pattern**

The Facade Pattern provides a simplified interface to a complex subsystem. It hides the complexities and provides an easy interface to the client.

**Example Code**:

```
// Subsystem Classes
class CPU {
    public void start() {
        System.out.println("CPU started.");
    }
}

class Memory {
    public void load() {
        System.out.println("Memory loaded.");
    }
}

class HardDrive {
    public void read() {
        System.out.println("HardDrive read data.");
    }
}

// Facade Class
class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;

    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void startComputer() {
        cpu.start();
        memory.load();
        hardDrive.read();
        System.out.println("Computer started successfully.");
    }
}

// Usage
public class FacadePatternExample {
    public static void main(String[] args) {
        ComputerFacade computer = new ComputerFacade();
        computer.startComputer(); // Simplified interface
    }
}
``` 

### 8. **Strategy Pattern**

The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the algorithm to vary independently from the clients that use it.

**Example Code**:

```
// Strategy Interface
interface PaymentStrategy {
    void pay(int amount);
}

// Concrete Strategies
class CreditCardPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid $" + amount + " using Credit Card.");
    }
}

class PaypalPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid $" + amount + " using PayPal.");
    }
}

// Context Class
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}

// Usage
public class StrategyPatternExample {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        // Pay using Credit Card
        cart.setPaymentStrategy(new CreditCardPayment());
        cart.checkout(100); // Output: Paid $100 using Credit Card.

        // Pay using PayPal
        cart.setPaymentStrategy(new PaypalPayment());
        cart.checkout(200); // Output: Paid $200 using PayPal.
    }
}
``` 

### 9. **Observer Pattern**

The Observer Pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

**Example Code**:

```
import java.util.ArrayList;
import java.util.List;

// Observer Interface
interface Observer {
    void update(String message);
}

// Concrete Observer
class User implements Observer {
    private String name;

    public User(String name) {
        this.name = name;
    }

    @Override
    public void update(String message) {
        System.out.println(name + " received: " + message);
    }
}

// Subject Interface
interface Subject {
    void registerObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers(String message);
}

// Concrete Subject
class Channel implements Subject {
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

// Usage
public class ObserverPatternExample {
    public static void main(String[] args) {
        Channel channel = new Channel();

        Observer user1 = new User("Alice");
        Observer user2 = new User("Bob");

        channel.registerObserver(user1);
        channel.registerObserver(user2);

        channel.notifyObservers("New video uploaded!"); 
        // Output: 
        // Alice received: New video uploaded!
        // Bob received: New video uploaded!
    }
}
``` 

### 10. **State Pattern**

The State Pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

**Example Code**:

```
// State Interface
interface State {
    void handle();
}

// Concrete States
class StartState implements State {
    @Override
    public void handle() {
        System.out.println("System is in start state.");
    }
}

class StopState implements State {
    @Override
    public void handle() {
        System.out.println("System is in stop state.");
    }
}

// Context Class
class Context {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public void applyState() {
        state.handle();
    }
}

// Usage
public class StatePatternExample {
    public static void main(String[] args) {
        Context context = new Context();

        // Switch to Start State
        context.setState(new StartState());
        context.applyState(); // Output: System is in start state.

        // Switch to Stop State
        context.setState(new StopState());
        context.applyState(); // Output: System is in stop state.
    }
}
``` 

Each pattern above is essential for building scalable, maintainable, and flexible software systems in Java. The code examples provided give you a foundational understanding of how to implement these patterns effectively.
