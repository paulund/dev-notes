# Handling Datetime In APIs

It's the standard in APIs to use the ISO8601 format for datetime. This standard is widely used and supported by most
programming languages.

## ISO8601

The ISO8601 standard is a way to represent datetime in a string format. It looks like this:

```plaintext
YYYY-MM-DDTHH:MM:SSZ
```

- `YYYY`: Year
- `MM`: Month
- `DD`: Day
- `THH`: Hour
- `MM`: Minute
- `SS`: Second
- `Z`: UTC timezone

This format to represent the UTC timezone for the datetime.

## PHP ISO8601 Problem

The above format is the defined format for many languages like JavaScript, Python, and Ruby. However, PHP doesn't
support this format by default.

A common PHP package to use when handling dates is the Carbon package.

When using Carbon, you can easily convert the ISO8601 format to a Carbon object like this:

```phpswagg
use Carbon\Carbon;

$datetime = Carbon::now()->toIso8601String();
```

This will return the current datetime in the ISO8601 format. Which looks like this `2019-02-01T03:45:27+00:00`.

Notice how the timezone is included in the string as `+00:00`. This is the UTC timezone. If you were to change to
Berlin timezone, it would look like this `2019-02-01T03:45:27+01:00`.

Now this is different to the PHP documentation for the ISO8601 format.
The [PHP documentation](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.iso8601)
states that the format should look like this `2005-08-15T15:52:01+0000`. This is not the same as the Carbon ISO8601
format,
notice the timezone is different.

Both of these are different to the UTC ISO8601 format.

## Rest API DateTime

When designing your Rest API this is important to keep this in mind as you can not use the PHP ISO8601 format of
`2019-02-01T03:45:27+01:00`.
This is because of the `+` sign in the format, if you were to include this in the URL for a `GET` request the `+` sign
would be
converted to a space. This would break the URL.

For this reason it's best to use the UTC ISO8601 format of `2019-02-01T03:45:27Z`. This would mean that your clients
would be required to convert all datetimes to UTC before sending them to the api.

The added benefit of this is that you do not need to convert datetimes to the correct timezone when storing them in the
database. You can just store them as UTC and convert them to the correct timezone when you need to display them.
