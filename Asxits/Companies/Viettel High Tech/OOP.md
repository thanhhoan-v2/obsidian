**When to use `@Override`**
- Interface `implements`
- Inheritance `extends`
- Object Methods, for example `toString()`

---
## Interface

```java
interface IPayment {
    void pay(double amount);
}

class CreditPayment implements IPayment {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " via Credit Card.");
    }
}

public class Main {
	public void static main(String[] args) {
		IPayment payment = new CreditPayment();
		payment.pay(12);
	}
}
```

## Inheritance

```java
class Animal {
    Animal() {
        System.out.println("Animal Constructor");
    }
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    Dog() {
        super(); // Calls the parent class constructor
        System.out.println("Dog Constructor");
    }
    void makeSound() {
        super.makeSound(); // Calls parent method
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.makeSound();
    }
}

// Animal Constructor
// Dog Constructor
// Animal makes a sound
// Dog barks
```

## Polymorphism
> It allows a method, function, or object to behave differently based on the context. Polymorphism enables **dynamic method resolution** and **method flexibility**, making applications easier to extend and maintain.

```java
interface Payment {
    void pay(double amount);
}

class CreditCardPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class PayPalPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

public class Main {
    public static void main(String[] args) {
        Payment payment;
        
        payment = new CreditCardPayment();
        payment.pay(100.50);
        
        payment = new PayPalPayment();
        payment.pay(200.75);
    }
}
```

## Abstraction 
> showing only the **essential details** and hiding the **implementation**. It allows programmers to focus on **what an object does** rather than **how it does it**.

```java
abstract class Payment {
    double amount;
    
    Payment(double amount) {
        this.amount = amount;
    }
    
    abstract void pay(); // Abstract method
}

class CreditCardPayment extends Payment {
    CreditCardPayment(double amount) {
        super(amount);
    }
    
    @Override
    void pay() {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class PayPalPayment extends Payment {
    PayPalPayment(double amount) {
        super(amount);
    }
    
    @Override
    void pay() {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

public class Main {
    public static void main(String[] args) {
        Payment payment;
        
        payment = new CreditCardPayment(150.75);
        payment.pay();
        
        payment = new PayPalPayment(200.50);
        payment.pay();
    }
}
```


## Encapsulation
> **wrapping** the data (variables) and code (methods) together into a single unit (class). It restricts direct access to some of an object's components, which helps protect data integrity and prevents unintended modifications.

```java
class PaymentProcessor {
    private String cardNumber;
    private double amount;

    public PaymentProcessor(String cardNumber, double amount) {
        this.cardNumber = maskCardNumber(cardNumber);
        this.amount = amount;
    }

    private String maskCardNumber(String cardNumber) {
        return "****-****-****-" + cardNumber.substring(cardNumber.length() - 4);
    }

    public void processPayment() {
        System.out.println("Processing payment of " + amount + " for card " + cardNumber);
    }
}

public class Main {
    public static void main(String[] args) {
        PaymentProcessor payment = new PaymentProcessor("1234567812345678", 250.00);
        payment.processPayment();
    }
}
```


## Aggregation
> an object of one class contains a reference to an object of another class. However, the contained object can exist independently of the container. This means that even if the container object is destroyed, the contained object can still be used elsewhere in the application.

```java
interface Teachable {
    void teach();
}

class Professor implements Teachable {
    private String name;
    private String subject;
    
    public Professor(String name, String subject) {
        this.name = name;
        this.subject = subject;
    }
    
    public void teach() {
        System.out.println(name + " is teaching " + subject);
    }
}

class University {
    private String universityName;
    private List<Teachable> professors;
    
    public University(String universityName) {
        this.universityName = universityName;
        this.professors = new ArrayList<>();
    }
    
    public void addProfessor(Teachable professor) {
        professors.add(professor);
    }
    
    public void showProfessors() {
        System.out.println("Professors at " + universityName + ":");
        for (Teachable professor : professors) {
            professor.teach();
        }
    }
}

import java.util.*;

public class InterfaceAggregationExample {
    public static void main(String[] args) {
        Professor prof1 = new Professor("Dr. Adams", "Physics");
        Professor prof2 = new Professor("Dr. Lee", "Chemistry");
        
        University university = new University("MIT");
        university.addProfessor(prof1);
        university.addProfessor(prof2);
        
        university.showProfessors();
    }
}
```

## Composition
>  one class contains an instance (or instances) of another class as a field. The contained class is often called a component, and the containing class is referred to as a composite class. This helps in building complex systems by combining simpler objects.

```java
class Engine {
    private int horsepower;

    public Engine(int horsepower) {
        this.horsepower = horsepower;
    }

    public void start() {
        System.out.println("Engine started with " + horsepower + " HP.");
    }
}

class Wheel {
    private String type;

    public Wheel(String type) {
        this.type = type;
    }

    public void rotate() {
        System.out.println("The " + type + " wheel is rotating.");
    }
}

class Transmission {
    private String type;

    public Transmission(String type) {
        this.type = type;
    }

    public void shiftGear() {
        System.out.println("Transmission shifted: " + type);
    }
}

class Car {
    private Engine engine;
    private Wheel wheel;
    private Transmission transmission;

    public Car(Engine engine, Wheel wheel, Transmission transmission) {
        this.engine = engine;
        this.wheel = wheel;
        this.transmission = transmission;
    }

    public void drive() {
        engine.start();
        wheel.rotate();
        transmission.shiftGear();
        System.out.println("Car is moving!");
    }
}

public class CompositionExample {
    public static void main(String[] args) {
        Engine engine = new Engine(150);
        Wheel wheel = new Wheel("Alloy");
        Transmission transmission = new Transmission("Automatic");
        
        Car car = new Car(engine, wheel, transmission);
        car.drive();
    }
}
```