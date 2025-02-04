# Setting Up MCP Servers in Cursor IDE

A comprehensive guide for implementing MCP servers (Sequential Thinking, Brave Search, and Puppeteer) in Cursor IDE. This guide accompanies my [YouTube tutorial](https://youtu.be/RCFe1L9qm3E) and provides detailed setup instructions.

## Prerequisites

- Cursor IDE version 0.4.5.9 or later
- Basic familiarity with Cursor
- API keys (if using Brave Search)

## Quick Setup

1. Open Cursor Settings
2. Navigate to Features
3. Scroll to MCP Servers section
4. Click "Add New MCP Server"

## Server Commands

### Brave Search
```bash
env BRAVE_API_KEY=[your-key] npx -y @modelcontextprotocol/server-brave-search
```

### Puppeteer
```bash
npx -y @modelcontextprotocol/server-puppeteer
```

### Sequential Thinking
```bash
npx -y @modelcontextprotocol/server-sequential-thinking
```

## Implementation Guide

1. Server Setup
   - Use Stdio/local setup method
   - Add servers one at a time
   - Verify status (green indicator)

2. Environment Variables
   - Add API keys securely
   - Use proper syntax for each server

3. Usage Tips
   - MCP only works in Composer and Agent mode
   - Explicitly instruct AI to use MCP tools
   - Accept tool usage prompts when they appear

## Troubleshooting

- If servers show yellow status, try refreshing
- Verify Cursor version is up to date
- Check API key formatting
- Restart Cursor if tools aren't appearing

## Additional Resources

- [Official Cursor MCP Documentation](https://docs.cursor.com/advanced/model-context-protocol)
- [MCP Server Documentation](https://github.com/modelcontextprotocol)
- More guides on my [YouTube Channel](https://youtube.com/@JeredBlu)

## Support

For additional help and tutorials:
- 📆 Book a Call: [Calendly](https://calendly.com/jeredblu)
- 📺 YouTube: [@JeredBlu](https://youtube.com/@JeredBlu)
- 🌐 Website: [jeredblu.com](https://jeredblu.com)

## Author

Created by [JeredBlu](https://github.com/JeredBlu), based on hands-on experience with Cursor IDE and MCP servers.
