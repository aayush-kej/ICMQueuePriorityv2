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

- add a severityScore to the icms based on the following criteria:
  - If Severity is "1", severityScore is 200000
  - If Severity is "2", severityScore is 100000
  - If Severity is "2.5", severityScore is 75000
  - If Severity is "3", severityScore is 50000