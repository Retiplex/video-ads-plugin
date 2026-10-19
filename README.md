# Retiplex plugin

Research a brand like a UGC creative strategist, then make the video ad with
[Retiplex](https://app.retiplex.com): script, AI actor, storyboard and the finished
vertical video. One install gives your agent both the Retiplex connector and the
skill that drives it.

Give your agent a product URL and say "make an ad for this". It will:

1. **Research** the product page, reviews, Reddit and competitors, and collect real
   customer language and proof.
2. **Propose two or three angles** with hooks, and let you pick one plus language,
   quality and length.
3. **Build the ad in Retiplex**, check the script against the research, show you the
   storyboard, confirm the cost and render the video.

No invented testimonials or numbers: every claim in the script traces back to
something the research found.

## What's inside

| Component | What it does |
|---|---|
| Connector `retiplex` | Remote MCP server `https://app.retiplex.com/mcp` (Streamable HTTP, OAuth sign-in). Creates projects, casts the actor, writes the script, plans the storyboard and renders the video on your Retiplex account. |
| Skill `retiplex-ugc-ad` | The research-to-ad workflow: research playbook, angle selection, mapping research to Retiplex's brief, actor and script, cost checks. |

## Install

**Claude Code**

```
/plugin marketplace add <owner>/<repo>
/plugin install retiplex@retiplex
```

Then run `/mcp`, choose Retiplex and sign in.

**Claude (desktop and web)**: install the `.plugin` file from the plugin directory
or upload it. The first time the skill calls Retiplex, a browser window asks you to
sign in and approve access.

**Other agents**: add the remote MCP server `https://app.retiplex.com/mcp` and copy
`skills/retiplex-ugc-ad` wherever your agent loads skills. Setup for ChatGPT,
Cursor, VS Code and others: https://app.retiplex.com/help/agents

## Requirements

- A Retiplex account: https://app.retiplex.com/sign-up
- A paid plan to render storyboards and videos (a free account can set up the
  project). Everything is charged in credits, exactly as in the app, and the agent
  tells you the cost before each step.
- Web search or web fetch in your agent, for the research.

## Files

```
.claude-plugin/plugin.json        plugin manifest
.claude-plugin/marketplace.json   lets Claude Code install it from this repo
.mcp.json                         the Retiplex connector
skills/retiplex-ugc-ad/           the skill and its references
```

To disconnect, open Account → Connected agents in Retiplex.
Questions: support@retiplex.com
