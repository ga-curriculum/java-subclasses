# ![[tktk Module Name] - tktk Microlesson Name](./assets/shapes-code-along.png)

**Learning objective:** By the end of this lesson, students will be able to practice appying superclasses and subclasses.

## Independent Practice: Shapes (40 min)

Let's get into a little more depth with an example about `Shapes`.

### Part 1 (5 min)

Let's start by coding out this first part together. Say we have a superclass called `Shape`, as follows:

```java
package com.generalassembly.oop;

public class Shape {
    public double getCircumference() {
        return 0;
    }

    public double getArea() {
        return 0;
    }
}
```

Our `Shape` class has methods `getCircumference()` and `getArea()` that, depending on the shape, will calculate the circumference and area of the shape.

As we define some subclasses to inherit from `Shape`, remember that each of these shapes has a slightly different function to determine its area or circumference.

Let's start by defining a subclass of `Shape` called `Triangle` that takes three sides in its constructor:

```java
package com.generalassembly.oop;

public class Triangle extends Shape {
    private double side1;
    private double side2;
    private double side3;

    public Triangle(double side1, double side2, double side3) {
        this.side1 = side1;
        this.side2 = side2;
        this.side3 = side3;
    }
}
```

### Part 2 (15 min)

Your assignment is to implement the `getCircumference()` and `getArea()` methods.

The circumference (or perimeter) is defined as the length of the outline of the shape. So, for a triangle, it's the sum of the sides, and for a circle, it's 2πr, where r is the radius and π is `Math.PI`.

The area is the square footage covered by the shape. For a triangle, we can use Heron's formula:

 ```
 √(p(p−a)*(p−b)*(p−c))    
 ```

In this case, `p = (a+b+c)/2`. To take a square root, use the `Math.sqrt()` method. 

When you're done, check your solution against the answer below.
 
<details>
<summary>Solution</summary>

```java 
package com.generalassembly.oop;

public class Triangle extends Shape{
    private double side1;
    private double side2;
    private double side3;

    public Triangle(double side1, double side2, double side3) {
        this.side1 = side1;
        this.side2 = side2;
        this.side3 = side3;
    }

    @Override
    public double getCircumference() {
        return side1 + side2 + side3;
    }

    @Override
    public double getArea() {
        double circumference = getCircumference();
        double p = circumference/2;
        double squared = p * (p-side1) * (p-side2) * (p-side3);
        double area = Math.sqrt(squared);
        return area;
    }
}
```

</details>

#### Part 2: Bonus

Do the same for `Circle` and `Rectangle`. Remember: These should all extend the `Shape` class.

<details>
<summary>Solution</summary>

```java
package com.generalassembly.oop;

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getCircumference() {
        return 2 * Math.PI * radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
}

package com.generalassembly.oop;

public class Rectangle extends Shape {
    private int length;
    private int width;

    public Rectangle(int length, int width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double getCircumference() {
        return 2*length + 2*width;
    }

    @Override
    public double getArea() {
        return length * width;
    }
}
```

</details>

### Part 3 (5 min)

Now let's say we want to create a `Square` shape. We remember from geometry class that a square is a special kind of rectangle in which all sides are equal.

Let's create a `Square` class that extends `Rectangle` and, instead of overriding the `getCircumference()` and `getArea()` methods, just provide a constructor that will pass in the same value for all sides into the `Rectangle` constructor.

> **Knowledge Check**: How do you think we should implement this? Shout out some possible solutions.

<details>
<summary>Solution</summary>

```java
public class Square extends Rectangle {
    public Square(int side) {
        super(side, side);
    }
}
```

</details>

Now this `Square` will automatically calculate its circumference and area.

### Part 4 (15 min)

Finally, let's test our code by calculating the circumference and area for each of the following four shapes:

* A triangle with sides 3, 4, 5.
* A rectangle with length = 4 and height = 5.
* A circle with radius = 4.
* A square with side = 4.

Assign each one of these shapes to a variable of the appropriate type, then pass that shape to a method with signature: 

```java
private static String getCircumferenceAndArea(Shape shape)
```

That will return a `String` such as `ShapeType circumference, area = x, y`, with the correct values for each shape.

**Sample expected output:**

```java
Triangle circumference, area = 12.0, 6.0
Rectangle circumference, area = 18.0, 20.0
Circle circumference, area = 25.132741228718345, 50.26548245743669
Square circumference, area = 16.0, 16.0
```

When you're ready, check your answer with the solution below.

<details>
<summary>Need a hint?</summary>

The important thing to note here is that the `getCircumferanceAndArea()` method accepts a `Shape` argument and, even though our shapes are `Triangle`, `Rectangle`, `Circle`, and `Square`, the method still accepts them, as they extend `Shape`. Anything that extends a class **is** that class, so a `Rectangle` is a `Shape`.
</details>

<details>
<summary>Solution</summary>

```java
package com.generalassembly.oop;

public class ShapeCalc {
    public static void main(String[] args) {
        Triangle triangle = new Triangle(3, 4, 5);
        Rectangle rectangle = new Rectangle(4, 5);
        Circle circle = new Circle(4);
        Square square = new Square(4);

        System.out.println("Triangle " + getCircumferenceAndArea(triangle));
        System.out.println("Rectangle " + getCircumferenceAndArea(rectangle));
        System.out.println("Circle " + getCircumferenceAndArea(circle));
        System.out.println("Square " + getCircumferenceAndArea(square));
    }

    private static String getCircumferenceAndArea(Shape shape) {
        String rval = "circumference, area = " + shape.getCircumference() + ", " + shape.getArea();
        return rval;
    }
}
```

</details>

----

## Conclusion (10 min)

That was an intense lesson! Let's recap.

### Tying Back to the Four Principles

Combining subclasses and visibility, we can hide the details of functionality inside a class and just expose an API (application programming interface). Now that we've got inheritance in our tool belt, the uses of encapsulation (which includes things like the access level), abstraction, and polymorphism become a lot more salient.

#### Remember Polymorphism?

Polymorphism may sound scary, and it will keep cropping up. But the concept is easier to understand than the word suggests.

Now that we have a specific example of polymorphism, let's revisit the topic. We see that we can create various subclasses of the same class and implement specific functionality for each.

In our first example, we had `Cat` and `Lion`, and both had a `getLives()` method. However, their implementation was different despite sharing the same name.

In our second example, we had five different shapes — `Shape`, `Triangle`, `Circle`, `Rectangle`, and `Square` — all with different implementations of their `getCircumference()` and `getArea()` functionality. Yet they all worked just fine, and we didn't need to alter how we used the instance of each shape.

### Summary

Inheritance is here for our convenience and to keep our code DRY (that stands for "Don't Repeat Yourself"). It might seem like an intimidating concept, but once you master it, it can be a powerful tool. You'll find many examples of inheritance and shared properties and functionality in real life, which is exactly what our programming languages are meant to emulate and model.