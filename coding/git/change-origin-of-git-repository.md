# Change Origin Of Git Repository

Set the new origin

```bash
git remote set-url origin git://{NEW_GIT_REPO_URL}
```

Test the origin has changes

```bash
git remote show origin
```

Push everything to the new origin

```bash
git push --all origin
```
