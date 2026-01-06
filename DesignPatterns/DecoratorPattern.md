# Decorator Design Pattern (Detailed Explanation)

## What Problem Does Decorator Solve?

The Decorator pattern allows you to add new behavior to objects dynamically without modifying their existing code or creating excessive subclasses.

### Problem with Inheritance
Using inheritance for every combination of features leads to class explosion and rigid designs.

---

## Intent

**Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing.**

---

## Real-World Analogy

Think of ordering coffee:
- Start with simple coffee
- Add milk
- Add sugar
- Add caramel

Each add-on wraps the previous one.

---

## Key Idea

- Decorators implement the same interface as the original object
- Decorators wrap the original object
- Client code remains unaware of decoration

---

## Structure

1. Component – Common interface
2. ConcreteComponent – Base object
3. Decorator – Abstract wrapper
4. ConcreteDecorator – Adds behavior

---

## Java Example: Coffee System

### Component Interface

```java
interface Coffee {
    double cost();
    String description();
}
```

### Concrete Component

```java
class SimpleCoffee implements Coffee {

    public double cost() {
        return 50;
    }

    public String description() {
        return "Simple Coffee";
    }
}
```

### Abstract Decorator

```java
abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}
```

### Concrete Decorators

#### Milk Decorator
```java
class MilkDecorator extends CoffeeDecorator {

    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    public double cost() {
        return coffee.cost() + 10;
    }

    public String description() {
        return coffee.description() + ", Milk";
    }
}
```

#### Sugar Decorator
```java
class SugarDecorator extends CoffeeDecorator {

    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    public double cost() {
        return coffee.cost() + 5;
    }

    public String description() {
        return coffee.description() + ", Sugar";
    }
}
```

---

## Client Code

```java
public class Main {
    public static void main(String[] args) {

        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.description() + " = " + coffee.cost());

        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);

        System.out.println(coffee.description() + " = " + coffee.cost());
    }
}
```

---

## Output

```
Simple Coffee = 50.0
Simple Coffee, Milk, Sugar = 65.0
```

---

## Real Java Examples

### Java I/O
```java
BufferedReader br =
    new BufferedReader(
        new InputStreamReader(System.in));
```

Each wrapper adds new behavior while keeping the same interface.

---

## Decorator vs Adapter

| Decorator | Adapter |
|---------|--------|
| Adds behavior | Converts interface |
| Same interface | Different interface |
| Can be stacked | Usually single use |

---

## Decorator vs Proxy

| Decorator | Proxy |
|---------|-------|
| Adds features | Controls access |
| Multiple layers | Usually single |

---

## Advantages

- Follows Open/Closed Principle
- Avoids subclass explosion
- Flexible and dynamic

---

## Disadvantages

- Many small classes
- Harder debugging
- Order of decorators matters

---

## Interview One-Liner

**Decorator pattern adds behavior to objects dynamically without modifying their structure by wrapping them with the same interface.**
