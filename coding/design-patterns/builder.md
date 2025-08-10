# Builder Design Pattern

One of the most popular design patterns is the Builder pattern. The Builder pattern is used to create complex objects with
a simple step-by-step approach. In this blog post, we will explore what the Builder pattern is, how it works, and some of its advantages.

The Builder pattern is a creational design pattern that separates the construction of a complex object from its representation.
The pattern allows the same construction process to create different representations. It involves creating a separate
builder class that handles the construction process of the complex object. The builder class takes care of all the necessary
steps to create the object and returns the final product.

The Builder pattern consists of several parts:

- Director: The Director is responsible for controlling the construction process. It creates a builder object and provides
it with the necessary information to create the product.
- Builder: The Builder is responsible for defining the steps required to create the product. It has a set of methods that
correspond to the steps in the construction process.
- Product: The Product is the final object that is created.
- Concrete Builder: The Concrete Builder implements the Builder interface and provides the necessary methods to create the product.

The process of creating the final product using the Builder pattern can be broken down into the following steps:

- The client creates a Director object.
- The Director creates a Concrete Builder object.
- The Director passes the Concrete Builder object to the client.
- The client calls the necessary methods on the Concrete Builder object to construct the product.
- The client retrieves the final product from the Concrete Builder object.

Advantages of the Builder pattern

The Builder pattern provides several advantages over other design patterns:

- Separation of concerns: The Builder pattern separates the construction of the object from its representation.
This allows for greater flexibility in creating different representations of the same object.
- Encapsulation: The Builder pattern encapsulates the construction process of the object in a separate builder class.
This keeps the client code clean and simple.
- Better control: The Builder pattern provides better control over the construction process. The client can specify the
desired representation of the object by passing different builder objects to the Director.
- Easy modification: The Builder pattern makes it easy to modify the construction process of the object. This is because
the construction process is encapsulated in the builder class.

# Real World Example

In this example, we will create a builder class to create a car. The car will have a color, model, and year.

```php
<?php
// Product class
class Car {
    private $model;
    private $year;
    private $color;

    public function setModel($model) {
        $this->model = $model;
    }

    public function setYear($year) {
        $this->year = $year;
    }

    public function setColor($color) {
        $this->color = $color;
    }

    public function getInfo() {
        return "Car Model: " . $this->model . ", Year: " . $this->year . ", Color: " . $this->color;
    }
}

// Builder interface
interface CarBuilderInterface {
    public function setModel($model);
    public function setYear($year);
    public function setColor($color);
    public function getResult(): Car;
}

// Concrete Builder
class CarBuilder implements CarBuilderInterface {
    private $car;

    public function __construct() {
        $this->car = new Car();
    }

    public function setModel($model) {
        $this->car->setModel($model);
        return $this;
    }

    public function setYear($year) {
        $this->car->setYear($year);
        return $this;
    }

    public function setColor($color) {
        $this->car->setColor($color);
        return $this;
    }

    public function getResult(): Car {
        return $this->car;
    }
}

// Director
class CarDirector {
    private $builder;

    public function __construct(CarBuilderInterface $builder) {
        $this->builder = $builder;
    }

    public function buildTesla() {
        $this->builder->setModel('Tesla')
                      ->setYear('2022')
                      ->setColor('Black');
    }
}

// Client code
$builder = new CarBuilder();
$director = (new CarDirector($builder))->buildTesla();
$car = $builder->getResult();
echo $car->getInfo(); // Output: "Car Model: Tesla, Year: 2022, Color: Black"
```
