# Chief-of-Staff Agent

An agent that reads your email, calendar, tasks, the web, and your own data each morning and turns it into one prioritized, actionable briefing.

```
Email + Calendar + Web + Tasks + Custom data → Synthesizer → Daily briefing → Chat / Email / Slack / Notion
```

## Project structure

```
chief-of-staff/
├── CLAUDE.md                          # loads automatically; points to everything below
├── constitution.md                    # who you are, priorities, what to ignore, briefing format
├── .mcp.json                          # MCP servers for tasks (Notion, Linear)
├── .claude/
│   └── skills/
│       └── chief-of-staff/
│           └── SKILL.md               # gather → filter → synthesize → draft → deliver → learn
└── memory/
    ├── preferences.md                 # what the agent should remember
    └── ignored.md                     # what you don't care about
```

## Setup

1. **Connect MCP.**
   - Gmail and Google Calendar: connect them at claude.ai → Settings → Connectors. Claude Code uses them when you're logged in with your claude.ai account.
   - Notion or Linear: open this folder in Claude Code, run `/mcp`, and sign in to the one you use. Remove the other from `.mcp.json`.
   - Web search is built in.
   - Custom data (Stripe, CRM, Sheets, your own API): add its MCP server to `.mcp.json`, or describe the API in the constitution's **Sources** section.
2. **Write the constitution.** Replace every `TODO` in `constitution.md`.
3. **Build the skill.** Already done: `.claude/skills/chief-of-staff/SKILL.md`.
4. **Test manually.** Run `/chief-of-staff`, read the draft briefing, and tell it what to change. Edit `constitution.md` or `SKILL.md` until the briefing is what you want.
5. **Schedule it.** Pick one:
   - **Cloud** (computer can be off): in Claude Code, run `/schedule every day at 06:00 run /chief-of-staff`. Push this repo first, since cloud runs use the pushed version.
   - **Local** (cron): headless runs can't answer permission prompts, so first run `/chief-of-staff` interactively and choose "don't ask again" for each tool. Then add to `crontab -e`:
     ```
     0 6 * * * cd /path/to/chief-of-staff && /path/to/claude -p "/chief-of-staff" >> briefings.log 2>&1
     ```
6. **Add memory.** Reply to a briefing with feedback ("Sarah is my cofounder", "stop showing GitHub notifications"). The agent writes it to `memory/`, and tomorrow's briefing uses it. Commit `memory/` so cloud runs see it too.

## The loop

Run → Briefing → Feedback → Better tomorrow.

The agent only proposes replies, follow-ups, and time blocks. It never sends, accepts, or changes anything except delivering the briefing to you.
