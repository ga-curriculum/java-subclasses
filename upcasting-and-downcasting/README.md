# ![[tktk Module Name] - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to apply upcasting and downcasting to manipulate objects with an inheritance hierarchy. 

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