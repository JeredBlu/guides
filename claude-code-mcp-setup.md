# Claude Code + MCP Setup Guide

[Full Video Here](https://www.youtube.com/watch?v=oM2dXJnD80c&t=9s)

⚫⚫⚫⚪⚪ Intermediate Difficulty | ⏱️ 15-30 Minutes Setup Time

This guide is a companion to my tutorial video on setting up Claude Code and configuring MCP servers. While the process should be straightforward, you might encounter some issues that require troubleshooting.

## Before You Begin

### What You'll Need

* Node.js installed
* Terminal access
* Anthropic API key
* Brave API key (optional, for search demo)
* Admin permissions

### Important Notes

* This guide has been tested on macOS
* Some commands may require different syntax on Windows
* Warp terminal users may encounter issues - use native terminal if problems occur
* Keep note of any error messages for troubleshooting

## Installation Steps

### 1. Install Claude Code

Run in your terminal:
```bash
npm install -g @anthropic-ai/claude-code
```
[Follow Full Claude Code Instructions Here] (https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)


### 2. Set Up Basic MCP Server (Fetch)

Run these commands:
```bash
claude mcp add fetch npx @modelcontextprotocol/server-fetch
```

### 3. Set Up Brave Search MCP (Optional)

There are two potential methods to set up Brave Search with environment variables:

Method 1 (--env):
```bash
claude mcp add brave-search npx @modelcontextprotocol/server-brave-search --env BRAVE_API_KEY=YOUR_API_KEY
```

Method 2 (-e):
```bash
claude mcp add brave-search npx @modelcontextprotocol/server-brave-search -e BRAVE_API_KEY=YOUR_API_KEY
```

### 4. Start MCP Server

Important: Run this in a separate terminal window:
```bash
claude mcp serve
```

### 5. Verify Setup

Start Claude Code:
```bash
claude
```

## Troubleshooting

### Common Issues

1. **Warp Terminal Issues**
   * Switch to native terminal
   * Restart terminal after configuration changes

2. **Environment Variables**
   * Try both --env and -e methods
   * Verify API key format
   * Check for extra spaces

3. **Context Issues**
   * Sometimes Claude needs multiple prompts to understand MCP context
   * Try closing and reopening Claude

4. **Connection Problems**
   * Ensure MCP serve is running in separate terminal
   * Check terminal outputs for errors
   * Verify API keys are valid

## Important Tips

* Always run `claude mcp serve` in a separate terminal window
* Restart terminal after making configuration changes
* When using Warp, switch to native terminal if issues occur
* Keep MCP serve running while using Claude
* Monitor terminal output for error messages

## Helpful Links

* Claude Code Documentation: https://docs.anthropic.com/claude/docs/claude-code
* Brave Search MCP Server: https://github.com/modelcontextprotocol/servers/tree/main/src/brave-search
* Fetch MCP Server: https://github.com/modelcontextprotocol/servers/tree/main/src/fetch


