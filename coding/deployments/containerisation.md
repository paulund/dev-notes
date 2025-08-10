# Containerisation

This method is similar to infrastructure as code depending on the large of your application. It involves packaging your
application and its dependencies into a container image and using automation to deploy it to production. This way, you
can easily replicate your production environment in other environments, such as staging and development. You can also
easily roll back to a previous version of your container image if you need to.

This methods makes it very easy to replicate your development environment in production.

It also makes upgrading your applications system code very easy. For example if you need to upgrade Linux or Nginx you
can do this locally by changing your docker base image, check if your application still works and then deploy to
production.

Ensure that the resources assigned to the container are enough to handle the amount of traffic that the application is
serving.
