# 🎶 Notes to remember :
- Identify the aspects of your application that vary and separate them from what stays the same.
- Program to an interface, not an implementation
- Favor composition over inheritance
- Strive for loosely coupled designs between objects that interact.

# 🖥 DESIGN PATTERNS:
## 1. Creational Design Pattern:

👉 Concerned with **object creation** (how objects are created, not how they’re used)
### 1.Singleton Design Pattern:

**Intent**
Ensure **only one instance** of a class exists and provide a **global access point**.

**When to use**

- Logging
    
- Configuration
    
- Database connection
    
- Cache

**Example (Thread-safe – Best Practice)**
```java
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

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
}
```

**Key Points (Interview)**

- Private constructor
    
- Static instance
    
- Thread safety matters
    
- Double-checked locking [[DoubleCheckedLocking]]

### 2.Factory Design Pattern:

**Intent**  

Create objects **without exposing the instantiation logic**.

**When to use**

- Object creation depends on input
    
- Decouple client from implementation
```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("Circle"); }
}

class ShapeFactory {
    public static Shape getShape(String type) {
        if ("CIRCLE".equals(type)) return new Circle();
        return null;
    }
}
```
```java
Shape shape = ShapeFactory.getShape("CIRCLE");
shape.draw();
```

### **3 . Abstract Factory Pattern**:

👉 Abstract Factory = **Factory of factories**

**Intent** 

Create **families of related objects** without specifying concrete classes.

**When to use**

- Multiple factories
    
- UI toolkits
    
- Platform-specific code

**Example (Concept)**
```java
interface GUIFactory {
    Button createButton();
}
```

When you want to switch entire families then you can use this pattern.

### **4. Builder Pattern**

**Intent**

Construct **complex objects step by step**.

**When to use**

- Many optional parameters
    
- Immutable objects
```java
class User {
    private String name;
    private int age;

    static class Builder {
        private String name;
        private int age;

        Builder name(String name) {
            this.name = name;
            return this;
        }

        Builder age(int age) {
            this.age = age;
            return this;
        }

        User build() {
            return new User(this);
        }
    }

    private User(Builder b) {
        this.name = b.name;
        this.age = b.age;
    }
}
```

### 5. **Prototype Pattern**:

**Intent**

Create new objects by **cloning existing ones**.

**When to use**

- Object creation is expensive
    
- Many similar objects

**Example**
```java
class Prototype implements Cloneable {
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

## 2. Structural Design Pattern:
👉 Concerned with **class/object composition**

### **6. Adapter Pattern**:
👉 **Adapter acts as a bridge**

**🔹 Scenario**

- Client expects **USB charging**
    
- Existing service provides **Type-C charging**

**🔹 Step 1: Target Interface**
```java
interface USBCharger {
    void chargeWithUSB();
}
```

**🔹 Step 2: Adaptee (Existing Class)**
```java
class TypeCCharger {
    public void chargeWithTypeC() {
        System.out.println("Charging with Type-C");
    }
}
```
**🔹 Step 3: Adapter**
```java
class ChargerAdapter implements USBCharger {

    private TypeCCharger typeCCharger;

    public ChargerAdapter(TypeCCharger typeCCharger) {
        this.typeCCharger = typeCCharger;
    }

    @Override
    public void chargeWithUSB() {
        typeCCharger.chargeWithTypeC();
    }
}
```
**🔹 Step 4: Client**
```java
public class Main {
    public static void main(String[] args) {

        TypeCCharger typeC = new TypeCCharger();
        USBCharger usbCharger = new ChargerAdapter(typeC);

        usbCharger.chargeWithUSB();
    }
}
```

### **7. Decorator Pattern**:

**Intent**

Add new behavior **dynamically** without modifying existing code.

**Example**

- Java I/O streams
    
- Adding toppings to pizza
[[DecoratorPattern]]

### **8. Proxy Pattern**:
[[ProxyPattern]]

### **9.Facade Pattern:**

The **Facade** pattern is a structural design pattern that provides a _simple, unified interface_ over a complex subsystem of many classes, so that clients can use it easily without dealing with internal complexity. It does not change behavior or interfaces of subsystem classes; it just organizes and simplifies access to them.

### **10. Composite Pattern:**

