# Reviewing Pull Requests

Reviewing the code of your peers is a critical part of the software development lifecycle. It allows you to ensure that
the code is of high quality, is well-documented, and follows best practices. It also allows you to learn from your peers and
share knowledge with them. This guide will help you understand the best practices for reviewing pull requests.

## 1. Understand the Purpose of the Change

Before you start reviewing the code, it's important to understand the purpose of the change.

- What problem is the change trying to solve?
- What is the expected outcome of the change?
- What are the success criteria for the change?

Understanding the purpose of the change will help you to provide better feedback and ensure that the change meets the requirements.

I've had multiple occasions where fully understanding the outcome required from the tasks changes the technical approach
that's outlined on the ticket. This is because sometimes the people who are writing the requirement or the ticket don't
have the full technical understanding of the system. This is why it's important to understand the purpose of the change so
that you can provide better feedback and ensure that the change meets the requirements. There might already be a config or a
feature flag that meets the requirements of the ticket.

The pull requests may be adding additional features into the change that may not be needed. Additional changes in the
code can add new bugs into an area of the code base that hasn't been asked for. The change should be kept to as small as
possible this makes the change easier to see, review and test.

The success criteria of the change will give you a good indication of what you should be looking for when reviewing the
code. This will also help to see what tests are needed to cover the change, if you see a success criteria has been missed then you
can ask for a test to cover that scenario.

## 2. Design Of The Code

When reviewing the code, it's important to consider the design of the code. There are lots of ways that make good code

### Is the code well-structured?

Every code base will follow a specific structure. It's important to ensure that the code follows the structure of the
code base. This will make it easier for other developers to understand the code and maintain it in the future. If the code doesn't
follow the structure of the code base, it can make it difficult to understand and maintain.

Is the code using good engineering principles and best practices such as SOLID, DRY, KISS, YAGNI, etc?

Can the code be easily understood? If the code can not be understood then it can be hard to test and refactor in the the
future.

### Write smaller changes

There are lots of occasions where code is over engineered that makes it very hard to understand. This can be due to the
use of design patterns that are not needed or the use of complex algorithms that are not needed.

Here's an example something that would be over engineered:

```javascript
function add(a, b) {
  return a + b;
}
```

vs

```javascript
class Addition {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }

  validate() {
    if (typeof this.a !== 'number' || typeof this.b !== 'number') {
      throw new Error('Both inputs must be numbers');
    }
  }

  calculate() {
    this.validate();
    return this.a + this.b;
  }
}

const addition = new Addition(1, 2);
console.log(addition.calculate());
```

If you can do something in an easier way then you should aim to use the simpler version. You don't
win anymore coding points for making the code more complex.

### Is the code easy to maintain?

Similar to the the above the code needs to be easy to maintain, there are multiple ways that can make a code base hard
to maintain. Such as having smaller files that are single responsibility, having good naming conventions, having good documentation,
having good tests, no magic numbers, etc.

### Has testing been considered when structuring the code?

Has the code been structured in a way that makes it easy to test? If the code is hard to test then it can be hard to
refactor in the future. If the code is hard to test then people will not automate the tests of this code, this makes it harder to understand the
intention of the code.

A lot of the time I will use tests during debug to understand the intention of the code as it should show all the acceptance criteria of the code.

Has the code got any dependencies, if so can they be easily mocked? If the code has dependencies that can't be easily
mocked then it can be hard to test or mock the code.

Is the code using any 3rd party services such as calling an API. You need to consider mocking these during your tests.
If you're changed by requests sent to the API then running tests could become very expensive. You also need to mock the API to ensure that
the tests are reliable, you can not rely on the API being up and running.

### Does the change need to be extend?

Having an overall understanding of the end product you'll understand if the change you're working on would
need to be extended in the future. Such as if you're building a provider service where a user can select who the provider can be, you know that in the
future there's the possibility of having multiple providers. This would mean that you would need to build the service in a way that it can be extended in the
future.

If the developer who made the code doesn't have this knowledge of the future of the product then you can provide this
feedback to them.

### Can this be reused by other parts of the code base?

Sometimes when people are working on a feature they will make it very specific to the feature they are working on. This
can be a good thing as it
can make the code easier to understand. But if the code can be reused by other parts of the code base then it should be
made generic. This will
reduce the amount of code that needs to be maintained and will make the code base easier to understand.

For example if you're making a contact us page you might want to provider a Google Map of your location, you could make
a `ContactUsMap` component or you could make a `Map` component that can be reused by other parts of the code base.

Understand whether something could be used in other areas of the codebase will save you a lot of time in the future.

### Would this be easy to debug?

You need to make sure that if you don't understand how to debug this code in the future then you need to make sure that
it's change to make it easier to debug.
Does the code have enough logging in place to help debug any issues in production.

### How can the change be deployed?

Sometimes with complex changes it can be hard to understand how the change can be deployed. You need to understand how
the change can be deployed and if this change will affect the deployment of other parts of the code base. If the change can not be easily deployed
then it can be hard to rollback.

## 3. Code Quality

When reviewing a PR it's important to consider the quality of the code. Here are some things to consider:

- Is the change more complex than it needs to be
- Does the code follow best practices
- Does it follow the coding standards of the project
- Is the code well-documented
- Are there any code smells
- Are there any performance issues
- Does it follow the naming conventions
- Are there any magic numbers
- Are there any hard coded strings
- Can the feature be wrapped in a feature flag
- Are there any configs that can be used

## 4. Test Coverage

It's important the any changes made into a project have been tested properly. When you're developing you don't want to manually test
every change you and make that very we try to focus on automating the tests. This is why it's important to have a good test coverage.

Are there enough tests to cover the change? You need to remember to cover the successful scenarios and the failure scenarios. You
need to consider the edge cases and the happy path and the unhappy path. You need to consider the different types of tests such as
unit tests, integration tests, end to end tests, etc and when you use the right ones.

Are the tests well-written? Are you over using mocking?

If you notice any tests that missing then you can ask the developer to add them in.

## 5. Performance

When checking over the code that you're reviewing you need to consider the performance of the code. A good example of this
is n+1 errors in Laravel eloquent. If you're not familiar with this it's where you're making a query to the database and
then for each result you're making another query to the database. This can be very expensive and slow down the application.

If you're working with large datasets in production but smaller datasets in your development environment these sort of performance
issues can be easily missed. Make sure you're aware of the differences between production and development environments that may
create issues when you deploy.

If you spot these problems then you can ask the developer to fix them.

## 6. Knowledge Sharing

Use reviewing code as a way of not only expanding your knowledge of an area of the codebase you may not be familiar with but
share your knowledge with the developer. If you see something that could be reused from another area of the code base you
can highlight is to the developer.

## 7. Automation

Don't waste you time doing things that can be automated, during a review don't spend time commenting on spaces or tabs or
brackets in the wrong place etc. This can be automated by using a linter, make sure the linter follows the same rules as the project standards.
The linter will mean you don't need to spend time comments on cosmetic changes and you focus on the logic of the change.

Run your tests on pull requests so that when people are reviewing the code they can see if the tests are passing or not. If the tests
have failed then the reviewer doesn't need to spend time on the pull request.

You could even deploy the PR to a staging environment at creation of the pull request where the reviewer and test before approving the change.

## 8. Be Kind When Commenting

Code reviews don't always have to be negative. It's important to be kind and respectful when commenting on the code. If
you see something that could be improved, provide constructive feedback and explain why it could be improved. If you see
something that is done well, provide positive feedback and explain why it is done well. This will help to create a
positive and collaborative environment and will help to build trust and respect between team members.

Maintain a culture or respect and kindness. Feedback should be professional, avoiding personal criticism.

Reviewers should be open to the author's approach, offering alternatives only if necessary, and treating each comment as
a learning opportunity.

- [Predicting Developers’ Negative Feelings about Code Review](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/7ac08fa960dfe10561c1f5953419a0c945279faa.pdf)
- [Destructive Criticism in Software Code Review Impacts Inclusion](https://research.google/pubs/pub51232/)
- [Detecting Interpersonal Conflict in Issues and Code Review: Cross Pollinating Open- and Closed-Source Approaches](https://research.google/pubs/pub51204/)
- [Using research to make code review more equitable](https://developers.googleblog.com/2022/06/Using-research-to-make-code-review-more-equitable.html)

## 9. Continuous improvement over perfection

This is taken from Google code review guidelines. A more seasoned developer may find a less experienced developer’s code
to not be up to their personal standard, but at Google they encourage continuous improvement over perfection. If the you leave the code better than you found it
then you've done a good job.
