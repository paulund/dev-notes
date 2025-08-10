# Logging

# Logging in Your Application

Logging is an essential practice in software development. Without proper logging, debugging issues in production can be
incredibly difficult. Logs provide valuable insights into your application's behavior, helping you diagnose problems,
monitor performance, and maintain security.

You should be logging:

- **Errors & Warnings**: Capturing unexpected failures or issues.
- **Informational Messages**: Events like user logins, background job completions, or system events.
- **Request & Response Data**: HTTP request details, execution times, and response payloads.
- **Resource Usage**: Memory, CPU, database queries, and cache hits/misses.

A good logging strategy ensures that you can quickly find relevant information when troubleshooting issues.

## Choosing a Logging Solution

There are many tools available for logging, including:

- [ELK Stack (Elasticsearch, Logstash, Kibana)](https://www.elastic.co/what-is/elk-stack)
- [Splunk](https://www.splunk.com/)
- [Sumo Logic](https://www.sumologic.com/)
- [Grafana Loki](https://grafana.com/oss/loki/)
- Cloud-native logging solutions such as AWS CloudWatch, Azure Monitor, and Google Cloud Logging.

## Logging Frugality

When logging, be mindful of the volume of logs generated. Excessive logging can lead to performance issues and increased
storage costs. Here are some tips to log efficiently:
- **Log Levels**: Use different log levels (debug, info, warning, error) to categorize logs. This allows you to filter
  logs based on severity.
- **Structured Logging**: Use structured logging formats (like JSON) to make it easier to parse and analyze logs.
- **Avoid Logging Sensitive Data**: Be cautious about logging sensitive information like passwords, credit card numbers,
  or personally identifiable information (PII).
- **Do not log huge objects**: Avoid logging large objects or data structures that can bloat your logs.
- **Pick attributes wisely**: Log only the attributes that are necessary for debugging or monitoring. Too many attributes can make
  logs hard to read and analyze.

## Log Uniqueness

When logging, ensure that each log entry is unique. TThis will ensure that you know exactly where the log entry came from and what
  it relates to. Ensure that you are not including variables in the log message for example

```php
Log::info('User logged in ' $user->id);
```

This is not unique and will make it hard to find in your code where the log entry was created. Instead you should be using
  the following format

```php
Log::info('User logged in', ['user_id' => $user->id]);
```

This way you can search for the whole message and see exactly where in the code this log entry was created.

## Logging in Laravel

If you're using Laravel, logging is made easy with the built-in Log facade, which supports multiple logging channels via
**Monolog**.

Here’s an example of logging different levels of messages:

```php
use Illuminate\Support\Facades\Log;

Log::debug('This is a debug message');
Log::info('This is an informational message');
Log::warning('This is a warning message');
Log::error('This is an error message');
Log::critical('This is a critical message');
```

Laravel can write logs to files, databases, syslog, or external log management services.

## Configuring Log Channels in Laravel

In your `config/logging.php` file, you can define different log channels:

```
'channels' => [
    'stack' => [
        'driver' => 'stack',
        'channels' => ['single', 'slack'],
    ],
    'single' => [
        'driver' => 'single',
        'path' => storage_path('logs/laravel.log'),
        'level' => 'debug',
    ],
    'slack' => [
        'driver' => 'slack',
        'url' => env('LOG_SLACK_WEBHOOK_URL'),
        'username' => 'Laravel Logs',
        'emoji' => ':boom:',
        'level' => 'critical',
    ],
],
```

## Log Storage with Loki

If you're looking for a scalable, efficient way to store and query logs, Loki is an excellent choice. Loki is a log
aggregation system built by Grafana, optimized for storing logs efficiently and integrating seamlessly with Grafana
dashboards.

## Why Use Loki?
- Log Aggregation: Collect logs from multiple sources.
- Efficient Storage: Loki indexes logs based on labels rather than full text, making it more cost-effective.
- Powerful Search: Query logs using LogQL, a Prometheus-inspired query language.
- Alerting & Monitoring: Set up alerts when specific log patterns appear.
- Grafana Integration: Visualize logs alongside metrics in Grafana.

## Setting Up Loki
Run Loki using Docker:

```bash
docker run -d --name=loki -p 3100:3100 grafana/loki:latest
```

Set up Promtail to send logs to Loki:
```bash
docker run -d --name=promtail -v /var/log:/var/log grafana/promtail:latest -config.file=/etc/promtail/config.yml
```

Configure your application to send logs to Loki.

## Querying Logs in Loki
You can search logs using LogQL in Grafana. Example queries:

Fetch all logs for a specific service:
```bash
{job="my-app"}
```

Find errors in logs:

```bash
{job="my-app"} |= "error"
```

Filter logs by a specific time range:

```bash
{job="my-app"} | logfmt | duration > 100ms
```

## Metrics and Logging
In addition to logging, metrics provide insights into application health. While logs help debug issues, metrics allow
you to track performance trends.

For metrics, Prometheus is a great choice:

- Collects system and application metrics.
- Stores time-series data.
- Integrates with Grafana for visualization.
-
Example: Exporting Laravel metrics to Prometheus:

```php

use Prometheus\CollectorRegistry;
use Prometheus\Storage\InMemory;

$registry = new CollectorRegistry(new InMemory());
$counter = $registry->getOrRegisterCounter('app', 'requests_total', 'Total Requests');
$counter->inc();
```

## Final Thoughts
Logging is a fundamental part of software development and system observability. With a proper logging strategy, you can
quickly diagnose issues, monitor performance, and improve application reliability.

If you're looking for a powerful log storage solution, Grafana Loki is an excellent choice, offering scalability, cost
efficiency, and seamless Grafana integration.

Start logging today to ensure your application runs smoothly in production!
