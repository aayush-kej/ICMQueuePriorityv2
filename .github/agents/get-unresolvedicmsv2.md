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

**Instructions:**

Your job is to pull a list of ICMs using the kusto MCP server. 
These are ICMs that do not have an owner and do not are still not resolved.
Run the below query to get the data:

```
IncidentsSnapshotV2
| where isempty( OwningContactAlias)
| where Status != "RESOLVED"
| where OwningTeamId in (31366)
| distinct IncidentId, CreateDate, Title, Severity, Status, SupportTicketId, Keywords, Tags
| extend Age = datetime_diff('day', now(), CreateDate)
```
---
**GET additional Information:**

  Call the agent get-customerinfo to get the customer information. Pass only the support ticket ids to that agent. 
  Opon receiving the response add the response to the existing table based on the support ticket id field.

  Call the get-icmhops agents to get the number of hops for each incident. Pass the incident ids to that agent.
  Opon receiving the response and add the number of hops to the existing table based on the incident id field.

  Call the get-lastchange agent to get the last changed date and user for each incident. Pass the incident ids to that agent.
  Opon receiving the response add the last changed date and user to the existing table based on the incident id field.
  if the last change date is empty set its value to the createddate

---
**SCORING Criteria:**

    Add a severityScore to the icms based on the following criteria:
    - If Severity is "1", severityScore is 200000
    - If Severity is "2", severityScore is 100000
    - If Severity is "2.5", severityScore is 75000
    - If Severity is "3", severityScore is 50000
    - If Severity is "4", severityScore is 25000

    Add a score911 column
      - If the Tags column contains "icm911" then set this to 75000 otherwise set it to 0

    Based on the ServiceName field create the following columns and set values based on below rule
    - set column scoreOED to 20000 if ServiceName has "O365 ED" else set it to 0
    - set column scoreSfMC to 15000 if ServiceName has "Mission Critical - Unified" else set it to 0
    - set column scoreARR to 10000 if serviceName has "SC | ARR - Unified" else set it to 0

    Add a scoreStatus column
    - If status contains "ACTIVE then set it to 30000 else set it to 0

    Add a scoreCAS column
      - If RestrictedAccessProgramName has "CitizenAllianceSupport" then set it to 15000 else set it to 0

    Add a scorePremier column 
      - If ServiceName has "Unified" or "Unfd" set the score to 5000 
      - else If Servicelevel has "Premier" set the score to 3000 else set it to 0

    Add a scoreStale column and set the value to the number of hours since the last update multiplied by 84 (to give more weight to stale incidents) 

    Add a scoreAged column and set the value to the number of hours since creation multiplied by 50 (to give more weight to older incidents)

    Add a scoreHop column and set the value to the number of hops multiplied by 1000 (to give more weight to incidents that have been bounced around teams)

    Finaly create a totalScore column which is the sum of all the individual score columns.
---
