# The Twelve Factors App

The Twelve-Factor App methodology is a set of best practices for building modern, scalable, and maintainable software
applications. It was created by [Heroku](https://www.heroku.com/) co-founder Adam Wiggins in 2011 and has since become a
widely adopted standard for developing cloud-native applications.

The Twelve-Factor App methodology is designed to help developers build applications that are easy to deploy, scale, and
maintain. It provides a set of guidelines that cover everything from codebase organization to deployment strategies. By
following these best practices, developers can create applications that are more reliable, scalable, and maintainable.

## The Twelve Factors

- Codebase
- Dependencies
- Config
- Backing Services
- Build, release, run
- Processes
- Port binding
- Concurrency
- Disposability
- Dev/prod parity
- Logs
- Admin processes

## 1. Codebase

Have one place that is the source of truth for your codebase. This is typically a version control system like Git.

The code versioned in the repository should be the same code that is running in production. With features that will be
deployed to other environments.

## 2. Dependencies

Ensure all dependencies are declared and isolated. This means that all dependencies should be explicitly declared and
managed in a way that is isolated from other dependencies. This can be done using a package manager like npm or pip or
composer.

## 3. Config

Ensure that your applications config can be changed by enivronment. This means that your application should read its
configuration from environment variables rather than hardcoding them in the codebase.

## 4. Backing Services

Use other services as backing services. This means that your application should treat services like databases, caches,
and message queues as attached resources that can be swapped out easily.

## 5. Build, release, run

Separate the build, release, and run stages of your application. This means that your application should have a clear
separation between the build process, the release process, and the run process.

## 6. Processes

Run your application as one or more stateless processes. This means that your application should be designed to run as
one or more stateless processes that can be easily scaled up or down.

This is important in cloud-native applications because it allows you to easily scale your application horizontally by
adding more instances of your application.

## 7. Port binding

Expose services via port binding. This means that your application should expose services via a port binding mechanism
that allows other services to connect to it.

## 8. Concurrency

Scale out via the process model. This means that your application should be designed to scale out by adding more
instances of your application rather than scaling up by adding more resources to a single instance.

The ability to add async workers to your application is also important for handling background tasks.

## 9. Disposability

Maximize robustness with fast startup and graceful shutdown. This means that your application should be designed to
start up quickly and shut down gracefully.

Again, this is important in cloud-native applications because it allows you to easily scale your application up or down
without causing downtime.

## 10. Dev/prod parity

Keep development, staging, and production as similar as possible. This means that your application should be designed to
run in the same way in development, staging, and production environments.

Docker containers are a great way to achieve this because they allow you to package your application and its
dependencies in a consistent way.

## 11. Logs

Treat logs as event streams that can be easily aggregated and analyzed. Ensure that you can run queries on your logs to
identify issues and trends.

## 12. Admin processes

Run admin/management tasks as one-off processes that can be easily run and then shut down.

By following the Twelve-Factor App methodology, developers can create applications that are more reliable, scalable, and
maintainable. This methodology provides a set of best practices that cover everything from codebase organization to
deployment strategies.
