---
name: get-icmhops
description: 'Get the number of times an incident has moved between teams'

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
Run the below kusto query to get the number of hops for each incident:

```
IncidentHistory
    | where IncidentId in (targetIncidentIds)
    | where ChangeDescription == "Transferred incident"
    | where isnotempty(OwningTeamName)
    | summarize AllOwningTeams=make_set(OwningTeamName), HopCount=count() by IncidentId
```