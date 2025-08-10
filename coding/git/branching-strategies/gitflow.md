# Gitflow

The idea around Gitflow is to try to isolate new development from finished work. This is done by using two main branches:
`master` and `develop`. The `master` branch is used to store the finished work and the `develop` branch is used to store
the new development. When a new feature is ready to be released, a new branch is created from the `develop` branch and
merged back into the `develop` branch when it is finished. When the `develop` branch is ready to be released, it is merged
into the `master` branch.

When working off the `develop` branch, it is common to use feature branches. This is where a new branch is created from the
`develop` branch and merged back into the `develop` branch when it is finished. This way, you can isolate new development
from finished work.

When working off the `master` branch, it is common to use release branches. This is where a new branch is created from the
`master` branch and merged back into the `master` branch when it is finished. This way, you can isolate new releases from
finished work.

If you need to use a hotfix you will do this off the `master` branch and then merged back into the `master` branch when it
is finished. You can also merge the hotfix into the `develop` branch to ensure that the new development is not affected.

The main advantage of this strategy is that it isolates new development from finished work. This can help to reduce the
risk of conflicts and merge issues. The main disadvantage is that it can be complex and difficult to understand. It also
requires a lot of discipline to follow the process correctly.

Any code in the master branch is always deployable. This is a key part of the Gitflow strategy.

## GitLab Flow

This strategy is similar to Gitflow but it uses environment branches such as master, pre production, production.

Deployment can be done from the master branch to the pre production branch and then to the production branch.

This makes it more simple to understand and follow than Gitflow. It also makes it easier to understand the different
environments that are being used. But it isn't very structured and can lead to some messy branches where you need to keep
track of the where to base the pull request from.

## GitHub Flow

The Github flow method is a simplified version of Gitflow. It is a lightweight, branch-based workflow that supports teams
and projects where deployments are made regularly. It is a great strategy for small teams working on small projects.

It has a main branch, usually `master`, and feature branches. When a new feature is ready to be released, a new branch is
created from the `master` branch and merged back into the `master` branch when it is finished. You can then deploy off
`master` when you are ready. This is why it suits projects where deployments are made regularly and the team is small.

In projects that have larger development teams there can be conflicts with working in this method.
