---
name: get-unresolvedICMsv2
description: 'Get a list of ICMs that do not have an owner and are not in the Resolved state'

mcp-servers:
  icm:
    type: local
    command: agency
    args: ["mcp", "icm"]
    tools: ["*"]
---

For the ICM queue "EEE Cloudnet" only get ICMs that are in State (Active or Resolved) and the assignedTo field is empty.

Create a table with
1. ICM ID
2. ICM Age
3. Title
4. Severity 