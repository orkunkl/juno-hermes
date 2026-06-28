# juno-hermes

[Hermes Agent](https://hermes-agent.nousresearch.com) **profile distribution** for juno network. It wraps the [juno-network-skill](https://github.com/CosmosContracts/juno-network-skill) into a ready-to-use Juno operations agent for hermes.

Out of the box you get a CosmWasm + DAO DAO–literate agent that can query chain state, sign transactions, manage sub-DAOs, and operate under a VetoConfig timelock..

---

## What's inside

```
juno-hermes/
├── distribution.yaml        # Hermes manifest (name, version, env requirements)
├── SOUL.md                  # Agent personality — terse, Cosmos-literate, safety-first
├── config.yaml              # Model + tool defaults
├── skills/
│   └── juno-network/        # ← git submodule: CosmosContracts/juno-network-skill
│       ├── SKILL.md
│       ├── references/      # chain.md, dao-dao.md, cosmwasm.md, transactions.md, …
│       └── scripts/         # instantiate-subdao.sh, propose-bank-send.sh, verify-tx.sh
├── cron/                    # (optional) scheduled jobs
└── mcp.json                 # (optional) MCP server connections
```

The skill is tracked as a **git submodule** — upstream updates from `CosmosContracts/juno-network-skill` pull in with `git submodule update --remote --merge`.

---

## Install

```bash
hermes profile install github.com:<USER>/juno-hermes --alias
```

This clones the distribution, writes the manifest, and creates a `juno-hermes` command alias. Your memories, sessions, and API keys stay untouched.

### After install

```bash
# Fill in required env vars
cp ~/.hermes/profiles/juno-hermes/.env.EXAMPLE ~/.hermes/profiles/juno-hermes/.env
# Edit .env with your values

# Start chatting
juno-hermes chat
```

---

## Configuration

### Required environment variables

| Variable | Description | Default |
|---|---|---|
| `JUNO_KEYRING_DIR` | Path to `junod` keyring-test directory | *(required)* |
| `JUNO_RPC` | Juno RPC endpoint | `https://juno-rpc.publicnode.com:443` |
| `JUNO_CHAIN_ID` | Chain ID for tx broadcast | `juno-1` |

Set them in `~/.hermes/profiles/juno-hermes/.env` or export them in your shell profile.

### Optional

| Variable | Description |
|---|---|
| `JUNO_GAS_PRICES` | Override default gas price (`0.075ujuno`) |
| `JUNO_GAS_ADJUSTMENT` | Override default gas adjustment (`1.4`) |

### Model

Default is `owl-alpha` via OpenRouter. Override in `config.yaml`:

```yaml
model:
  default: owl-alpha
  provider: openrouter
```

Or switch at runtime: `/model gpt-5` inside the chat.

---

## Updating

When a new version is tagged upstream:

```bash
hermes profile update juno-hermes
```

This replaces distribution-owned files (SOUL.md, skills, cron, config) while preserving your local `config.yaml` overrides, memories, sessions, and `.env`.

To force-reset config to the distribution default:

```bash
hermes profile update juno-hermes --force-config --yes
```

---

## Usage examples

```
> Query my JUNO balance
> Delegate 10 JUNO to node101
> Show me open proposals on DAO juno1…
> Create a sub-DAO with a 48h veto window
> Propose a bank send of 100 JUNO to juno1…
> Upload this wasm contract
```

The agent reads `skills/juno-network/SKILL.md` and its references automatically when it detects Juno-related context.

---

## Updating the bundled skill

To pull the latest `juno-network-skill` into your local checkout:

```bash
cd ~/.hermes/profiles/juno-hermes
git submodule update --remote --merge
git add skills/juno-network
git commit -m "bump juno-network-skill to latest"
git push
```

---

## Uninstall

```bash
hermes profile delete juno-hermes
```

This removes the profile, alias, and all local data. Re-install anytime with the install command above.

---

## License

MIT. The bundled `juno-network-skill` retains its own license.
