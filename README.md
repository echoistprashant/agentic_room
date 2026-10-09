# Agentic Room

Agentic Room is a local [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server that gives an MCP-compatible AI client tools for running commands and Python code, searching for files and text, and reading or changing files and folders.

> **Use with care:** The server runs with the permissions of the account that starts it. Its command-execution tools can run arbitrary commands, and `delete_folder` recursively deletes the specified folder. Only connect it to clients and projects you trust.

## Requirements

- Python 3.12 or later
- [uv](https://docs.astral.sh/uv/)
- An MCP-compatible client

## Install

From the project directory, sync the dependencies:

```powershell
uv sync
```

To build a wheel:

```powershell
uv build
```

To install the built wheel into the active Python environment from the project directory:

```powershell
uv pip install ".\dist\agentic_room-0.1.0-py3-none-any.whl"
```

## Run

Start the server from the project directory:

```powershell
uv run agentic_room
```

The server uses the MCP `stdio` transport. It waits for an MCP client to communicate over standard input and output, so it is expected to keep running when launched directly. Press **Ctrl+C** to stop a manually started server.

### Configure VS Code

Add an MCP server entry to your VS Code workspace `.vscode/mcp.json`. Replace the example project path with the full path to your local checkout:

```json
{
  "servers": {
    "agentic-room": {
      "type": "stdio",
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "C:\\path\\to\\mcppipi",
        "agentic_room"
      ]
    }
  }
}
```

If `uv` is not available on the client process's `PATH`, set `command` to the full path to the `uv` executable.

## Available tools

| Tool | Inputs | Description |
| --- | --- | --- |
| `bash` | `command` | Runs a command through the server process's shell and returns its output. |
| `python_code` | `code` | Runs Python code using `python -c`. |
| `python_file` | `file` | Runs a Python file using `python`. |
| `glob` | `pattern` | Returns paths matching a glob pattern. |
| `grep` | `pattern`, `file` | Searches a file for matching lines using Windows `findstr`. |
| `read_file` | `file` | Reads and returns a text file. |
| `write_file` | `file`, `content` | Writes text to a file, creating it if needed. |
| `create_folder` | `folder` | Creates a folder and any missing parent folders. |
| `delete_folder` | `folder` | Recursively deletes a folder if it exists. |

File and folder paths are interpreted by the server process on the machine where it runs. The Python tools use the `python` executable available to that process.