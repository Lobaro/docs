# Cron Expressions

We use CRON expressions setup the timing of our hardware during device configuration using the [Lobaro Maintenance Tool](../tools/lobaro-tool) or remotely over the air.

The CRON expressions consists of 6 fields, separated by space: e.g. `* 0/15 * * * *` , which describes time points every 15 minutes starting from minute 0.

Meaning of the fields from left to right:

| | Second  | Minute | Hour | Day of Month | Month of year | Day of Week |
|-|-------- |--------|------|--------------|---------------|-------------|
|Range | `(0-59)`|`(0-59)`|`(0-23)`|`(1-31)`|`(1-12)`|`(1-7)`|          
|Alternative |         |        |        |        | `JAN-DEC` | `SUN-SAT` |
|Allowed special chars | `, - * /`| `, - * /`| `, - * /`| `, - * / ?`| `, - * /` | `, - * /` |

<div class="alert alert-primary" role="alert">
  Note: CRON expression allow you define specific POINTS in time not intervals. They can be seen has the start time to trigger an action, for example
  initiating the sensor measurement or wireless sendout.
</div>

##Examples

##Special Characters:

### star, asterisk (`*`)
Used to select all values within a field. For example, "*" in the minute field means "every minute".
### question mark (`?`) 
Useful when you need to specify something in one of the two fields in which the character is allowed, but not the other. For example, if I want my trigger to fire on a particular day of the month (say, the 10th), but don’t care what day of the week that happens to be, I would put “10” in the day-of-month field, and “?” in the day-of-week field. See the examples below for clarification.
### dash, minus (`-`) 
Used to specify ranges. For example, “10-12” in the hour field means “the hours 10, 11 and 12”.
### comma (`,`) 
Used to specify additional values. For example, “MON,WED,FRI” in the day-of-week field means “the days Monday, Wednesday, and Friday”.
### comma (`/`) 
Used to specify increments. For example, “0/15” in the seconds field means “the seconds 0, 15, 30, and 45”. And “5/15” in the seconds field means “the seconds 5, 20, 35, and 50”. You can also specify ‘/’ after the ‘’ character - in this case ‘’ is equivalent to having ‘0’ before the ‘/’. ‘1/3’ in the day-of-month field means “fire every 3 days starting on the first day of the month”.



##Online CRON Generators and Tester:

* [https://crontab-generator.org/](https://crontab-generator.org/) - "command" need to be set to some random string. The generated CRON has no "seconds".
* [https://crontab.guru/](https://crontab.guru/) - Also without "seconds" field.
* [https://www.freeformatter.com/cron-expression-generator-quartz.html](https://www.freeformatter.com/cron-expression-generator-quartz.html)

##Further Reading
All our CRON expressions are in the same format as the Java Quarz scheduler, 
without the optional "year" field. 
A good documentation including examples can be found here: 

- [http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)
