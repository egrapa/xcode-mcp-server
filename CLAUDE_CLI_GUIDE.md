# Xcode MCP Server - Claude CLI Quick Start Guide

## Overview
This guide helps you verify and use the Xcode MCP server with Claude CLI (Claude Code).

**⚠️ IMPORTANT: This MCP server is ONLY for iOS/macOS Xcode projects!**
- ✅ Use with: iOS, macOS, watchOS, tvOS projects (.xcodeproj, .xcworkspace)
- ❌ DO NOT use with: Android projects (use Gradle/Android Studio tools instead)

## Configuration Location

### MCP Server Configuration
**File**: `/Users/eugeniograpa/Documents/_Zyntx/.mcp.json`

This file tells Claude CLI where to find the Xcode MCP server when working in any project under `_Zyntx/`.

### Server Environment
**File**: `/Users/eugeniograpa/MCP/xcode-mcp-server/.env`

Contains the projects directory setting:
```
PROJECTS_BASE_DIR=/Users/eugeniograpa/Documents/_Zyntx
```

## Quick Start Checklist

### 1. Verify Configuration Files Exist

```bash
# Check MCP configuration
cat /Users/eugeniograpa/Documents/_Zyntx/.mcp.json

# Check server environment
cat ~/MCP/xcode-mcp-server/.env
```

Expected `.mcp.json` content:
```json
{
  "mcpServers": {
    "xcode": {
      "command": "node",
      "args": ["/Users/eugeniograpa/MCP/xcode-mcp-server/dist/index.js"],
      "env": {
        "PROJECTS_BASE_DIR": "/Users/eugeniograpa/Documents/_Zyntx"
      }
    }
  }
}
```

### 2. Test Server Manually (Optional)

```bash
# Navigate to an Xcode project
cd /Users/eugeniograpa/Documents/_Zyntx/Art-Show-Tracker/iOS/Art-Show-Tracker-iOSApp/ArtShowTracker

# Test server startup
node ~/MCP/xcode-mcp-server/dist/index.js

# Press Ctrl+C to stop the test
```

You should see:
- "Xcode MCP Server started successfully"
- Detection of your Xcode projects

### 3. Using MCP Server in Claude CLI

#### Start Claude CLI in Your Projects Directory
```bash
cd /Users/eugeniograpa/Documents/_Zyntx/[your-project]
claude
```

#### First-Time Approval
When you first try to use MCP tools, Claude CLI will prompt:
```
This project contains MCP server configuration (.mcp.json).
Do you want to use the "xcode" MCP server?
```

Select **Yes** or **Always** to approve.

#### Available MCP Tools

The Xcode MCP server provides 50+ tools. Key categories:

**Project Management:**
- `set_project_path` - Set active Xcode project
- `get_active_project` - Get current project info
- `find_projects` - Find Xcode projects in directories
- `detect_active_project` - Auto-detect project

**File Operations:**
- `read_file` - Read file contents
- `write_file` - Write/update files
- `search_in_files` - Search for text in files
- `list_directory` - List directory contents

**Build & Test:**
- `build_project` - Build Xcode project
- `run_tests` - Run tests
- `clean_project` - Clean build directory
- `list_available_schemes` - List build schemes

**Dependencies:**
- `pod_install` / `pod_update` - CocoaPods management
- `add_swift_package` - Add Swift Package dependencies
- `update_swift_package` - Update packages

**iOS Simulator:**
- `list_simulators` - List available simulators
- `boot_simulator` - Start simulator
- `install_app` - Install app on simulator
- `launch_app` - Launch app

#### Example Usage in Claude CLI

```
# Ask Claude to use MCP tools:

"Use the xcode MCP server to list all schemes in the Art Show Tracker project"

"Build the iOS app using the Debug configuration"

"Search for files containing 'ArtworkEntity' in the project"

"List all available iOS simulators"
```

Claude will automatically use the appropriate MCP tools to fulfill these requests.

## Troubleshooting

### ⚠️ Issue: Using Xcode MCP server with Android projects
**IMPORTANT**: The Xcode MCP server ONLY works with iOS/macOS Xcode projects!

**Wrong - Android Project:**
```bash
❌ /Users/eugeniograpa/Documents/_Zyntx/Art-Show-Tracker/Android/
   DO NOT use Xcode MCP server here - this is an Android/Gradle project!
```

**Correct - iOS/Xcode Projects:**
```bash
✅ /Users/eugeniograpa/Documents/_Zyntx/Art-Show-Tracker/iOS/Art-Show-Tracker-iOSApp/ArtShowTracker/
✅ /Users/eugeniograpa/Documents/_Zyntx/universal-alphabet-now/
✅ /Users/eugeniograpa/Documents/_Zyntx/quiz-now-apps-swift/
```

### Issue: MCP server not found
**Solution**: Ensure you're in a directory under `/Users/eugeniograpa/Documents/_Zyntx/`

```bash
pwd  # Should show path under _Zyntx
```

### Issue: "Unable to locate a Java Runtime" error
**Cause**: You're in an Android project directory, not an iOS/Xcode project
**Solution**: Navigate to an iOS/Xcode project instead

```bash
cd /Users/eugeniograpa/Documents/_Zyntx/Art-Show-Tracker/iOS/Art-Show-Tracker-iOSApp/ArtShowTracker
```

### Issue: Server doesn't detect projects
**Check**: Verify PROJECTS_BASE_DIR in `.env`

```bash
cat ~/MCP/xcode-mcp-server/.env | grep PROJECTS_BASE_DIR
# Should show: PROJECTS_BASE_DIR=/Users/eugeniograpa/Documents/_Zyntx
```

### Issue: Permission denied errors
**Solution**: Ensure server has executable permissions

```bash
chmod +x ~/MCP/xcode-mcp-server/dist/index.js
```

### Reset MCP Approval Choices
If you need to reset which MCP servers you've approved:

```bash
claude mcp reset-project-choices
```

## Debug Output

The MCP server supports detailed debug logging to help troubleshoot issues.

### Enable Debug Output

Debug is currently **ENABLED** in both configuration files:

**In `.env` file:**
```bash
DEBUG=true
```

**In `.mcp.json` file:**
```json
"env": {
  "PROJECTS_BASE_DIR": "/Users/eugeniograpa/Documents/_Zyntx",
  "DEBUG": "true"
}
```

### What Debug Shows

When enabled, you'll see:
- Server startup details
- Projects base directory being used
- Project detection and scanning
- Workspace and scheme detection
- Tool execution details
- File operations and paths
- Build command details
- Detailed error messages and stack traces

### Test Debug Output

```bash
cd ~/MCP/xcode-mcp-server
node dist/index.js
# You should see "Debug mode enabled" message
# Press Ctrl+C to stop
```

### Disable Debug (if too verbose)

Set `DEBUG=false` in both `.env` and `.mcp.json` if output is too verbose.

## Updating the Server

If you update the Xcode MCP server code:

```bash
cd ~/MCP/xcode-mcp-server
npm install
npm run build
```

## Your Xcode Projects

The server is configured for projects under:
```
/Users/eugeniograpa/Documents/_Zyntx/
```

**Valid iOS/macOS Xcode Projects:**
- ✅ Art Show Tracker (iOS/macOS) - `/Art-Show-Tracker/iOS/`
- ✅ Universal Alphabet Now - `/universal-alphabet-now/`
- ✅ YiddishQuizNow - `/quiz-now-apps-swift/`
- ✅ Art-Show-Tracker-Core - `/Art-Show-Tracker/iOS/Art-Show-Tracker-Core/`

**Not for Xcode MCP Server:**
- ❌ Art Show Tracker Android - `/Art-Show-Tracker/Android/` (Use Gradle/Android tools)

## Quick Reference Commands

```bash
# Check MCP config
cat /Users/eugeniograpa/Documents/_Zyntx/.mcp.json

# Test server
cd /Users/eugeniograpa/Documents/_Zyntx/Art-Show-Tracker/iOS/Art-Show-Tracker-iOSApp/ArtShowTracker
node ~/MCP/xcode-mcp-server/dist/index.js

# Start Claude CLI in project
cd /Users/eugeniograpa/Documents/_Zyntx/[project]
claude

# View MCP server logs (in Claude CLI session)
# Claude automatically shows MCP tool usage in the conversation
```

## Additional Resources

- **MCP Server README**: `~/MCP/xcode-mcp-server/README.md`
- **Tools Documentation**: `~/MCP/xcode-mcp-server/docs/tools-overview.md`
- **User Guide**: `~/MCP/xcode-mcp-server/docs/user-guide.md`
- **Claude Code MCP Docs**: https://code.claude.com/docs/en/mcp

---

**Last Updated**: November 6, 2025
**MCP Server Version**: 1.0.3
**Server Location**: `/Users/eugeniograpa/MCP/xcode-mcp-server/`
