---
description: Install `ai` sdk
---

## Preflight

Verify local versions

```zsh
bun why ai
bun why @ai-sdk/openai
bun why @ai-sdk/anthropic
```

Compare with remote
versions

```zsh
bun info ai
bun info @ai-sdk/openai
bun info @ai-sdk/anthropic
```
If not installed
or outdated, run:

```zsh
bun i ai@latest
bun i @ai-sdk/openai@latest
bun i @ai-sdk/anthropic@latest
```
If not installed
or outdated, run:

```zsh
bun i -D aisdk@latest
bun i -D @ai-sdk/openai@latest
```

## Add env variables 

```zsh
for var in OPENAI_API_KEY ANTHROPIC_API_KEY; do rg -q "^$var=" .env && echo "$var already exists in .env" || { echo "$var not found, adding..."; echo -e "\n# Added by /aisdk.init.prompt.md on $(date -u +"%Y-%m-%dT%H:%M:%SZ")\n$var=FIXME" >> .env; }; done
```

## Docs

Prompt the user to read the docs for further information: https://github.com/vercel/ai
