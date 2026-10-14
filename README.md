# Liftory for Claude

Liftory is a training and coaching app. This plugin connects Claude to your Liftory account through Liftory's
remote MCP server, `https://mcp.liftory.ch/mcp`, and adds skills that teach Claude the common Liftory workflows.
It works in Claude chat, Cowork and Claude Code.

For athletes:
- Create a training plan from a description, using Liftory's exercise catalog in five languages.
- Review your current training and nutrition plans and your recent workouts.
- Read and answer messages from your coach.

For coaches:
- See the clients you coach, their activity and adherence.
- Create and review training and nutrition plans for a client.
- Read and answer client conversations.

## Install

**From Anthropic's directory** (once Liftory is listed there):
- In Claude (web, desktop or mobile), open **Customize > Plugins > Discover**, search for Liftory and add it.
- In Claude Code, run `/plugin directory` in a session and pick Liftory. A plugin you add on claude.ai also reaches
  Claude Code on its own, as a synced plugin.

**Manually**, from this repository:
- Claude (chat and Cowork): download this repository as a ZIP (Code, Download ZIP), then open
  **Customize > Plugins > Add > Upload plugin** and select the ZIP.
- Claude Code: this repository is also a plugin marketplace that lists Liftory, so in a session run

  ```text
  /plugin install liftory --marketplace liftory/liftory-claude-plugin
  ```

  and choose a scope (Claude Code 2.1.275 or later; on an earlier version, run
  `/plugin marketplace add liftory/liftory-claude-plugin` first, then `/plugin install liftory@liftory`). To try a local copy for
  one session instead, clone the repository and run `claude --plugin-dir ./liftory`. In the session, `/mcp` shows
  the Liftory server and its connection state.

## Sign in

The plugin holds no key or password. The Liftory server signs you in with OAuth, with your own Liftory account:
- In Claude chat and Cowork, open the plugin's **Connectors** tab and connect Liftory. If you already added the
  Liftory connector, the plugin uses the same server, so you see one set of Liftory tools, not two.
- In Claude Code, run `/mcp`, select the Liftory server and choose to authenticate.

Either way, you sign in on Liftory's own page and approve the connection on Liftory's consent page.

You stay in control:
- Claude acts only as you, with your own Liftory permissions.
- A coach can only reach clients who currently grant them coaching access.
- The skills show you each change and ask before saving a plan, activating a menu or sending a message, and every
  tool that changes something is marked as a write, so Claude asks before it runs.
- You can disconnect at any time in the Liftory app under Settings, Connected assistants.

## What the plugin sends and runs

The plugin contains only Markdown skills, a manifest, an icon and the server address. It runs no script, hook or
local program and installs no package. When you use a Liftory tool, Claude sends the tool's arguments (for example
a plan you approved, a search term or a message you approved) to `https://mcp.liftory.ch/mcp`, which reads and
writes your data in your Liftory account at Liftory's API. Nothing is sent anywhere else. How Liftory handles your
data is in the [privacy policy](https://liftory.ch/privacy).

## Skills

| Skill | What it does |
|---|---|
| `build-training-plan` | Designs a new training plan from your requirements, for you or a client |
| `review-training-plan` | Reads a plan and the workout history, and saves an adapted plan as a new one |
| `digitize-training-plan` | Transcribes a coach's training plan from a document or picture |
| `build-nutrition-plan` | Puts together a menu from your targets and preferences |
| `digitize-nutrition-plan` | Transcribes a coach's menu from a document or picture |
| `answer-training-questions` | Answers a fitness question from Liftory's knowledge base and exercise catalog |
| `message-inbox` | Summarizes your conversations, drafts a reply, and sends it after you approve it |

## Links

- Website: https://liftory.ch
- Privacy policy: https://liftory.ch/privacy
- Terms: https://liftory.ch/terms
- Contact: info@liftory.ch
