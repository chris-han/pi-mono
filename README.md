<!-- OSS_WEEKEND_START -->
# 🏖️ OSS Weekend

**Issue tracker reopens Monday, April 13, 2026.**

OSS weekend runs Thursday, April 2, 2026 through Monday, April 13, 2026. New issues and PRs from unapproved contributors are auto-closed during this time. Approved contributors can still open issues and PRs if something is genuinely urgent, but please keep that to pressing matters only. For support, join [Discord](https://discord.com/invite/3cU7Bz4UPx).

> _Current focus: at the moment i'm deep in refactoring internals, and need to focus._
<!-- OSS_WEEKEND_END -->

---

<p align="center">
  <a href="https://shittycodingagent.ai">
    <img src="https://shittycodingagent.ai/logo.svg" alt="pi logo" width="128">
  </a>
</p>
<p align="center">
  <a href="https://discord.com/invite/3cU7Bz4UPx"><img alt="Discord" src="https://img.shields.io/badge/discord-community-5865F2?style=flat-square&logo=discord&logoColor=white" /></a>
  <a href="https://github.com/badlogic/pi-mono/actions/workflows/ci.yml"><img alt="Build status" src="https://img.shields.io/github/actions/workflow/status/badlogic/pi-mono/ci.yml?style=flat-square&branch=main" /></a>
</p>
<p align="center">
  <a href="https://pi.dev">pi.dev</a> domain graciously donated by
  <br /><br />
  <a href="https://exe.dev"><img src="packages/coding-agent/docs/images/exy.png" alt="Exy mascot" width="48" /><br />exe.dev</a>
</p>

# Pi Monorepo

> **Looking for the pi coding agent?** See **[packages/coding-agent](packages/coding-agent)** for installation and usage.

Tools for building AI agents and managing LLM deployments.

## Packages

| Package | Description |
|---------|-------------|
| **[@mariozechner/pi-ai](packages/ai)** | Unified multi-provider LLM API (OpenAI, Anthropic, Google, etc.) |
| **[@mariozechner/pi-agent-core](packages/agent)** | Agent runtime with tool calling and state management |
| **[@mariozechner/pi-coding-agent](packages/coding-agent)** | Interactive coding agent CLI |
| **[@mariozechner/pi-mom](packages/mom)** | Slack bot that delegates messages to the pi coding agent |
| **[@mariozechner/pi-tui](packages/tui)** | Terminal UI library with differential rendering |
| **[@mariozechner/pi-web-ui](packages/web-ui)** | Web components for AI chat interfaces |
| **[@mariozechner/pi-pods](packages/pods)** | CLI for managing vLLM deployments on GPU pods |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines and [AGENTS.md](AGENTS.md) for project-specific rules (for both humans and agents).

## Development

```bash
npm install          # Install all dependencies
npm run build        # Build all packages
npm run check        # Lint, format, and type check
./test.sh            # Run tests (skips LLM-dependent tests without API keys)
./pi-test.sh         # Run pi from sources (can be run from any directory)
```

> **Note:** `npm run check` requires `npm run build` to be run first. The web-ui package uses `tsc` which needs compiled `.d.ts` files from dependencies.

## License

MIT

---

## LLM provider in pi:

  Configuration Methods

  1. Environment Variables (Quickstart)

  # Anthropic (default)
  export ANTHROPIC_API_KEY=sk-ant-...
  pi

  # OpenAI
  export OPENAI_API_KEY=sk-...
  pi --provider openai --model gpt-4o

  # Google Gemini
  export GEMINI_API_KEY=...
  pi --provider google

  # Azure OpenAI
  export AZURE_OPENAI_API_KEY=...
  export AZURE_OPENAI_BASE_URL=https://your-resource.openai.azure.com
  pi --provider azure-openai

  # AWS Bedrock
  export AWS_PROFILE=your-profile
  # or
  export AWS_ACCESS_KEY_ID=...
  export AWS_SECRET_ACCESS_KEY=...
  pi --provider amazon-bedrock

  # Other providers: GROQ_API_KEY, CEREBRAS_API_KEY, XAI_API_KEY, etc.

  2. Auth File (~/.pi/agent/auth.json)

  Store credentials persistently (file created with 0600 permissions):

  {
    "anthropic": { "type": "api_key", "key": "sk-ant-..." },
    "openai": { "type": "api_key", "key": "sk-..." },
    "google": { "type": "api_key", "key": "..." }
  }

  The key field supports:
  - Literal value: "sk-ant-..."
  - Shell command: "!op read 'op://vault/item/credential'" (1Password)
  - Env var reference: "MY_ANTHROPIC_KEY" (uses that env var's value)

  3. OAuth / Subscription Login

  pi
  /login  # Then select: Claude Pro/Max, ChatGPT Plus/Pro, GitHub Copilot, Google Gemini
  CLI

  4. CLI Flags (One-time override)

  pi --provider openai --model gpt-4o --api-key sk-... "Hello"

  5. Model Selection

  # Interactive selector
  pi
  /model

  # Or via CLI
  pi --model claude-sonnet-4-6
  pi --model openai/gpt-4o           # provider/id format
  pi --model sonnet:high              # with thinking level

  Resolution Priority

  1. CLI --api-key flag
  2. ~/.pi/agent/auth.json entry
  3. Environment variable
  4. Custom provider keys from ~/.pi/agent/models.json

  Custom Providers

  Add Ollama, LM Studio, or other compatible APIs via ~/.pi/agent/models.json. See
  docs/models.md for details.

  For providers needing custom OAuth or APIs, create an extension. See
  examples/extensions/custom-provider-anthropic/.