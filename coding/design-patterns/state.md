# State Design Pattern

The State Design Pattern is a behavioral design pattern that allows an object to change its behavior when its internal
state changes. The pattern encapsulates the states of an object into separate classes and delegates the state-specific
behavior to these classes. This pattern is useful when an object's behavior depends on its state and changes dynamically
based on the state.

## Problem

Consider a scenario where you have a class that represents a traffic light. The traffic light can be in one of three
states: red, yellow, or green. The behavior of the traffic light changes based on its current state. For example, when
the traffic light is red, cars must stop, and when it is green, cars can go. Implementing this behavior using
conditional statements can lead to a complex and hard-to-maintain codebase.

## Solution

The State Design Pattern solves this problem by encapsulating the states of an object into separate classes and
delegating the state-specific behavior to these classes. Each state class represents a specific state of the object and
defines the behavior associated with that state. The context class, which represents the object whose behavior changes
based on its state, maintains a reference to the current state object and delegates the behavior to the state object.

## Structure

The State Design Pattern consists of the following components:

- **Context**: Represents the object whose behavior changes based on its state. It maintains a reference to the current
  state object and delegates the behavior to the state object.
- **State**: Defines an interface for encapsulating the behavior associated with a particular state of the context
  object.
- **ConcreteState**: Implements the behavior associated with a particular state of the context object.

The following UML diagram illustrates the structure of the State Design Pattern:

```plaintext
+-----------------+        +-----------------+
|     Context     |        |      State      |
+-----------------+        +-----------------+
| - state: State  |        | + handle(): void|
| + request(): void|        +-----------------+
+-----------------+
                                   /_\
                           +-----------------+
                           |  ConcreteState  |
                           +-----------------+
                           | + handle(): void|
                           +-----------------+
```

## Example

Let's implement the State Design Pattern using a traffic light example. We will create a `TrafficLight` class that
represents the context object and three concrete state classes: `RedLight`, `YellowLight`, and `GreenLight`. Each
concrete state class will implement the `State` interface and define the behavior associated with the corresponding
state.

State machines can normally be implementmented by using conditional statements like this.

```php
<?php

class TrafficLight
{
    private string $state;

    public function __construct(string $state)
    {
        $this->state = $state;
    }

    public function request(): void
    {
        if ($this->state === 'red') {
            echo "Stop! The light is red.\n";
        } elseif ($this->state === 'yellow') {
            echo "Prepare to stop! The light is yellow.\n";
        } elseif ($this->state === 'green') {
            echo "Go! The light is green.\n";
        }
    }
}

// Usage
$trafficLight = new TrafficLight('red');
$trafficLight->request();

$trafficLight = new TrafficLight('yellow');
$trafficLight->request();

$trafficLight = new TrafficLight('green');
$trafficLight->request();
```

The problem we get when using conditions is that the code becomes hard to maintain and extend. If we want to add a new
state or change the behavior of an existing state, we have to modify the `TrafficLight` class, which violates the
Open/Closed Principle. If we also want to change an existing state we might need to change the strings 'red' in multiple
places in the code base. Having this ecapulated in a class would make it easier to change. Adding a new state would also
be easier as we would just need to create a new class and implement the `State` interface, rather than change
the `TrafficLight` class.

The solution to the above would be to separate the behavior of each state into separate classes and delegate the
behavior to these classes. This is how we can implement the State Design Pattern.

```php
<?php

interface State
{
    public function handle(): void;
}

class RedLight implements State
{
    public function handle(): void
    {
        echo "Stop! The light is red.\n";
    }
}

class YellowLight implements State
{
    public function handle(): void
    {
        echo "Prepare to stop! The light is yellow.\n";
    }
}

class GreenLight implements State
{
    public function handle(): void
    {
        echo "Go! The light is green.\n";
    }
}

class TrafficLight
{
    private State $state;

    public function __construct(State $state)
    {
        $this->state = $state;
    }

    public function setState(State $state): void
    {
        $this->state = $state;
    }

    public function request(): void
    {
        $this->state->handle();
    }
}

// Usage
$trafficLight = new TrafficLight(new RedLight());
$trafficLight->request();

$trafficLight->setState(new YellowLight());
$trafficLight->request();

$trafficLight->setState(new GreenLight());
$trafficLight->request();
```

In this example, we have defined the `State` interface and three concrete state classes: `RedLight`, `YellowLight`,
and `GreenLight`. The `TrafficLight` class represents the context object and maintains a reference to the current state
object. The `request` method delegates the behavior to the state object.

## Pros and Cons

### Pros

- **Simplifies code**: The State Design Pattern simplifies code by encapsulating the behavior associated with each state
  into separate classes.
- **Improves maintainability**: The pattern improves maintainability by allowing you to add new states or change the
  behavior of existing states without modifying the context class.
- **Promotes flexibility**: The pattern promotes flexibility by allowing objects to change their behavior dynamically
  based on their state.
- **Promotes reusability**: The pattern promotes reusability by allowing you to reuse state classes in different
  contexts.
- **Promotes testability**: The pattern promotes testability by allowing you to test state classes independently of the
  context class.
- **Promotes separation of concerns**: The pattern promotes separation of concerns by separating the behavior associated
  with each state into separate classes.

### Cons

- **Increases complexity**: The State Design Pattern can increase complexity by introducing multiple classes to
  represent different states.
- **May lead to a large number of classes**: The pattern may lead to a large number of classes if there are many states
  and transitions between states.
- **May lead to tight coupling**: The pattern may lead to tight coupling between the context class and state classes if
  not implemented correctly.
- **May lead to performance overhead**: The pattern may lead to performance overhead due to the dynamic dispatch of
  method calls to state objects.
