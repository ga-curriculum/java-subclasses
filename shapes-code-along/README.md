<h1>
  <span class="headline">Java Subclasses</span>
  <span class="subhead">Shapes Code-Along</span>
</h1>

**Learning objective:** By the end of this lesson, you'll be able to write Java programs using the techniques of working with subclasses and superclasses.

## Independent practice: Shapes (40 min)

Let's get into a little more depth with an example about `Shapes`.

### Part 1 (5 min)

Let's start by coding out this first part together. Say we have a superclass called `Shape`, as follows:

```java
package org.example.oop;

public class Shape {
    public double getPerimeter() {
        return 0;
    }

    public double getArea() {
        return 0;
    }
}
```

Our `Shape` class has methods `getPerimeter()` and `getArea()` which, depending on the shape, will calculate the perimeter and area of the shape.

As we define some subclasses to inherit from `Shape`, remember that each shape has a slightly different function to determine its area or perimeter.

Let's start by defining a subclass of `Shape` called `Triangle` that takes three sides in its constructor:

```java
package org.example.oop;

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

Your assignment is to implement the `getPerimeter()` and `getArea()` methods.

The perimeter is defined as the total distance around any closed two-dimensional shape. So, for a triangle, it's the sum of the sides, and for a circle, it's 2πr, where r is the radius and π is `Math.PI`.

The area is the amount of surface enclosed inside a shape. For a triangle, we can use Heron's formula:

```plaintext
√(s(s−a)*(s−b)*(s−c))    
```

In this case, `s = (a+b+c)/2` (half the perimeter). To take a square root, we can use the `Math.sqrt()` method.

When you're done, check your solution against the answer below.

```java
package org.example.oop;

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
    public double getPerimeter() {
        return side1 + side2 + side3;
    }

    @Override
    public double getArea() {
        double perimeter = getPerimeter();
        double s = perimeter/2;
        double squaredArea = s * (s-side1) * (s-side2) * (s-side3);
        double area = Math.sqrt(squaredArea);
        return area;
    }
}
```

#### Part 2: Bonus

Do the same for `Circle` and `Rectangle`. Remember: These should all extend the `Shape` class. A potential solution is provided below.

```java
package org.example.oop;

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
}

package org.example.oop;

public class Rectangle extends Shape {
    private int length;
    private int width;

    public Rectangle(int length, int width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double getPerimeter() {
        return 2*length + 2*width;
    }

    @Override
    public double getArea() {
        return length * width;
    }
}
```

### Part 3 (5 min)

Now, let's say we want to create a `Square` shape. You may remember from geometry class that a square is a special kind of rectangle with all sides equal.

Let's create a `Square` class that extends `Rectangle` and, instead of overriding the `getPerimeter()` and `getArea()` methods, just provide a constructor that will pass the same value for all sides into the `Rectangle` constructor.

> **Knowledge Check**: How should we implement this? Let's discuss some possible solutions. A potential solution is provided below.

```java
public class Square extends Rectangle {
    public Square(int side) {
        super(side, side);
    }
}
```

Now, this `Square` will automatically calculate its perimeter and area.

### Part 4 (15 min)

Finally, let's test our code by calculating the perimeter and area for each of the following four shapes:

- A triangle with sides with a length of 3, 4, and 5.
- A rectangle with length of 4 and height of 5.
- A circle with a radius of 4.
- A square with sides that have a length of 4.

Assign each one of these shapes to a variable of the appropriate type, then pass that shape to a method with signature:

```java
private static String getPerimeterAndArea(Shape shape)
```

That will return a `String` such as `ShapeType perimeter, area = x, y`, with the correct values for each shape.

**Sample expected output:**

```java
Triangle perimeter, area = 12.0, 6.0
Rectangle perimeter, area = 18.0, 20.0
Circle perimeter, area = 25.132741228718345, 50.26548245743669
Square perimeter, area = 16.0, 16.0
```

When you're ready, check your answer with the solution below.

<details>
<summary>Need a hint?</summary>

The important thing to note here is that the <code>getCircumferanceAndArea()</code> method accepts a <code>Shape</code> argument and, even though our shapes are <code>Triangle</code>, <code>Rectangle</code>, <code>Circle</code>, and <code>Square</code>, the method still accepts them, as they extend <code>Shape</code>. Anything that extends a class is that class, so a <code>Rectangle</code> is a <code>Shape</code>.
</details>

A possible solution is provided below:

```java
package org.example.oop;

public class ShapeCalc {
    public static void main(String[] args) {
        Triangle triangle = new Triangle(3, 4, 5);
        Rectangle rectangle = new Rectangle(4, 5);
        Circle circle = new Circle(4);
        Square square = new Square(4);

        System.out.println("Triangle " + getPerimeterAndArea(triangle));
        System.out.println("Rectangle " + getPerimeterAndArea(rectangle));
        System.out.println("Circle " + getPerimeterAndArea(circle));
        System.out.println("Square " + getPerimeterAndArea(square));
    }

    private static String getPerimeterAndArea(Shape shape) {
        String rval = "perimeter, area = " + shape.getPerimeter() + ", " + shape.getArea();
        return rval;
    }
}
```

## Conclusion

Inheritance is here for our convenience and to keep our code DRY (that stands for **Don't Repeat Yourself**). It might seem like an intimidating concept, but once we master it, it can be a powerful tool.

We'll find many examples of inheritance and shared properties and functionality in real life, which is exactly what our programming languages are meant to emulate and model.
