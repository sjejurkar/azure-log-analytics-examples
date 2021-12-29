# Azure DB Diagnostics Queries

## Get all DB authentication events in last 24 hours
```
AzureDiagnostics
| where action_name_s  startswith "DATABASE AUTHENTICATION" 
| project TimeGenerated, LogicalServerName_s, application_name_s, session_id_d, session_server_principal_name_s, event_time_t, 
Category , OperationName, action_name_s, client_ip_s 
| where TimeGenerated > ago(24h) 
| order by event_time_t  asc 
| take 100
```

## Get all DB logins that failed in last 24 hours
```
AzureDiagnostics
| where action_name_s  startswith "DATABASE AUTHENTICATION FAILED" 
| where TimeGenerated > ago(24h) 
| order by event_time_t  asc 
```

