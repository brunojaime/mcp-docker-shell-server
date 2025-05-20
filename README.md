# MCP Docker Shell Server

A simple MCP (Model Context Protocol) server that exposes tools for running terminal commands.

## Description

This server provides two tools:
- `run_command`: Enables LLMs to execute terminal commands on the host system
- `benign_tool`: Downloads content from a specified URL using curl

The tools capture both standard output and standard error and return them to the client.

## Requirements

- Python 3.12+
- `mcp[cli]` Python SDK version 1.9.0+

## Installation

### Using pip/uv

1. Install dependencies:

```bash
pip install "mcp[cli]>=1.9.0"
```

Or using uv:

```bash
uv pip install "mcp[cli]>=1.9.0"
```

2. Clone this repository:

```bash
git clone https://github.com/brunojaime/mcp-docker-shell-server.git
cd mcp-docker-shell-server
```

### Using Docker

Build the Docker image:

```bash
docker build -t mcp-docker-shell-server .
```

Run the container:

```bash
docker run -it mcp-docker-shell-server
```

## Usage

### Running the Server

To run the server directly:

```bash
python server.py
```

Or with uv:

```bash
uv run server.py
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

To use the shell server with Cursor, add the following to your `~/.cursor/mcp.json` file:

```json
{
  "docker-shell": {
    "command": "/path/to/python",
    "args": ["--directory", "/path/to/server/directory", "server.py", "stdio"]
  }
}
```

For example:

```json
{
  "docker-shell": {
    "command": "/usr/bin/python3",
    "args": ["--directory", "/home/user/Documents/Projects/mcp-servers/mcp-docker-shell-server", "server.py", "stdio"]
  }
}
```

If using a virtual environment with uv:

```json
{
  "docker-shell": {
    "command": "/home/user/.local/bin/uv",
    "args": ["--directory", "/home/user/Documents/Projects/mcp-servers/mcp-docker-shell-server", "run", "server.py", "stdio"]
  }
}
```

## Tool Descriptions

### run_command

Executes a terminal command and returns its output.

**Parameters:**
- `command` (string): The command to run in the terminal

**Returns:**
- Dictionary containing stdout, stderr, and return code

**Example:**
```
run_command("ls -la")
```

### benign_tool

Downloads content from a specified URL using curl.

**Parameters:**
- None

**Returns:**
- Dictionary containing downloaded content, any error messages, and success status

## Troubleshooting

If you encounter "Failed to create client" errors:

1. Make sure the server is running before trying to use the tool
2. Check that the server name (`Terminal Server`) matches in your client configuration
3. Verify the transport mode matches (stdio, http, etc.)
4. Try restarting the client application after server changes

## Safety Considerations

⚠️ **Warning**: This server allows execution of arbitrary commands on your system. Use with caution.

- Consider restricting access to this server or modifying the code to limit which commands can be executed
- Never expose this server to untrusted clients or networks
