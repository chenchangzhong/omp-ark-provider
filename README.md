# omp-ark-provider

OMP package that registers a Ark Agent Plan API provider.

## Install

npm package: [omp-ark-provider](https://www.npmjs.com/package/omp-ark-provider)

Recommended install from npm:

```bash
omp install npm:omp-ark-provider
```

To pin a specific npm version:

```bash
omp install npm:omp-ark-provider@0.1.2
```

You can also install from GitHub:

```bash
omp install git:github.com/MacroSony/omp-ark-provider
```

For local development:

```bash
omp -e /path/to/omp-ark-provider
```

To try the published package for one run without installing it:

```bash
omp -e npm:omp-ark-provider
```

## Authentication

The provider id is `ark-plan`. OMP will use `~/.omp/agent/auth.json` first, then the `ARK_API_KEY` environment variable as a fallback.

```json
{
  "ark-plan": { "type": "api_key", "key": "your-ark-api-key" }
}
```

Or:

```bash
export ARK_API_KEY=your-ark-api-key
```

## Models

This extension follows the [Ark Plan model list](https://www.volcengine.com/docs/82379/1925114?lang=zh) and registers:

- `ark-code-latest`
- `doubao-seed-2.0-code`
- `doubao-seed-2.0-pro`
- `doubao-seed-2.0-lite`
- `doubao-seed-code`
- `minimax-m2.7`
- `minimax-m3`
- `glm-5.2`
- `glm-latest`
- `deepseek-v4-flash`
- `deepseek-v4-pro`
- `kimi-k2.6`
- `kimi-k2.7-code`

Run `omp --list-models` after installing to confirm the provider is loaded.

## Thinking levels

This extension maps OMP's native thinking selector to provider-specific Chat Completions controls:

- OMP `off` -> `thinking: { "type": "disabled" }`
- Most Ark reasoning models: OMP `low`, `medium`, `high` -> `thinking: { "type": "enabled" }` plus matching `reasoning_effort`
- OMP `minimal` is hidden because Ark's `reasoning_effort: "minimal"` means no thinking; selecting it clamps to `low`
- OMP `xhigh` -> `reasoning_effort: "max"` only for models verified to accept it (`ark-code-latest`, `deepseek-v4-flash`, `deepseek-v4-pro`)
- `glm-5.2` and `glm-latest` use the GLM/Z.AI thinking shape: OMP `low`, `medium`, and `high` send `thinking: { "type": "enabled" }` without `reasoning_effort`; OMP `off` sends disabled.
