<h1>
  <span class="headline">Java Subclasses</span>
  <span class="subhead">Superclasses and Subclasses</span>
</h1>

**Learning objective:** By the end of this lesson, You'll be able to write java code to:
- Inherit a subclass from a superclass
- Redefine superclass methods inside the inherited subclass
- Block a superclass method from being redefined inside its subclasses
- Block a class from being inherited

## Inheriting classes in Java

In Java’s inheritance model, the superclass (or parent class) provides the form (properties) and function(methods) that is shared with its subclasses (or child classes). The subclass inherits all the properties and methods from the superclass, and can also add its own specific properties and methods. As opposed to other OOP languages, Java supports single inheritance only, meaning that a class can only inherit from one superclass. 

### `extends` keyword
The `extends` keyword is used to create a subclass that inherits from another superclass. We use it when we need to model a relationship where one class is a specialized version of another. For example, if animal is a class, then a mammal is a specialized subclass of animal that can do all what an animal can generally do. However, it  can do some specialized things what any animal, in general cannot do.


In the below example, subclasses `Mammal` and `Bird` inherit the `name` property and `eat()` method from the superclass `Animal`. And, they also have their own methods.
```java
class Animal {
    String name;

    void eat() {
        System.out.println(name + " is eating.");
    }
}

class Mammal extends Animal {
    void nurse() {
        System.out.println(name + " is nursing its young.");
    }
}

class Bird extends Animal {
    void fly() {
        System.out.println(name + " is flying.");
    }
}
```
### Things to be mindful of when using `extends`
During class inheritance using `extends`, java encapsulation also is in effect. Hence, we need to be mindful of the access modifiers of the superclass members.
- `public`, `protected`, `private` and default members of a superclass can be inherited by subclasses defined in the **same file**.
- Only `public`, `protected` and default members of a superclass can be inherited by the subclass defined inside the same package.
- Only `public` and `protected` members of a superclass can be inherited by subclasses defined in different packages.



## Redefining superclass methods inside subclass
While `extends` enables reuse of code from superclass, sometimes a subclass needs to redefine (override) a method from its superclass to provide specialized behavior. 

### `@Override` annotation
The `@Override` annotation allows a subclass to provide its own implementation for a superclass method. For example, a bird has a peculiar way of eating compared to all other animals.
```java
class Animal {
    String name;

    void eat() {
        System.out.println(name + " is eating.");
    }
}

class Bird extends Animal {
    @Override
    void eat() {
        System.out.println(name + " is pecking at seeds.");
    }
}
```

### Things to be mindful of when using `@Override`
- `@Override` annotation, like all annotations, is a message for the compiler and not the Java Runtime Engine (JRE). In other words, `@Override` annotation is optional for running the program. However, when the compiler encounters an `@Override` annotation above a method, it checks for the existence of a method with the same method signature (method name + arguments), in the superclass hierarchy of the current class. If the check finds no such method, then it throws a compile-time exception (Checked exception). Hence, using `@Override` annotation is a best-practice convention that intimates programmers when they modify method signatures without realising that there are subclasses that are using the same method signature.

## Blocking a superclass method from being redefined
Sometimes we want to prevent certain behaviors from being modified by subclasses as the program logic might dictate so. For example, we might want a behaviour in `Animal` to be unmodified if all animals do it universally. For example, breathing. 

### `final` keyword
We use `final` keyword in the method declaration to preserve critical behaviors or prevent unintended changes by subclasses.

```java
class Animal {
    final void breathe() {
        System.out.println("All animals breathe.");
    }
}

// The below class definition would cause an error:
// class Mammal extends Animal {
//     @Override
//     void breathe() { ... }
// }
```

## Blocking a class from being inherited
Using the same `final` keyword, we can stop a class from being **extended** any further. This is useful when we want to prevent the class from being modified in unintended ways by subclasses. Using the `final` keyword to prevent a class from being extended is a powerful way to enforce design decisions and maintain the integrity of our code. In the example below, marking Bird as final ensures that no further subclassing can occur, keeping the taxonomy consistent.

```java
// Superclass
class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    void eat() {
        System.out.println(name + " is eating.");
    }
}


// Final Subclass
final class Bird extends Animal {
    Bird(String name) {
        super(name);
    }

    void fly() {
        System.out.println(name + " is flying.");
    }
}

// This class definition would cause a compile-time error
// class Parrot extends Bird {
//     void mimic() {
//         System.out.println("Parrot is mimicking sounds.");
//     }
// }

public class Main {
    public static void main(String[] args) {
        Bird bird = new Bird("Eagle");
        bird.eat();
        bird.fly();

        // Uncommenting the below line would result in an error
        // Parrot parrot = new Parrot();
    }
}
```