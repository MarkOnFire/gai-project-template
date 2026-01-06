# CLAUDE.md

> **See [AGENTS.md](./AGENTS.md)** for complete project instructions.

This file contains only Claude Code-specific notes. All general project documentation, commands, agent definitions, and architecture information is in AGENTS.md.

## Claude Code Features

### TodoWrite for Task Tracking

When working on multi-step tasks, use the TodoWrite tool to track progress. This helps maintain visibility into task completion and demonstrates thoroughness.

**When to use TodoWrite:**
- Complex multi-step tasks (3+ distinct steps)
- Non-trivial and complex tasks requiring careful planning
- User explicitly requests todo list
- User provides multiple tasks to complete

**When NOT to use:**
- Single straightforward task
- Trivial tasks (< 3 steps)
- Purely conversational requests

### Long-Running Development Sessions

For projects using the long-running development pattern (see AGENTS.md), Claude Code agents should:

1. **Use TodoWrite** to track feature implementation steps
2. **Read session files first** (claude-progress.txt, feature_list.json)
3. **Run init.sh** before starting work
4. **Test thoroughly** before marking features complete
5. **Update progress log** at end of session
6. **Commit with clean tree** before ending session

### Browser Automation Testing

For web applications, use MCP Puppeteer for verification testing.

**Setup** (add to `.claude/settings.json`):
```json
{
  "mcpServers": {
    "puppeteer": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  }
}
```

Coding agents should use browser tools to:
- Navigate to pages and verify rendering
- Fill forms and submit data
- Click buttons and test interactions
- Verify end-to-end workflows
- Capture screenshots for documentation

## Available MCP Servers

Model Context Protocol (MCP) servers extend Claude's capabilities with specialized tools. These are deployed globally and available in all projects.

### CLI Agent Server (Multi-LLM Code Review & Queries)

**Purpose**: Get independent perspectives from other AI models (Gemini, Claude CLI, Codex) for code review, architecture decisions, or general queries. **Cost-effective alternative to parallel agent execution.**

**Location**: `/Users/mriechers/Developer/the-lodge/mcp-servers/cli-agent-server/`
**Status**: ✅ Active and deployed globally
**HTTP API**: Available at `http://localhost:3001` (runs automatically via launchd)

**Why Use This Instead of Parallel Agents:**
- **Lower cost**: Uses local CLI agents with your existing API keys
- **Faster**: Direct CLI execution without agent overhead
- **Simpler**: Single tool call vs. complex agent coordination
- **Multi-LLM**: Access Gemini, Claude, and Codex perspectives in one place

**Available Tools:**

1. **`codex_review_code`** - Independent code review with structured feedback
   ```
   # Automatically available when you say:
   "Review this code using Codex"

   # Provides:
   - Security vulnerability detection
   - Performance issue identification
   - Code style and maintainability analysis
   - Severity-ranked issues with line numbers
   ```

2. **`query_agent`** - Ask any CLI agent a question
   ```
   # Usage examples:
   "Ask Gemini: What's the best caching strategy for this API?"
   "Get Claude's opinion on this architecture decision"
   "Ask Codex to explain this algorithm"

   # Parameters:
   - prompt: Your question (required)
   - agent: "claude", "gemini", or "codex" (default: claude)
   - model: Optional model override
   - system_prompt: Optional instructions
   ```

3. **`query_multiple_agents`** - Get consensus from multiple AIs
   ```
   # Usage:
   "Ask all agents: Should I use Redis or Memcached for caching?"

   # Automatically synthesizes responses or shows individual perspectives
   # Much cheaper than spawning parallel agent tasks!
   ```

4. **`create_agent_session`** - Start persistent conversation
5. **`send_message_to_session`** - Continue conversation
6. **`get_session_history`** - Review past messages
7. **`close_session`** - End conversation
8. **`share_context_with_agent`** - Inject files/data into session
9. **`aggregate_agent_responses`** - Synthesize multiple responses
10. **`list_active_sessions`** - Show all open conversations

**When to Use CLI Agent Server vs. Parallel Agents:**

✅ **Use CLI Agent Server for:**
- Code review from different AI perspective
- Getting "second opinions" on architecture decisions
- Exploring different approaches to a problem
- Quick queries that don't need full agent context
- Cost-sensitive multi-LLM queries

❌ **Use Parallel Agent Tasks for:**
- Complex multi-step tasks requiring tool use
- Tasks needing access to filesystem/bash/edit tools
- Long-running autonomous work
- Tasks requiring agent-specific specialization

**HTTP API Endpoints:**

See `the-lodge/mcp-servers/cli-agent-server/CLAUDE.md` for complete HTTP API documentation, including:
- `POST /query` - Query single agent
- `POST /query-multiple` - Query multiple agents
- `POST /code-review` - Get structured code review
- `POST /session/create` - Start conversation session
- `POST /session/message` - Send message to session
- And more...

**Configuration:**
- Already deployed globally via `the-lodge/scripts/deploy_mcp_configs.sh`
- API keys stored in macOS Keychain (service: `developer.workspace.*`)
- Logs: `/Users/mriechers/Developer/the-lodge/mcp-servers/cli-agent-server/logs/`

**Example Workflows:**

```
# Code review workflow
1. "Review this function using Codex"
2. Get structured feedback with security/performance/style issues
3. Apply fixes based on recommendations

# Architecture decision workflow
1. "Ask Gemini and Claude: What's the best state management approach for React?"
2. Get synthesized consensus or individual perspectives
3. Use aggregate_agent_responses to combine insights

# Multi-turn exploration
1. create_agent_session with Gemini
2. send_message_to_session: "How does caching work in Redis?"
3. send_message_to_session: "Show me an example with Node.js"
4. close_session when done
```

### Other Available MCP Servers

**uw-policy-agent**: UW-Madison HR policy search
**editorial-assistant**: PBS Wisconsin video metadata and transcripts
**obsidian-vault**: Access and search Obsidian notes
**claude-chat-export**: Export Claude conversation threads
**fantastical**: Calendar event management

See `the-lodge/CLAUDE.md` for complete MCP server documentation.

## Secrets Management

**IMPORTANT**: API keys are stored in macOS Keychain, NOT .env files.

When a project needs API keys:
1. Ask user to add secret to Keychain: `security add-generic-password -s "developer.workspace.KEY_NAME" -a "$USER" -w`
2. Retrieve in Python: `from scripts.keychain_secrets import KeychainSecrets; secrets = KeychainSecrets(); key = secrets.get("KEY_NAME")`
3. Retrieve in shell: `./scripts/get-secret.sh KEY_NAME`

**Never**:
- Write secrets to .env files
- Commit secrets to git
- Store secrets in code

See `/Users/mriechers/Developer/the-lodge/conventions/SECRETS_MANAGEMENT.md` for complete documentation.

## Initial Agent Workflow

When first working in a project derived from this template:
1. **Invoke agent-bootstrap-guide** to guide through the setup process, OR:
2. Ask the repository owner to clarify the project's purpose and which documentation needs to be collected
3. Use Crawl4AI via `scripts/crawl_docs.py` to harvest requested documentation sources (or invoke crawl4ai-knowledge-harvester)
4. Save gathered references in `knowledge/` with clear source attribution
5. Document the knowledge update strategy (cadence, tooling, ownership) in the project README
6. Stage and commit the onboarding work; ask owner to configure remote if needed

## Notes for Claude Code

1. **Read AGENTS.md first** - Contains all project-specific instructions, commands, and architecture
2. **Use TodoWrite appropriately** - Only for multi-step tasks, not single-step operations
3. **Leverage MCP servers** - CLI Agent Server provides cost-effective multi-LLM queries
4. **Follow long-running pattern** - When implementing features via coding-agent, follow strict guardrails
5. **Test thoroughly** - Use browser automation (MCP Puppeteer) for web apps
6. **Secrets in Keychain** - Never write API keys to .env files
7. **Commit with attribution** - Include `[Agent: <name>]` in all commits
