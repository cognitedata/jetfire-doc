# Schedule
![schedule](schedule.png)

Once a Schedule is added, the corresponding Recipe and Configuration will become read-only (except for the Recipe name), in order to prevent any unintentional changes that may affect the scheduled future Jobs. 

Note that incremental load is not automatically turned on for scheduled transformations, which means the full data set will be transformed in each iteration.

## Cron Format
Jetfire Schedules should be written in Cron format consisting of five fields: 

`<Minute> <Hour> <Day_of_the_Month> <Month_of_the_Year> <Day_of_the_Week>`

[Learn more](https://en.wikipedia.org/wiki/Cron)

## HTTP API
#### Step-by-Step instructions
* Create the transform you want to schedule and run it once with the correct destination etc. (These settings will be stored and used when scheduled)
* Write down the ID of your transform ![](03_transform_url.png)
* Send a POST request in this format, where interval is the schedule in cron format with 5 vars.
```sh
curl -X POST \
https://jetfire.cogniteapp.com/api/schedule/config/{{ID of your transformation}}\
-H 'Content-Type: application/json' \
-H 'api-key: {{api-key}}' \
-H 'cache-control: no-cache' \
-d '{ "interval": "* * * * *" }'
```
Real world example:
```sh
curl -X POST \
  https://jetfire.cogniteapp.com/api/schedule/config/92 \
  -H 'Content-Type: application/json' \
  -H 'api-key: WfHzzHnmIKIZgnMkujhggbNPJkmjkHjmkHbHhj' \
  -H 'cache-control: no-cache' \
  -d '{ "interval": "0 0 * * *" }'
```

Available calls:

* GET -> https://jetfire.cogniteapp.com/api/schedule/config/{{ID of your transformation}}
Get information about a scheduled config


* POST -> https://jetfire.cogniteapp.com/api/schedule/config/{{ID of your transformation}}
Create a new schedule for the config (only one schedule is supported per config). The body should be
{ "interval": "* * * * *" // Cron formatted interval }


* DELETE -> https://jetfire.cogniteapp.com/api/schedule/config/{{ID of your transformation}}
Delete a schedule
