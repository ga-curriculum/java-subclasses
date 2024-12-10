<h1>
  <span class="headline">Java Subclasses</span>
  <span class="subhead">Object Typecasting </span>
</h1>

**Learning objective:** By the end of this lesson, You'll be able to write java code to:
- Treat a subclass object as an instance of its superclass
- Access subclass-specific members from a superclass object

## An introduction to object typecasting
Object typecasting allows a programmer to convert objects of one type into another within a class' inheritance hierarchy. This is particularly useful in Object-Oriented Programming (OOP) when working with inheritance, enabling flexibility and reusability in code.

## Upcasting
Polymorphism allows a superclass object reference to point to a subclass object. This is useful when we want to treat different subclasses in the same way, focusing only on shared behavior. This technique is called **upcasting**. Upcasting is safe because a subclass object always "is-a" superclass object. For example, a `Bird` always "is-an" `Animal`.

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
        // animal.fly(); // Compile-time error. Bird's exclusive methods cannot be called by Animal object.
    }
}
```
Upcasting is **implicit**. In other words, there's no need of any separate statement, keyword or syntax to tell the compiler and JRE that upcasting is done.

## Downcasting
When we need to access subclass-specific members from a superclass object reference, then we cast the object reference back to the subclass type. This technique is called **downcasting**. As downcasting converts a superclass object reference into a subclass object reference, it is not always safe. For example, The statement "_This `Animal` is a `Bird`_" might be true only in certain contexts in the real world. Every `Animal` cannot be a `Bird`.

For this reason, downcasting needs caution. If the object being cast does not match the target type, JRE may throw a `ClassCastException`. To exercise caution we follow the below rules:
1. We use downcasting when the actual type of an object reference is known, and we need access to subclass-specific methods.
2. While accessing a subclass member using superclass object reference, we explicitly use a syntax to tell JRE that we're downcasting.
   - `((subclass-name) superclass-reference).subclass-member`
   - Example:
      ```java
      Animal myAnimal = new Bird();
      ((Bird) myAnimal).fly(); // Access Bird-specific method
3. As a best practice, we always use the `instanceof` boolean operator before downcasting to avoid `ClassCastException`
   - Example:
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























## Guided Practice: Upcasting and Downcasting (15 min)

There's a subtle feature going on here that's leveraged in just about every Java library you'll use: You can declare a variable to be of a certain type and assign it any subtype.

For example:

````java
Cat cat = new Lion();
````

> **Knowledge Check**: If you call `cat.getLives()` after declaring it like we did above, what do you expect will be returned: the nine lives of the `Cat` or the one life of the `Lion`?

<details>
<summary>The answer may be surprising.</summary>

The rule to remember is that, when you call a method on a variable, you're really calling the method on the instance contained in that variable. So, in our case, because the `cat` variable contains a `Lion` instance, the `getLives()` method will return the one life of the `Lion` and not the nine of the parent `Cat`. This is generally a good thing, because library vendors can now define subclasses, and they'll function as the vendor intends.

In plain English, maybe you want a collection (like an `Array` or `ArrayList`) of cats, but you want to be able to put lions, cats, and anything else in the cat family — say tigers or leopards — in that same collection. That's known as **upcasting**.

Did the answer surprise you?

</details>

### Downcasting

Downcasting works as well, as long as you're casting to the correct object type.

So, we can say `Cat cat = new Lion()`, which — as we saw above — works fine because a lion is a cat. But we can't say `Lion lion = new Cat()` because, in general, cats are not lions.

However, given the assignment `Cat cat = new Lion()`, we know the `cat` variable contains a `Lion` instance; therefore, we should be able to assign it:

````java
Cat cat = new Lion();
Lion lion = cat;
````

However, that fails to compile, because the compiler has no way to be sure that, by the time the `cat` is assigned, it still contains a `Lion` instance. All lions are cats, but not all cats are lions.

To make that assignment, you must explicitly downcast it using the **cast** operator, as follows:

```java
Lion lion = (Lion)cat;
```

By preceding the `cat` variable with `(Lion)`, you're **casting** the `cat` instance to a `Lion` class, which is your way of signing a pact with the compiler guaranteeing that the variable will contain a `Lion` instance.

And if you break that pact, like below...

```java
public static void main(String[] args) {
    Cat cat = new Cat();
    Lion lion = (Lion)cat;
}
```
...You'll get a `ClassCastException` from the compiler.