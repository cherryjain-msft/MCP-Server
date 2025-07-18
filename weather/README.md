# Weather MCP Server

A Model Context Protocol server that provides weather forecasts and alerts using the US National Weather Service API.

## Quick Start

1. **Install dependencies:**
   ```powershell
   uv sync
   ```

2. **Run the server:**
   ```powershell
   uv run weather.py
   ```

3. **Configure Claude for Desktop:**

   a. **Install Claude for Desktop** from [claude.ai/download](https://claude.ai/download)
   
   b. **Locate the configuration file:**
      - **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
      - **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
      - **Linux:** `~/.config/Claude/claude_desktop_config.json`
   
   c. **Open the configuration file** in a text editor:
      ```powershell
      # Windows - using notepad
      notepad "%APPDATA%\Claude\claude_desktop_config.json"
      
      # Or using VS Code
      code "%APPDATA%\Claude\claude_desktop_config.json"
      ```
   
   d. **Add the weather server configuration:**
      ```json
      {
        "mcpServers": {
          "weather": {
            "command": "uv",
            "args": [
              "--directory", 
              "C:\\ABSOLUTE\\PATH\\TO\\YOUR\\weather",
              "run", 
              "weather.py"
            ]
          }
        }
      }
      ```
      
      **⚠️ Important Notes:**
      - Replace `C:\\ABSOLUTE\\PATH\\TO\\YOUR\\weather` with your actual path
      - Use double backslashes (`\\`) in Windows paths
      - If you don't have `uv`, use Python directly:
        ```json
        {
          "mcpServers": {
            "weather": {
              "command": "python",
              "args": ["C:\\ABSOLUTE\\PATH\\TO\\YOUR\\weather\\weather.py"]
            }
          }
        }
        ```
      - You may need the full path to `uv.exe` (find it with `where uv`)
   
   e. **Save the file and restart Claude for Desktop**
   
   f. **Verify the connection:**
      - Look for the "Search and tools" 🔧 icon in Claude for Desktop
      - Click it to see if the weather tools are listed
      - If successful, you should see `get_alerts` and `get_forecast` tools

## Available Tools

- `get_alerts(state)` - Get weather alerts for a US state (e.g., "CA", "TX")
- `get_forecast(latitude, longitude)` - Get forecast for coordinates

## Example Queries

- "What are the weather alerts in California?"
- "Get the forecast for San Francisco coordinates 37.7749, -122.4194"

## Troubleshooting Claude for Desktop

**Server not showing up?**
1. Check that the JSON configuration is valid (no trailing commas, proper quotes)
2. Verify the absolute path is correct and uses double backslashes
3. Restart Claude for Desktop completely (close and reopen)
4. Check that `uv` is in your PATH: `where uv` in PowerShell

**Tools not appearing in Claude?**
1. Look for the 🔧 "Search and tools" icon in the chat interface
2. If missing, the server configuration may be incorrect
3. Try using the full path to uv.exe instead of just "uv"

**Getting path-related errors?**
1. Use absolute paths only (no relative paths like `./` or `../`)
2. Ensure the path exists and contains `weather.py`
3. Test the command manually in PowerShell first:
   ```powershell
   uv --directory "C:\your\actual\path\weather" run weather.py
   ```

For detailed setup instructions, see the main [README.md](../README.md).