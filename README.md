# big4.cloud — AI Strategic Consulting Skill

Institutional-grade strategic analysis engine. 70+ cognitive gates, 13 consulting frameworks, adversarial stress-test, multi-model curator panel.

## Install

### Method 1: Download from GitHub
Download `skill.md` from this repository and paste its content into your AI client's custom instructions:
- **Claude Desktop**: Settings → Custom Instructions → paste content
- **Cursor**: Save as `.cursorrules` in your project root

### Method 2: Direct download
```
curl -o skill.md https://raw.githubusercontent.com/al3max/big4skill/main/skill.md
```

### Configure MCP Server
Add this to your MCP client configuration:

```json
{
  "mcpServers": {
    "big4cloud": {
      "url": "https://big4.cloud/api/v1/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

Get your API key free at https://big4.cloud/register.html

## What it does

When installed, this skill makes your AI assistant (Claude, Cursor, Codex, etc.) automatically:

- **Detect strategic questions** in 5 languages (EN/IT/ES/FR/DE) and route them to the big4.cloud engine
- **Run a pre-analysis questionnaire** when context is missing
- **Convert documents** automatically for grounded analysis
- **Manage a knowledge base** for persistent context across sessions
- **Choose the right analysis depth** (superfast/fast/normal/deep)

## Requirements

- A big4.cloud API key (get one at https://big4.cloud/register.html)
- MCP server configured: `https://big4.cloud/api/v1/mcp`

## License

Proprietary — big4.cloud
