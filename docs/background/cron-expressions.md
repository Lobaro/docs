# Cron Expressions

We use CRON expressions setup the timing of our hardware during device configuration using the [Lobaro Maintenance Tool](/tools/lobaro-tool/) or remotely over the air.

The CRON expressions consists of 6 fields, separated by space: e.g. `0 0/15 * * * *` , which describes time points every 15 minutes starting from minute 0.

<div class="alert alert-primary" role="alert">
  CRON expression allow you define specific POINTS in time they are NOT helpful to define time durations. They can 
  be seen has the start time to trigger an action, for example initiating the sensor measurement or wireless sendout.
  The duration of an action is defined by a separate configuration parameter if needed.
</div>

##Meaning of the six fields:

`0 0/15 * * * *` 

| | Second  | Minute | Hour | Day of Month | Month of year | Day of Week |
|-|-------- |--------|------|--------------|---------------|-------------|
|Range | `(0-59)`|`(0-59)`|`(0-23)`|`(1-31)`|`(1-12)`|`(1-7)`|          
|Alternative |         |        |        |        | `JAN-DEC` | `SUN-SAT` |
|Allowed special chars | `, - * /`| `, - * /`| `, - * /`| `, - * / ?`| `, - * /` | `, - * /` |


##Examples

| Cron definition | Description  | Trigger time point(s) (hh:mm:ss) |
|-----------------|--------------|-------------|
|`0 5 * * * *`    |Hourly at minute 5, second 0 | 00:05:00, 01:05:00, 02:05:00... |
|`0 1/10 * * * *` |Every 10 minutes starting from minute 1, second 0 | 00:01:00, 00:11:00, 00:21:00, [...], 01:01:00, [...] |
|`0 0 6 * * *`    | Daily on hour 6, minute 0, second 0 | 06:00:00 |
|`0 0 13 1,15 * *`    | Hour 13, minute 0, second 0 on day 1 and 15 | 13:00:00 at 1st and 15th of month |
|`0 15 9 1-5 * *`    | Hour 9, minute 15, second 0 on day 1 to 5 | 09:15:00 at 1st to 5th of month |

!!! note
    Some Lobaro nodes do not keep the __real time__ internally since for many sensor applications it is enough to configure 
    repetition intervals. For example it may be a important configuration parameter that a specific sensor data gets 
    transmitted every 15 minutes, but it does not matter if the send out takes place on [12:00h,12:15h...] or [12:05h,
    12:20h...]. Times are __relative__ to the random time when the device (re)starts or the batteries are inserted. 
    If needed by your target application Lobaro can deliver on request special firmware support for keeping data 
    acquisition intervals based on a real time clock (RTC) which stays in sync with the real time on your wrist. 

##Special Characters:

### star, asterisk (`*`)
Used to select all values within a field. For example, "*" in the minute field means "every minute".
### question mark (`?`) 
Useful when you need to specify something in one of the two fields in which the character is allowed, but not the other. For example, if I want my trigger to fire on a particular day of the month (say, the 10th), but don’t care what day of the week that happens to be, I would put “10” in the day-of-month field, and “?” in the day-of-week field. See the examples below for clarification.
### dash, minus (`-`) 
Used to specify ranges. For example, “10-12” in the hour field means “the hours 10, 11 and 12”.
### comma (`,`) 
Used to specify additional values. For example, “MON,WED,FRI” in the day-of-week field means “the days Monday, Wednesday, and Friday”.
### slash (`/`) 
Used to specify increments. For example, “0/15” in the seconds field means “the seconds 0, 15, 30, and 45”. And “5/15” in the seconds field means “the seconds 5, 20, 35, and 50”. You can also specify ‘/’ after the ‘’ character - in this case ‘’ is equivalent to having ‘0’ before the ‘/’. “1/3” in the day-of-month field means “fire every 3 days starting on the first day of the month”.


##Online CRON Generators and Tester:

* [https://crontab-generator.org/](https://crontab-generator.org/) - "command" need to be set to some random string. The generated CRON has no "seconds".
* [https://crontab.guru/](https://crontab.guru/) - Also without "seconds" field.
* [https://www.freeformatter.com/cron-expression-generator-quartz.html](https://www.freeformatter.com/cron-expression-generator-quartz.html)

##Further Reading
All our CRON expressions are in the same format as the Java Quarz scheduler, 
without the optional "year" field. 
A good documentation including examples can be found here: 

- [http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)
