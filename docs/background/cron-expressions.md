# Cron Expressions

We use CRON expressions setup the timing of our hardware during device configuration using the [Lobaro Maintenance Tool](../tools/lobaro-tool.md) or remotely over the air.

The CRON expressions consists of 6 fields, separated by space: e.g. `0 0/15 * * * *` , which describes time points every 15 minutes starting from minute 0.

<div class="alert alert-primary" role="alert">
  CRON expression allow you define specific POINTS in time they are NOT helpful to define time durations. They can 
  be seen has the start time to trigger an action, for example initiating the sensor measurement or wireless sendout.
  The duration of an action is defined by a separate configuration parameter if needed.
</div>

# The documentation has moved.

* Please find the manual on its new location at [https://doc.lobaro.com](https://doc.lobaro.com/doc/background-articles/cron-expressions)
