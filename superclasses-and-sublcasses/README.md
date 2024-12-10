<h1>
  <span class="headline">Java Subclasses</span>
  <span class="subhead">Superclasses and Subclasses</span>
</h1>

**Learning objective:** By the end of this lesson, You'll be able write java code to:
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





![](resources/cat-lion.png)

<!-- Taken from https://odditymall.com/includes/content/thumb1366252920.png. -->

We can say that `Lion` is a subclass of `Cat` and will inherit basic properties and functionality from its **superclass** (its parent class). 

> **Note**: You may also hear subclasses and superclasses referred to as "parent" and "child" classes, which may help visualize what is meant by "inheritance."

-----

## Guided Practice: Superclasses and Subclasses (15 min)

Let's create a `Cat` class: 

```java
public class Cat {
    private String name;
    
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public boolean hasWhiskers() {
        return true;
    }

    protected String favoriteSnack() {
        return "Mice";
    }

    public int getLives() {
        return 9;
    }
}
```

We create a subclass of `Cat` by using the `extends` keyword:

````java
class Lion extends Cat {
    @Override
    public int getLives() {
        return 1;
    }
}
````

In this case, we say `Lion` `extends` `Cat`, or `Lion` is a **subclass** of `Cat`. And we say `Cat` is a **superclass** of `Lion`. 

The `Lion` class inherits members from its superclass, so the calls below are legal. Even though they're not in the `Lion` class, they're inherited from the `Cat` class:

```java
Lion lion = new Lion();
lion.hasWhiskers();     
lion.favoriteSnack();  
lion.getLives();       
```

> **Think**: What will each of the calls above return?

<details>
<summary>Answers</summary>

* `hasWhiskers()` returns `true` (inherited from the `Cat` class).
* `favoriteSnack()` returns `"Mice"` (inherited from the `Cat` class).
* `getLives()` returns `1`. Why? A cat may have nine lives, but a `Lion` doesn't, because its `getLives()` method was **overridden**.

</details>

A subclass will inherit every field and method it can see, i.e., every `public` member, `protected` member, and default member (provided they're in the same package).

`private` members can't be inherited, except by classes defined in the **same file**.
 
### Best Practice: `@Override`

For the `Lion` subclass, notice how we put `@Override` above the `getLives()` method:

````java
class Lion extends Cat {
    @Override
    public int getLives() {
        return 1;
    }
}
````

It's best practice to annotate an overridden method by placing the `@Override` annotation above it. That tells the compiler to ensure it's overriding a method from the base class.

Although this isn't required, it's a good defensive practice. In the future, if the base method happens to get renamed along the way, the code will fail to compile (because it's no longer overriding anything). Therefore, this will help with early detection of some subtle bugs that would result from calling an overridden method, which is actually a new method.

An overridden method can be of the same visibility as the method it's overriding, or it can be more public. However, it can't be more private.

Here are a few quick-hit facts about annotations:
* They start with the `@` symbol.
* They don't affect code functionality.
* They do affect the compiler's actions.
* They're not required, but it's best practice to use them.

> For more details about annotations, read through [this article](https://www.geeksforgeeks.org/annotations-in-java/).

### Restricting Inheritance

There are times when you want to ensure that no subclasses can be created from your class. In such cases, you can declare your class to be `final`. If you try to create a subclass from a `final` class, the compiler will warn you with a message such as:

```java
Error: java: cannot inherit from final class
``` 

Sometimes, you might want your class to be inherited from, but perhaps you don't want the subclass to change a particular method or field in your class. If needed, particular members can be marked `final`.

A subclass can override any method visible to it — except if the method is `final` or `static` — by defining the method using the exact same name and method signature as the base method it's overriding.