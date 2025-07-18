# Weather MCP Server

A Model Context Protocol (MCP) server that provides weather forecasts and alerts using the US National Weather Service API.

## Features

This MCP server exposes two main tools:

- **get_alerts**: Get active weather alerts for any US state
- **get_forecast**: Get detailed weather forecast for any location (latitude/longitude)

## Prerequisites

- Python 3.10 or higher
- uv package manager (recommended) or pip
- MCP-compatible client (e.g., Claude for Desktop)

## Installation

### 1. Install uv (recommended)

**Windows (PowerShell):**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**macOS/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. Set up the project

Navigate to the weather directory and install dependencies:

```powershell
cd weather
uv venv
.venv\Scripts\Activate.ps1  # Windows
# source .venv/bin/activate  # macOS/Linux
uv sync
```

Alternatively, using pip:
```powershell
cd weather
pip install -r requirements.txt
```

## Running the Server

### Standalone Testing

To test the server directly:

```powershell
# Using uv (recommended)
uv run weather.py

# Or using Python directly
python weather.py
```

The server will start and listen for MCP requests via STDIO.

### Integration with Claude for Desktop

1. **Install Claude for Desktop** from [claude.ai/download](https://claude.ai/download)

2. **Configure Claude for Desktop** by editing the configuration file:

   **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
   **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
   **Linux:** `~/.config/Claude/claude_desktop_config.json`

3. **Add the weather server configuration:**

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

   **Important:** Replace `C:\\ABSOLUTE\\PATH\\TO\\YOUR\\weather` with the actual absolute path to your weather directory.

   If you don't have uv, you can use Python directly:
   ```json
   {
     "mcpServers": {
       "weather": {
         "command": "python",
         "args": [
           "C:\\ABSOLUTE\\PATH\\TO\\YOUR\\weather\\weather.py"
         ]
       }
     }
   }
   ```

4. **Restart Claude for Desktop**

5. **Test the integration** by asking Claude questions like:
   - "What's the weather forecast for Sacramento?" (requires latitude/longitude)
   - "What are the active weather alerts in Texas?"
   - "Get weather alerts for California"

## Usage Examples

Once configured with Claude for Desktop, you can use natural language to interact with the weather tools:

### Weather Alerts
- "Are there any weather alerts for Florida?"
- "Show me active weather alerts in Texas"
- "What weather warnings are active in CA?"

### Weather Forecast
- "What's the forecast for latitude 38.7749, longitude -122.4194?" (San Francisco)
- "Get the weather forecast for coordinates 40.7128, -74.0060" (New York)

## API Reference

### get_alerts(state: str)
Get active weather alerts for a US state.

**Parameters:**
- `state`: Two-letter US state code (e.g., "CA", "NY", "TX")

**Returns:** Formatted string with alert details including event type, area, severity, description, and instructions.

### get_forecast(latitude: float, longitude: float)
Get weather forecast for a specific location.

**Parameters:**
- `latitude`: Latitude coordinate (float)
- `longitude`: Longitude coordinate (float)

**Returns:** Formatted string with forecast periods including temperature, wind, and detailed forecast information.

## Troubleshooting

### Common Issues

1. **Server not showing up in Claude for Desktop:**
   - Verify the absolute path in the configuration file
   - Ensure uv is in your PATH or use the full path to uv.exe
   - Restart Claude for Desktop after configuration changes
   - Check that the configuration JSON is valid

2. **"Unable to fetch" errors:**
   - Ensure you have internet connectivity
   - The National Weather Service API only works for US locations
   - Check that coordinates are valid (latitude: -90 to 90, longitude: -180 to 180)

3. **Import errors:**
   - Verify all dependencies are installed: `uv sync` or `pip install httpx mcp[cli]`
   - Ensure you're using Python 3.10 or higher

### Logging

The server uses stderr for logging to avoid interfering with the MCP protocol. If you need to debug:

1. Add logging statements that write to stderr or a file
2. Never use `print()` statements in the main server code as they will break the STDIO transport

## Development

The server is built using:
- **FastMCP**: Simplified MCP server framework
- **httpx**: Async HTTP client for API requests
- **National Weather Service API**: Free weather data source

To modify or extend the server:
1. Edit `weather.py`
2. Add new tools using the `@mcp.tool()` decorator
3. Follow MCP best practices for error handling and response formatting

## License

This project is open source and available under the MIT License.
