# App Service HTTP Log Queries
## Get daily average response time and count by URL
```
AppServiceHTTPLogs
| summarize AvgTT = avg(TimeTaken), CountReq = count() by Day = format_datetime(TimeGenerated, "yyyy-MM-dd"), URL= CsUriStem
| where AvgTT > 1000
| where CountReq > 5
| project Day, URL, AvgTT, CountReq
| order by Day asc, URL asc
```
