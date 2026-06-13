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

## 10xDevs AI Toolkit - Module 2, Lesson 2

Turn one roadmap item into the first implementation cycle with the **change planning chain**:

```
/10x-roadmap -> /10x-new -> /10x-plan -> /10x-plan-review -> /10x-implement
```

`/10x-new`, `/10x-plan`, `/10x-plan-review`, and `/10x-implement` are the lesson focus. `/10x-frame` and `/10x-research` are not required rituals here; they are escalation paths introduced in the next lesson.

### Task Router - Where to start

| Skill | Use it when |
| --- | --- |
| **Change setup (lesson focus)** | |
| `/10x-new <change-id>` | You selected a roadmap item and need a stable change folder. Creates `context/changes/<change-id>/change.md` so planning, implementation, progress, commits, and later review all share one identity. Use AFTER roadmap selection, BEFORE `/10x-plan`. |
| **Planning (lesson focus)** | |
| `/10x-plan <change-id>` | You have a change folder and need a reviewable implementation plan. Reads roadmap context, foundation docs, codebase evidence, and any existing change notes; writes `plan.md` and `plan-brief.md` with phases, file contracts, success criteria, and `## Progress`. |
| **Plan readiness (lesson focus)** | |
| `/10x-plan-review <change-id>` | You have `plan.md` and need a light pre-code readiness check. Use it to catch missing end state, weak contracts, malformed progress, scope drift, or blind spots before code changes begin. |
| **Implementation (lesson focus)** | |
| `/10x-implement <change-id> phase <n>` | You have an approved plan and want to execute one phase with verification, manual gate, commit ritual, and SHA write-back to `## Progress`. |
| **Lifecycle closure** | |
| `/10x-archive <change-id>` | A change is merged or intentionally closed. Move it out of active `context/changes/` into archive state. |

### How the chain hands off

- `/10x-new` creates the durable change identity.
- `/10x-plan` turns that identity into an implementation contract.
- `/10x-plan-review` checks the plan before the agent mutates code.
- `/10x-implement` executes one planned phase, verifies, asks for manual confirmation when needed, commits, and records progress.

### Lesson boundaries

- Plan is the default router after roadmap selection. Start with `/10x-plan` unless the problem is unclear or external evidence is blocking.
- Do not run `/10x-frame + /10x-research` as ceremony for every change.
- Do not turn this lesson into a full end-to-end product build. A checkpoint with a planned and partially or fully implemented stream is valid.
- Code review of the implemented diff belongs to Lesson 3 via `/10x-impl-review`.
- Lifecycle closure via `/10x-archive` after a change is merged or intentionally closed.

### Paths used by this lesson

- `context/foundation/roadmap.md` - upstream roadmap
- `context/changes/<change-id>/change.md` - change identity
- `context/changes/<change-id>/plan.md` - implementation contract
- `context/changes/<change-id>/plan-brief.md` - compressed handoff
- `context/foundation/lessons.md` - recurring rules and pitfalls
- `docs/reference/contract-surfaces.md` - load-bearing names registry

Skills must not write to `context/archive/`. Archived changes are immutable; if a resolved target path starts with `context/archive/`, abort with: "This change is archived. Open a new change with `/10x-new` instead."

<!-- END @przeprogramowani/10x-cli -->
