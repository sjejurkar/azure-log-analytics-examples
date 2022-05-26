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

### Join with AppRequest to get HTTP result code and extract specific text from message
```
let AppRequestResults=
AppRequests | extend InvocationId = tostring(Properties.InvocationId)
|distinct ResultCode, InvocationId;
FunctionAppLogs
| join (AppRequestResults) on $left.FunctionInvocationId == $right.InvocationId
| where Category startswith "Function."
| where Message startswith "reqBody="
| project TimeGenerated, ResultCode, Status=extractjson("$.status", replace_string(Message,"messageBody=","")), Message=replace_string(Message,"messageBody=",""),  FunctionName, Level, FunctionInvocationId
| order by TimeGenerated desc, FunctionInvocationId
```
