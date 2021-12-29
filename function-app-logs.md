# Function App Log Queries

## Get Errors in past 5 hours
```
FunctionAppLogs
| where Category startswith "Function."
| where Level == "Error" 
| where TimeGenerated > ago(5h)
| project TimeGenerated, FunctionName, Level, FunctionInvocationId, parse_csv(Message)
| order by TimeGenerated desc, FunctionInvocationId
```
