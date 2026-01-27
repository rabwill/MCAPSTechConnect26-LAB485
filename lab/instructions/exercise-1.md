
In this exercise, you'll run a complete Model Context Protocol (MCP) server for Zava Insurance's claims system and integrate it with Declarative Agent in Microsoft 365 Copilot that you will create, enabling natural language interactions with real claims data through secure, standardized AI agent communication.


## Step 1: Test the Hosted MCP Server

Verify the hosted MCP server is accessible and explore available tools.

### Launch MCP Inspector

Open a terminal and run:

```
npx @modelcontextprotocol/inspector --transport http --server-url https://claims-mcp-app.yellowcliff-c66c6908.eastus.azurecontainerapps.io/mcp/messages
```
> Note: If not already installed, you'll be prompted to install the @modelcontextprotocol/inspector@0.18.0 package. Proceed with the installation. 

This opens a web interface to test MCP tools.

### Explore Available Tools

In the MCP Inspector, you'll see **15 tools**:

| Resource | Available Tools |
|----------|---------------------|
| **Claims** | get_claims, get_claim, create_claim, update_claim, delete_claim |
| **Inspections** | get_inspections, get_inspection, create_inspection, update_inspection |
| **Contractors** | get_contractors, get_inspectors |
| **Purchase Orders** | get_purchase_orders, create_purchase_order, update_purchase_order, delete_purchase_order |


### Test Get Claims Tool

1. Select the **get_claims** tool
2. Click **Run Tool**
3. Verify you receive JSON response with claims data (e.g., claim CN202504990 for John Smith)
4. 
!IMAGE[mcp-inspector.png](instructions331914/mcp-inspector.png)

---

## Step 2: Create a New Declarative Agent Project

In this exercise, you'll use the Microsoft 365 Agents Toolkit to create a new Declarative Agent project that will connect to Zava's claims system.

### Create New Agent using Microsoft 365 Agents Toolkit

1. Open a new window in **VS Code**
2. Click the **Microsoft 365 Agents Toolkit** icon in the Activity Bar (left sidebar)

If you see a dialog as below, select **Yes, I trust the authors**.

!IMAGE[trust.jpg](instructions331914/trust.jpg)

#### Create New Agent Project

1. In the Agents Toolkit panel, click **"Create a New Agent/App"**
2. Select **"Declarative Agent"** from the template options
3. Next choose **"Add an Action"** to add to your agent
4. Next select **Start with an MCP server (preview)**
5. Enter the publicly accessible MCP Server URl : `https://claims-mcp-app.yellowcliff-c66c6908.eastus.azurecontainerapps.io/mcp/messages`
6. Choose the Default folder to scaffold the agent (or choose a preferred location in your machine)
7. When prompted for project details:

   - **Application Name**: `Zava Claims Assistant`

You will be directed to the newly created project which has the file `.vscode/mcp.json` open. This is the MCP server configuration file for VS Code to use.

- Select **Start** button to fetch tools from your server.
- Once started you will see the number of tools and prompts available 1️⃣. 
- Select **ATK:Fetch action from MCP** 2️⃣ to select tools you want to add to the agent. 

!IMAGE[atk-fetch.png](instructions331914/atk-fetch.png)

- When you select  **ATK:Fetch action from MCP**, you will be asked to provide the action manifest, select **ai-plugin.json**.
- Select the tools you want to add to the agent. Select all. 

    - create_claim
    - create_inspection
    - get_claim
    - get_claims
    - get_contractors
    - get_inspection
    - get_inspections
    - update_claim
    - update_inspection
    - get_inspectors

This step will populate the action manifest **ai-plugin.json** with the required functions, MCP server url, etc. that is needed for actions in an agent.

### Understand the Action manifest update from previous step

Open `appPackage/ai-plugin.json` and examine the structure with your chosen tools and MCP server url pre-populated:

```json
{
     "$schema": "https://aka.ms/json-schemas/copilot-extensions/v2.1/plugin.schema.json",
    "schema_version": "v2.4",
    "name_for_human": "Zava Claims Assistant",
    "description_for_human": "Zava Claims Assistant${{APP_NAME_SUFFIX}}",
    "contact_email": "publisher-email@example.com",
    "namespace": "zavaclaimsassistant",
    "functions": [
        {
            "name": "create_claim",
            "description": "Create a new insurance claim",
            "parameters": {
                ...
}
```

You now have a basic Declarative Agent that is connected to your MCP Server with tools ready for use.

---

## Step 3: Configure the Agent for Zava's Claims Operations

Transform the basic agent into Zava's intelligent claims assistant by configuring its identity, instructions,  capabilities, and conversation starters.

### Update Agent Identity and Description

Replace the content of `appPackage/declarativeAgent.json` with Zava's configuration:

```json
{
    "version": "v1.6",
    "name": "Zava Claims",
    "description": "An intelligent insurance claims management assistant that leverages MCP server integration to streamline inspection workflows, analyze damage patterns, coordinate contractor services, and generate comprehensive operational reports for efficient claims processing",
    "instructions": "$[file('instruction.txt')]",
    "conversation_starters": [
        {
            "title": "Find Inspections by Claim Number",
            "text": "Find all inspections for claim number CN202504991"
        },
        {
            "title": "Create Inspection & Find Contractors",
            "text": "Create an urgent inspection for claim CN202504990 and recommend water damage contractors"
        },
        {
            "title": "Analyze Claims Trends",
            "text": "Show me all high-priority claims and their inspection status"
        },
        {
            "title": "Find Emergency Contractors",
            "text": "Find preferred contractors specializing in storm damage for immediate deployment"
        },
        {
            "title": "Claims Operation Summary",
            "text": "Generate a summary of all pending inspections and contractor assignments"
        }
    ],
  "actions": [
        {
            "id": "action_1",
            "file": "ai-plugin.json"
        }
    ]
}
```

### Create Detailed Agent Instructions

Update `appPackage/instruction.txt` with comprehensive instructions for the agent:

```plaintext
# Zava Claims Operations Assistant

## Role
You are an intelligent insurance claims management assistant with access to the Zava Claims Operations MCP Server. Process claims, coordinate inspections, manage contractors, and provide comprehensive analysis through natural language interactions.

## Core Functions

### Claims Management
- Retrieve and analyze all claims using natural language queries
- Get specific claim details by claim number or partial information
- Create new insurance claims with complete documentation
- Update existing claim information and status
- Use fuzzy matching for partial claim information to help users find what they need

### Inspection Operations
- Filter inspections by claim ID, status, priority, or workload
- Retrieve detailed inspection data and schedules
- Create new inspection tasks with appropriate priority levels
- Modify existing inspection details and assignments
- Access inspector availability and specialties
- Automatically determine priorities: safety hazards = 'urgent', water damage = 'high', routine = 'medium'

### Contractor Services
- Find contractors by specialty, location, and preferred status
- Access contractor ratings, availability, and past performance
- Coordinate contractor assignments with inspection schedules
- Track purchase orders and contractor costs

## Decision Framework

### For Inspections:
1. Assess urgency based on damage type and safety requirements
2. Select appropriate task type: 'initial', 'reinspection', 'emergency', 'final'  
3. Generate detailed instructions with specific focus areas
4. Consider inspector specialties and contractor availability for scheduling

### For Claims Analysis:
1. Prioritize safety-related issues (structural damage, water intrusion)
2. Group similar damage types for efficient processing
3. Identify patterns that might indicate fraud or systemic issues
4. Recommend preventive measures based on damage trends

## Response Guidelines

**Always Include:**
- Relevant claim numbers and context
- Clear next steps and action items
- Priority levels and urgency indicators
- Safety risk assessments when applicable

**For Complex Requests:**
1. Break down the request into specific components
2. Retrieve relevant claim and inspection data
3. Execute appropriate MCP server functions
4. Provide integrated analysis with actionable recommendations
5. Suggest follow-up actions or monitoring

**Communication Style:**
- Professional yet approachable for insurance professionals
- Use industry terminology appropriately
- Provide clear explanations for complex procedures
- Always prioritize customer service and regulatory compliance
```

### Update the Teams App Manifest

Open `appPackage/manifest.json` and update it with Zava's branding:

```json
{
    "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.23/MicrosoftTeams.schema.json",
    "manifestVersion": "1.23",
    "version": "1.0.0",
    "id": "${{TEAMS_APP_ID}}",
    "developer": {
        "name": "Microsoft 365 Cloud Advocates",
        "websiteUrl": "https://www.zavainsurance.com",
        "privacyUrl": "https://www.zavainsurance.com/privacy",
        "termsOfUseUrl": "https://www.zavainsurance.com/terms"
    },
    "icons": {
        "color": "color.png",
        "outline": "outline.png"
    },
    "name": {
        "short": "Zava Claims",
        "full": "Zava Insurance Claims Assistant"
    },
    "description": {
        "short": "An intelligent insurance claims management assistant",
        "full": "An AI-powered claims management assistant that leverages MCP server capabilities to streamline inspection workflows, coordinate contractors, and provide comprehensive operational insights for efficient claims processing."
    },
    "accentColor": "#0078D4",
    "composeExtensions": [],
    "copilotAgents": {
        "declarativeAgents": [            
            {
                "id": "declarativeAgent",
                "file": "declarativeAgent.json"
            }
        ]
    },
    "permissions": [
        "identity",
        "messageTeamMembers"
    ],
    "validDomains": []
}
```

Your agent now has a clear identity as Zava's claims assistant with comprehensive instructions.

---

## Step 4: Update tools in plugin manifest
> This step is to avoid a known issue with agents built using Agents Toolkit Pre-release where where tool definitions are separated in a file.

- Copy the contents of file **appPackage/mcp-tools.json** 
- Open file **appPackage/ai-plugin.json**
- find property **mcp_tool_description**:
```
  "mcp_tool_description": {
                    "file": "mcp-tools.json"
                }
```
- Replace its contents with what was copied over from **mcp-tools.json** file
- The final look of the property is as below:

```
   "mcp_tool_description": {
                    "tools": [
                        {
                            "name": "create_claim",
                            "description": "Create a new insurance claim",
                            "inputSchema": {
                                "type": "object",
                                "properties": {
                                    "claimNumber": {
                                        "type": "string",
                                        "description": "Unique claim number"
                                    },
                                    "policyNumber": {
                                        "type": "string",
                                        "description": "Policy number"
                                    },...
```

## Step 5: Test the Agent Integration

Test your Declarative Agent to ensure it can successfully communicate with the MCP server and handle claims operations.

### Sign into the Microsoft 365 Agents Toolkit 

You'll need to sign into the Microsoft 365 Agents Toolkit in order to upload and test your agent from within it.

-	Within the project window, select the Microsoft 365 Agents Toolkit icon <img width="24" alt="m365atk-icon" src="https://github.com/user-attachments/assets/b5a5a093-2344-4276-b7e7-82553ee73199" /> again, from the left side menu. This will open the agent toolkit's activity bar with sections like Accounts, Environment, Development etc. 
-	Under **Accounts** section select **Sign in to Microsoft 365**. This will open a dialog from the editor to sign in or create a Microsoft 365 developer sandbox or Cancel. Select "Sign in". Log into the Microsoft 365 tenant using below credentials:

**Username: +++@lab.CloudPortalCredential(User1).Username+++**

**TAP Token:+++@lab.CloudPortalCredential(User1).AccessToken+++**

-	Once signed in, close the browser and go back to the project window.


### Provision the Agent

In VS Code with your `zava-claims-agent` project open:

1. Open the **Microsoft 365 Agents Toolkit** panel
2. Click **"Provision"** in the Lifecycle section
3. Wait for provisioning to complete - this creates and uploads the agent package


### Test in Microsoft 365 Copilot

1. Open browser from the machine and go to Copilot chat using URL https://m365.cloud.microsoft/chat/ 

> [!hint] 
> Use the same credentials to log in as before:  **Username: +++@lab.CloudPortalCredential(User1).Username+++**  **TAP Token:+++@lab.CloudPortalCredential(User1).AccessToken+++**

2. Under Agents on left hand side, find **Zava Claims Assistant**, and select it.  
3. Try the conversation starters:
   - `Find all inspections for claim number CN202504991`
   - `Show me all high-priority claims and their inspection status`

### Test Natural Language Queries

Try these natural language queries to test the agent's capabilities:

```
What claims do we have for storm damage?
```

```
Create a new urgent inspection for claim CN202504990 to assess water damage in the basement
```

```
Find contractors who specialize in roofing and are marked as preferred
```

```
Show me the details for claim number CN202504991
```

```
Create a new claim for Alice Johnson at 456 Oak Street with fire damage from yesterday
```

Your agent should successfully respond to natural language queries and interact with the MCP server data.


### Debug the agent 

1. In the chat with the Zava Claims agent, send message `-developer on`
2. This will enable debugging of these conversations 
3. Continue testing the agent with queries  

Analyze debugger information in the Agent debug info panel at the end of each agent response. 

!IMAGE[debugger.png](instructions331914/debugger.png)

☑️  Well done! You've successfully created and deployed Zava Insurance's Declarative Agent that seamlessly integrates with their MCP server.  Select **Next >**  to build more agents to help prepare you for multi-agent orchestration.