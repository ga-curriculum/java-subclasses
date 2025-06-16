<h1>
  <span class="headline">Java Subclasses</span>
  <span class="subhead">Object Typecasting</span>
</h1>

**Learning objective:** By the end of this lesson, you'll be able to use object typecasting to manipulate objects across a class hierarchy, enhancing code flexibility and reusability.

## An introduction to object typecasting

Object typecasting allows a programmer to convert objects of one type into another within a class' inheritance hierarchy. This is particularly useful in Object-Oriented Programming (OOP) when working with inheritance, enabling flexibility and reusability in code.

## Upcasting

Polymorphism enables a superclass reference to refer to a subclass object. This allows us to handle different subclasses uniformly, focusing on their shared behaviors. This process is known as **upcasting**. Upcasting is safe because every subclass instance is also an instance of its superclass—for example, every `Bird` is an `Animal`.

```java
class Animal {
    void eat() {
        System.out.println("Animal is eating.");
    }
}

class Bird extends Animal {
    void fly() {
        System.out.println("Bird is flying.");
    }
}

public class Main {
    public static void main(String[] args) {
        Bird bird = new Bird();
        Animal animal = bird; // Upcasting
        animal.eat(); // Animal's any method can be called
        // animal.fly(); // Compile-time error.
        // Bird's exclusive methods cannot be called by Animal objects.
    }
}
```

Upcasting is **implicit**. In other words, there's no need for any separate statement, keyword, or syntax to tell the compiler and JRE that upcasting is done.

## Downcasting

When we need to access subclass-specific members from a superclass object reference, we cast the object reference back to the subclass type. This technique is called **downcasting**.

As downcasting converts a superclass object reference into a subclass object reference, it is not always safe. For example, the statement *"This `Animal` is a `Bird`"* is not true about every animal in the world (because not every `Animal` is a `Bird`).

For this reason, downcasting needs caution. If the object being cast does not match the target type, JRE may throw a `ClassCastException`. Therefore, when downcasting, we follow the below rules:

1. We use downcasting when the actual type of an object reference is known and we need access to subclass-specific methods.
2. While accessing a subclass member using superclass object reference, we explicitly use this syntax to tell JRE that we're downcasting: `((subclass-name) superclass-reference).subclass-member`. For example:

   ```java
   Animal myAnimal = new Bird();
   ((Bird) myAnimal).fly(); // Access Bird-specific method
   ```

3. As a best practice, we always use the `instanceof` boolean operator before downcasting to avoid `ClassCastException`. For example:

   ```java
   public class Main {
       public static void main(String[] args) {
           Animal animal = new Bird(); // Upcasting
           if (animal instanceof Bird) { // Check before downcasting
               Bird bird = (Bird) animal; // Downcasting
               bird.fly(); // Now fly() can be called
           }
       }
   }
   ```

## Dynamic method dispatch

Dynamic method dispatch allows a superclass reference to call overridden methods in a subclass at runtime. This is a classic example of **runtime polymorphism** in Java, where the method that gets executed is determined at runtime based on the actual object type.

```java
class Animal {
    void eat() {
        System.out.println("Animal is eating.");
    }
}

class Bird extends Animal {
    @Override
    void eat() {
        System.out.println("Bird is eating seeds.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Bird(); // Upcasting
        animal.eat(); // Calls Bird's eat() method
    }
}
```
