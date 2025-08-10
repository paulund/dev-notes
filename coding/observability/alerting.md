# Alerting

Alerting from your applications can only happen after you have the monitoring and the logging setup. You need to have the
monitoring setup inorder to alert on increase use of resource, you need to have the logging setup in order to alert on a
specific error.

With the monitoring you can should be alerting with things like response times, error rates, and resource usage.

With the logging you can setup log metrics and alert on these metrics. For example you can alert on the number of 500
errors in your application. You can have a log for failing payments and then feed this into log metrics and alert when this
goes over a certain threshold within a certain time frame.

You can use tools like PagerDuty, OpsGenie, VictorOps, etc to alert on your application.

If you are using a cloud provider like AWS, Azure, GCP, etc you can use their alerting services to alert on your application.

If you have a 24 hour application then alerting is required in order to have an on-call procedure. Tools like PagerDuty
can be used to alert the on-call person when something goes wrong. PagerDuty will notify the on-call person via SMS, Email
and even a phone call. This will ensure that someone is always available to fix the issue, if you have a ecommerce site
or a 24 hour application then this is a must. But you have to make sure that the logging and monitoring is setup correctly.
