# CLAUDE.md

This file provides guidance to Claude Code when working with n8n workflows in this repository.

## Project Overview

This is an n8n workflows repository containing workflow definitions, guides, and documentation for automation projects using n8n (pronounced "n-eight-n").

## Project Structure

```
n8n/
├── CLAUDE.md                          # This file - project guidance
├── README.md                          # Project documentation (Polish)
├── n8n-workflow-guide.md             # Comprehensive n8n guide (Polish)
├── .gitignore                        # Git ignore rules
└── workflows/                        # All n8n workflow JSON files
    ├── AI Superagent - Main.json
    ├── Contact Finder.json
    ├── Fireflies Meeting Summary.json
    ├── Google Maps.json
    ├── Notion Things Processing.json
    ├── Voice Agent.json
    └── README.md                     # Workflow documentation
```

## Technology Stack

- **n8n**: Open-source workflow automation platform
- **Format**: JSON workflow definitions
- **Language**: Documentation in Polish (PL)

## Working with n8n Workflows

### Workflow Files
- All workflow files are JSON format with `.json` extension
- Workflows are exported from n8n and stored in `workflows/` directory
- Use descriptive names: `webhook-email-notification.json`, `daily-report-scheduler.json`

### Exporting Workflows from n8n
```bash
# Export a single workflow
n8n export:workflow --id=<workflow-id> --output=workflows/My-Workflow.json

# Export all workflows
n8n export:workflow --all --output=workflows/
```

### Importing Workflows to n8n
```bash
# Import a workflow
n8n import:workflow --input=workflows/My-Workflow.json

# Import all workflows from directory
n8n import:workflow --input=workflows/
```

### Running n8n Locally

**Docker (recommended):**
```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

**npm:**
```bash
npm install n8n -g
n8n start
```

Access n8n at: `http://localhost:5678`

## Workflow Organization

### Directory Structure
All workflow files are stored directly in the `workflows/` directory - simple, flat structure without subdirectories.

### Naming Conventions
Use descriptive names that clearly indicate the workflow's purpose:

Examples:
- `Fireflies Meeting Summary.json` - ✅ Clear and descriptive
- `Notion Things Processing.json` - ✅ Clear and descriptive
- `Daily Sales Report.json` - ✅ Simple and understandable
- `Customer Signup Notification.json` - ✅ Describes the purpose

## Security Best Practices

### Credentials Management
- **NEVER** commit actual credentials, API keys, or secrets to git
- Use n8n's built-in Credentials feature in the n8n UI
- Configure all credentials directly in n8n (Settings → Credentials)
- Credentials are stored locally in n8n, not in workflow JSON files

### Sensitive Data
- Review workflows before committing to ensure no secrets are included
- Workflow JSON should not contain:
  - API keys or tokens
  - Passwords
  - Database credentials
- Use n8n's credential system and environment variables

## Workflow Development Best Practices

### 1. Documentation
- Add descriptive names to all nodes
- Use Sticky Notes in workflows to explain complex logic
- Include comments in Code nodes
- Document workflow purpose in filename

### 2. Error Handling
- Always implement Error Trigger nodes for production workflows
- Add try-catch logic in Code nodes
- Configure retry logic for critical operations
- Set up error notifications (email, Slack, etc.)

### 3. Testing
- Use Manual Trigger for development workflows
- Test workflows thoroughly before activating
- Check execution history regularly
- Validate data transformations at each step

### 4. Performance
- Filter data early in the workflow to reduce processing
- Avoid unnecessary node operations
- Use batch operations when possible
- Monitor execution times in n8n dashboard

### 5. Modularity
- Break complex workflows into smaller sub-workflows
- Create reusable templates for common patterns
- Use Execute Workflow node to call other workflows
- Keep workflows focused on single responsibility

## Common n8n Patterns

### Data Access (Expressions)
```javascript
// Access current item data
{{ $json.fieldName }}

// Access specific node output
{{ $node["Node Name"].json.data }}

// JavaScript expressions
{{ $json.price * 1.23 }}

// Date formatting
{{ $now.format('YYYY-MM-DD') }}
```

### Code Node Template
```javascript
// Process all items
for (const item of $input.all()) {
  item.json.processed = true;
  item.json.timestamp = new Date().toISOString();
  // Add your logic here
}

return $input.all();
```

## Version Control

### Workflow Changes
- Export updated workflows after making changes
- Commit with descriptive messages
- Include workflow name and change summary

Example commit messages:
```
Add Daily Sales Report workflow
Update Customer Signup: add email validation
Fix error handling in Database Sync workflow
```

### Git Workflow
```bash
# After updating workflow in n8n UI
n8n export:workflow --id=123 --output=workflows/My-Workflow.json

# Review changes
git diff workflows/My-Workflow.json

# Commit
git add workflows/My-Workflow.json
git commit -m "Update My Workflow: add error notifications"
git push
```

## Troubleshooting

### Common Issues

**Workflow not triggering:**
- Check if workflow is activated
- Verify trigger configuration (schedule, webhook URL, etc.)
- Check n8n logs for errors

**Data not passing between nodes:**
- Verify node connections
- Check expression syntax in {{ }}
- Use Test Workflow to debug data flow

**Credentials not working:**
- Re-authenticate in n8n Credentials
- Check API key permissions
- Verify environment variables

### Debugging
1. Use "Test Workflow" (Ctrl+Enter) to run entire workflow
2. Use "Test Step" to run individual nodes
3. Check Input/Output tabs for each node
4. Review execution history in n8n UI
5. Add Sticky Notes with debug information

## Resources

- [Official n8n Documentation](https://docs.n8n.io/)
- [n8n Workflow Templates](https://n8n.io/workflows/)
- [n8n Community Forum](https://community.n8n.io/)
- [n8n Blog & Tutorials](https://blog.n8n.io/tag/tutorial/)

## Language Note

Documentation in this repository is primarily in **Polish (PL)**. When working with:
- Documentation files: Use Polish
- Code comments: Use English or Polish
- Workflow node names: Use English (n8n best practice)
- Git commit messages: Use English

## Quick Reference

### Keyboard Shortcuts (n8n UI)
- `Tab` - Open node menu
- `Ctrl+Enter` - Execute workflow
- `Ctrl+S` - Save workflow
- `Ctrl+C/V` - Copy/Paste node
- `Delete` - Delete selected node
- `Ctrl+Z` - Undo
- `Space` - Pan canvas

### Useful Commands
```bash
# Start n8n
n8n start

# Export workflow
n8n export:workflow --id=<id> --output=<path>

# Import workflow
n8n import:workflow --input=<path>

# List workflows
n8n list:workflow

# Execute workflow
n8n execute --id=<workflow-id>
```
