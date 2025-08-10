# Simple Factory Design Pattern

A simple factory is an object that is used to generate an instance of an object for the client.

The simplest way of understanding a factory is a object for creating other objects.

## Real World Example

For example if we had 3 types of cars and wanted to create a factory that will create the car classes for us.

We can start with an interface to define a car.

```
<?php

namespace SimpleFactory;

interface Car
{

}
```

Then each car will implement this car interface.

```
<?php

namespace SimpleFactory;

class Tesla implements Car
{

}
```

Then we'll create the factory to make these car classes.

```
<?php

namespace SimpleFactory;

class CarFactory
{
    public function makeTesla() : Car
    {
        return new Tesla;
    }

    public function makeBmw() : Car
    {
        return new Bmw;
    }

    public function makeMercedes() : Car
    {
        return new Mercedes;
    }
}
```

## When To use

This can be very useful when you need a centralised location for creating objects. Let's say these objects need to be
created in a specific way with specific parameters each time the factory pattern allows us to put this into a factory
instead of duplicating the code throughout the application.
