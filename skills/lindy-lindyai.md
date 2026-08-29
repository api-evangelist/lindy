---
name: Lindyai
description: Use when helping users set up, configure, or troubleshoot Lindy AI — an AI teammate that lives in Slack and automates work across email, meetings, calendar, and 1,000+ integrated tools. Reach for this skill when users need to create routines, manage integrations, set up team workspaces, teach Lindy custom skills, or delegate tasks.
metadata:
    mintlify-proj: lindyai
    version: "1.0"
---

# Lindy AI Skill

## Product Summary

Lindy is an AI teammate that lives in Slack and automates work across email, meetings, calendar, and 1,000+ integrated tools. It handles recurring tasks through **routines** (scheduled automation), **ad hoc tasks** (on-demand requests), and **skills** (reusable playbooks). Lindy connects to tools via **integrations** (OAuth-based, with guardrails) or **credentials** (API keys for custom calls). The primary docs site is https://docs.lindy.ai. Key entry points: Routines page (automation), Integrations page (tool connections), Skills page (custom playbooks), and Settings (workspace controls).

## When to Use

Reach for this skill when:
- A user wants to automate recurring work (daily briefs, email drafting, meeting prep, follow-ups)
- A user needs to connect Lindy to a tool (Gmail, Slack, Notion, HubSpot, or 1,000+ others)
- A user is setting up a team workspace or managing members and credits
- A user wants to teach Lindy a custom skill or routine
- A user is troubleshooting integration issues, guardrails, or approval workflows
- A user needs to delegate one-off tasks (research, scheduling, reminders, CRM updates)
- A user is configuring MCP servers for tools without native integrations

## Quick Reference

### Core Concepts

| Concept | What It Does | When to Use |
|---------|-------------|-----------|
| **Routine** | Saved instruction with trigger, prompt, and destination | Recurring work: daily briefs, email drafting, meeting notes, follow-ups |
| **Ad Hoc Task** | One-off request you ask Lindy to do right now | Research, scheduling, reminders, knowledge lookups, CRM updates |
| **Skill** | Named playbook Lindy picks automatically when relevant | Teach Lindy how you work; it reuses the skill on future requests |
| **Integration** | OAuth or API key connection with guardrails and scope | Connect tools Lindy has native support for (Gmail, Slack, Notion, etc.) |
| **Credential** | Saved API key for custom calls and shell commands | Connect tools without native integrations; used for direct API calls |
| **MCP Server** | Hosted server that exposes tools via Model Context Protocol | Add 100+ tools from a single product without waiting for native integration |

### Routine Triggers

| Trigger Type | Examples |
|--------------|----------|
| **Schedule** | Daily at 9am, weekly Monday, monthly on the 1st |
| **Event** | Email received/sent, Slack message, calendar event starting, meeting ending |
| **Smart Filter** | "Only emails that promise a follow-up", "Only from VIP contacts" |

### Routine Destinations

| Destination | Who Sees It |
|-------------|-----------|
| Default proactive channel | You in Lindy chat |
| Slack DM | Private to you |
| Text message | SMS to verified phone |
| Slack channel | Whole team (workspace routines only) |
| Slack thread reply | Workspace routines only |

### Built-in Routines (Toggle On/Off)

- Daily briefs — prepares you for your day
- Email drafting — drafts replies in your voice
- Email labeling — organizes inbox
- Urgent email alerting — flags time-sensitive emails
- Follow-up bumps — drafts bumps when sent emails get no reply
- Meeting note taking — joins meetings, records, transcribes, summarizes
- Meeting prep — gives context before meetings
- Meeting scheduling — coordinates meetings and proposes times

### Integration Guardrails

| Setting | Behavior |
|---------|----------|
| **Always allow** | Lindy writes without asking |
| **Require approval** | Lindy asks in Slack before writing |
| **Don't offer** | Lindy never writes with this tool |

**Important**: Guardrails only apply to writes in shared Slack threads. Reads are never guarded. DMs, web app chat, iMessage, and SMS bypass guardrails.

### Credential Naming Convention

Use `SCREAMING_SNAKE_CASE`:
- `NOTION_API_KEY` — clear what tool and credential type
- `STRIPE_RESTRICTED_KEY` — distinguishes from full-access key
- Avoid: `key1`, `api_key`, `secret` — too vague

### Slack Commands

| Command | What It Does |
|---------|------------|
| `/lindy usage` | Show Slack diagnostics for last 7 days |
| `/lindy unlink` | Unlink your Slack account from Lindy |

## Decision Guidance

### Integration vs Credential

| Situation | Use Integration | Use Credential |
|-----------|-----------------|----------------|
| Tool is in Lindy's integration list | ✓ | |
| Tool has no native integration | | ✓ |
| You want guardrails and approval workflows | ✓ | |
| You're calling an API directly | | ✓ |
| You want to share with the workspace | ✓ | ✓ |
| You need OAuth flow | ✓ | |

### Personal vs Workspace Routine

| Aspect | Personal | Workspace |
|--------|----------|-----------|
| Who creates | Anyone | Admins only |
| Who sees results | Just you | Whole team |
| Delivery options | Chat, DM, text, channel | Slack thread, channel |
| Use case | Your daily brief, personal reminders | Team digests, shared reports |

### Routine vs Skill

| Aspect | Routine | Skill |
|--------|---------|-------|
| Trigger | Schedule or event | Automatic (when request matches description) |
| Invocation | Runs on its own | Lindy picks it when relevant |
| Use case | Recurring work on a schedule | Reusable playbooks for repeated procedures |
| Example | "Every Monday at 9am, email me my pipeline" | "How I write my weekly report" |

### MCP vs Native Integration

| Aspect | Native Integration | MCP Server |
|--------|-------------------|-----------|
| Setup time | Immediate | Requires server URL |
| Tool count | One tool per integration | 3-100+ tools per server |
| Guardrails | Yes | Yes |
| When to use | Tool is in Lindy's list | Tool publishes MCP but no native integration |

## Workflow

### Setting Up a New Routine

1. **Identify the work**: What recurring task eats your time? (e.g., "I draft email replies every morning")
2. **Open Routines**: Click **Routines** in the left sidebar
3. **Choose scope**: Personal (just you) or Workspace (team, admins only)
4. **Create**: Click **New** or describe it in chat: *"Every weekday at 9, summarize my unread email"*
5. **Set trigger**: Schedule (daily/weekly/monthly) or event (email received, meeting ends, etc.)
6. **Write the prompt**: Plain English instruction of what Lindy should do
7. **Pick destination**: Where results go (chat, DM, text, channel)
8. **Test**: Let it run once, review the output
9. **Refine**: Ask Lindy to adjust: *"Change the time to 8am"* or *"Add more detail to the summary"*

### Connecting a Tool

1. **Check if it's integrated**: Open **Integrations**, search for the tool
2. **If found**: Click it, connect with OAuth or API key, set guardrails
3. **If not found**: Check if the tool publishes an MCP server
4. **If MCP available**: Go to **Integrations**, click **+ Add**, choose **MCP**, paste server URL
5. **If neither**: Create a **Credential** (Settings → Credentials), save the API key, reference it in custom calls
6. **Set scope**: Personal (just you) or shared with workspace (owners/admins only)
7. **Test**: Ask Lindy to use the tool: *"What's in my Notion database?"*

### Teaching Lindy a Skill

1. **Describe the process**: Tell Lindy how you want something handled in chat or on the Skills page
2. **In chat**: *"When I ask for my weekly report, pull numbers from the dashboard, lead with top 3 changes, keep it under a page"*
3. **On Skills page**: Click **New skill**, name it, write a clear description (so Lindy knows when to use it), add instructions
4. **Test**: Ask Lindy to use it: *"Give me my weekly report"*
5. **Refine**: Edit the skill if Lindy misses the mark
6. **Share**: Make it a team skill (admins only) so everyone's Lindy works the same way

### Setting Up Team Workspace

1. **Add Lindy to Slack**: Admin installs from Slack marketplace
2. **Members join**: They @mention Lindy or admin invites them
3. **Set roles**: Go to **Settings → Members and groups**, assign Member/Admin/Owner
4. **Allocate credits**: Click **...** on a member, set monthly credit limit
5. **Enable features**: Go to **Settings → Settings** (under Workspace), toggle features on/off
6. **Create team routines**: Admins create workspace routines that run for everyone
7. **Share integrations**: Owners/admins share personal integrations with the workspace

### Troubleshooting Integration Issues

1. **Check connection status**: Open **Integrations**, look for health indicator (healthy, needs reconnecting, not set up)
2. **Reconnect if expired**: Click the integration, tap **Reconnect** (usually OAuth token expired)
3. **Verify guardrails**: Check if the tool is set to "Always allow", "Require approval", or "Don't offer"
4. **Test in Slack**: Ask Lindy to use the tool in a thread; if guardrail is "Require approval", approve the action
5. **Check permissions**: Verify the connected account has permission to do what you're asking
6. **Rotate credential if needed**: Settings → Credentials, click **...**, choose **Rotate value**

## Common Gotchas

- **Guardrails only block writes in shared Slack threads**: DMs, web chat, iMessage, and SMS bypass guardrails. If you need approval workflows, use shared Slack threads.
- **Routine defaults to 9am daily**: New routines run every day at 9am unless you change the trigger. Check the schedule before you save.
- **Credentials are write-only**: You can't read a credential's value back. Keep the original in a password manager. If lost, rotate it with a fresh key from the tool.
- **MCP server must be publicly reachable**: The server URL must be a public HTTPS endpoint. Private or localhost servers won't work.
- **Skill descriptions drive routing**: If Lindy isn't picking a skill when you expect, make the description more specific about when it applies.
- **Guardrails apply per integration, not per account**: You can't set different rules for your work Gmail and personal Gmail under the same integration.
- **Workspace routines need Lindy Teammate set up**: You can't create workspace routines without Lindy Teammate in Slack.
- **Meeting recording requires calendar connection**: Lindy joins meetings from your calendar. If it's not connected, meeting recording won't work.
- **Smart Filters are plain English**: Use natural language like "only emails that promise a follow-up", not regex or code.
- **Turning off a routine doesn't delete it**: You can toggle routines on/off without removing them. Turn off first, then delete if you're sure.
- **Shared MCP servers affect everyone**: Setting a shared MCP server's availability to "Only you" removes its tools from everyone else's Lindy. Use the Actions toggles first if the problem is noise.

## Verification Checklist

Before submitting work with Lindy:

- [ ] **Routine created**: Trigger is set correctly (schedule or event), prompt is clear, destination is right
- [ ] **Integration connected**: Tool shows "healthy" status, guardrails are set appropriately for the use case
- [ ] **Credential saved**: Named in `SCREAMING_SNAKE_CASE`, visibility is correct (personal or shared)
- [ ] **Skill tested**: Lindy picks it when you expect, instructions are clear enough for routing
- [ ] **Guardrails understood**: If approval is needed, test in a shared Slack thread; if not, DMs work fine
- [ ] **Team setup complete**: Members have roles, credit limits are set, features are enabled
- [ ] **MCP server reachable**: Server URL is public HTTPS, tools appear in the Actions list
- [ ] **No silent failures**: Ask Lindy to use the tool/routine once to confirm it works before declaring done

## Resources

- **Full documentation**: https://docs.lindy.ai/llms.txt — comprehensive page-by-page navigation for agents
- **Integrations & Credentials**: https://docs.lindy.ai/integrations/overview — how to connect tools and set guardrails
- **Routines**: https://docs.lindy.ai/coming-soon/routines — create and manage recurring automation
- **Skills**: https://docs.lindy.ai/coming-soon/skills — teach Lindy custom playbooks

---

> For additional documentation and navigation, see: https://docs.lindy.ai/llms.txt