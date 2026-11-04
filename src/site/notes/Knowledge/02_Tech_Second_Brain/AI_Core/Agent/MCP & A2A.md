---
updated:
  - 2025-11-23T00:28:23+07:00
  - 2025-06-13T14:34:38+07:00
  - 2025-06-11T18:15:09+07:00
  - 2025-06-11T18:10:02+07:00
  - 2025-06-11T17:46:29+07:00
  - 2025-06-09T17:45:30+07:00
  - 2025-06-09T11:02:38+07:00
  - 2025-06-09T11:00:52+07:00
  - 2025-06-09T10:51:56+07:00
  - 2025-06-09T10:48:11+07:00
  - 2025-06-09T10:47:35+07:00
  - 2025-06-09T10:38:53+07:00
  - 2025-10-18T17:24:20+07:00
  - 
created:
dg-publish: true
dg-home:
tags:
  - type/howto
  - topic/agent-mcp
  - status/stable
dg-pinned: "false"
aliases:
cssclasses:
---
# Link
- [MCP vs A2A](https://medium.com/@anil.jain.baba/agentic-mcp-and-a2a-architecture-a-comprehensive-guide-0ddf4359e152)
- 
# Khái nhiệm và kiến trúc

**Agentic MCP (Model Context Protocol)** is a protocol that helps AI models, like chatbots, connect to external systems such as databases or business tools. It’s like giving the AI a set of tools to fetch information or perform tasks during a conversation, making it smarter and more helpful.

**A2A (Agent-to-Agent) Architecture** is about letting different AI agents talk to each other. Imagine a team where each agent has a job, like one handles customer queries and another manages tickets — they can work together smoothly using A2A.

- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828119.png)


# MCP

## Core Components

MCP follows a client-server architecture with the following key components:

1. **MCP Hosts** — Programs using LLMs (like Claude Desktop or IDEs) that initiate connections to access external data and tools
2. **MCP Clients** — Protocol clients embedded within the host application that maintain 1:1 connections with servers
3. **MCP Servers** — Lightweight programs that expose specific capabilities through the standardized protocol
4. **Data Sources** — Both local (files, databases) and remote services (APIs) that MCP servers can access

- ![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828312.png)

## Works
![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828503.png)

MCP standardizes three main types of capabilities:

1. **Tools** — Functions that can be called by the LLM (with user approval)
2. **Resources** — File-like data that can be read by clients (API responses, file contents)
3. **Prompts** — Pre-written templates to help users accomplish specific tasks

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828647.png)

### SSE & STDIO

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828778.png)
# A2A Architecture
## Core components
A2A architecture centers around facilitating communication between agents with these key components:

1. **Client Agent** — Formulates tasks and communicates them to remote agents
2. **Remote Agent** — Acts on tasks to provide information or perform actions
3. **Agent Card** — JSON metadata file describing an agent’s capabilities and endpoints
4. **Task Management** — Defines task objects with lifecycle stages and outputs
5. **Messaging System** — Allows agents to exchange context, replies, and artifacts

![](/img/user/assets/images/Knowledge/02_Tech_Second_Brain/AI_Core/Agent/IMG-20251122215828972.png)

