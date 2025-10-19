---
description: Bootstrap a project with drizzle for PostgreSQL
---

<!-- 
# date -u +"%Y-%m-%dT%H:%M:%SZ"
lastModified: 2025-10-19T15:59:40Z
-->

## Preflight

You **MUST** ensure the following dependencies in `package.json`
- dependency: 
  - `drizzle-orm` >= 0.44.6
  - `postgres` >= 3.4.7
- devDependency: 
  - `drizzle-kit` >= 0.31.5


Verify local versions

```zsh
bun why drizzle-orm
bun why drizzle-kit
```

Check remote versions

```zsh
bun info drizzle-orm
bun info drizzle-kit
```

If not installed or outdated, run:

```zsh
bun i -D drizzle-kit
bun i drizzle-orm
```

## Create Drizzle Config

Create the following file at the root of the project: 

```ts filename="drizzle.config.ts"
import type { Config } from "drizzle-kit";

export default {
  schema: "./lib/database/schema.ts",
  out: "./lib/database/migrations",
  dialect: "postgresql",
  dbCredentials: {
    // PostgreSQL migration issue in AWS RDS with Drizzle ORM: 'no pg_hba.conf entry for host' error
    // https://stackoverflow.com/questions/76818549/postgresql-migration-issue-in-aws-rds-with-drizzle-orm-no-pg-hba-conf-entry-fo
    // [BUG]: dbCredentials.ssl is not working while introspecting #831
    // https://github.com/drizzle-team/drizzle-orm/issues/831
    url: `${process.env.DATABASE_URL!}?sslmode=require`,
    ssl: true,
  },
} satisfies Config;
```

## Initialize schema

Add the following files, creating directories as needed:

```ts filename="lib/database/schema.ts"
import { pgTable, text, timestamp, boolean } from "drizzle-orm/pg-core";

export const testTable = pgTable("test_table", {
  id: text("id").primaryKey(),
  verified: boolean("email_verified").default(false).notNull(),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at")
    .defaultNow()
    .$onUpdate(() => new Date())
    .notNull(),
  deletedAt: timestamp("deleted_at"),
});

```

This fake table helps bootstrap the migration process to make it easy to start running and validating migrations.

## Generate initial migration

```zsh
bunx drizzle-kit generate
``` 