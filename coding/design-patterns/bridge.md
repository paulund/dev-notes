# Bridge Design Pattern

The Bridge pattern is a structural design pattern that decouples an abstraction from its implementation so that the two
can vary independently. This pattern is useful when we need to create a system that can support multiple platforms or
devices.

The Bridge pattern consists of two parts:

- Abstraction: The Abstraction defines the interface for the "control" part of the system. It maintains a reference to
  an Implementor object and delegates the implementation to it.
- Implementor: The Implementor defines the interface for the "platform" part of the system. It provides concrete
  implementations for the operations defined by the Abstraction.

The Bridge pattern allows us to create a system that can support multiple platforms or devices by separating the "control" part of the system from the "platform" part. This separation allows us to change the implementation of the "platform" part without affecting the "control" part, and vice versa.

## The Problem

Suppose we are developing a drawing application that renders different shapes on different platforms, such as Windows
and Linux. We want to create a system that can support multiple platforms and shapes without having to create a separate
class for each combination of platform and shape.

## The Solution

The Bridge pattern provides a solution to this problem by decoupling the abstraction (the "control" part of the system)
from its implementation (the "platform" part of the system). This separation allows us to create a system that can
support multiple platforms and shapes by defining separate hierarchies for the abstraction and implementation.

## Real World Example

In this example, we will create a drawing application that supports rendering different shapes on different platforms.
We will define an abstraction for the "control" part of the system (the `Shape` class) and an implementation for the "
platform" part of the system (the `Renderer` interface). We will then create concrete implementations of the `Shape`
class (such as `Circle` and `Square`) and the `Renderer` interface (such as `WindowsRenderer` and `LinuxRenderer`) to
render the shapes on different platforms.

```php
<?php

// Implementor interface
interface Renderer {
    public function renderCircle();
    public function renderSquare();
}

// Concrete Implementor: WindowsRenderer
class WindowsRenderer implements Renderer {
    public function renderCircle() {
        echo "Rendering circle on Windows platform\n";
    }

    public function renderSquare() {
        echo "Rendering square on Windows platform\n";
    }
}

// Concrete Implementor: LinuxRenderer
class LinuxRenderer implements Renderer {
    public function renderCircle() {
        echo "Rendering circle on Linux platform\n";
    }

    public function renderSquare() {
        echo "Rendering square on Linux platform\n";
    }
}

// Abstraction: Shape
abstract class Shape {
    protected $renderer;

    public function __construct(Renderer $renderer) {
        $this->renderer = $renderer;
    }

    abstract public function draw();
}

// Concrete Abstraction: Circle
class Circle extends Shape {
    public function draw() {
        $this->renderer->renderCircle();
    }
}

// Concrete Abstraction: Square
class Square extends Shape {
    public function draw() {
        $this->renderer->renderSquare();
    }
}

// Usage
$windowsRenderer = new WindowsRenderer();
$linuxRenderer = new LinuxRenderer();

$circle = new Circle($windowsRenderer);
$circle->draw(); // Output: Rendering circle on Windows platform

$square = new Square($linuxRenderer);
$square->draw(); // Output: Rendering square on Linux platform
```

In this example, we have defined an abstraction for the "control" part of the system (the `Shape` class) and an
implementation for the "platform" part of the system (the `Renderer` interface). We have then created concrete
implementations of the `Shape` class (such as `Circle` and `Square`) and the `Renderer` interface (such
as `WindowsRenderer` and `LinuxRenderer`) to render the shapes on different platforms.

By using the Bridge pattern, we can create a system that can support multiple platforms and shapes without having to
create a separate class for each combination of platform and shape. This separation allows us to change the
implementation of the "platform" part without affecting the "control" part, and vice versa.

## Benefits of the Bridge Pattern

The Bridge pattern offers several benefits, including:

- Separation of concerns: The Bridge pattern separates the abstraction from its implementation, allowing them to vary
  independently.
- Encapsulation: The Bridge pattern encapsulates the implementation details in separate classes, making the code easier
  to maintain and extend.
- Flexibility: The Bridge pattern allows us to change the implementation of the abstraction and the implementation
  independently, providing greater flexibility in the design.
- Reusability: The Bridge pattern promotes code reuse by allowing different abstractions to share the same
  implementation.
- Testability: The Bridge pattern makes it easier to test the abstraction and implementation separately, improving the
  overall testability of the system.

By using the Bridge pattern, we can create a system that is more flexible, maintainable, and testable, making it easier
to support multiple platforms and devices.

## Conclusion

The Bridge pattern is a powerful design pattern that allows us to create systems that can support multiple platforms or
devices by decoupling the abstraction from its implementation. By separating the "control" part of the system from the "
platform" part, the Bridge pattern provides greater flexibility, maintainability, and testability in the design. If you
need to create a system that can support multiple platforms or devices, consider using the Bridge pattern to achieve a
more modular and extensible design.
