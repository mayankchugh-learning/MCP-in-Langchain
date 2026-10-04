# MCP in LangChain — CineBot Edition

Hands-on notebooks showing how to connect LangChain agents to [Model Context Protocol (MCP)](https://modelcontextprotocol.io) servers, using small [FastMCP](https://gofastmcp.com) servers themed around a movie-booking bot ("CineBot").

## What's inside

| File | Purpose |
| --- | --- |
| `MCP_in_Langchain.ipynb` | Main walkthrough of the MCP adapter in LangChain |
| `MCP_MultiServer_Live_Demo.ipynb` | Live demo: one agent over several MCP servers |
| `cinebot_mcp_server.py` | Basic server with a `check_showtimes` tool |
| `cinebot_error_server.py` | Server with a `risky_lookup` tool that raises on bad booking IDs |
| `cinebot_elicit_server.py` | Server with a `cancel_booking_confirm` tool that asks the client to confirm (elicitation) |
| `Order Payment Workflow-*.png/svg` | Workflow diagrams used alongside the notes |

### `MCP_in_Langchain.ipynb`

1. Using the MCP adapter in LangChain
2. Calling a tool directly, without an LLM or agent
3. Tool output as multimodal content blocks
4. Structured output
5. Error handling
6. Elicitation: when the server needs to ask a question mid-call
7. Connecting to multiple servers
8. Authentication
9. `langchain.mcp` vs. `langchain-mcp-adapters`: what changed

### `MCP_MultiServer_Live_Demo.ipynb`

Connects to the local CineBot server over stdio and to a remote HTTP server (a TimeTrack MCP server on Vercel), prints a summary of which servers connected, builds a `create_agent` agent (`openai:gpt-5-mini`) over every tool it got, and then runs a small input loop so you can ask the agent questions.

## Requirements

- Python 3.11+
- An OpenAI API key
- [uv](https://docs.astral.sh/uv/) (recommended) or pip

## Setup

1. Create a `.env` file in the project root:

   ```env
   OPENAI_API_KEY=sk-...
   ```

2. Install dependencies.

   With uv (uses `pyproject.toml` / `uv.lock`):

   ```bash
   uv sync
   ```

   Or with pip:

   ```bash
   python -m venv .venv
   .venv\Scripts\activate        # Windows
   # source .venv/bin/activate   # macOS / Linux
   pip install -r requirements.txt
   ```

3. Open a notebook in VS Code or Cursor and select the `.venv` interpreter as the kernel.

## Running a server on its own

The notebooks start the servers for you over stdio. To run one by hand:

```bash
uv run python cinebot_mcp_server.py
```

## Notes

- `.env`, `.venv/`, `__pycache__/`, and the `*.log` files the servers write at runtime are git-ignored.
- `uv.lock` is committed so installs are reproducible.
