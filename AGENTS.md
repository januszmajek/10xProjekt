# Repository Guidelines

Perfect Training Planner is an Astro 6 SSR app with React islands, TypeScript, Tailwind 4, Supabase auth, and Cloudflare Workers deployment. Use @context/foundation/prd.md, @context/foundation/tech-stack.md, and @context/foundation/infrastructure.md for product and stack decisions.

## Hard Rules

- Use pnpm, not npm or bun; package manager and version are locked in @package.json.
- Treat this as a Cloudflare Workers app. Use Wrangler/Workers commands only; do not switch to Cloudflare Pages commands.
- Keep `.env`, `.env.production`, `.dev.vars`, Supabase keys, and Cloudflare tokens out of git and chat.
- `SUPABASE_KEY` must be a publishable or anon key, never a `service_role` or `sb_secret_...` key.
- Do not write to `context/archive/`; archived changes are immutable.

## Commands

- `pnpm install --frozen-lockfile` installs dependencies for CI-equivalent checks.
- `pnpm dev` starts Astro dev with the Cloudflare adapter runtime.
- `pnpm lint` runs type-aware ESLint and Prettier checks.
- `pnpm build` runs the production Astro SSR build.
- `pnpm preview` previews the built app locally.
- `pnpm exec wrangler deploy` deploys the Worker after build gates pass.

## Project Structure

- `src/pages/` contains Astro routes; auth API endpoints live in `src/pages/api/auth/`.
- `src/components/` contains Astro and React UI; shadcn-style primitives live in `src/components/ui/`.
- `src/lib/` holds shared helpers such as Supabase and `cn()` class merging.
- `src/middleware.ts` resolves `context.locals.user` and protects routes listed in `PROTECTED_ROUTES`.
- `context/foundation/` stores planning contracts; `context/deployment/` stores deployment history and runbooks.

## Coding Conventions

Use `@/*` imports for `src/*` paths, 2-space formatting, double quotes, semicolons, and 120-character lines per @.prettierrc.json and @tsconfig.json. Prefer Astro components for static route/layout work; use React components only for interactive UI. Use `cn()` from `@/lib/utils` for conditional Tailwind class merging.

## Testing And CI

There is no test runner configured yet, so do not invent test commands. Before handoff, run `pnpm lint` and `pnpm build`; CI also runs `pnpm astro sync` on pushes and PRs to `master` via @.github/workflows/ci.yml.

## Commits And PRs

Recent history uses short imperative messages, sometimes with Conventional Commit prefixes such as `ci:`. Keep commits focused and mention any deployment, secret, Supabase, or auth impact in PR notes.

<!-- BEGIN @przeprogramowani/10x-cli -->

## 10xDevs AI Toolkit - Module 2, Lesson 1

Move from sprint-zero setup to project orchestration with the **roadmap chain**:

```
(Module 1 foundation docs) -> /10x-roadmap -> backlog-ready roadmap items
```

`/10x-roadmap` is the lesson focus. `/10x-new` is intentionally introduced in Module 2, Lesson 2, when a selected roadmap item becomes an implementation change folder.

### Task Router - Where to start

| Skill | Use it when |
| --- | --- |
| **Roadmap (lesson focus)** | |
| `/10x-roadmap` | You have `context/foundation/prd.md` and a scaffolded project baseline, and you need a vertical-first MVP roadmap. The skill reads the PRD, inspects the code baseline, uses available foundation docs such as `tech-stack.md`, `infrastructure.md`, and `deploy-plan.md`, then writes `context/foundation/roadmap.md`. Use it BEFORE creating per-change folders or implementation plans. |
| **Re-run upstream if needed** | |
| `/10x-shape` / `/10x-prd` / `/10x-tech-stack-selector` / `/10x-bootstrapper` / `/10x-agents-md` / `/10x-infra-research` | Bundled from Module 1 so foundation contracts can be fixed before roadmap sequencing. If roadmap generation exposes a PRD gap, repair the PRD before pretending the backlog is ready. |

### How the chain hands off

- `/10x-roadmap` bridges product and implementation. It does not choose frameworks, design schemas, or write a per-change implementation plan.
- The output is `context/foundation/roadmap.md`: ordered milestones, vertical slices, bounded foundations, dependencies, unknowns, risk, and backlog handoff fields.
- Roadmap items should receive stable human-readable identifiers in backlog tools. The actual `context/changes/<change-id>/` folder is created in Lesson 2 with `/10x-new`.

### Roadmap boundaries

- Default to vertical slices: user-visible outcomes that cross UI, data, business logic, and integrations.
- Horizontal work is allowed only as a bounded enabler that names the downstream vertical milestone it unlocks.
- Avoid orphan horizontal work such as "build the whole database", "build all API endpoints", or "design the whole UI" before the first user-visible flow.
- Roadmap is not a calendar estimate. Do not invent dates, story points, or sprint velocity unless the user explicitly asks for a separate planning artifact.

### Foundation paths used by this lesson

- `context/foundation/prd.md` - input
- `context/foundation/tech-stack.md` - optional input
- `context/foundation/infrastructure.md` - optional input
- `context/deployment/deploy-plan.md` - optional input
- `context/foundation/roadmap.md` - output
- `context/foundation/lessons.md` - recurring rules and pitfalls
- `docs/reference/contract-surfaces.md` - load-bearing names registry

Skills must not write to `context/archive/`. Archived changes are immutable; if a resolved target path starts with `context/archive/`, abort with: "This change is archived. Open a new change with `/10x-new` instead."

<!-- END @przeprogramowani/10x-cli -->
