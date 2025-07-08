# n8n-vibe 🤖✨

An AI-powered n8n workflow generator that converts natural language descriptions into ready-to-use n8n automation workflows.

## Overview

n8n-vibe leverages Claude AI to understand your automation needs described in plain language and automatically generates complete n8n workflow JSON files. Simply describe what you want to automate, and get a working n8n workflow in seconds!

## Features

- 🗣️ **Natural Language Input**: Describe your automation needs in plain language
- 🔄 **Automatic Workflow Generation**: Get complete n8n JSON workflows instantly
- 📦 **Ready-to-Import**: Generated workflows can be directly imported into n8n
- 🧩 **400+ Integrations**: Supports all n8n nodes and integrations
- 🎯 **Smart Pattern Recognition**: Understands common automation patterns
- 📝 **Comprehensive Documentation**: Detailed instructions in CLAUDE.md

## Quick Start

1. **Describe Your Need** (Example):
   ```
   每天早上自動從 Gmail 讀取未讀郵件，摘要內容後發送到 Slack
   ```

2. **Get Generated Workflow**:
   - Complete JSON workflow file
   - All required nodes configured
   - Proper connections established
   - Ready for import

3. **Import to n8n**:
   - Open n8n editor
   - Import the generated JSON
   - Configure credentials
   - Activate workflow

## Example Workflows

### 📧 Email to Messaging
Automatically forward email summaries to Slack channels

### 📊 Scheduled Reports
Fetch data from APIs and save to Google Sheets on schedule

### 🔀 Conditional Processing
Route form submissions based on content or urgency

### 📰 Web Scraping & Analysis
Scrape websites, analyze with AI, and send formatted reports

## Project Structure

```
n8n-vibe/
├── README.md           # This file
├── CLAUDE.md          # Instructions for Claude AI
└── n8n-workflows/     # Generated workflow JSON files
    └── yahoo-finance-news-ai-analysis.json
```

## How It Works

1. **CLAUDE.md** contains comprehensive instructions about n8n's structure, node types, and workflow patterns
2. Claude AI uses these instructions to understand your requirements
3. The AI generates valid n8n workflow JSON based on your description
4. Workflows are saved to the `n8n-workflows/` directory

## Example: Yahoo Finance News Analyzer

The included example workflow demonstrates:
- **Scheduled Trigger**: Runs every 6 hours
- **Web Scraping**: Fetches Yahoo Finance news
- **Data Processing**: Extracts headlines and links
- **AI Analysis**: Uses OpenAI to analyze market trends
- **Email Reports**: Sends formatted HTML reports

## Requirements

- n8n instance (self-hosted or cloud)
- Claude AI access (for generating workflows)
- Required credentials for nodes used in workflows

## Usage Tips

- Be specific about trigger types (webhook, schedule, email, etc.)
- Mention all services/apps you want to integrate
- Describe the data flow clearly
- Specify any conditions or filters needed

## Contributing

Feel free to contribute by:
- Adding more workflow examples
- Improving the CLAUDE.md instructions
- Sharing your generated workflows
- Reporting issues or suggestions

## License

This project is open source and available under the MIT License.

---

Built with ❤️ using Claude AI and n8n