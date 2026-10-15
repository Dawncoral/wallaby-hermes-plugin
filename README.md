# Wallaby for Hermes Agent

A model-provider plugin that puts Wallaby in Hermes' provider list. Pick it in the setup wizard, paste your key, and Hermes runs on Kimi K3 — no YAML editing, no endpoint trivia.

Wallaby is curated inference for open-weight models: per-token billing, itemized statements on every call, and $0.50 of free credit to start. (The "token" in Wallaby Token is the LLM metering unit, not a cryptocurrency.)

## Install

```bash
hermes plugins install Dawncoral/wallaby-hermes-plugin
```

Answer `y` when it asks to enable the plugin (or run `hermes plugins enable wallaby-provider` later).

Or by hand:

```bash
mkdir -p ~/.hermes/plugins/model-providers
git clone https://github.com/Dawncoral/wallaby-hermes-plugin ~/.hermes/plugins/model-providers/wallaby
```

## Set up

Run the wizard and pick **Wallaby**:

```bash
hermes model
```

It asks for your API key (create one at [wallabytoken.com](https://wallabytoken.com) — new accounts get the $0.50 trial credit) and stores it in your keychain. The Base URL defaults to Wallaby's endpoint — press Enter once more to accept it. Then pick `kimi-k3` as the model, press Enter again to confirm, and choose a reasoning effort when prompted (`medium` is a fine default).

Tested on Hermes Agent v0.19.0.

Prefer config files? Two lines in `~/.hermes/config.yaml`, plus one environment variable:

```yaml
model:
  default: kimi-k3
  provider: wallaby
```

```bash
export WALLABY_API_KEY=sk-your-wallaby-key
```

## Verify

```bash
hermes -z "What model are you? Answer in one sentence."
```

A working setup answers immediately and introduces itself as Kimi.

## What this plugin is

Two files. `__init__.py` registers a `ProviderProfile` (name, base URL, env var, fallback model list); `plugin.yaml` declares `kind: model-provider` so Hermes discovers it under `~/.hermes/plugins/`. The setup wizard, `hermes auth`, the model picker, and the transport all wire themselves to that profile — no core changes, no hooks, no telemetry. The plugin makes no network calls of its own; it only tells Hermes where the API is.

User plugins override bundled providers of the same name, so this also works as a patch layer if Wallaby ever ships in-tree.

## Links

- Wallaby Token: https://wallabytoken.com
- Hermes Agent: https://github.com/NousResearch/hermes-agent

## License

MIT
