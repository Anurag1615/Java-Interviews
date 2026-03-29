# Factory Design Pattern

## 🔥 Definition
> Factory Design Pattern provides an interface for creating objects, but allows subclasses or factory methods to decide which class to instantiate.

---

# 1. Problem First (Without Factory)

## Scenario
We want to create different types of objects (Circle, Rectangle).

---

## Implementation

```java
class Circle {
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Rectangle {
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
}
```

---

## Usage

```java
Circle c = new Circle();
Rectangle r = new Rectangle();
```

---

## ❌ Problems

### ❌ Tight Coupling
Client depends on concrete classes

### ❌ Hard to Extend
Adding new class → modify client

### ❌ Violates Open Closed Principle

---

# 2. Solution → Simple Factory

## Idea

👉 Centralize object creation  

```
Client → Factory → Object
```

---

## Implementation

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        System.out.println("Circle");
    }
}

class Rectangle implements Shape {
    public void draw() {
        System.out.println("Rectangle");
    }
}

class ShapeFactory {

    public static Shape getShape(String type) {

        if (type.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        } else if (type.equalsIgnoreCase("RECTANGLE")) {
            return new Rectangle();
        }

        return null;
    }
}
```

---

## Usage

```java
Shape shape = ShapeFactory.getShape("CIRCLE");
shape.draw();
```

---

# 🚨 Problems with Simple Factory

## ❌ 1. Violates Open Closed Principle

Adding new shape:

```java
class Triangle implements Shape {}
```

👉 Must modify factory ❌

---

## ❌ 2. Violates Single Responsibility Principle

Factory does 2 things:

1. Selection logic  
2. Object creation  

---

## 🎯 Conclusion

> Simple Factory is easy but not scalable.

---

# 3. Solution → Factory Method Pattern (GoF)

## Idea

👉 Delegate object creation to subclasses  

```
Client → Factory → Subclass Factory → Object
```

---

## Implementation

### Step 1: Interface

```java
interface Shape {
    void draw();
}
```

---

### Step 2: Concrete Classes

```java
class Circle implements Shape {
    public void draw() {
        System.out.println("Circle");
    }
}

class Rectangle implements Shape {
    public void draw() {
        System.out.println("Rectangle");
    }
}
```

---

### Step 3: Factory Interface

```java
interface ShapeFactory {
    Shape createShape();
}
```

---

### Step 4: Concrete Factories

```java
class CircleFactory implements ShapeFactory {
    public Shape createShape() {
        return new Circle();
    }
}

class RectangleFactory implements ShapeFactory {
    public Shape createShape() {
        return new Rectangle();
    }
}
```

---

### Step 5: Client

```java
ShapeFactory factory = new CircleFactory();
Shape shape = factory.createShape();
shape.draw();
```

---

# 🚨 Limitation of Factory Method

👉 No direct way to select based on input (type)

---

# 4. Final Solution → Factory Registry (Best Practice)

## Idea

👉 Combine Factory Method + Map-based selection  
👉 Avoid if-else  
👉 Maintain OCP  

```
Client → Registry → Factory → Object
```

---

## Implementation

### Step 1: Factory Interface

```java
interface ShapeFactory {
    Shape create();
}
```

---

### Step 2: Concrete Factories

```java
class CircleFactory implements ShapeFactory {
    public Shape create() {
        return new Circle();
    }
}

class RectangleFactory implements ShapeFactory {
    public Shape create() {
        return new Rectangle();
    }
}
```

---

### Step 3: Registry

```java
import java.util.HashMap;
import java.util.Map;

class FactoryRegistry {

    private static final Map<String, ShapeFactory> registry = new HashMap<>();

    static {
        registry.put("CIRCLE", new CircleFactory());
        registry.put("RECTANGLE", new RectangleFactory());
    }

    public static Shape getShape(String type) {
        ShapeFactory factory = registry.get(type.toUpperCase());
        if (factory == null) {
            throw new IllegalArgumentException("Unknown type");
        }
        return factory.create();
    }
}
```

---

### Step 4: Client

```java
Shape shape = FactoryRegistry.getShape("CIRCLE");
shape.draw();
```

---

# 🧠 Adding New Shape (OCP Friendly)

```java
class Triangle implements Shape {
    public void draw() {
        System.out.println("Triangle");
    }
}

class TriangleFactory implements ShapeFactory {
    public Shape create() {
        return new Triangle();
    }
}
```

👉 Register:

```java
registry.put("TRIANGLE", new TriangleFactory());
```

👉 No existing code modified ✅  

---

# 🧩 UML Diagram (Simple Factory)

```
Client → ShapeFactory → Shape → (Circle / Rectangle)
```

---

# 🧩 UML Diagram (Factory Method)

```
        +------------------+
        |      Client      |
        +------------------+
                 |
                 v
        +----------------------+
        |    ShapeFactory      |  <<interface>>
        +----------------------+
        | createShape()        |
        +----------------------+
            /           \
           /             \
          v               v
+----------------+   +----------------+
| CircleFactory  |   | RectangleFactory|
+----------------+   +----------------+
| createShape()  |   | createShape()  |
+----------------+   +----------------+
        |                    |
        v                    v
    +--------+         +-----------+
    | Circle |         | Rectangle |
    +--------+         +-----------+
        ^
        |
   +----------+
   |  Shape   |
   +----------+
   | draw()   |
   +----------+
```

---

# 5. Flow

```
Client → Registry → Factory → Object
```

---

# 6. Key Concepts (WHY)

✔ Interface → abstraction  
✔ Factory → encapsulates creation  
✔ Registry → handles selection  
✔ Client → depends on abstraction  

---

# 7. Advantages

✔ Loose coupling  
✔ OCP compliant  
✔ No if-else  
✔ Scalable design  

---

# 8. Disadvantages

❌ More classes  
❌ Slight complexity  

---

# 9. When to Use

✔ Multiple object types  
✔ Need runtime selection  
✔ Want scalable design  

---

# 10. Real-World Examples

- Spring BeanFactory / ApplicationContext  
- LoggerFactory  
- Payment gateway selection  
- Database drivers  

---

# 11. Interview One-Liner

> Factory pattern decouples object creation from usage, and with a registry-based approach, it supports dynamic selection while maintaining OCP.

---

# 12. Pro Tip

> Avoid if-else in factories. Use a registry (Map) to make the design scalable.

---

# 🔥 Final Insight

```
Simple Factory → Easy but breaks OCP & SRP
Factory Method → SOLID but lacks selection
Factory + Registry → Best real-world solution
```
