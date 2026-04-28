---
name: get-unackedIcms
description: 'Get a list of ICMs that have not been acknowledged'

mcp-servers:
  ado:
    type: local
    command: agency
    args: ["mcp", "ado"]
    tools: ["*"]
---

For the ICM queue "EEE Cloudnet" pull all the icms in the queue using the mcp server icm , that matches the condition:
1. has no owner and 
2. is not in the Resolved State


Create a table with
1. ICM ID
2. ICM Age
3. Title
4. Severity 