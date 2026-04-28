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
| distinct IncidentId, CreateDate, Title, Severity, Status, SupportTicketId, Keywords
| extend Age = datetime_diff('day', now(), CreateDate)
```
For the below tasks do not include ICMS that do not have a SupportTicket ID

  Call the agent get-customerinfo to get the customer information. Pass only the support ticket ids to that agent. 
  Opon receiving the response add the response to the existing table based on the support ticket id field.

  Call the get-icmhops agents to get the number of hops for each incident. Pass the incident ids to that agent.
  Opon receiving the response and add the number of hops to the existing table based on the incident id field.

  Call the get-lastchange agent to get the last changed date and user for each incident. Pass the incident ids to that agent.
  Opon receiving the response add the last changed date and user to the existing table based on the incident id field.

  Add a severityScore to the icms based on the following criteria:
  - If Severity is "1", severityScore is 200000
  - If Severity is "2", severityScore is 100000
  - If Severity is "2.5", severityScore is 75000
  - If Severity is "3", severityScore is 50000
  - If Severity is "4", severityScore is 25000

Create two tables in the end. One with ICMs that has support ticket id and the other one with ICMs that do not have a support ticket id. 
Only the first table should have the additional information. 

