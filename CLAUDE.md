# n8n Workflow Generator - Claude Instructions

## Project Overview

This project enables users to describe automation workflows in natural language, and Claude will automatically generate corresponding n8n workflow JSON files. Users can simply explain what they want to automate, and receive a ready-to-import n8n workflow.

## n8n Background

n8n is an open-source workflow automation platform that allows technical teams to build complex automations. Key features:
- **400+ integrations** including Gmail, Slack, Google Sheets, HTTP requests, databases, and more
- **Visual workflow editor** with node-based interface
- **Code capabilities** for custom logic and transformations
- **Self-hostable** or cloud deployment options
- **Fair-code licensed** with source available

### Core Concepts

1. **Nodes**: Building blocks of workflows
   - **Regular nodes**: Execute during workflow (e.g., HTTP Request, Set, Code)
   - **Trigger nodes**: Start workflows (e.g., Webhook, Cron, Email trigger)
   - **Webhook nodes**: Receive external calls

2. **Workflows**: Collections of connected nodes that define automation logic

3. **Connections**: Define data flow between nodes

4. **Credentials**: Stored authentication for services

## Workflow JSON Structure

n8n workflows are stored as JSON with this basic structure:

```json
{
  "name": "Workflow Name",
  "nodes": [
    {
      "parameters": {
        // Node-specific parameters
      },
      "id": "unique-id",
      "name": "Node Name",
      "type": "n8n-nodes-base.nodeType",
      "typeVersion": 1,
      "position": [x, y]
    }
  ],
  "connections": {
    "Node Name": {
      "main": [
        [
          {
            "node": "Target Node Name",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {},
  "versionId": "unique-version-id",
  "id": "workflow-id",
  "tags": []
}
```

## Instructions for Generating Workflows

When a user describes an automation need:

1. **Parse Requirements**
   - Identify data sources (Gmail, databases, APIs)
   - Determine actions needed (send, transform, store)
   - Note any conditions or scheduling requirements

2. **Select Appropriate Nodes**
   - Match user requirements to n8n nodes
   - Common nodes:
     - `n8n-nodes-base.gmail` - Gmail operations
     - `n8n-nodes-base.slack` - Slack messaging
     - `n8n-nodes-base.googleSheets` - Google Sheets operations
     - `n8n-nodes-base.httpRequest` - API calls
     - `n8n-nodes-base.code` - Custom JavaScript
     - `n8n-nodes-base.set` - Data transformation
     - `n8n-nodes-base.if` - Conditional logic
     - `n8n-nodes-base.cron` - Scheduled triggers
     - `n8n-nodes-base.webhook` - External triggers

3. **Design Data Flow**
   - Start with trigger node if scheduled/event-based
   - Add processing nodes in logical sequence
   - Include error handling where appropriate

4. **Generate Node Positions**
   - Start at [250, 300]
   - Space nodes 280 units horizontally
   - Use vertical spacing for parallel branches

5. **Create Connections**
   - Link nodes based on data flow
   - Support multiple outputs for branching logic

## Common Automation Patterns

### 1. Email to Messaging
**Description**: "Read unread emails and send summaries to Slack"
```json
{
  "nodes": [
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMinute"
            }
          ]
        }
      },
      "id": "gmail-trigger",
      "name": "Gmail Trigger",
      "type": "n8n-nodes-base.gmailTrigger",
      "position": [250, 300]
    },
    {
      "parameters": {
        "channel": "#general",
        "text": "=New Email from {{$json.from}}: {{$json.subject}}\n\n{{$json.snippet}}"
      },
      "id": "slack-node",
      "name": "Send to Slack",
      "type": "n8n-nodes-base.slack",
      "position": [530, 300]
    }
  ],
  "connections": {
    "Gmail Trigger": {
      "main": [[{"node": "Send to Slack", "type": "main", "index": 0}]]
    }
  }
}
```

### 2. Scheduled Data Collection
**Description**: "Every day at 9 AM, fetch data from API and save to Google Sheets"
```json
{
  "nodes": [
    {
      "parameters": {
        "triggerTimes": {
          "item": [
            {
              "hour": 9,
              "minute": 0
            }
          ]
        }
      },
      "id": "cron-trigger",
      "name": "Daily at 9 AM",
      "type": "n8n-nodes-base.cron",
      "position": [250, 300]
    },
    {
      "parameters": {
        "url": "https://api.example.com/data",
        "method": "GET"
      },
      "id": "http-request",
      "name": "Fetch API Data",
      "type": "n8n-nodes-base.httpRequest",
      "position": [530, 300]
    },
    {
      "parameters": {
        "operation": "append",
        "sheetId": "sheet-id",
        "range": "A:Z"
      },
      "id": "google-sheets",
      "name": "Save to Sheets",
      "type": "n8n-nodes-base.googleSheets",
      "position": [810, 300]
    }
  ],
  "connections": {
    "Daily at 9 AM": {
      "main": [[{"node": "Fetch API Data", "type": "main", "index": 0}]]
    },
    "Fetch API Data": {
      "main": [[{"node": "Save to Sheets", "type": "main", "index": 0}]]
    }
  }
}
```

### 3. Conditional Processing
**Description**: "Process form submissions, send to different channels based on urgency"
```json
{
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "form-webhook"
      },
      "id": "webhook",
      "name": "Form Webhook",
      "type": "n8n-nodes-base.webhook",
      "position": [250, 300]
    },
    {
      "parameters": {
        "conditions": {
          "string": [
            {
              "value1": "={{$json.urgency}}",
              "operation": "equals",
              "value2": "high"
            }
          ]
        }
      },
      "id": "if-node",
      "name": "Check Urgency",
      "type": "n8n-nodes-base.if",
      "position": [530, 300]
    },
    {
      "parameters": {
        "channel": "#urgent",
        "text": "🚨 Urgent: {{$json.message}}"
      },
      "id": "slack-urgent",
      "name": "Urgent Channel",
      "type": "n8n-nodes-base.slack",
      "position": [810, 200]
    },
    {
      "parameters": {
        "channel": "#general",
        "text": "New submission: {{$json.message}}"
      },
      "id": "slack-general",
      "name": "General Channel",
      "type": "n8n-nodes-base.slack",
      "position": [810, 400]
    }
  ],
  "connections": {
    "Form Webhook": {
      "main": [[{"node": "Check Urgency", "type": "main", "index": 0}]]
    },
    "Check Urgency": {
      "main": [
        [{"node": "Urgent Channel", "type": "main", "index": 0}],
        [{"node": "General Channel", "type": "main", "index": 0}]
      ]
    }
  }
}
```

## Workflow Generation Guidelines

1. **Always include required fields** for each node:
   - `id`: Unique identifier
   - `name`: Human-readable name
   - `type`: Correct n8n node type
   - `position`: [x, y] coordinates
   - `parameters`: Node-specific configuration

2. **Use expressions** for dynamic values:
   - `{{$json.fieldName}}` - Access current node data
   - `{{$node["Node Name"].json.fieldName}}` - Access data from specific node

3. **Handle errors gracefully**:
   - Add error workflow connections where appropriate
   - Use Try/Catch patterns with Code nodes for complex logic

4. **Optimize for clarity**:
   - Use descriptive node names
   - Arrange nodes left-to-right for main flow
   - Use vertical arrangement for branches

5. **Common node parameters**:
   - Gmail: `operation`, `messageId`, `labelIds`
   - Slack: `channel`, `text`, `attachments`
   - HTTP Request: `method`, `url`, `headers`, `body`
   - Google Sheets: `operation`, `sheetId`, `range`, `values`

## Testing Generated Workflows

1. **Validate JSON structure** - Ensure proper formatting
2. **Check node types** - Verify all node types exist in n8n
3. **Verify connections** - All referenced nodes must exist
4. **Test expressions** - Ensure data references are valid

## Response Format

When generating workflows, provide:
1. The complete workflow JSON
2. Brief explanation of what the workflow does
3. Any credentials needed (e.g., "Requires Gmail OAuth2 credentials")
4. Customization suggestions if applicable

Remember: Users can import the JSON directly into n8n through the workflow editor's import function.