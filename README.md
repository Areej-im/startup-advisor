# Startup Advisor for Claude

A plugin that turns Claude into a startup advisor. Once added, every time you ask about your startup, Claude answers using a six-stage founder guide built on Y Combinator, Paul Graham, Steve Blank, Lean Startup, MIT, and a16z:

1. The decision to start
2. The idea and its evaluation
3. Building the founding team
4. Planning the MVP
5. Launch
6. Growth and metrics

Answers come in the language you write in, Arabic or English.

## Install

Pick the option that matches how you use Claude. Options 1 and 2 keep you on the latest version automatically.

### Option 1: Claude on the web or desktop app (paid plans)

1. Open [claude.ai](https://claude.ai) and click **Customize** in the left sidebar.
2. Open the **Plugins** tab, click **Add**, and choose **Add marketplace**.
3. Enter:
   ```
   YOUR-GITHUB-USERNAME/startup-advisor
   ```
4. Find **startup-advisor** in the list and click **Add**.
5. Open the marketplace and turn on **Sync automatically** so you get every update.

> Button names may differ slightly depending on your app version.

### Option 2: Claude Code

Run these two commands inside Claude Code:

```
/plugin marketplace add YOUR-GITHUB-USERNAME/startup-advisor
/plugin install startup-advisor@startup-advisor-marketplace
```

To pull updates later:

```
/plugin marketplace update startup-advisor-marketplace
```

### Option 3: Any plan, manual upload (no automatic updates)

1. Download `startup-advisor.zip` from the [`dist`](dist) folder. **Do not unzip it.**
2. In claude.ai, go to **Customize → Skills**, click **+ Add**, and upload the zip as it is.
3. To update, download the new zip and upload it again.

## How to use it

Open a new chat and ask about your startup in plain language. Claude switches the advisor on by itself when your question is about a startup.

To call it directly, type `/` in the message box and pick **startup-advisor**, or type `/startup-advisor:advisor` in Claude Code.

## Example questions

- "I have an idea for a sports-field booking app in Jeddah. Can you evaluate it?"
- "Give me questions to interview 5 potential customers."
- "I'm bringing in a co-founder. How do we split equity fairly?"
- "What's the smallest MVP I can build to test my idea?"
- "200 people signed up but nobody comes back. What's wrong?"
- "Here are our monthly numbers. Have we reached product-market fit?"

## What to expect

- It identifies which of the six stages you're in.
- It asks one or two questions if information is missing.
- It separates compliments from real evidence and tells you honestly where your evidence stands.
- It ends with one step you can take this week.

The more detail you give, the sharper the advice: who exactly your customer is, what you've tried, and what happened.

## Repository layout

```
.claude-plugin/marketplace.json          # Marketplace catalog
plugins/startup-advisor/
├── .claude-plugin/plugin.json           # Plugin manifest
└── skills/advisor/
    ├── SKILL.md                         # How the advisor works and which file to read
    └── references/                      # The six-stage guide
        ├── 01-decision.md
        ├── 02-idea.md
        ├── 03-founding-team.md
        ├── 04-mvp.md
        ├── 05-launch.md
        └── 06-growth-metrics.md
dist/startup-advisor.zip                 # Standalone skill for manual upload
```

## For maintainers: shipping an update

1. Edit files under `plugins/startup-advisor/skills/advisor/`.
2. Rebuild the zip for Option 3: `cd plugins/startup-advisor/skills && rm -f ../../../dist/startup-advisor.zip && cp -r advisor /tmp/startup-advisor && sed -i 's/^name: advisor$/name: startup-advisor/' /tmp/startup-advisor/SKILL.md && (cd /tmp && zip -rqX - startup-advisor) > ../../../dist/startup-advisor.zip && rm -rf /tmp/startup-advisor`
3. Commit and push. `plugin.json` has no `version` field on purpose, so every commit counts as a new version and users with automatic sync get it without any version bump.
