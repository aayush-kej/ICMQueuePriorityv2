---
name: get-unresolvedICMsv2
description: 'Get a list of ICMs that do not have an owner and are not in the Resolved state'

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

Your job is to pull a list of ICMs using the kusto MCP server. 
These are ICMs that do not have an owner and do not are still not resolved.
Run the below query to get the data:

```
IncidentsSnapshotV2
| where isempty( OwningContactAlias)
| where Status != "RESOLVED"
| where OwningTeamId in (31366)
| distinct IncidentId, CreateDate, Title, Severity, Status, SupportTicketId
| extend Age = datetime_diff('day', now(), CreateDate)
```

Summarize the output in a table.