---
name: get-customerinfo-IncidentID
description: 'Gets customer information for a list of provided incident IDs (support ticket ids) using the Kusto MCP server.'

mcp-servers:
  kusto:
    type: local
    command: agency
    args: 
    - "mcp"
    - "kusto"
    - "--service-uri" 
    - "https://supportrptwus3prod.westus3.kusto.windows.net"
    - "--database"
    - "KPISupportData"
    tools: ["*"]
---

- you will be provided with a list of incident IDs
- Save the incidentids as a ksuto list in a variable called targetIncidentIds
- Run the below query to get the customer information for the provided incident IDs:

```
AllCloudsSupportIncidentWithReferenceModelVNext
| where IncidentId in (targetIncidentIds) 
| project SupportTicketId=IncidentId, ServiceName, Servicelevel, Customer_CloudCustomerName, Customer_Strategic400Flag, Customer_TPID, SupportProductName, RestrictedAccessProgramName
```

