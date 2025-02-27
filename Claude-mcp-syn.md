# Sync Claude's Memory Between Computers: Advanced Cloud Setup Guide

[Full Video Here](https://youtu.be/YxPo_4tkl6A)

⚫⚫⚫⚫⚪ Advanced Difficulty | ⏱️ 30-45 Minutes Setup Time

This guide is a companion to my YouTube video on syncing Claude's MCP memory between computers. While I demonstrate using Dropbox, this method works with any cloud storage provider (Google Drive, iCloud, etc.).

> ⚠️ **Important**: This is an advanced tutorial. You should be comfortable with terminal commands, symbolic links, and independent troubleshooting. If you haven't set up Claude's MCP memory server yet, please watch my [previous tutorial](https://youtu.be/qeru0ZdudD4) first.

## Before You Begin

### What You'll Need
- Claude Desktop installed and running
- Memory MCP server configured in claude_desktop_config.json
- Existing Claude memories you want to preserve
- Cloud storage provider installed and synced (Dropbox, Google Drive, etc.)
- Terminal familiarity
- Admin permissions

### A Note About Your Memories
If you're using Claude on multiple computers already, consolidate your memories first:
1. Have Claude show you its memories on each computer
2. Save all memories somewhere safe
3. Add them to your primary computer
4. Then proceed with this guide

### Choose Your Cloud Provider
While this guide uses Dropbox, the process works with any cloud storage provider:
- Google Drive
- iCloud Drive
- Other cloud storage solutions

Just adjust the paths according to your provider's folder structure.

### Safety First
- This guide has only been tested on macOS
- Windows implementation may require modifications
- **Always backup your memory.json file before starting**
- Consider keeping multiple backups in different locations

## Setting Up Your First Computer

### Step 1: Locate Your Memory File
First, we need to find where Claude stores its memories.

1. Open Claude Desktop
2. Create a test memory (type anything like "test memory")
3. Open Terminal
4. Run this command:
```bash
ps aux | grep mcp-server-memory
```
5. Look for a line containing:
```bash
node /Users/USERNAME/.npm/_npx/RANDOM_ID/node_modules/.bin/mcp-server-memory
```
6. Copy this path and modify it:
   - Remove `.bin/mcp-server-memory`
   - Add `@modelcontextprotocol/server-memory/dist/memory.json`

For example:
```bash
# Original path:
/Users/username/.npm/_npx/15b07286cbcc3329/node_modules/.bin/mcp-server-memory

# Modified path:
/Users/username/.npm/_npx/15b07286cbcc3329/node_modules/@modelcontextprotocol/server-memory/dist/memory.json
```

Save this modified path - you'll need it for the next steps.

### Step 2: Prepare Cloud Storage
1. Open your cloud storage app (Dropbox in this example)
2. Create a new folder named "Claude Cloud Memory"
3. Right-click the folder while holding Option key
4. Select "Copy as Pathname"
5. Save this path - you'll need it shortly
6. Ensure your cloud storage is fully synced

### Step 3: Create the Cloud Link
Open Terminal and run these commands one at a time, replacing the paths with your saved ones:

```bash
# Verify your NPX path exists
ls 'YOUR_NPX_PATH'

# Create backup of current memory
cp 'YOUR_NPX_PATH' 'YOUR_NPX_PATH.backup'

# Copy memory to cloud storage
cp 'YOUR_NPX_PATH' 'YOUR_DROPBOX_PATH'

# Verify copy was successful
ls -l 'YOUR_DROPBOX_PATH/memory.json'

# Remove original memory file
rm 'YOUR_NPX_PATH'

# Create symbolic link
ln -s 'YOUR_DROPBOX_PATH/memory.json' 'YOUR_NPX_PATH'

# Verify symlink
ls -l 'YOUR_NPX_PATH'
```

### Step 4: Test the Setup
1. Restart Claude Desktop completely
2. Ask Claude to recall a previous memory
3. Create a new test memory
4. Check your cloud storage folder - memory.json should update

## Adding Additional Computers

### Step 1: Find the New Computer's Path
⚠️ **Important**: NPX can be different on each computer!

1. Follow the same process as Step 1 above to find the path
2. Remember to modify it the same way:
   - Remove `.bin/mcp-server-memory`
   - Add `@modelcontextprotocol/server-memory/dist/memory.json`

### Step 2: Connect to Cloud Storage
1. Ensure your cloud storage is installed and synced
2. Find the "Claude Cloud Memory" folder
3. Copy its pathname (Right-click + Option key)
4. Note: This path might be different from your first computer

Example path differences:
```bash
# Computer 1:
/Users/user1/Library/CloudStorage/Dropbox/Claude Cloud Memory

# Computer 2:
/Users/user2/Dropbox/Claude Cloud Memory
```

### Step 3: Create the Link
Run these commands in Terminal:

```bash
# Verify NPX path
ls 'YOUR_NPX_PATH'

# Backup empty memory file (optional)
cp 'YOUR_NPX_PATH' 'YOUR_NPX_PATH.backup'

# Remove empty memory file
rm 'YOUR_NPX_PATH'

# Create symlink to cloud memory
ln -s 'YOUR_DROPBOX_PATH/memory.json' 'YOUR_NPX_PATH'

# Verify symlink
ls -l 'YOUR_NPX_PATH'
```

### Step 4: Verify Everything Works
1. Restart Claude Desktop
2. Ask Claude to recall memories from your first computer
3. Create a new memory
4. Check that it appears on your other computer

## Troubleshooting

### Common Issues

#### "No such file or directory" Error
- Ensure paths start with /
- Use single quotes around paths
- Check folder permissions
- Verify Claude is running

#### Empty Memory After Setup
- Check cloud storage sync status
- Verify symlink points correctly
- Restart Claude completely

### Important Notes
- NPX paths might change after Claude updates
- Always backup before making changes
- Keep note of exact paths used
- Monitor symlinks after updates

## Getting Help
If you get stuck:
- Use AI search tools like Perplexity, ChatGPT with Search, or Claude with Brave Search
- Check error messages carefully
- Compare your paths exactly with examples

