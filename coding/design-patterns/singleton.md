# Singleton Design Pattern

Singleton is a design pattern that lets you ensure that a class only has one instance, while also allowing you to have global
access to the single object.

The problem this solves is to ensure that there is only ever one object of this class, why would you want this?

Think of a class that needed to connect to a database, this is the perfect situation for a singleton class. You wouldn't
want or need there for your application to open up multiple connections to your database. You only ever need one connect
to the database and all your queries will be made through this single connection.

Having global access to this single object will ensure that a new object (connection) isn't needed to be created and you can
reuse the existing object with an already open connection to the database.

This is where a singleton pattern will come in allowing you to create a single connection to the database.

There are some cons with this pattern as it can lend itself to bad design if used incorrectly and it can be hard to test
unless you ensure the object is destroyed each time.

# Example Class

```php
<?php

namespace DesignPatterns\Singleton;

/**
 * The Singleton class will have a `GetInstance` method that serves as a way
 * of creating or returning the instance of the object
 */
class Singleton
{
    /**
     * The Singleton's instance is stored in a static field.
     */
    private static $instance = null;

    /**
     * Change the constructor to private to ensure new objects using the `new` keyword can not be done.
     */
    protected function __construct() { }

    /**
     * Singletons should not be cloneable.
     */
    protected function __clone() { }

    /**
     * The getInstance method is used to access the object. On the first entry it will
     * create the object afterwards it will get the instance of the old object
     *
     */
    public static function getInstance(): Singleton
    {
        if (self::$instance === null) {
            self::$instance = new static();
        }

        return self::$instance;
    }

    /**
     * Business logic methods
     */
    public function methods()
    {
    }
}

/**
 * Consume the code
 */
function someCode()
{
    $singleton1 = Singleton::getInstance();
    $singleton2 = Singleton::getInstance();

    if ($singleton1 === $singleton2) {
        echo 'Singleton getInstance returned the same object';
    } else {
        echo 'Singleton getInstance didnt return the same object';
    }
}

someCode();
```
