
## What You'll Build

**Zava Insurance** is a fictional mid-sized insurance company that serves over 150,000 homes across the Pacific Northwest. Like many companies, they faced a challenge: their manual claims process was slow and inefficient. When severe storms hit in October 2025, they received 2,000 claims in just 48 hours-and it took weeks to process them all.

### The Problem
Manual processing meant:
- **3-week delays** for customers waiting on claims
- **Coordination issues** when handling multiple urgent cases
- **Overwhelmed staff** unable to focus on complex cases that need human expertise

### The Solution
Zava's development team decided to build an **AI-powered claims system** that would:

1. **Automate routine tasks** so adjusters can focus on what matters most
2. **Provide instant access to data** through natural language conversations
3. **Work seamlessly with tools** employees already use (like Microsoft 365)

To make this happen, they built two key components:

**🔧 MCP Server (Model Context Protocol Server)**
- Think of this as a "translator" that lets AI agents securely access claims data
- Built on Azure infrastructure for security and reliability
- Provides real-time information about damage assessments, contractors, and inspection schedules

**💬 Declarative Agents with Microsoft 365 Copilot**
- Allows adjusters to use simple, natural language like "Show me all urgent storm damage claims"
- No need to learn complex systems or navigate multiple databases
- Works right inside Microsoft 365 tools they use every day

## Lab Objectives

By the end of this lab, you will:

✅ **Understand** how MCP servers bridge the gap between AI agents and backend systems  
✅ **Build and run** Zava's MCP server with real insurance claims data  
✅ **Create** Declarative Agents using Microsoft 365 Agents Toolkit that:
   - Connect to your MCP Server
   - Access knowledge and data from SharePoint and other sources
   
✅ **Build** an orchestrator agent that coordinates multiple specialized agents  
✅ **Test** your agent using natural language queries with real claims data

---

☑️ **Great!** You now understand what you'll be building. Select **Next >** to begin the lab.