# Trunk Based Development

Trunk Based Development is a version control strategy where all developers work on a single branch. This is the simplest
strategy and is best suited for small teams working on small projects. It is also best suited for projects that require
continuous deployment. The main advantage of this strategy is that it is simple and easy to understand. The main
disadvantage is that it can lead to conflicts and merge issues if multiple developers are working on the same codebase.

Common practice is to use feature toggles to hide new features until they are ready to be released. This way, you can
deploy new features to production without affecting all users. If the new feature has any issues, you can easily disable it
for all users.

Pair programming is also a common practice with this strategy. This way, you can ensure that all developers are working on
the same codebase and are aware of any changes that are being made. This can help with code sharing and knowledge sharing
of the different features.
