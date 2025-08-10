# Blue / Green Deployment

This method is a good strategy to choose if you require zero downtime. It involves running two identical production
environments, one of which is live and the other is idle. When you need to deploy a new version of your application, you
can switch the traffic from the live environment to the idle environment. This way, you can test the new version in
production without affecting the live environment. If the new version has any issues, you can easily switch back to the
live environment.

You can even roll this out slowly by switching a small percentage of the traffic to the new environment and gradually
increasing it.

The challenge with this method is that it requires a lot of resources to maintain two identical environments. It also
requires a lot of automation to switch the traffic between the two environments. However, if you require zero downtime,
this is the best strategy to choose.
