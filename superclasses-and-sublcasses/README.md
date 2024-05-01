# ![[tktk Module Name] - tktk Microlesson Name](./assets/superclasses-and-subclasses.png)

**Learning objective:** By the end of this lesson, students will be able to engage with superclasses and subclasses using the `extend` keyword, `@Override` annotations, and `finals` to restrict inheritance. 

## Opening (5 min)

In our use of language and observations from the world, we naturally categorize objects of similar types. For example, we understand what a cat is and that a lion is a member of the cat family. And, although we have no confusion between a cat and a lion, we know that lions share qualities with cats.

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