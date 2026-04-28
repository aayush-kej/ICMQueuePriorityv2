---
name: get-lastchange
description: 'Get the last changed date and by whom'

mcp-servers:
  kusto:
    type: local
    command: agency
    args: 
    - "mcp"
    - "kusto"
    - "--service-uri" 
    - "https://icmcluster.kusto.windows.net"
    - "--database"
    - "IcMDataWarehouse"
    tools: ["*"]
---

You will be provided with a list of ICMs 
Convert them to a kusto list and store them in a variable called targetIncidentIds
Run the below kusto query to get the last change date and user:

```
IncidentHistory
    | where IncidentId in (targetIncidentIds)
    | where ChangeDate >= ago(8d)
    | where ChangedBy != 'gautosvc'
    | summarize arg_max(ChangeDate, IncidentId) by ChangedBy
    | extend ChangedBy = iif(ChangedBy contains "@", tostring(split(ChangedBy, '@')[0]), ChangedBy)
    | summarize LastUpdateDateTime=arg_max(ChangeDate, ChangedBy) by IncidentId
```