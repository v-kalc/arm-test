The Diversity and Inclusion app logs telemetry to [Azure Application Insights](https://azure.microsoft.com/en-us/services/monitor/). You can go to the Application Insights blade of the Azure App Service to view basic telemetry about your services, such as requests, failures, and dependency errors.

The Teams Bot integrates with Application Insights to gather bot activity analytics, as described [here](https://blog.botframework.com/2019/03/21/bot-analytics-behind-the-scenes/).

The app logs a few kinds of events:

`Trace` logs keeps the track of application events.

`Exceptions` logs keeps the records of exceptions tracked in the application.

*Application Insights queries:*

- This query gives total number of unique users of bot.
```
customEvents
| extend User = tostring(customDimensions.userId)
| summarize dcount(User)
```

- This query gives number of users creating employee resource groups.
```
customEvents
| extend User = tostring(customDimensions.userId)
| project name, User
| where name == "Resource group - HTTP post call succeeded"
| summarize count() by User
```

- This query gives number of  employee resource groups created per week/month.
```
customEvents
| project name, timestamp
| where name contains "Resource group - HTTP post call succeeded" and (timestamp >= datetime('<<Start date time>>') and timestamp <= datetime('<<End date time>>'))
| summarize count() by name
```

- This query gives total number of feedback shared per week/month.
```
customEvents
| project name, timestamp
| where name contains "Feedback submitted successfully" and (timestamp >= datetime('<<Start date time>>') and timestamp <= datetime('<<End date time>>'))
| summarize count() by name
```

- This query gives total number of pair up notification card sent per week/month.
```
customEvents
| project name, timestamp
| where name contains "Pairup notification card sent successfully" and (timestamp >= datetime('<<Start date time>>') and timestamp <= datetime('<<End date time>>'))
| summarize count() by name
```

- This query gives average response time of requests.
```
requests
| summarize avg(duration)
