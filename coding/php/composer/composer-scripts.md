# Simple Composer Scripts

Use Composer scripts to avoid long commands and to run lint + tests together.

```jsonc
{
  "scripts": {
    "lint": "phpcs src",
    "test": "phpunit",
    "check": ["@lint", "@test"]
  }
}
```

## Usage

```
composer lint   # code style check
composer test   # run tests
composer check  # run lint then tests
```
