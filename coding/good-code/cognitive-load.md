# Cognitive Load

```
Cognitive load is how much a developer needs to think im order to complete a task.
```

When you're reading code in a codebase you will need to take into account many things in your head including:

- Variables
- Methods
- Classes
- Logic flow

The average person can only hold 7±2 items in their working memory at once. Once your code reaches this many points it
can
be hard to understand.

This is why it's important to keep your code simple and easy to understand.

## What Can Lead To Cognitive Load In Code

Cognitive load refers to the amount of mental effort required to process information, make decisions, and complete
tasks. It’s a concept rooted in cognitive psychology, often used to understand how people learn and perform complex
tasks. In programming, cognitive load affects how well you can:

- Understand code (yours or someone else’s).
- Debug and fix issues.
- Design scalable solutions.
- Balance multiple tasks like writing code, testing, and documentation.

For example if you have a condition that looks like this:

```php
if ($user->role === 1 && $user->status === 1 || $user->role === 2 && $user->status === 1) {
    // Do something
}
```

The problem with this condition is that you need to understand what role 1 and 2 is and you need to understand what
status 1 is.

You can improve this to make it easier to understand by defining methods for what you need to check

```php
if ($user->isAdmin() || $user->isModerator()) {
    // Do something
}
```

Inside these methods you will do the checks for the role and status and return a boolean. Just by reading this method
you can understand what the condition is checking. Also you can look inside this `isAdmin` method to see `role === 1`
and you will understand that role 1 is for an admin user.

## Types Of Cognitive Load

There are three main types of cognitive load:

- *Intrinsic Load*: The inherent difficulty of the task. For example, learning PHP’s syntax and basic constructs is
  easier
  than mastering advanced concepts like dependency injection or creating custom frameworks.
- *Extraneous Load*: Unnecessary distractions or complexities that make a task harder. Poorly written code, unclear
  documentation, or an over-complicated IDE setup contribute to this.
- *Germane Load*: The mental effort directed toward understanding and solving problems. Writing algorithms or
  implementing
  design patterns typically falls into this category.

## The Impact of Cognitive Load on Coding

1. Mental Fatigue

High cognitive load can lead to mental fatigue, reducing your ability to focus and increasing the likelihood of errors.
For example, juggling multiple PHP frameworks in a single project might overwhelm your mental capacity.

2. Decreased Code Quality

When cognitive load is high, you’re more prone to cutting corners or missing edge cases. This can result in:

- Spaghetti code.
- Poorly named variables.
- Functions that do too much.

3. Slower Debugging

Debugging requires attention to detail and logical reasoning. If your cognitive load is maxed out, tracing a bug through
a complex codebase can feel like finding a needle in a haystack.

4. Hindered Learning

When learning new PHP tools, libraries, or best practices, high cognitive load can slow your progress. For instance,
trying to learn Laravel while simultaneously building a production application can be overwhelming.

## Strategies to Reduce Cognitive Load

1. Write Clean and Simple Code

Adopt coding standards like PSR (PHP Standards Recommendations) to maintain consistency. Avoid overengineering;
simplicity is key to reducing intrinsic and extraneous loads.

Here’s an example:

Before:

```php
function processData($data) {
    if (isset($data["name"])) {
        $name = $data["name"];
    } else {
        $name = "Unknown";
    }

    if (isset($data["age"])) {
        $age = $data["age"];
    } else {
        $age = 0;
    }

    return ["name" => $name, "age" => $age];
}
```

After:

```php
function processData($data) {
    $name = $data["name"] ?? "Unknown";
    $age = $data["age"] ?? 0;

    return ["name" => $name, "age" => $age];
}
```

2. Use Tools and Automation

- Linting Tools: Automatically catch syntax and styling issues.
- IDEs: Modern IDEs like PHPStorm provide intelligent suggestions and refactoring tools.
- Code Snippets: Reuse boilerplate code to save mental effort.

3. Break Tasks into Smaller Steps

Decompose complex features into smaller, manageable tasks. For instance, building an API endpoint can be divided into
defining routes, creating controllers, and testing.

4. Refactor Regularly

Refactor code to reduce complexity. Break large functions into smaller ones and replace magic numbers with constants.

Example:

```php
function calculateDiscount($price) {
    $discountRate = 0.1; // Magic number
    return $price - ($price * $discountRate);
}
```

Refactored:

```php
const DISCOUNT_RATE = 0.1;

function calculateDiscount($price) {
    return $price - ($price * DISCOUNT_RATE);
}
```

5. Prioritize Documentation

Write clear, concise comments and maintain up-to-date documentation. This reduces the cognitive load for you and others
revisiting the code later.

6. Limit Multitasking

Focus on one task at a time to avoid spreading your mental resources too thin.

7. Invest in Learning

Dedicate time to learning PHP fundamentals and advanced concepts. A strong foundation reduces intrinsic cognitive load.

## Final Thoughts

Cognitive load management is an often-overlooked aspect of software development. By understanding its impact and
adopting strategies to reduce it, PHP developers can improve their efficiency, write cleaner code, and enjoy a more
satisfying coding experience.

Taking small, deliberate steps to minimize cognitive load can have a profound impact on your career. Start with one or
two strategies today and see how they transform your workflow.
