# Building n8n Workflows with MCP Servers and Claude Skills

[Full Video Here](https://youtu.be/qulrnI-xgNU)

⚫⚫⚫⚪⚪ Intermediate Difficulty | ⏱️ 20-30 Minutes Setup Time

This guide walks you through connecting the unofficial n8n MCP server to any MCP-enabled agent (Claude Code, Cursor, Claude Desktop, etc.) to build and debug n8n workflows. While n8n's official MCP server only has 3 tools for working with existing workflows, this unofficial server can create workflows, edit them, view executions, and help you debug—all from your AI coding environment.

> 💡 **Note**: This works with any MCP-supported agent, not just Claude Code. The concepts apply to Cursor, Claude Desktop, Windsurf, or any other MCP-enabled client.

## Before You Begin

### What You'll Need

* n8n running (Cloud or self-hosted)
* Docker Desktop installed and running
* An MCP-enabled agent (Claude Code, Cursor, etc.)
* Basic familiarity with n8n concepts (triggers, nodes, workflows)
* Terminal access

### Important Notes

* This guide has been tested on macOS
* Windows/Linux may require path adjustments
* If you're new to n8n, I recommend reviewing [n8n basics](https://docs.n8n.io/) first
* The unofficial MCP server runs locally either via NPX or Docker (not remote MCP), we will be using Docker

### Self-Hosting n8n

If you want to self-host n8n at a fraction of the cost of n8n Cloud, I recommend [Hostinger](https://hostinger.com/jeredblu) (use code **JEREDBLU** for 10% off). The video covers the full setup process.

## Part 1: Upgrading to n8n v2 (Self-Hosted)

If you're running n8n on a self-hosted instance, here's how to upgrade to v2.

> ⚠️ **Important**: Before upgrading, check the [Migration Report](https://docs.n8n.io/1-x-migration-checklist/) page in n8n to identify any workflow issues. There are breaking changes between v1 and v2.

### Upgrade Commands

Open your terminal (via your hosting panel or SSH) and run these commands:

```bash
# Pull the latest n8n image
docker compose pull

# Stop the current container
docker compose down

# Start the updated container
docker compose up -d
```

Verify the upgrade by going to **Settings** in n8n—you should see version 2.x.

## Part 2: Installing Docker Desktop

The unofficial n8n MCP server can run via NPX or Docker. We're using the Docker installation because it gives us more functionality and stability.

1. Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop/)
2. Install and launch Docker Desktop
3. No additional configuration needed—just ensure it's running

## Part 3: Setting Up the n8n MCP Server

### Resources

* **n8n MCP Server**: [github.com/czlonkowski/n8n-mcp](https://github.com/czlonkowski/n8n-mcp)

### Step 1: Pull the Docker Image

Open a terminal and run:

```bash
docker pull ghcr.io/czlonkowski/n8n-mcp:latest
```

### Step 2: Get Your n8n URL

Your n8n URL depends on how you're running n8n:

* **n8n Cloud**: Use your cloud URL (e.g., `https://your-instance.app.n8n.cloud`)
* **Hostinger**: Check your Hostinger dashboard for the instance URL
* **Local Docker**: Usually `http://localhost:5678`
* **Other self-hosted**: Check your deployment configuration

### Step 3: Create an n8n API Key

1. Open your n8n instance
2. Go to **Settings → API**
3. Click **Create API Key**
4. Give it a name (e.g., "MCP")
5. Define the scopes (full access recommended for MCP)
6. Set an expiration date (keep it relatively short for security)
7. Copy the API key—you'll need it for the config

### Step 4: Create the mcp.json Configuration

Create a `.mcp.json` file in your project root (or configure it in your MCP client's settings).

We're using the **full configuration with n8n management tools** for maximum functionality:

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "--init",
        "-e", "MCP_MODE=stdio",
        "-e", "LOG_LEVEL=error",
        "-e", "DISABLE_CONSOLE_OUTPUT=true",
        "-e", "N8N_API_URL=https://your-n8n-instance.com",
        "-e", "N8N_API_KEY=your-api-key",
        "ghcr.io/czlonkowski/n8n-mcp:latest"
      ]
    }
  }
}
```

Replace:
* `https://your-n8n-instance.com` with your n8n instance URL
* `your-api-key` with the API key you created

## Part 4: Installing Claude Skills for n8n

The same developer created Claude Skills that teach your AI agent best practices for building n8n workflows. This adds crucial context that improves output quality.

### Resources

* **n8n Claude Skills**: [github.com/czlonkowski/n8n-skills](https://github.com/czlonkowski/n8n-skills)

### For Claude Code

1. Open Claude Code
2. Type `/plugins`
3. Go to **Install**
4. Enter: `czlonkowski/n8n-skills`
5. Press **Yes** to install
6. Restart Claude Code to see plugin

### Verify Installation

Type `/plugins` → **Installed** to confirm the n8n MCP skills are active.

## Part 5: Building Workflows

Now you're ready to build n8n workflows with your MCP-connected agent.

### Recommended Approach

1. **Start with a PRD**: Start with a plan, I recommend creating a Product Requirements Document outlining what you want to build. Check out my [PRD Creator video](https://youtu.be/0seaP5YjXVM?si=NL-KsJ1pQButI5Tp) and [PRD Creator custom instruction](https://github.com/JeredBlu/custom-instructions/blob/main/prd-creator-3-25.md).
2. **Use Plan Mode**: In Claude Code, use plan mode (Shift+Tab twice) for complex workflows, read the plan, iterate on it before accepting
3. **Use a capable model**: Claude Opus 4.5 is particularly good at building n8n workflows, but other SOTA models work well too (GPT5.2, Gemini3)!
4. **Iterate**: Don't expect perfection on the first try—debugging is part of the process

### Example Prompt

```
Look at the following PRD and use the n8n skill and n8n MCP to build this new workflow.

[Your PRD here]
```

### Important Workflow Tips

* **Always save before switching**: Save your n8n workflow and return to the overview page before letting the agent make changes via MCP. This prevents version conflicts between what you're editing and what the agent is editing.

* **Debug with executions**: The MCP server can view past executions to help figure out what's going wrong.

* **Copy/paste when needed**: Sometimes you'll need to copy node inputs/outputs to your agent for detailed debugging.

* **Handle large workflows locally**: When workflows get too big, the MCP server can struggle to pass all the information back and forth reliably (this is a n8n limitation). What I like to do to prevent this is:
  1. Once initial version in n8n is created, save it in n8n
  2. Copy the entire workflow JSON
  3. Paste it into a new `.json` file in my local project directorty
  4. Have the agent work on the latest version of local file using the MCP and its file editing tools
  5. Once complete, copy the JSON and paste it back into n8n, save, and test
  
  This approach saves tokens, avoids connection issues with large payloads, and gives you more control over the editing process.

## Troubleshooting

### Common Issues

1. **Docker not running**
   * Ensure Docker Desktop is open and running
   * Check the Docker icon in your system tray

2. **API key issues**
   * Verify the API key hasn't expired
   * Check that scopes are properly set
   * Regenerate if needed

3. **Connection refused**
   * Verify your n8n URL is correct
   * Check that n8n is running and accessible
   * For self-hosted: ensure firewall rules allow access

4. **MCP server not appearing**
   * Restart your MCP client (Claude Code, Cursor, etc.)
   * Verify the `.mcp.json` syntax is correct
   * Check Docker logs for errors

### Debugging Workflow Issues

* Use the MCP server's execution viewing capability
* Check individual node inputs/outputs in n8n
* Review the n8n execution logs
* Share specific error messages with your AI agent

## Useful Links

* **n8n MCP Server**: [github.com/czlonkowski/n8n-mcp](https://github.com/czlonkowski/n8n-mcp)
* **n8n Claude Skills**: [github.com/czlonkowski/n8n-skills](https://github.com/czlonkowski/n8n-skills)
* **Docker Desktop**: [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
* **PRD Creator Video**: [youtu.be/0seaP5YjXVM](https://youtu.be/0seaP5YjXVM?si=NL-KsJ1pQButI5Tp)
* **PRD Creator Instruction**: [github.com/JeredBlu/custom-instructions](https://github.com/JeredBlu/custom-instructions/blob/main/prd-creator-3-25.md)
* **n8n Documentation**: [docs.n8n.io](https://docs.n8n.io/)
* **Self-host n8n on Hostinger** (10% off with code JEREDBLU): [hostinger.com/jeredblu](https://hostinger.com/jeredblu)
* **Book a call**: [yedatechs.com](https://yedatechs.com/#container06)
