---
description: Initialize BetterAuth in a Next.js project, with postgres.
---

## Preflight

Verify installed dependencies in `package.json`.

Verify local versions

```zsh
bun why better-auth
```

Compare with remote versions

```zsh
bun info better-auth
```
If not installed or outdated, run:
  
```zsh
bun i better-auth@latest
```

## Files

### Create auth config

```ts filename="lib/auth.ts"
import { betterAuth } from "better-auth";
import { drizzleAdapter } from "better-auth/adapters/drizzle";

import { db } from "@/lib/database/db";
import * as schema from "@/lib/database/schema";

export const auth = betterAuth({
  baseURL: process.env.BETTER_AUTH_URL || "http://localhost:3000",
  basePath: "/api/auth",
  secret: process.env.BETTER_AUTH_SECRET || "",
  database: drizzleAdapter(db, {
    provider: "postgres",
    // https://www.better-auth.com/docs/adapters/drizzle#additional-information
    usePlural: true,
  }),
  emailAndPassword: {
    enabled: false,
  },
  socialProviders: {
    google: {
      clientId: process.env.GOOGLE_CLIENT_ID || "",
      clientSecret: process.env.GOOGLE_CLIENT_SECRET || "",
    },
  },
});

export type Session = typeof auth.$Infer.Session;
```

## Add env vars 

Add the following environment variables to your `.env` file:

```zsh

# Added by /betterauth.init.prompt.md on $(date -u +"%Y-%m-%dT%H:%M:%SZ")
BETTER_AUTH_SECRET=$(openssl rand -hex 32)
BETTER_AUTH_URL="http://localhost:3000" # Your APP URL
```

## Generate auth schema

```zsh
bunx @better-auth/cli generate --y \
  --config lib/auth.ts \
  --output lib/auth-schema.ts
```

## Merge contents of `lib/auth-schema.ts` into your existing `lib/database/schema.ts` file.

Help merge the contents of newly created `lib/auth-schema.ts` into your existing `lib/database/schema.ts` file.
