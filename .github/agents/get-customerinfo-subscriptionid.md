.md

---
name: get-customerinfo-subscriptionid
description: 'Gets customer information for a list of provided Subscription IDs  using the Kusto MCP server.'

mcp-servers:
  kusto:
    type: local
    command: agency
    args: 
    - "mcp"
    - "kusto"
    - "--service-uri" 
    - "https://customerdomrptwus3prod.westus3.kusto.windows.net"
    - "--database"
    - "customerdomdata"
    tools: ["*"]
---

- you will be provided with a list of Subscription Ids
- Save the Subscription ids as a ksuto list in a variable called targetSubscriptionIds
- Run the below query to get the customer information for the provided subscription IDs:

```
AllCloudsSupportIncidentWithReferenceModelVNext
Product360CustomerSubscriptions
| where SubscriptionId in~ (targetSubscriptionIds)
| project SubscriptionId, IsS500FromSubId=Strategic400Flag, TPIDFromSubId=TPID
```

