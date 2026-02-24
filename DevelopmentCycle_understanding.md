# Development Cycle Understanding - The God's Word for Solopreneur Web Development

> **Source:** Comprehensive analysis of [coleam00/link-in-bio-page-builder](https://github.com/coleam00/link-in-bio-page-builder)
> 
> **Purpose:** This document serves as the definitive guide for building professional web applications as a solopreneur. Follow these patterns, methodologies, and best practices religiously.

---

## Table of Contents

1. [Philosophy & Mindset](#philosophy--mindset)
2. [Technology Stack Selection](#technology-stack-selection)
3. [Project Scaffolding Phase](#project-scaffolding-phase)
4. [Planning & Requirements Phase](#planning--requirements-phase)
5. [Database Design Phase](#database-design-phase)
6. [Authentication Phase](#authentication-phase)
7. [API Design Phase](#api-design-phase)
8. [Frontend Development Phase](#frontend-development-phase)
9. [Testing Phase](#testing-phase)
10. [Deployment Phase](#deployment-phase)
11. [Code Quality & Standards](#code-quality--standards)
12. [Naming Conventions](#naming-conventions)
13. [File Organization](#file-organization)
14. [Security Best Practices](#security-best-practices)
15. [Performance Optimization](#performance-optimization)
16. [Common Patterns & Recipes](#common-patterns--recipes)

---

## Philosophy & Mindset

### Core Principles

1. **Atomic Tasks:** Break everything into atomic, testable units. Each task should be independently verifiable.
2. **Validate Early, Validate Often:** Never write code without immediate validation. Each task ends with a validation step.
3. **Documentation First:** Write plans before code. If you can't explain it, you can't build it.
4. **User Stories Drive Everything:** Start with user stories, derive technical tasks from them.
5. **Local State, Explicit Save:** Don't auto-save. Give users control. Simpler, fewer race conditions.
6. **Security by Design:** Never add auth as an afterthought. Design security from day one.
7. **Type Safety is Non-Negotiable:** Use TypeScript strictly. No `any`, proper inference everywhere.
8. **Testing is Mandatory:** Unit tests for logic, E2E tests for user journeys.

### The "God's Word" Approach

- **This document is canonical:** When in doubt, check here first.
- **Follow patterns exactly:** Don't reinvent. The repository shows battle-tested patterns.
- **Every decision has a reason:** If you deviate, understand why the pattern exists.

---

## Technology Stack Selection

### Recommended Stack (As of Feb 2026)

#### Frontend Framework
- **Next.js 15+ (App Router)**
  - `src/` directory structure
  - Server Components by default
  - Route Groups for logical organization: `(auth)`, `(dashboard)`
  - **CRITICAL:** Version >= 15.2.3 to avoid CVE-2025-29927

#### Styling
- **Tailwind CSS v4** (NOT v3)
  - CSS-based config, no `tailwind.config.js`
  - `@import "tailwindcss"` in `globals.css`
  - Use `@tailwindcss/postcss` PostCSS plugin

#### UI Components
- **shadcn/ui** (not npm packages, copy-paste)
  - Install with `npx shadcn@latest add <component>`
  - Components live in `src/components/ui/`
  - Extensible, customizable, uses Radix UI primitives

#### Database & ORM
- **Neon Postgres** (serverless PostgreSQL)
- **Drizzle ORM** (NOT Prisma, NOT TypeORM)
  - Schema-first: define tables in TypeScript
  - Type-safe queries out of the box
  - `drizzle-orm/neon-http` driver (simpler than serverless)

#### Authentication
- **Neon Auth** (`@neondatabase/auth`)
  - Beta package (v0.1.0-beta.20+ as of Feb 2026)
  - Handles user management, sessions, OAuth
  - Zero-config Google OAuth in dev
  - Custom fields stored in your own tables

#### Validation
- **Zod** (runtime type validation)
  - All API inputs validated
  - All client-side forms validated
  - Single source of truth for data shapes

#### Drag & Drop
- **dnd-kit** (NOT react-beautiful-dnd)
  - `@dnd-kit/core`, `@dnd-kit/sortable`, `@dnd-kit/utilities`
  - Better accessibility, smaller bundle, more flexible

#### Testing
- **Vitest** (unit tests)
  - Fast, Jest-compatible
  - `jsdom` environment for React tests
- **Vercel Agent Browser CLI** (E2E)
  - `agent-browser` npm package
  - NOT Playwright test files
  - Bash shell scripts with CLI commands

#### Code Quality
- **Biome** (linter + formatter, replaces ESLint + Prettier)
  - Faster, integrated tool
  - Tab indentation, 100 char line width
  - Double quotes, trailing commas, semicolons always

#### Development Tools
- **TypeScript** strict mode
- **Git** with meaningful commits
- **Drizzle Kit** (DB migrations, studio)

### Why This Stack?

1. **All-in-One Ecosystem:** Next.js + Vercel tooling is battle-tested
2. **Type Safety End-to-End:** TypeScript + Drizzle + Zod
3. **Developer Experience:** Fast iterations, great DX
4. **Scalability:** Neon scales, Next.js scales, patterns scale
5. **Cost:** Generous free tiers, serverless pricing
6. **Community:** Massive support, easy to find help

---

## Project Scaffolding Phase

### Phase Goals

Get from empty directory to running dev server with all tooling configured.

### Step-by-Step Process

#### 1. Create Next.js Project

```bash
npx create-next-app@latest . --typescript --tailwind --biome --app --src-dir --import-alias "@/*" --yes
```

**Flags Explained:**
- `--typescript`: Enable TypeScript
- `--tailwind`: Enable Tailwind CSS (will upgrade to v4)
- `--biome`: Use Biome instead of ESLint
- `--app`: Use App Router (not Pages)
- `--src-dir`: Use `src/` directory
- `--import-alias "@/*"`: Set import alias
- `--yes`: Auto-confirm all prompts

#### 2. Upgrade to Tailwind CSS v4

```bash
npm uninstall tailwindcss postcss autoprefixer
npm install tailwindcss @tailwindcss/postcss postcss
rm -f tailwind.config.ts
```

Create `postcss.config.mjs`:
```js
const config = {
  plugins: {
    "@tailwindcss/postcss": {},
  },
};
export default config;
```

Update `src/app/globals.css`:
```css
@import "tailwindcss";
```

**Validation:** `npm run dev` starts without CSS errors.

#### 3. Configure Biome

Update `biome.json`:
```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "vcs": { "enabled": true, "clientKind": "git", "useIgnoreFile": true },
  "files": { "ignore": ["node_modules", ".next", "dist", "build", "drizzle"] },
  "formatter": { "enabled": true, "indentStyle": "tab", "lineWidth": 100 },
  "linter": { "enabled": true, "rules": { "recommended": true } },
  "javascript": {
    "formatter": { "quoteStyle": "double", "trailingCommas": "all", "semicolons": "always" }
  }
}
```

**Validation:** `npm run lint` passes.

#### 4. Initialize shadcn/ui

```bash
npx shadcn@latest init -d
```

Install required components:
```bash
npx shadcn@latest add button card input textarea label dialog toast tabs avatar dropdown-menu separator sonner
```

**Validation:** `src/components/ui/button.tsx` exists.

#### 5. Install All Dependencies

```bash
# Database & Auth
npm install drizzle-orm @neondatabase/serverless @neondatabase/auth

# Dev deps for Drizzle
npm install -D drizzle-kit

# Drag and drop
npm install @dnd-kit/core @dnd-kit/sortable @dnd-kit/utilities

# Charts
npm install recharts

# Validation
npm install zod

# Testing
npm install -D vitest @vitejs/plugin-react jsdom @testing-library/react @testing-library/dom @testing-library/jest-dom vite-tsconfig-paths

# E2E
npm install -g agent-browser
agent-browser install
```

**Validation:** All packages listed in `npm ls`.

#### 6. Configure Vitest

Create `vitest.config.mts`:
```ts
import react from "@vitejs/plugin-react";
import tsconfigPaths from "vite-tsconfig-paths";
import { defineConfig } from "vitest/config";

export default defineConfig({
  plugins: [tsconfigPaths(), react()],
  test: {
    environment: "jsdom",
    globals: true,
    setupFiles: ["./src/test/setup.ts"],
    include: ["src/**/*.{test,spec}.{ts,tsx}", "tests/unit/**/*.{test,spec}.{ts,tsx}"],
    css: true,
  },
});
```

Create `src/test/setup.ts`:
```ts
import "@testing-library/jest-dom/vitest";
```

Update `tsconfig.json`: Add `"vitest/globals"` to types.

**Validation:** Create smoke test, `npm run test:run` passes.

#### 7. Configure Environment

Create `.env.example`:
```env
DATABASE_URL=postgresql://user:pass@ep-xxx.us-east-2.aws.neon.tech/dbname?sslmode=require
NEON_AUTH_BASE_URL=https://ep-xxx.neonauth.us-east-1.aws.neon.tech/neondb/auth
NEON_AUTH_COOKIE_SECRET=generate-with-openssl-rand-base64-32
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Create `.env.local` (fill with real values, not committed).

Update `.gitignore`: Ensure `.env.local` is ignored.

**Validation:** `.env.example` exists with all vars.

#### 8. Initialize Git

```bash
git init
git add -A
git commit -m "chore: scaffold Next.js 15 project with Tailwind v4, shadcn/ui, Biome, Vitest"
```

**Validation:** `git log --oneline` shows commit.

### Scaffolding Checklist

- [ ] Next.js 15 project created
- [ ] Tailwind CSS v4 configured
- [ ] Biome configured with project rules
- [ ] shadcn/ui initialized
- [ ] All dependencies installed
- [ ] Vitest configured
- [ ] Environment variables documented
- [ ] Git initialized with initial commit
- [ ] Dev server runs without errors
- [ ] Build succeeds

---

## Planning & Requirements Phase

### Phase Goals

Translate business requirements into a detailed, actionable plan.

### Step-by-Step Process

#### 1. Write User Stories

Format each user story as:

**US-{N}: As a [user type], I want [action], so that [benefit].**

Examples:
- US-1: As a new user, I want to sign up with email and username, so that I get a personal URL.
- US-2: As a logged-in user, I want to edit my profile, so that I can customize my page.

#### 2. Define Problem Statement

Write one paragraph answering:
- What are we building?
- Why are we building it?
- What's the current state (greenfield or existing)?
- What constraints exist?

#### 3. Define Solution Statement

Write one paragraph answering:
- How will we solve it?
- What technologies will we use?
- What's the high-level architecture?

#### 4. Break Down into Features

Each feature should have:
- Feature Type: New Capability, Enhancement, Bug Fix
- Estimated Complexity: Low, Medium, High
- Primary Systems Affected
- Dependencies

#### 5. Create Implementation Plan

Break features into phases. Each phase should be:
- Independently valuable
- Completable in 1-2 weeks
- Has clear deliverables

#### 6. List Context References

Link to all relevant documentation:
- Library docs
- API references
- Best practices
- Internal PRD

#### 7. Create File List

List every file that needs to be created with:
- File path
- Purpose description

#### 8. Define Patterns

Document all patterns to follow:
- Naming conventions
- Component patterns
- API route patterns
- Database patterns

#### 9. Create Step-by-Step Tasks

Break work into atomic tasks. Each task should:
- Have a clear goal
- Be testable independently
- Include validation steps
- Be numbered sequentially

**Example Task:**

### Task 12: CREATE Validation Unit Tests

**Goal:** Test all Zod schemas for edge cases.

**Files to create:**
- `src/lib/__tests__/validations.test.ts`

**Test cases:**
- Slug validation: valid, too short, too long, reserved, uppercase, special chars
- Profile validation: valid, empty name ok, bio too long, invalid URL
- Link item validation: link needs title+url, header needs title, divider needs nothing

**Validation:** `npm run test:run` — all validation tests pass

#### 10. Define Acceptance Criteria

List all criteria that must be met for the feature to be considered done. Use checkboxes.

### Planning Checklist

- [ ] User stories written
- [ ] Problem statement defined
- [ ] Solution statement defined
- [ ] Features broken down
- [ ] Implementation plan created
- [ ] Context references documented
- [ ] File list created
- [ ] Patterns documented
- [ ] Step-by-step tasks created
- [ ] Acceptance criteria defined

---

## Database Design Phase

### Phase Goals

Design a database schema that is normalized, indexed, and scalable.

### Step-by-Step Process

#### 1. Identify Entities

List all data entities:
- Users
- Profiles
- Links
- Click Events
- etc.

#### 2. Define Relationships

- One-to-One: User ↔ Profile
- One-to-Many: Profile ↔ Links
- Many-to-One: Link ↔ Click Events

#### 3. Design Schema

For each table, define:
- Primary key (usually `uuid`)
- Foreign keys (references with cascade delete)
- Indexes (on frequently queried columns)
- Unique constraints (prevent duplicates)
- Default values
- Timestamps (created_at, updated_at)

**Example Schema:**

```typescript
import { relations } from "drizzle-orm";
import { index, integer, pgTable, text, timestamp, uniqueIndex, uuid } from "drizzle-orm/pg-core";

export const profiles = pgTable(
  "profiles",
  {
    id: uuid("id").defaultRandom().primaryKey(),
    userId: text("user_id").notNull().unique(),
    slug: text("slug").notNull().unique(),
    displayName: text("display_name").notNull().default(""),
    bio: text("bio").notNull().default(""),
    avatarUrl: text("avatar_url").notNull().default(""),
    theme: text("theme").notNull().default("minimal"),
    createdAt: timestamp("created_at", { withTimezone: true }).defaultNow().notNull(),
    updatedAt: timestamp("updated_at", { withTimezone: true }).defaultNow().notNull(),
  },
  (table) => [
    uniqueIndex("idx_profiles_slug").on(table.slug),
    index("idx_profiles_user_id").on(table.userId),
  ],
);

export const linkItems = pgTable(
  "link_items",
  {
    id: uuid("id").defaultRandom().primaryKey(),
    profileId: uuid("profile_id")
      .notNull()
      .references(() => profiles.id, { onDelete: "cascade" }),
    type: text("type").notNull().default("link"),
    title: text("title").notNull().default(""),
    url: text("url").notNull().default(""),
    sortOrder: integer("sort_order").notNull().default(0),
    createdAt: timestamp("created_at", { withTimezone: true }).defaultNow().notNull(),
    updatedAt: timestamp("updated_at", { withTimezone: true }).defaultNow().notNull(),
  },
  (table) => [index("idx_link_items_profile_id").on(table.profileId)],
);

export const profilesRelations = relations(profiles, ({ many }) => ({
  linkItems: many(linkItems),
}));

export const linkItemsRelations = relations(linkItems, ({ one }) => ({
  profile: one(profiles, {
    fields: [linkItems.profileId],
    references: [profiles.id],
  }),
}));
```

#### 4. Create Drizzle Config

Create `drizzle.config.ts`:
```ts
import { defineConfig } from "drizzle-kit";

export default defineConfig({
  dialect: "postgresql",
  schema: "./src/lib/db/schema.ts",
  out: "./drizzle",
  dbCredentials: {
    url: process.env.DATABASE_URL!,
  },
});
```

#### 5. Generate Migrations

```bash
npm run db:generate
```

Review the generated migration SQL. Ensure it's correct.

#### 6. Apply Migrations

```bash
npm run db:migrate
```

Or for development (no migration file):
```bash
npm run db:push
```

#### 7. Create Type Exports

Infer TypeScript types from schema:
```ts
export type Profile = InferSelectModel<typeof profiles>;
export type LinkItem = InferSelectModel<typeof linkItems>;
```

#### 8. Create Database Client

Create `src/lib/db/index.ts`:
```ts
import { neon } from "@neondatabase/serverless";
import { drizzle } from "drizzle-orm/neon-http";
import * as schema from "./schema";

const sql = neon(process.env.DATABASE_URL!);
export const db = drizzle(sql, { schema });
```

### Database Design Checklist

- [ ] Entities identified
- [ ] Relationships defined
- [ ] Schema designed with proper constraints
- [ ] Indexes added for performance
- [ ] Foreign keys with cascade delete
- [ ] Timestamps on all tables
- [ ] Drizzle config created
- [ ] Migrations generated
- [ ] Migrations applied
- [ ] Type exports created
- [ ] Database client created

---

## Authentication Phase

### Phase Goals

Implement secure, user-friendly authentication.

### Step-by-Step Process

#### 1. Set Up Neon Auth Server

Create `src/lib/auth/server.ts`:
```ts
import { createNeonAuth } from "@neondatabase/auth/next/server";

export const auth = createNeonAuth({
  baseUrl: process.env.NEON_AUTH_BASE_URL!,
  cookies: {
    secret: process.env.NEON_AUTH_COOKIE_SECRET!,
  },
});
```

#### 2. Set Up Neon Auth Client

Create `src/lib/auth/client.ts`:
```ts
"use client";

import { createAuthClient } from "@neondatabase/auth/next";

export const authClient = createAuthClient();
```

#### 3. Create Auth API Route

Create `src/app/api/auth/[...path]/route.ts`:
```ts
import { auth } from "@/lib/auth/server";

export const { GET, POST } = auth.handler();
```

#### 4. Create Auth Provider

Create `src/components/auth/auth-provider.tsx`:
```tsx
"use client";

import { authClient } from "@/lib/auth/client";
import { NeonAuthUIProvider } from "@neondatabase/auth/react";
import { useRouter } from "next/navigation";
import Link from "next/link";

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const router = useRouter();
  return (
    <NeonAuthUIProvider
      authClient={authClient}
      navigate={router.push}
      replace={router.replace}
      onSessionChange={() => router.refresh()}
      Link={Link}
      social={{ providers: ["google"] }}
    >
      {children}
    </NeonAuthUIProvider>
  );
}
```

#### 5. Wrap Root Layout

Update `src/app/layout.tsx`:
```tsx
import { AuthProvider } from "@/components/auth/auth-provider";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body>
        <AuthProvider>{children}</AuthProvider>
      </body>
    </html>
  );
}
```

**IMPORTANT:** Add `suppressHydrationWarning` to `<html>` tag.

#### 6. Add Auth CSS

Update `src/app/globals.css`:
```css
@import "tailwindcss";
@import "@neondatabase/auth/ui/tailwind";
```

#### 7. Create Middleware

Create `middleware.ts` (project root):
```ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const allCookies = request.cookies.getAll();
  const sessionCookie = allCookies.find(c => c.name.includes("neon-auth.session_token"));

  if (!sessionCookie?.value) {
    const loginUrl = new URL("/login", request.url);
    loginUrl.searchParams.set("callbackUrl", request.nextUrl.pathname);
    return NextResponse.redirect(loginUrl);
  }

  return NextResponse.next();
}

export const config = {
  matcher: ["/editor", "/editor/:path*", "/analytics", "/analytics/:path*", "/settings", "/settings/:path*"],
};
```

#### 8. Create Signup Form

Create `src/components/auth/signup-form.tsx`:
```tsx
"use client";

import { authClient } from "@/lib/auth/client";
import { useRouter } from "next/navigation";
// ... form UI implementation

export function SignupForm() {
  const router = useRouter();
  
  async function handleSubmit(formData: FormData) {
    const result = await authClient.signUp.email({
      email: formData.get("email") as string,
      password: formData.get("password") as string,
      name: formData.get("name") as string,
    });
    
    if (result.error) {
      // Show error
      return;
    }
    
    // Create profile row
    await fetch("/api/profile", {
      method: "POST",
      body: JSON.stringify({ slug: formData.get("slug") }),
    });
    
    router.push("/editor");
  }
  
  // ... form JSX
}
```

#### 9. Create Login Form

Create `src/components/auth/login-form.tsx`:
```tsx
"use client";

import { authClient } from "@/lib/auth/client";
import { useRouter } from "next/navigation";

export function LoginForm() {
  const router = useRouter();
  
  async function handleSubmit(formData: FormData) {
    const result = await authClient.signIn.email({
      email: formData.get("email") as string,
      password: formData.get("password") as string,
    });
    
    if (result.error) {
      // Show error
      return;
    }
    
    router.push("/editor");
  }
  
  // ... form JSX
}
```

#### 10. Create Google OAuth Button

Create `src/components/auth/google-button.tsx`:
```tsx
"use client";

import { authClient } from "@/lib/auth/client";

export function GoogleButton() {
  async function handleGoogleSignIn() {
    await authClient.signIn.social({ provider: "google", callbackURL: "/editor" });
  }
  
  return (
    <button onClick={handleGoogleSignIn}>
      Continue with Google
    </button>
  );
}
```

### Authentication Checklist

- [ ] Neon Auth server instance created
- [ ] Neon Auth client instance created
- [ ] Auth API route created
- [ ] Auth provider component created
- [ ] Root layout wrapped with provider
- [ ] Auth CSS imported
- [ ] Middleware created
- [ ] Signup form created
- [ ] Login form created
- [ ] Google OAuth button created
- [ ] Protected routes redirect to login
- [ ] Unprotected routes accessible

---

## API Design Phase

### Phase Goals

Design RESTful API routes that are secure, validated, and documented.

### Step-by-Step Process

#### 1. Define API Contracts

For each route, define:
- HTTP method (GET, POST, PUT, DELETE)
- Request body schema (if applicable)
- Response schema
- Authentication requirement
- Rate limit

#### 2. Create Validation Schemas

All input validation happens via Zod schemas.

**Example:**

```ts
import { z } from "zod";

export const profileSchema = z.object({
  displayName: z.string().max(50, "Name must be at most 50 characters"),
  bio: z.string().max(160, "Bio must be at most 160 characters"),
  avatarUrl: z.string().url("Must be a valid URL").or(z.literal("")),
  theme: z.enum(["minimal", "dark", "colorful", "professional"]),
});

export const linkItemSchema = z.object({
  type: z.enum(["link", "header", "divider"]),
  title: z.string().max(100).optional(),
  url: z.string().url("Must be a valid URL").optional(),
}).refine(
  (data) => {
    if (data.type === "link") return !!data.title && !!data.url;
    if (data.type === "header") return !!data.title;
    return true;
  },
  { message: "Links require title and URL; headers require title" },
);
```

#### 3. Implement API Route Pattern

Every API route follows this pattern:

```ts
import { auth } from "@/lib/auth/server";
import { db } from "@/lib/db";
import { profiles } from "@/lib/db/schema";
import { profileSchema } from "@/lib/validations";
import { eq } from "drizzle-orm";
import { NextResponse } from "next/server";
import { apiRateLimiter } from "@/lib/rate-limit";

export async function GET(request: NextRequest) {
  // 1. Rate limit
  const ip = request.headers.get("x-forwarded-for") ?? "anonymous";
  const { success } = apiRateLimiter.check(ip);
  if (!success) {
    return NextResponse.json({ error: "Too many requests" }, { status: 429 });
  }

  // 2. Authenticate
  const user = await getUser();
  if (!user) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  // 3. Query database
  const profile = await db.query.profiles.findFirst({
    where: eq(profiles.userId, user.id),
  });

  if (!profile) {
    return NextResponse.json({ profile: null });
  }

  return NextResponse.json({ profile });
}

export async function PUT(request: NextRequest) {
  // 1. Rate limit
  const ip = request.headers.get("x-forwarded-for") ?? "anonymous";
  const { success } = apiRateLimiter.check(ip);
  if (!success) {
    return NextResponse.json({ error: "Too many requests" }, { status: 429 });
  }

  // 2. Authenticate
  const user = await getUser();
  if (!user) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  // 3. Validate request body
  const body = await request.json();
  const result = profileSchema.safeParse(body);
  if (!result.success) {
    return NextResponse.json({ error: result.error.issues[0]?.message }, { status: 400 });
  }

  // 4. Update database
  const [updated] = await db
    .update(profiles)
    .set({
      displayName: result.data.displayName,
      bio: result.data.bio,
      avatarUrl: result.data.avatarUrl,
      theme: result.data.theme,
      updatedAt: new Date(),
    })
    .where(eq(profiles.userId, user.id))
    .returning();

  if (!updated) {
    return NextResponse.json({ error: "Profile not found" }, { status: 404 });
  }

  return NextResponse.json({ profile: updated });
}
```

#### 4. Implement Rate Limiting

Create `src/lib/rate-limit.ts`:
```ts
interface RateLimitEntry {
  count: number;
  resetTime: number;
}

export function createRateLimiter(maxRequests: number, windowMs: number) {
  const store = new Map<string, RateLimitEntry>();

  setInterval(() => {
    const now = Date.now();
    for (const [key, entry] of store) {
      if (now > entry.resetTime) store.delete(key);
    }
  }, windowMs);

  return {
    check(key: string): { success: boolean; remaining: number } {
      const now = Date.now();
      const entry = store.get(key);

      if (!entry || now > entry.resetTime) {
        store.set(key, { count: 1, resetTime: now + windowMs });
        return { success: true, remaining: maxRequests - 1 };
      }

      if (entry.count >= maxRequests) {
        return { success: false, remaining: 0 };
      }

      entry.count++;
      return { success: true, remaining: maxRequests - entry.count };
    },
  };
}

export const apiRateLimiter = createRateLimiter(30, 60 * 1000); // 30 requests per minute
```

#### 5. Handle Ownership Checks

For mutations on owned resources:

```ts
// Get user's profile
const user = await getUser();
const profile = await db.query.profiles.findFirst({
  where: eq(profiles.userId, user.id),
});

// Verify ownership before deletion
const linkItem = await db.query.linkItems.findFirst({
  where: eq(linkItems.id, id),
});

if (!linkItem || linkItem.profileId !== profile.id) {
  return NextResponse.json({ error: "Not found" }, { status: 404 });
}

// Proceed with deletion
await db.delete(linkItems).where(eq(linkItems.id, id));
```

### API Design Checklist

- [ ] All routes have validation schemas
- [ ] All routes have rate limiting
- [ ] All routes check authentication
- [ ] Ownership checks on mutations
- [ ] Consistent error responses
- [ ] Proper HTTP status codes
- [ ] Response schemas documented

---

## Frontend Development Phase

### Phase Goals

Build a responsive, accessible, and user-friendly frontend.

### Step-by-Step Process

#### 1. Component Architecture

**Server Components by Default:**
- Use server components for data fetching
- Use server components for SEO-critical pages
- Only use client components when needed (forms, interactivity)

**Example Server Component:**
```tsx
import { auth } from "@/lib/auth/server";

export default async function DashboardPage() {
  const { data: session } = await auth.getSession();
  
  if (!session?.user) {
    redirect("/login");
  }
  
  return <div>Welcome, {session.user.name}</div>;
}
```

**Example Client Component:**
```tsx
"use client";

import { useState } from "react";

export function InteractiveForm() {
  const [value, setValue] = useState("");
  
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}
```

#### 2. Form Patterns

All forms follow this pattern:

1. Controlled inputs
2. Client-side validation (Zod)
3. Submit handler with loading state
4. Error display
5. Success feedback

```tsx
"use client";

import { useState } from "react";
import { profileSchema } from "@/lib/validations";

export function ProfileForm() {
  const [formData, setFormData] = useState({
    displayName: "",
    bio: "",
    avatarUrl: "",
  });
  const [errors, setErrors] = useState<Record<string, string>>({});
  const [isSaving, setIsSaving] = useState(false);

  async function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    
    // Validate
    const result = profileSchema.safeParse(formData);
    if (!result.success) {
      const newErrors: Record<string, string> = {};
      result.error.issues.forEach(issue => {
        const path = issue.path[0] as string;
        newErrors[path] = issue.message;
      });
      setErrors(newErrors);
      return;
    }
    
    // Submit
    setIsSaving(true);
    try {
      const response = await fetch("/api/profile", {
        method: "PUT",
        body: JSON.stringify(formData),
      });
      
      if (!response.ok) {
        throw new Error("Failed to save");
      }
      
      // Show success
    } catch (error) {
      // Show error
    } finally {
      setIsSaving(false);
    }
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        value={formData.displayName}
        onChange={e => setFormData({ ...formData, displayName: e.target.value })}
      />
      {errors.displayName && <div className="error">{errors.displayName}</div>}
      <button type="submit" disabled={isSaving}>
        {isSaving ? "Saving..." : "Save"}
      </button>
    </form>
  );
}
```

#### 3. Data Fetching Hooks

Create custom hooks for data fetching:

```ts
// src/hooks/use-profile.ts
"use client";

import { useEffect, useState } from "react";

export function useProfile() {
  const [profile, setProfile] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchProfile() {
      try {
        const response = await fetch("/api/profile");
        const data = await response.json();
        setProfile(data.profile);
      } catch (err) {
        setError(err);
      } finally {
        setIsLoading(false);
      }
    }
    
    fetchProfile();
  }, []);

  return { profile, isLoading, error, mutate: fetchProfile };
}
```

#### 4. Drag-and-Drop with dnd-kit

```tsx
"use client";

import {
  DndContext,
  closestCenter,
  KeyboardSensor,
  PointerSensor,
  useSensor,
  useSensors,
} from "@dnd-kit/core";
import {
  arrayMove,
  SortableContext,
  sortableKeyboardCoordinates,
  verticalListSortingStrategy,
  useSortable,
} from "@dnd-kit/sortable";
import { CSS } from "@dnd-kit/utilities";

function DraggableItem({ id, children }: { id: string; children: React.ReactNode }) {
  const { attributes, listeners, setNodeRef, transform, transition } = useSortable({ id });
  
  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
  };
  
  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {children}
    </div>
  );
}

export function SortableList({ items }: { items: { id: string }[] }) {
  const sensors = useSensors(
    useSensor(PointerSensor),
    useSensor(KeyboardSensor, {
      coordinateGetter: sortableKeyboardCoordinates,
    })
  );
  
  function handleDragEnd(event: any) {
    const { active, over } = event;
    
    if (active.id !== over?.id) {
      const oldIndex = items.findIndex(item => item.id === active.id);
      const newIndex = items.findIndex(item => item.id === over?.id);
      const newItems = arrayMove(items, oldIndex, newIndex);
      // Update state
    }
  }
  
  return (
    <DndContext sensors={sensors} collisionDetection={closestCenter} onDragEnd={handleDragEnd}>
      <SortableContext items={items} strategy={verticalListSortingStrategy}>
        {items.map(item => (
          <DraggableItem key={item.id} id={item.id}>
            {/* Item content */}
          </DraggableItem>
        ))}
      </SortableContext>
    </DndContext>
  );
}
```

#### 5. Responsive Layouts

Use Tailwind breakpoints:
- Mobile: default
- Tablet: `md:`
- Desktop: `lg:`

```tsx
<div className="grid grid-cols-1 lg:grid-cols-2 gap-4">
  <div className="lg:col-span-1">Editor Panel</div>
  <div className="lg:col-span-1">Preview Panel</div>
</div>
```

For mobile-specific patterns:
```tsx
<div className="hidden lg:block">
  Desktop content
</div>

<div className="block lg:hidden">
  <Tabs defaultValue="edit">
    <TabsList>
      <TabsTrigger value="edit">Edit</TabsTrigger>
      <TabsTrigger value="preview">Preview</TabsTrigger>
    </TabsList>
    <TabsContent value="edit">Editor</TabsContent>
    <TabsContent value="preview">Preview</TabsContent>
  </Tabs>
</div>
```

#### 6. Loading States

Show loading indicators for async operations:

```tsx
{isLoading && <div className="spinner">Loading...</div>}
{error && <div className="error">{error.message}</div>}
{data && <div>{/* Content */}</div>}
```

#### 7. Toast Notifications

Use shadcn/ui's sonner:

```tsx
import { toast } from "sonner";

function handleSave() {
  toast.success("Changes saved!");
  toast.error("Failed to save");
  toast.promise(savePromise, {
    loading: "Saving...",
    success: "Saved!",
    error: "Failed to save",
  });
}
```

### Frontend Development Checklist

- [ ] Server components used by default
- [ ] Client components only when needed
- [ ] All forms have validation
- [ ] All forms have loading states
- [ ] All forms have error handling
- [ ] Responsive layouts implemented
- [ ] Loading indicators shown
- [ ] Toast notifications for feedback
- [ ] Accessibility (aria labels, keyboard nav)
- [ ] Drag-and-drop working (if applicable)

---

## Testing Phase

### Phase Goals

Ensure code quality and correctness through comprehensive testing.

### Step-by-Step Process

#### 1. Unit Tests with Vitest

Test all utility functions, validation logic, and pure functions.

**Example Validation Test:**

```ts
import { describe, expect, it } from "vitest";
import { slugSchema, profileSchema } from "../validations";

describe("slugSchema", () => {
  it("accepts valid slugs", () => {
    expect(slugSchema.safeParse("cole").success).toBe(true);
    expect(slugSchema.safeParse("my-page").success).toBe(true);
  });

  it("rejects slugs that are too short", () => {
    expect(slugSchema.safeParse("ab").success).toBe(false);
  });

  it("rejects reserved slugs", () => {
    expect(slugSchema.safeParse("admin").success).toBe(false);
    expect(slugSchema.safeParse("login").success).toBe(false);
  });
});
```

#### 2. Component Tests

Test React components with React Testing Library:

```ts
import { render, screen } from "@testing-library/react";
import { userEvent } from "@testing-library/user-event";
import { Button } from "./button";

describe("Button", () => {
  it("renders correctly", () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole("button")).toHaveTextContent("Click me");
  });

  it("calls onClick when clicked", async () => {
    const handleClick = vi.fn();
    const user = userEvent.setup();
    
    render(<Button onClick={handleClick}>Click me</Button>);
    await user.click(screen.getByRole("button"));
    
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

#### 3. E2E Tests with Agent Browser

All E2E tests are bash shell scripts that chain `agent-browser` CLI commands.

**Example E2E Test:**

```bash
#!/bin/bash
set -e

echo "=== E2E: Signup Flow ==="

agent-browser open http://localhost:3000/signup
agent-browser wait --load networkidle
agent-browser screenshot tests/e2e/screenshots/signup-page.png

agent-browser snapshot -i

agent-browser find label "Name" fill "Test User"
agent-browser find label "Email" fill "test@example.com"
agent-browser find label "Password" fill "TestPassword123!"
agent-browser find label "Username" fill "testuser"

agent-browser find role button click --name "Create Account"

agent-browser wait --url "**/editor"

URL=$(agent-browser get url)
if [[ "$URL" == *"/editor"* ]]; then
  echo "PASS: Redirected to editor"
else
  echo "FAIL: Expected /editor"
  exit 1
fi

echo "=== Signup Flow: PASSED ==="
```

**Key Agent Browser Commands:**
- `agent-browser open <url>` - Navigate
- `agent-browser snapshot -i` - Get refs
- `agent-browser find label "X" fill "Y"` - Fill input
- `agent-browser find role button click --name "X"` - Click button
- `agent-browser wait --load networkidle` - Wait for network
- `agent-browser wait --url "**/path"` - Wait for URL
- `agent-browser get url` - Get current URL
- `agent-browser screenshot <path>` - Take screenshot

#### 4. Test Runner Script

Create `tests/e2e/run-all.sh`:

```bash
#!/bin/bash
set -e

PASSED=0
FAILED=0

run_test() {
  local name=$1
  local script=$2
  echo "Running: $name"
  if bash "$script"; then
    PASSED=$((PASSED + 1))
  else
    FAILED=$((FAILED + 1))
  fi
}

run_test "Signup Flow" "tests/e2e/signup.sh"
run_test "Login Flow" "tests/e2e/login.sh"
run_test "Profile Editing" "tests/e2e/editor.sh"

echo "Results: $PASSED passed, $FAILED failed"

if [ $FAILED -gt 0 ]; then
  exit 1
fi
```

### Testing Checklist

- [ ] Unit tests for all validation schemas
- [ ] Unit tests for utility functions
- [ ] Component tests for critical UI
- [ ] E2E tests for user journeys
- [ ] E2E tests for auth flows
- [ ] E2E tests for CRUD operations
- [ ] Test runner script created
- [ ] All tests pass before committing

---

## Deployment Phase

### Phase Goals

Deploy the application to production with proper environment configuration.

### Step-by-Step Process

#### 1. Prepare Environment Variables

Create production environment variables:
- `DATABASE_URL` - Production Neon database
- `NEON_AUTH_BASE_URL` - Production Neon Auth URL
- `NEON_AUTH_COOKIE_SECRET` - Random 32-byte base64 string
- `GOOGLE_CLIENT_ID` - Google OAuth client ID (production)
- `GOOGLE_CLIENT_SECRET` - Google OAuth secret (production)
- `NEXT_PUBLIC_APP_URL` - Production app URL

#### 2. Run Database Migrations

```bash
npm run db:migrate
```

#### 3. Build Application

```bash
npm run build
```

Fix any build errors before deploying.

#### 4. Deploy to Vercel

```bash
vercel
```

Or via Vercel dashboard:
1. Connect GitHub repository
2. Configure environment variables
3. Deploy

#### 5. Configure Domain

Add custom domain in Vercel dashboard:
1. Settings → Domains
2. Add domain
3. Update DNS records

#### 6. Set Up Monitoring

- Vercel Analytics
- Error tracking (Sentry)
- Uptime monitoring

### Deployment Checklist

- [ ] Production environment variables configured
- [ ] Database migrations applied
- [ ] Build succeeds without errors
- [ ] Application deployed
- [ ] Custom domain configured
- [ ] DNS propagated
- [ ] Monitoring set up
- [ ] SSL certificate valid

---

## Code Quality & Standards

### TypeScript Configuration

Always use strict mode:

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true
  }
}
```

### Linting with Biome

Run before every commit:

```bash
npm run lint
npm run format
```

Auto-fix issues:

```bash
npm run lint:fix
```

### Git Workflow

**Commit Message Format:**

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `refactor` - Code refactoring
- `docs` - Documentation
- `test` - Tests
- `chore` - Maintenance

**Examples:**

```
feat(auth): add Google OAuth support

Implements sign-in with Google using Neon Auth.

Closes #123

feat(editor): add drag-and-drop link reordering

Users can now reorder links via drag handles.

fix(api): handle rate limit exceeded properly

Returns 429 status when rate limit exceeded.
```

### Code Review Checklist

Before merging PR, verify:
- [ ] All tests pass
- [ ] No linting errors
- [ ] TypeScript compiles
- [ ] Build succeeds
- [ ] Documentation updated
- [ ] No TODO comments left
- [ ] Security review completed

---

## Naming Conventions

### Files

- **Components:** PascalCase with `.tsx` extension → `UserProfile.tsx`
- **Utilities:** camelCase with `.ts` extension → `formatDate.ts`
- **Hooks:** camelCase with `use` prefix → `useProfile.ts`
- **Types:** camelCase with `.ts` extension → `types.ts`
- **Tests:** `.test.ts` or `.test.tsx` → `validations.test.ts`
- **E2E Tests:** `.sh` → `signup.sh`

### Variables & Functions

- **camelCase:** `displayName`, `avatarUrl`, `getUserProfile()`
- **Constants:** UPPER_SNAKE_CASE → `MAX_LINKS`, `RESERVED_SLUGS`
- **React Components:** PascalCase → `UserProfile`, `LinkItem`
- **Custom Hooks:** camelCase with `use` prefix → `useProfile`, `useAuth`

### Database

- **Tables:** snake_case → `profiles`, `link_items`
- **Columns:** snake_case → `display_name`, `avatar_url`, `sort_order`

### API Routes

- **Routes:** kebab-case → `/api/profile`, `/api/links/reorder`
- **Query Params:** camelCase → `?slug=myusername`
- **Request Body:** camelCase → `{ displayName: "John" }`

### CSS Classes

- **Tailwind:** Use utility classes directly → `flex items-center gap-4`
- **Custom:** kebab-case → `.custom-class-name`

---

## File Organization

### Recommended Directory Structure

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/            # Auth route group
│   │   ├── login/
│   │   └── signup/
│   ├── (dashboard)/        # Dashboard route group
│   │   ├── editor/
│   │   ├── analytics/
│   │   └── settings/
│   ├── api/               # API routes
│   │   ├── auth/
│   │   ├── profile/
│   │   └── links/
│   ├── layout.tsx
│   └── page.tsx
├── components/            # React components
│   ├── auth/            # Auth components
│   ├── editor/          # Editor components
│   ├── preview/         # Preview components
│   ├── themes/          # Theme components
│   └── ui/             # shadcn/ui components
├── lib/                 # Utilities
│   ├── auth/            # Auth utilities
│   ├── db/              # Database utilities
│   └── __tests__/      # Unit tests
├── hooks/               # Custom React hooks
├── test/                # Test setup
├── types/               # TypeScript types
└── middleware.ts        # Next.js middleware

tests/
├── e2e/                # E2E test scripts
│   ├── screenshots/     # Test screenshots
│   └── run-all.sh      # Test runner
└── unit/               # Unit test files

drizzle/                 # Drizzle migrations
```

---

## Security Best Practices

### 1. Never Expose Secrets

✅ **DO:**
```ts
const dbUrl = process.env.DATABASE_URL!;
```

❌ **DON'T:**
```ts
const dbUrl = "postgresql://user:pass@...";
```

### 2. Validate All Inputs

✅ **DO:**
```ts
const result = schema.safeParse(body);
if (!result.success) {
  return NextResponse.json({ error: result.error.issues[0]?.message }, { status: 400 });
}
```

❌ **DON'T:**
```ts
await db.insert(profiles).values(body); // No validation!
```

### 3. Check Authentication

✅ **DO:**
```ts
const user = await getUser();
if (!user) {
  return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
}
```

❌ **DON'T:**
```ts
// Proceed without checking user
```

### 4. Verify Ownership

✅ **DO:**
```ts
const linkItem = await db.query.linkItems.findFirst({
  where: eq(linkItems.id, id),
});

if (!linkItem || linkItem.profileId !== profile.id) {
  return NextResponse.json({ error: "Not found" }, { status: 404 });
}
```

❌ **DON'T:**
```ts
// Delete without ownership check
await db.delete(linkItems).where(eq(linkItems.id, id));
```

### 5. Use HTTPS

- Always use HTTPS in production
- Enforce HTTPS via Vercel settings
- Set `Secure` flag on cookies

### 6. Rate Limiting

Rate limit all API endpoints to prevent abuse:

```ts
const { success } = apiRateLimiter.check(ip);
if (!success) {
  return NextResponse.json({ error: "Too many requests" }, { status: 429 });
}
```

### 7. SQL Injection Prevention

✅ **DO:**
```ts
// Drizzle ORM prevents SQL injection
db.select().from(profiles).where(eq(profiles.id, id));
```

❌ **DON'T:**
```ts
// Never concatenate strings!
db.query(`SELECT * FROM profiles WHERE id = '${id}'`);
```

### 8. CORS Configuration

Configure CORS properly in production:

```ts
export const config = {
  runtime: "edge",
  regions: ["iad1"],
  headers: {
    "Access-Control-Allow-Origin": process.env.ALLOWED_ORIGIN,
    "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE, OPTIONS",
    "Access-Control-Allow-Headers": "Content-Type, Authorization",
  },
};
```

### 9. Content Security Policy

Add CSP headers in `next.config.ts`:

```ts
const securityHeaders = [
  {
    key: "X-DNS-Prefetch-Control",
    value: "on",
  },
  {
    key: "Strict-Transport-Security",
    value: "max-age=63072000; includeSubDomains; preload",
  },
  {
    key: "X-Frame-Options",
    value: "SAMEORIGIN",
  },
  {
    key: "X-Content-Type-Options",
    value: "nosniff",
  },
  {
    key: "Referrer-Policy",
    value: "origin-when-cross-origin",
  },
];
```

### 10. Dependency Updates

Keep dependencies updated:

```bash
npm audit
npm update
```

Review security advisories regularly.

---

## Performance Optimization

### 1. Use Server Components

Server components are faster:
- No client-side JavaScript
- Data fetched server-side
- Better SEO

### 2. Optimize Images

Use Next.js Image component:

```tsx
import Image from "next/image";

<Image
  src="/avatar.jpg"
  alt="User avatar"
  width={200}
  height={200}
  loading="lazy"
/>
```

### 3. Code Splitting

Use dynamic imports for heavy components:

```tsx
import dynamic from "next/dynamic";

const HeavyChart = dynamic(() => import("./HeavyChart"), {
  loading: () => <div>Loading chart...</div>,
  ssr: false,
});
```

### 4. Database Indexing

Add indexes to frequently queried columns:

```ts
index("idx_profiles_slug").on(table.slug),
index("idx_profiles_user_id").on(table.userId),
```

### 5. Caching

Cache expensive operations:

```ts
import { unstable_cache } from "next/cache";

export const getCachedProfile = unstable_cache(
  async (userId: string) => {
    return db.query.profiles.findFirst({
      where: eq(profiles.userId, userId),
    });
  },
  ["profile"],
  { revalidate: 60, tags: ["profile"] }
);
```

### 6. Lazy Loading Routes

Use `loading.tsx` for route-level loading states:

```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <div>Loading dashboard...</div>;
}
```

### 7. Minimize Bundle Size

Tree-shake unused code:

```tsx
// Good: Import only what you need
import { Button } from "@/components/ui/button";

// Bad: Import everything
import * as UI from "@/components/ui";
```

### 8. Use Edge Runtime for Fast APIs

For simple APIs, use Edge Runtime:

```ts
export const runtime = "edge";

export async function GET() {
  return Response.json({ message: "Hello" });
}
```

---

## Common Patterns & Recipes

### Pattern 1: API Route Template

```ts
import { auth } from "@/lib/auth/server";
import { db } from "@/lib/db";
import { schema } from "@/lib/db/schema";
import { eq } from "drizzle-orm";
import { NextResponse } from "next/server";
import { apiRateLimiter } from "@/lib/rate-limit";
import { validationSchema } from "@/lib/validations";

export async function GET(request: NextRequest) {
  // Rate limit
  const ip = request.headers.get("x-forwarded-for") ?? "anonymous";
  if (!apiRateLimiter.check(ip).success) {
    return NextResponse.json({ error: "Too many requests" }, { status: 429 });
  }

  // Authenticate
  const user = await getUser();
  if (!user) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  // Query
  const data = await db.query.schema.findFirst({
    where: eq(schema.userId, user.id),
  });

  return NextResponse.json({ data });
}

export async function POST(request: NextRequest) {
  // Rate limit
  const ip = request.headers.get("x-forwarded-for") ?? "anonymous";
  if (!apiRateLimiter.check(ip).success) {
    return NextResponse.json({ error: "Too many requests" }, { status: 429 });
  }

  // Authenticate
  const user = await getUser();
  if (!user) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  // Validate
  const body = await request.json();
  const result = validationSchema.safeParse(body);
  if (!result.success) {
    return NextResponse.json({ error: result.error.issues[0]?.message }, { status: 400 });
  }

  // Mutate
  const [created] = await db.insert(schema).values(result.data).returning();

  return NextResponse.json({ data: created }, { status: 201 });
}
```

### Pattern 2: Form Component Template

```tsx
"use client";

import { useState } from "react";
import { z } from "zod";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { toast } from "sonner";

const formSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email"),
});

export function MyForm() {
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
  });

  async function onSubmit(values: z.infer<typeof formSchema>) {
    setIsSubmitting(true);
    try {
      const response = await fetch("/api/endpoint", {
        method: "POST",
        body: JSON.stringify(values),
      });

      if (!response.ok) {
        throw new Error("Failed to submit");
      }

      toast.success("Saved successfully!");
      form.reset();
    } catch (error) {
      toast.error("Failed to save");
    } finally {
      setIsSubmitting(false);
    }
  }

  return (
    <form onSubmit={form.handleSubmit(onSubmit)}>
      {/* Form fields */}
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Saving..." : "Save"}
      </button>
    </form>
  );
}
```

### Pattern 3: Custom Hook Template

```ts
"use client";

import { useEffect, useState } from "react";

export function useResource<T>(endpoint: string) {
  const [data, setData] = useState<T | null>(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  async function fetch() {
    try {
      setIsLoading(true);
      setError(null);
      const response = await fetch(endpoint);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      const json = await response.json();
      setData(json);
    } catch (err) {
      setError(err as Error);
    } finally {
      setIsLoading(false);
    }
  }

  useEffect(() => {
    fetch();
  }, [endpoint]);

  return { data, isLoading, error, mutate: fetch };
}
```

### Pattern 4: Draggable List Template

```tsx
"use client";

import { useState } from "react";
import {
  DndContext,
  closestCenter,
  KeyboardSensor,
  PointerSensor,
  useSensor,
  useSensors,
} from "@dnd-kit/core";
import {
  arrayMove,
  SortableContext,
  sortableKeyboardCoordinates,
  verticalListSortingStrategy,
  useSortable,
} from "@dnd-kit/sortable";
import { CSS } from "@dnd-kit/utilities";

interface DraggableItemProps {
  id: string;
  children: React.ReactNode;
}

function DraggableItem({ id, children }: DraggableItemProps) {
  const { attributes, listeners, setNodeRef, transform, transition } = useSortable({ id });
  
  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
  };
  
  return (
    <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
      {children}
    </div>
  );
}

export function SortableList({ items, onChange }: { items: { id: string }[]; onChange: (items: { id: string }[]) => void }) {
  const sensors = useSensors(
    useSensor(PointerSensor),
    useSensor(KeyboardSensor, {
      coordinateGetter: sortableKeyboardCoordinates,
    })
  );
  
  function handleDragEnd(event: any) {
    const { active, over } = event;
    
    if (active.id !== over?.id) {
      const oldIndex = items.findIndex(item => item.id === active.id);
      const newIndex = items.findIndex(item => item.id === over?.id);
      onChange(arrayMove(items, oldIndex, newIndex));
    }
  }
  
  return (
    <DndContext sensors={sensors} collisionDetection={closestCenter} onDragEnd={handleDragEnd}>
      <SortableContext items={items} strategy={verticalListSortingStrategy}>
        {items.map(item => (
          <DraggableItem key={item.id} id={item.id}>
            {children}
          </DraggableItem>
        ))}
      </SortableContext>
    </DndContext>
  );
}
```

### Pattern 5: Protected Page Template

```tsx
import { auth } from "@/lib/auth/server";
import { redirect } from "next/navigation";

export default async function ProtectedPage() {
  const { data: session } = await auth.getSession();
  
  if (!session?.user) {
    redirect("/login");
  }
  
  return (
    <div>
      <h1>Welcome, {session.user.name}</h1>
      {/* Page content */}
    </div>
  );
}
```

---

## Final Words

### The Golden Rules

1. **Never skip validation** - All inputs, always.
2. **Never skip authentication** - Check session, always.
3. **Never skip testing** - Test everything, always.
4. **Never skip documentation** - Document decisions, always.
5. **Never skip the review** - Code review matters, always.

### Continuous Improvement

- This document evolves. Update it when you learn.
- Share patterns with the community.
- Stay updated with best practices.
- Question assumptions. Optimize when needed.

### You Are Not Alone

- Read the source code: [coleam00/link-in-bio-page-builder](https://github.com/coleam00/link-in-bio-page-builder)
- Check documentation: Next.js, Drizzle, Neon Auth
- Ask for help: Discord, GitHub Issues, Stack Overflow

---

**Remember:** This is your guide. Follow it, improve it, make it yours.

*Last Updated: 2026-02-24*
