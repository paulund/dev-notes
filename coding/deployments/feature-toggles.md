# Feature Toggles

This method involves deploying the new version to production but not enabling it for all users. Instead, you can enable
the new version for a small percentage of users or for specific users. This way, you can test the new version in
production without affecting all users. If the new version has any issues, you can easily disable it for all users.

This method works well if you have a new feature that you want to test in production without affecting all users. Having
something like a cookie that can set to enable the feature means only selected users will see the new feature.

You can use this as a way of beta testing new features to get feedback from your users.

## Resources

- [Unleash](https://github.com/Unleash/unleash) - Open source feature toggle service.
