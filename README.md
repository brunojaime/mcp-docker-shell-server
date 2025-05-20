# MCP Terminal Server

A simple MCP (Model Context Protocol) server that exposes a tool for running terminal commands.

## Description

This server provides a tool called `run_terminal_command` that enables LLMs to execute terminal commands on the host system. The tool captures both standard output and standard error from the commands and returns them to the client.

## Requirements

- Python 3.8+
- `mcp` Python SDK

## Installation

1. Install dependencies:

```bash
pip install "mcp[cli]"
```

Or using uv:

```bash
uv pip install "mcp[cli]"
```

2. Clone this repository or download the server file:

```bash
git clone https://your-repository-url.git
cd mcp-terminal-server
```

## Usage

### Running the Server

To run the server directly:

```bash
python server.py
```

### Development Mode

To test the server with the MCP Inspector:

```bash
mcp dev server.py
```

### Installing for Claude Desktop

To install the server in Claude Desktop:

```bash
mcp install server.py
```

### Configuration for Cursor

To use the terminal server with Cursor, add the following to your `~/.cursor/mcp.json` file:

```json
{
  "shell": {
    "command": "/path/to/python",
    "args": ["--directory", "/path/to/server/directory", "server.py", "stdio"]
  }
}
```

For example:

```json
{
  "shell": {
    "command": "/usr/bin/python3",
    "args": ["--directory", "/home/user/Documents/Projects/mcp-servers/shellserver", "server.py", "stdio"]
  }
}
```

If using a virtual environment with uv:

```json
{
  "shell": {
    "command": "/home/user/.local/bin/uvx",
    "args": ["--directory", "/home/user/Documents/Projects/mcp-servers/shellserver", "run", "server.py", "stdio"]
  }
}
```

## Tool Description

### run_terminal_command

Executes a terminal command and returns its output.

**Parameters:**
- `command` (string): The command to run in the terminal

**Returns:**
- String containing the command's stdout and stderr

**Example:**
```
run_terminal_command("ls -la")
```

## Troubleshooting

If you encounter "Failed to create client" errors:

1. Make sure the server is running before trying to use the tool
2. Check that the server name (`terminal-server`) matches in your client configuration
3. Verify the transport mode matches (stdio, http, etc.)
4. Try restarting the client application after server changes

## Safety Considerations

⚠️ **Warning**: This server allows execution of arbitrary commands on your system. Use with caution.

- The server has a 60-second timeout for commands to prevent long-running operations
- Consider restricting access to this server or modifying the code to limit which commands can be executed
- Never expose this server to untrusted clients or networks

## License

[Your chosen license]
