# MCAPS Tech Connect 2026 - LAB485: Building Advanced Declarative Agents with Microsoft 365 Agents Toolkit

Build AI-powered insurance claims processing agents using MCP servers, embedded knowledge, and connected agents—all within Microsoft 365 Copilot.

## 🎯 Scenario

**Zava Insurance** is a fictional mid-sized insurance company that serves over 150,000 homes across the Pacific Northwest. This lab walks you through building an AI-powered claims system that automates routine tasks, provides instant access to data through natural language, and integrates seamlessly with Microsoft 365.

## 🧠 Key Concepts

| Concept | Description |
|---------|-------------|
| **MCP Server** | Model Context Protocol server that acts as a "translator" allowing AI agents to securely access claims data |
| **Declarative Agents** | Agents defined through configuration that enable natural language interactions with Microsoft 365 Copilot |
| **Actions** | Tools/functions that agents can invoke to perform operations (e.g., get claims, create inspections) |
| **Embedded Knowledge** | Local files (PDFs, documents) embedded directly into an agent for instant access without live queries |
| **Connected Agents** | An orchestrator agent that coordinates multiple specialized agents into a unified experience |

## 📚 Lab Exercises

| Exercise | Description |
|----------|-------------|
| [Introduction](lab/instructions/introduction.md) | Learn about Zava Insurance's challenge and what you'll build |
| [Exercise 1: MCP Server & Declarative Agent](lab/instructions/exercise-1.md) | Run the MCP server, create a Declarative Agent with Microsoft 365 Agents Toolkit, and connect it to claims data |
| [Exercise 2: Embedded Knowledge & Connected Agents](lab/instructions/exercise-2.md) | Build an agent with embedded knowledge files and create ZavaCare—an orchestrator that coordinates multiple agents |

## ✅ What You'll Learn

By completing this lab, you will:

- Understand how MCP servers bridge AI agents and backend systems
- Build and run an MCP server with real insurance claims data
- Create Declarative Agents using Microsoft 365 Agents Toolkit with:
  - MCP server actions for live data access
  - Embedded knowledge from local files
- Build a connected agent that orchestrates multiple specialized agents
- Test agents using natural language queries

## 🚀 Getting Started

Start with the [Introduction](lab/instructions/introduction.md) to understand the scenario, then proceed through Exercise 1 and Exercise 2.

## Prerequisites

- Visual Studio Code with Microsoft 365 Agents Toolkit extension (pre-release version)
- Microsoft 365 developer account
- Node.js installed
