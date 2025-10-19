---
description: Bootstrap a project with prettier
---

## Preflight

Verify installed dependencies in `package.json`.

Verify local versions

```zsh
bun why prettier
bun why prettier-plugin-tailwindcss
```

Compare with remote versions

```zsh
bun info prettier
bun info prettier-plugin-tailwindcss
```
If not installed or outdated, run:
  
```zsh
bun i -D prettier@latest
bun i -D prettier-plugin-tailwindcss@latest
```

## Create Prettier Config

Create the following file at the root of the project: 

```json filename=".prettierrc.json"
{
  "semi": true,
  "trailingComma": "all",
  "printWidth": 100,
  "arrowParens": "always",
  "tabWidth": 2,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```
