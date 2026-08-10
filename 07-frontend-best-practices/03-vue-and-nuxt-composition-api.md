---
title: "Vue's Composition API and Nuxt, Judged Fairly Against Next.js"
author: Mengty LIM
category: 07-frontend-best-practices
last_updated: 2026
---

# Vue's Composition API and Nuxt, Judged Fairly Against Next.js

Vue 3's Composition API and React hooks solve the same problem — reusable stateful logic — and arrive at different answers: Vue tracks dependencies for you, React re-runs and asks you to declare them. Neither is wrong, and a team choosing between Nuxt and Next.js in 2026 is choosing conventions and hiring pool far more than capability.

## The Real-World Problem

An enterprise-solutions group at a pensions administrator is rebuilding the **member self-service platform** — 900,000 scheme members checking projected benefits, updating beneficiaries, and running retirement-illustration calculators. The existing app is Vue 2 with the Options API, an `EventBus`, and Vuex, and it is at end of life.

The rebuild committee split. The five engineers who had shipped the Vue 2 app wanted Nuxt 4. The two engineers hired from a bank's digital team wanted Next.js. The argument ran for six weeks and generated a 40-page comparison document that answered the wrong question — "which framework is better" — instead of the two that mattered: *can this team ship and operate it*, and *does either framework fail a requirement we actually have?*

The requirements that decided it were mundane. The illustration calculator needed a heavy actuarial rules engine that must not reach the browser (both frameworks handle this). The scheme-literature section needed SEO and CMS-driven revalidation (both). The member dashboard was per-member and personalised (both). And the team had five engineers with three years of production Vue and two with React. There was no technical differentiator; there was a staffing one. They chose Nuxt, and shipped in five months.

The document was still useful — for the parts that were genuinely different. Those are below.

## The Concept

### Composition API fundamentals

```ts
import { ref, reactive, computed, watch, onMounted } from 'vue'
```

| Primitive | Purpose | Gotcha |
|---|---|---|
| `ref(v)` | Reactive box for any value | Access via `.value` in script; auto-unwrapped in templates |
| `reactive(obj)` | Deeply reactive object (Proxy) | Destructuring loses reactivity; cannot be reassigned wholesale |
| `computed(fn)` | Derived, cached, re-evaluates only when deps change | Must be pure — no side effects |
| `watch(src, cb)` | React to a specific change; gives old + new value | Lazy by default; pass `{ immediate: true }` to run at once |
| `watchEffect(fn)` | Auto-tracks everything read inside | Convenient, easy to over-trigger; prefer explicit `watch` in complex code |

The key mechanical difference from React: Vue's reactivity is **fine-grained and dependency-tracked**. A component function runs once; the render effect re-runs only when the specific reactive values it read change. React re-runs the whole component function and reconciles. That is why Vue has no `useMemo`/`useCallback` culture and no dependency arrays, and why React 19 needed a compiler to reach a comparable place.

**Composables** are Vue's hooks: plain functions named `useX` that create refs and return them. Same idea, fewer rules — no call-order constraint, so they can live inside conditionals, though lifecycle hooks registered inside must still run during setup.

### `<script setup>`

`<script setup>` is the only form worth writing in new code. Top-level bindings are exposed to the template automatically; `defineProps`/`defineEmits` are compiler macros with full TypeScript inference.

```vue
<script setup lang="ts">
const props = defineProps<{ memberId: string; projectionAge: number }>()
const emit = defineEmits<{ recalculate: [age: number] }>()
</script>
```

That is the whole ceremony. No `export default`, no `components: {}` registration.

### Pinia, briefly

Pinia replaced Vuex and is the official store. A store is a composable with an identity — same `ref`/`computed` primitives, plus devtools, SSR serialization, and cross-component sharing.

**The important discipline is the same as in React:** a store is for *client* state (UI preferences, a multi-step wizard's draft, the authenticated member's profile). Server data belongs in the data layer — `useAsyncData`/`useFetch` in Nuxt, or TanStack Query, which has a first-class Vue adapter. Putting API responses in Pinia recreates the Vuex-as-cache mistake: manual invalidation, stale data, no request de-duplication.

### Nuxt vs. Next.js: where they genuinely differ

**File routing.** Both are directory-based. Nuxt uses `pages/` with `[id].vue` and `[...slug].vue`; Next.js App Router uses `app/` with `page.tsx` and folder-based dynamic segments. Nuxt's route groups are `(group)` in recent versions too. Effectively equivalent.

**Data fetching — the real divergence.**

- **Next.js:** the component *is* the data fetch. A Server Component `await`s directly; the result is serialized into the RSC payload. There is no client-side fetch for initial data, and the fetching code never ships.
- **Nuxt:** `useAsyncData` / `useFetch` run during SSR, and the result is transferred to the client via `useState`-backed payload for hydration. The same composable also works on the client for subsequent navigation. Nuxt Server Components (`.server.vue`) exist and are useful, but the mainstream Nuxt path is isomorphic composables, not server-only components.

The practical consequence: in Next.js, keeping a heavy library out of the bundle is a matter of *where the component lives*. In Nuxt, it is a matter of putting that code in `server/` (a server route or `server/utils`) and calling it over `$fetch`. Both work; the Nuxt version is more explicit, the Next.js version more automatic — and more surprising when you get the boundary wrong.

**Server routes.** Nuxt ships Nitro with `server/api/*.ts` — full backend routes, typed end-to-end, deployable to any runtime Nitro supports. Next.js has Route Handlers (`app/api/*/route.ts`) plus Server Actions. Nuxt's server story feels more like a real backend; Next's Server Actions are a smoother mutation path.

**Rendering modes.** Nuxt's `routeRules` is the cleanest expression of hybrid rendering in either ecosystem — one config block declaring per-route strategy:

```ts
routeRules: {
  '/': { prerender: true },
  '/literature/**': { isr: 3600 },
  '/dashboard/**': { ssr: true, cache: false },
  '/calculator': { ssr: false },   // SPA island
}
```

Next.js expresses the same intent per segment via `export const dynamic`, `revalidate`, and `fetch` options. Nuxt is more centralised and reviewable; Next.js is more local and colocated.

### Comparison table

| Dimension | Nuxt 4 | Next.js 15+ App Router |
|---|---|---|
| Reactivity | Fine-grained, dependency-tracked; no dep arrays | Re-render + reconcile; React Compiler handles memoization |
| Component model | SFC (`<template>/<script setup>/<style scoped>`) | JSX/TSX, function components |
| Initial data | `useAsyncData`/`useFetch`, isomorphic, payload-hydrated | Server Components `await` directly; no client fetch |
| Keeping code off the client | Put it in `server/`, call via `$fetch` | Component placement + `'use client'` boundary |
| Mutations | `server/api` route + `$fetch` | Server Actions (`'use server'`) |
| Rendering config | Centralised `routeRules` (prerender / isr / ssr / swr) | Per-segment exports + per-`fetch` options |
| Client state | Pinia (official) | Zustand / Context / URL state — no official store |
| Styling | Scoped CSS built in; Tailwind via module | Tailwind / CSS Modules |
| Auto-imports | Yes — components, composables, utils | No — explicit imports |
| Hiring pool | Smaller, strong in EU enterprise and internal tooling | Largest; most enterprise contractors default here |
| Ecosystem depth | Excellent modules; fewer niche libraries | Broadest third-party library coverage |

Auto-imports deserve a note: they cut boilerplate substantially and are genuinely pleasant, but they make provenance less obvious in code review, which some regulated-change processes dislike. It is a real trade-off, not a bug.

## How It Works

```mermaid
flowchart TD
    subgraph nuxt["Nuxt 4 request"]
        NR["Request /dashboard"] --> NS["Nitro server<br/>routeRules: ssr"]
        NS --> NC["Vue component setup()<br/>useAsyncData('projection')"]
        NC --> NAPI["server/api/projection.get.ts<br/>actuarial engine stays here"]
        NAPI --> NP["Render HTML + serialize payload"]
        NP --> NH["Client hydrates from payload<br/>no refetch"]
    end

    subgraph next["Next.js 15 request"]
        XR["Request /dashboard"] --> XS["Server: RSC render"]
        XS --> XC["Server Component<br/>await getProjection()"]
        XC --> XE["actuarial engine imported<br/>server-side only"]
        XE --> XP["Stream RSC payload"]
        XP --> XH["Client renders payload<br/>'use client' leaves hydrate"]
    end

    style nuxt fill:#1f513f,color:#fff
    style next fill:#7a4a1e,color:#fff
```

Both keep the actuarial engine off the client and both avoid a duplicate client fetch. The difference is *how* the boundary is declared: a directory in Nuxt, a component's location in Next.js.

## Practical Example

The pensions projection panel, written both ways.

**Nuxt — server route + composable:**

```ts
// server/api/projection.get.ts
import { z } from 'zod'
import { calculateProjection } from '~~/server/actuarial/engine' // never bundled

const query = z.object({
  memberId: z.string().uuid(),
  retirementAge: z.coerce.number().int().min(55).max(75),
})

export default defineEventHandler(async (event) => {
  const session = await requireMemberSession(event)
  const { memberId, retirementAge } = query.parse(getQuery(event))

  if (session.memberId !== memberId) {
    throw createError({ statusCode: 403, statusMessage: 'Forbidden' })
  }

  const record = await db.contributions.forMember(memberId)
  return calculateProjection(record, retirementAge) // { annualPension, lumpSum, assumptions }
})
```

```vue
<!-- app/pages/dashboard/projection.vue -->
<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import { useMemberStore } from '~/stores/member'

const member = useMemberStore()
const retirementAge = ref(65)

const { data, status, error, refresh } = await useAsyncData(
  () => `projection:${member.id}:${retirementAge.value}`, // key includes every input
  () =>
    $fetch('/api/projection', {
      query: { memberId: member.id, retirementAge: retirementAge.value },
    }),
  { watch: [retirementAge], server: true },
)

const annualPension = computed(() =>
  data.value
    ? new Intl.NumberFormat('en-GB', { style: 'currency', currency: 'GBP' })
        .format(data.value.annualPension)
    : '—',
)

// explicit watch: audit trail for a regulated calculator
watch(retirementAge, (age, previous) => {
  if (previous !== undefined) trackEvent('projection_age_changed', { from: previous, to: age })
})
</script>

<template>
  <section class="space-y-4">
    <h1 class="text-lg font-semibold">Projected benefits</h1>

    <label class="block text-sm">
      Retirement age
      <input
        v-model.number="retirementAge"
        type="range"
        min="55"
        max="75"
        class="w-full"
        aria-describedby="projection-value"
      />
    </label>

    <p v-if="status === 'pending'" aria-live="polite">Recalculating…</p>
    <p v-else-if="error" role="alert" class="text-amber-700">
      Projection unavailable.
      <button type="button" class="underline" @click="refresh()">Retry</button>
    </p>
    <p v-else id="projection-value" class="text-2xl tabular-nums">
      {{ annualPension }} <span class="text-sm text-neutral-500">per year</span>
    </p>
  </section>
</template>
```

**Nuxt — a composable and a Pinia store, showing the client/server state split:**

```ts
// app/composables/useDensity.ts — client UI state, no server involvement
import { ref, watchEffect } from 'vue'

export function useDensity() {
  const density = ref<'comfortable' | 'compact'>('comfortable')
  watchEffect(() => {
    if (import.meta.client) document.documentElement.dataset.density = density.value
  })
  return { density }
}
```

```ts
// app/stores/member.ts — identity, not server cache
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useMemberStore = defineStore('member', () => {
  const id = ref<string>('')
  const schemeName = ref<string>('')
  const isDeferred = ref(false)

  const displayScheme = computed(() =>
    isDeferred.value ? `${schemeName.value} (deferred)` : schemeName.value,
  )

  function hydrate(payload: { id: string; schemeName: string; isDeferred: boolean }) {
    id.value = payload.id
    schemeName.value = payload.schemeName
    isDeferred.value = payload.isDeferred
  }

  return { id, schemeName, isDeferred, displayScheme, hydrate }
})
```

**Next.js — the same panel, for comparison:**

```tsx
// app/(member)/dashboard/projection/page.tsx  — Server Component
import { calculateProjection } from '@/server/actuarial/engine' // stays server-side
import { requireMemberSession } from '@/server/auth'
import { AgeSlider } from './age-slider' // 'use client' — the only interactive leaf

export const dynamic = 'force-dynamic'

export default async function ProjectionPage({
  searchParams,
}: {
  searchParams: Promise<{ age?: string }>
}) {
  const session = await requireMemberSession()
  const { age } = await searchParams
  const retirementAge = Math.min(75, Math.max(55, Number(age ?? 65)))

  const record = await db.contributions.forMember(session.memberId)
  const projection = calculateProjection(record, retirementAge)

  const formatted = new Intl.NumberFormat('en-GB', {
    style: 'currency',
    currency: 'GBP',
  }).format(projection.annualPension)

  return (
    <section className="space-y-4">
      <h1 className="text-lg font-semibold">Projected benefits</h1>
      <AgeSlider value={retirementAge} />
      <p className="text-2xl tabular-nums">
        {formatted} <span className="text-sm text-neutral-500">per year</span>
      </p>
    </section>
  )
}
```

**A composable test — Vue logic is testable without a DOM:**

```ts
// app/composables/useRetirementAge.test.ts
import { describe, expect, it } from 'vitest'
import { ref, computed, nextTick } from 'vue'

function useRetirementAge(initial = 65) {
  const age = ref(initial)
  const isEarly = computed(() => age.value < 65)
  function bump(by: number) {
    age.value = Math.min(75, Math.max(55, age.value + by))
  }
  return { age, isEarly, bump }
}

describe('useRetirementAge', () => {
  it('clamps to the scheme minimum', () => {
    const { age, bump } = useRetirementAge(56)
    bump(-10)
    expect(age.value).toBe(55)
  })

  it('recomputes isEarly reactively', async () => {
    const { age, isEarly } = useRetirementAge(65)
    expect(isEarly.value).toBe(false)
    age.value = 60
    await nextTick()
    expect(isEarly.value).toBe(true)
  })
})
```

## Enterprise Trade-offs

| Approach | Pros | Cons | When to use (Banking / Insurance / Enterprise) |
|---|---|---|---|
| **Nuxt 4 + `routeRules` hybrid** | One reviewable block declares per-route rendering; Nitro is a genuine backend; auto-imports cut boilerplate | Smaller hiring pool; auto-imports obscure provenance in review | Teams with existing Vue depth; internal tooling and member portals where the same repo also serves API routes |
| **Next.js 15 App Router + RSC** | Largest ecosystem and contractor pool; heaviest code stays server-side by placement; Server Actions | Serialization boundary surprises; caching defaults have historically shifted | Greenfield customer-facing work in organisations already standardised on React |
| **Vue 3 Composition API + `<script setup>`** | No dependency arrays; fine-grained updates; excellent TS inference in SFCs | Two idioms still in the wild (Options API legacy code) | All new Vue code — never start a new component with the Options API |
| **Pinia for client state only** | Simple, typed, devtools, SSR-safe | Tempting to misuse as a server cache | Auth/profile, wizard drafts, UI preferences. Not API responses |
| **Nuxt/Vue + TanStack Query** | Proper server-state caching, invalidation, retries in the Vue ecosystem | Two data layers if mixed carelessly with `useAsyncData` | Data-dense operational screens with heavy filtering and refetching |
| **Migrating a working Vue 2 app to React "for the ecosystem"** | — | Rewrite risk, knowledge loss, no requirement it satisfies | Only if a hard requirement genuinely fails — the ecosystem alone is not one |

## Why This Still Matters Through 2030

The two ecosystems are converging on the same architecture — server-rendered by default, fine-grained reactivity, islands of interactivity, per-route rendering strategy — which is the strongest signal that these are properties of the problem rather than fashions. Vue's reactivity model in particular has aged extremely well: signals arrived in Angular, Solid, Svelte 5, and Preact after Vue had shipped the idea, and React's compiler exists largely to recover ergonomics that dependency-tracked reactivity gives for free. That means the Composition API is not a Vue-specific skill; it is the mainstream reactive model, and knowing it makes every other signals-based framework legible. For an enterprise choosing between Nuxt and Next.js, the durable advice is unchanged and slightly deflating: both will still be maintained and capable in 2030, the framework will not be why your project succeeds or fails, and the decision should be made on the team you have, the runtime you can operate, and the specific requirements one of them actually fails — not on a 40-page feature matrix.

→ Next: [04-styling-with-tailwind-css.md](04-styling-with-tailwind-css.md) · Related: [../00-project-setup-roadmap/03-project-structure.md](../00-project-setup-roadmap/03-project-structure.md) · [../05-apis-and-integration/01-rest-api-design-principles.md](../05-apis-and-integration/01-rest-api-design-principles.md)
