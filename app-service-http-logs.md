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

## Get POST requests that caused HTTP 500 errors
```
AppServiceHTTPLogs
| where ScStatus == 500
| where CsMethod == "POST"
| project TimeGenerated, CsUriStem, CsMethod, ScStatus, ScSubStatus, CIp, UserAgent
| order by TimeGenerated desc
```

## Get count of requests for a specific URL with one minute interval
```
AppServiceHTTPLogs
| where CsUriStem == '/my/url/path'
| summarize brew_count = count() by bin(TimeGenerated,1m)
| order by bin(TimeGenerated,1m) desc
```
