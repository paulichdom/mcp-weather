
# MCP Weather Server

This project was built by following the official Anthropics MCP server quickstart guide. It is designed to work with Claude for Desktop and other MCP-compatible clients.

## Overview

It demonstrates how to build a simple MCP weather server and connect it to a host (Claude for Desktop). The setup starts basic and can be extended for more advanced use cases.

## What This Project Does

Many large language models (LLMs) can't fetch weather forecasts or severe weather alerts out of the box. By following the MCP quickstart, I built a server that exposes weather tools to solve this problem.

The server provides two main tools:

- `get_alerts`: Fetches severe weather alerts for a given location.
- `get_forecast`: Retrieves the weather forecast for a given location.

With this project, you can:

- Set up your own MCP weather server
- Expose the `get_alerts` and `get_forecast` tools
- Connect the server to an MCP host (like Claude for Desktop)
