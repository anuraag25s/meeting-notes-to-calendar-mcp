# Automated Meeting Notes to Calendar Workflow via MCP

An automated system integration that connects background meeting transcripts to Google Calendar using Claude.ai and the Model Context Protocol (MCP).

## 📌 Project Overview
Busy professionals waste significant post-meeting time manually parsing action items and scheduling follow-ups. This project builds a secure, low-overhead pipeline that connects **Granola** (a silent, background meeting recorder) and **Google Calendar** directly to **Claude.ai** using the Model Context Protocol (MCP).

By leverage MCP custom connectors, Claude can query meeting context from Granola and instantly register structured events directly onto Google Calendar with a single prompt.

## 🛠️ Tech Stack & Key Protocols
- **AI Orchestrator:** Claude.ai (Pro/Enterprise Custom Connectors)
- **Data Source:** Granola Desktop (Meeting Notes MCP Server)
- **Target Integration:** Google Calendar API (Google-hosted Remote MCP Server)
- **Protocols & Auth:** Model Context Protocol (MCP), OAuth 2.0 (Google Cloud Console, Client ID, Client Secrets)
- **Platform:** macOS

---

## 🏗️ System Architecture
The integration uses a cloud-to-local and cloud-to-cloud architecture governed by the Model Context Protocol:

```
[ Granola Desktop (Mac) ] <---> [ Granola Cloud MCP ] <--- (OAuth) ---> [ Claude.ai ]
                                                                             ^
                                                                             |
[ Google Calendar API ] <------ [ Google Calendar MCP ] <---- (OAuth) --------+
```

1. **Granola** records local meeting audio silently (without invasive bots) and saves notes to its workspace.
2. **Claude.ai** queries the Granola MCP server to search and retrieve recent meeting transcripts.
3. **Claude.ai** extracts actionable tasks, timelines, and attendees.
4. **Claude.ai** calls Google Calendar's hosted MCP tools to write scheduled blocks securely to the user's calendar.

---

## 🚀 Key Engineering & Implementation Steps

### 1. Google Cloud OAuth 2.0 Infrastructure
- Configured a private developer project in the **Google Cloud Console**.
- Set up an OAuth Consent Screen in Testing Mode, specifying user-access scopes for managing Google Calendar events (`https://www.googleapis.com/auth/calendar.events`).
- Provisioned secure OAuth Client Credentials (Client ID & Client Secret) and mapped the callback redirect URI: `https://claude.ai/api/mcp/auth_callback`.

### 2. Claude.ai Remote MCP Configuration
- Registered Google's hosted MCP server endpoint (`https://calendarmcp.googleapis.com/mcp/v1`) inside Claude's Custom Connector portal.
- Connected the private OAuth credentials and authenticated the flow.

### 3. Workflow Design & Execution
- Formulated an optimization prompt that instructs Claude to retrieve Granola data, isolate milestones, and schedule 30-minute time-blocks without overlap.

---

## ⚡ Challenges Overcome & Key Insights

### 💡 Bypassing Onboarding Waitlists
**Challenge:** Granola enforces a strict waitlist restriction for personal email addresses (Gmail, Outlook) during initial account signup.
**Solution:** Bypassed the restriction by utilizing an institutional/college domain account, recognizing that educational single-sign-on (SSO) systems are classified as secure, pre-approved workspaces.

### 🐛 Claude Connector Registry "URL Already Exists" Bug
**Challenge:** Encountered a known Claude.ai bug where deleting and re-registering an MCP server URL throws a `409 Conflict` (A server with this URL already exists), caused by orphaned background database records.
**Solution:** Solved this by appending a query parameter trick (`?v=1`) to the remote server URL. This forced Claude to recognize the URL as unique while Google's API safely ignored the parameter.

---

## 📷 Live Demo & Validation
*Include screenshots here!*
- **Claude's Tool Call:** [Insert screenshot showing Claude asking permission to run `create_event` tool]
- **Google Calendar Result:** [Insert screenshot of your calendar displaying the events Claude successfully created]

---

## 🔗 References & Credits
- Guided via [NextWork](https://nextwork.ai)
- [Model Context Protocol (MCP) Documentation](https://modelcontextprotocol.io)
- [Google Calendar MCP Reference](https://developers.google.com/workspace/calendar/api/guides/configure-mcp-server)
- [Granola MCP Integrations](https://docs.granola.ai/help-center/sharing/integrations/mcp)
