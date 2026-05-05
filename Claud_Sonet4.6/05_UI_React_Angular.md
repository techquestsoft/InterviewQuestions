# Interview Prep — File 5 of 8
# UI Development — React & Angular

> **Tailored for:** JPMorgan Chase — Senior Manager of Software Engineering, BBAO team. UI scope: customer acquisition and account origination journeys.
>
> **Rule 1:** You are interviewing as an Engineering Manager, not as a senior front-end IC. Lead with architecture, quality, team practices, and trade-offs — not framework syntax.
> **Rule 2:** Be honest about depth. You worked on JavaScript/jQuery in 2014, then moved to middleware and data engineering. Modern React/Angular is your team's craft, not your daily code. Frame answers from that lens.
> **Rule 3:** UI quality in banking = security + accessibility + performance + reliability. All four are non-negotiable.

---

## CROSS-FILE INDEX

This file owns: React vs Angular decision, UI architecture, state management, performance, accessibility, UI security, design system collaboration, UI delivery practices.
- API contract design and Spring Boot backend → File 04
- CI/CD for UI builds, deployment strategies → File 06
- A/B testing and feature flags → File 06

---

## HONESTY FRAMING (USE WHEN ASKED ABOUT UI HANDS-ON)

If an interviewer probes whether you code React/Angular daily, do not pretend.

> "I want to be honest about where my hands-on depth is. I have a strong JavaScript and jQuery foundation from earlier in my career. Since 2014, I've been deeper on middleware, data platforms, and engineering leadership. My UI engineers are the ones writing production React and Angular daily — my role is architecture, quality standards, design-system alignment, performance budgets, and shipping discipline. I stay close enough to make decisions and review approaches, but I don't claim to be the strongest React engineer in the room."

This answer is stronger than fake fluency. Hiring managers respect calibrated self-awareness.

---

## SECTION A — UI ARCHITECTURE & FRAMEWORK CHOICE

---

### Q1: How do you guide your team in building modern UI applications?

**Memory Hook:** Architecture → Component Library → State → Performance → Accessibility

> "Five disciplines I enforce, regardless of framework.
>
> **Architecture decided upfront.** Routing, state management, API layer, folder structure — documented before the first feature ships. Engineers shouldn't be inventing patterns story-by-story.
>
> **Reusable component library.** Buttons, forms, modals, data tables — built or adopted from a shared design system (Material, Ant, in-house). Engineers compose; they don't rebuild primitives.
>
> **State management strategy.** Local state where possible. Server state via React Query, RTK Query, or NgRx Data. Global state only for truly cross-cutting concerns — auth, current user, feature flags. The most common UI antipattern is putting everything in Redux.
>
> **Performance discipline.** Bundle size budgets per route, code splitting, lazy loading, memoization where it actually helps.
>
> **Accessibility — WCAG 2.1 AA minimum.** Banking journeys are regulated. Customer-facing flows must work for screen readers, keyboard-only navigation, and high-contrast modes."

---

### Q2: React vs Angular — how do you decide?

**Memory Hook:** Team Skill → Ecosystem → Project Size → Existing Stack

> "Four factors.
>
> **Team skill.** Use what the team is strong in. Forcing a switch costs months of productivity. This is the dominant factor in my experience.
>
> **Ecosystem.** React for flexibility and a rich library ecosystem. Angular for batteries-included opinions — DI, routing, forms, RxJS — that suit large enterprise apps.
>
> **Project size and complexity.** Angular's structure shines on big enterprise apps with multiple teams. React's lighter footprint fits faster, more focused projects.
>
> **Existing stack.** If JPMC has a Java/Spring Boot + React or Angular platform standardized, align with it. Fragmentation is more expensive than the marginal benefit of the 'better' tool.
>
> Honest framing: as a manager, I don't choose 'my preferred framework.' I choose the one that lets the team ship reliably for the next three to five years."

---

### Q3: How do you think about state management in a complex UI app?

**Memory Hook:** Local → Server → Global → Form

> "Four buckets, each with the right tool.
>
> **Local state** — UI concerns like modal-open, input-focus, hover. React useState or Angular component state. Don't promote everything to global.
>
> **Server state** — data fetched from APIs. React Query, RTK Query, or Angular's HttpClient + signals. Cache, refetch, sync — solved problem, don't reinvent.
>
> **Global app state** — auth, current user, theme, feature flags. Redux/Zustand for React, NgRx for Angular. Only for truly cross-cutting concerns.
>
> **Form state** — dedicated tools. React Hook Form or Formik; Angular Reactive Forms. Forms have unique needs — validation, dirty/pristine, submission state — generic state libraries handle them poorly.
>
> The antipattern: putting server data in Redux. That conflates two different problems and makes both harder."

---

## SECTION B — PERFORMANCE & QUALITY

---

### Q4: How do you ensure UI performance for customer-facing journeys?

**Memory Hook:** Measure → Bundle → Render → Network → Perceived

> "Five focuses.
>
> **Measure with real data.** Lighthouse, Web Vitals (LCP, FID, CLS), real-user monitoring in production. Without RUM you're optimizing what feels slow on your laptop, not what's slow for actual customers.
>
> **Bundle optimization.** Code splitting per route, tree-shaking, lazy loading non-critical chunks, removing unused dependencies. Bundle size budget enforced in CI.
>
> **Render optimization.** React.memo / useMemo / useCallback where they actually help — not blanket. Angular OnPush change detection. Profile before optimizing.
>
> **Network discipline.** Batch API calls, cache responses, use CDN for static assets, prefetch what's likely next.
>
> **Perceived performance.** Skeleton screens, optimistic UI updates, progress indicators. Speed is felt, not just measured. A 300ms response with a skeleton feels faster than 200ms with a blank screen."

---

### Q5: How do you ensure UI quality through testing?

**Memory Hook:** Unit → Component → Integration → E2E → Visual

> "Five levels, each with a specific purpose.
>
> **Unit tests** — pure logic and utilities. Jest. Fast, run on every commit.
>
> **Component tests** — React Testing Library, Angular TestBed. Test behavior, not implementation. Don't assert on internal state — assert on what the user sees.
>
> **Integration tests** — components with their data layer mocked at the API boundary.
>
> **End-to-end tests** — Cypress or Playwright. Selective — only critical user journeys (login, account origination flow). E2E tests are slow and brittle; over-investing here is a common mistake.
>
> **Visual regression** — Chromatic, Percy, or Storybook + visual diff. Catches design drift that functional tests miss.
>
> Coverage targets are signals, not goals. 80% on business logic is more meaningful than 100% on boilerplate."

---

### Q6: How do you handle API integration and error states in the UI?

**Memory Hook:** Loading → Success → Empty → Error → Retry

> "Always design four states for every data-driven view.
>
> **Loading** — skeleton or spinner. Tells the user the system is alive.
>
> **Success** — the happy path with data.
>
> **Empty** — no data yet, or no results match. Different from loading. Explicit message and next action.
>
> **Error** — graceful message, retry option where appropriate, fallback to cached data when possible.
>
> Implementation discipline: centralized API client with interceptors for auth, retries, logging. Correlation IDs sent on every request and surfaced in the error UI for support. Optimistic updates with rollback on failure for snappy interactions like toggles or likes.
>
> Skipping any of the four states creates a bad UX — usually 'empty' is the one teams forget."

---

## SECTION C — SECURITY & ACCESSIBILITY

---

### Q7: How do you secure customer-facing UI applications in banking?

**Memory Hook:** Auth → XSS → CSRF → Secrets → Headers

> "Five non-negotiables.
>
> **Authentication.** OAuth2/OIDC with PKCE for SPAs. Tokens in HttpOnly secure cookies, never in localStorage — localStorage is reachable by injected scripts.
>
> **XSS prevention.** Framework escaping by default (React/Angular handle this well). Avoid dangerouslySetInnerHTML / innerHTML. Sanitize any user-generated HTML server-side.
>
> **CSRF protection.** SameSite cookies, anti-CSRF tokens for sensitive POST endpoints, double-submit cookie pattern when needed.
>
> **No secrets in the frontend.** Frontend code is public. API keys, business rules, credentials all belong on the server. Anything shipped to the browser is leaked by definition.
>
> **Security headers.** CSP, X-Frame-Options, HSTS, Referrer-Policy. Configured at the gateway or CDN, not per-app.
>
> In banking add: session timeout enforcement on the client, idle warning before logout, no sensitive data in URL parameters or browser history, screen-record / screenshot detection for highly sensitive flows where required."

---

### Q8: How do you handle accessibility in a banking UI?

**Memory Hook:** Semantic → Keyboard → ARIA → Contrast → Test

> "Five-step practice. Banking is regulated — accessibility is a compliance requirement, not a nice-to-have.
>
> **Semantic HTML first.** button, nav, form, proper heading hierarchy. Most accessibility is solved by using HTML correctly. Custom div soup is the root cause of most a11y bugs.
>
> **Keyboard navigation.** Every interactive element reachable and operable by keyboard. Focus order matches visual order. Focus indicators always visible.
>
> **ARIA only where needed.** For genuinely custom components — combo boxes, tabs, dialogs. Wrong ARIA is worse than no ARIA.
>
> **Color contrast.** WCAG AA (4.5:1 for body text, 3:1 for large text). Never rely on color alone — pair with icons, text, or patterns.
>
> **Test with tools and users.** axe, Lighthouse, manual screen-reader testing with NVDA or VoiceOver. For critical flows, real-user testing with assistive technology users when possible."

---

## SECTION D — DESIGN SYSTEM, COLLABORATION & DELIVERY

---

### Q9: How do you ensure design-system consistency across multiple UI applications?

**Memory Hook:** Centralize → Versioned → Documented → Adopted

> "Four practices.
>
> **Centralized component library.** One repo, published as an npm package, consumed by every UI app. Standardized buttons, forms, tables, modals, navigation.
>
> **Semantic versioning.** Breaking changes require a major version bump. Consumers upgrade on their own schedule.
>
> **Documented with Storybook.** Every component shows examples, prop tables, accessibility notes, do's and don'ts. New engineers learn the system in a day, not a sprint.
>
> **Design tokens.** Colors, spacing, typography in a shared tokens file consumed by both code and Figma. Designers and engineers stay in sync.
>
> Adoption is the hard part. Make the right thing the easy thing. If the library is hard to use, teams build their own. I push for ergonomics over completeness."

---

### Q10: How do you guide a team through a major UI migration (e.g., legacy → React/Angular)?

**Memory Hook:** Strangler → Slice → Coexist → Migrate → Retire

> "Five-phase approach. Big-bang rewrites fail; incremental migration works.
>
> **Strangler fig pattern.** New UI replaces legacy piece by piece, behind the same URL or shell.
>
> **Slice by route or feature.** Start with self-contained, high-traffic, or high-pain screens. Document upload, account dashboard, login — depending on what hurts most.
>
> **Coexistence.** Iframe, micro-frontend, or shared shell so old and new run side-by-side during migration. No flag day.
>
> **Migrate in order of value.** Highest-traffic, most-buggy, or most-customer-complained-about first. Show ROI early — that funds the rest.
>
> **Retire deliberately.** Once legacy traffic drops below threshold and no critical flows remain, formally retire and decommission. Otherwise, you carry two systems forever.
>
> Real lesson: at Optum during the OpenShift to Kubernetes migration, the same playbook applied — pilot first, parallel run, traffic shift, retire. The pattern is the same; only the tech changes."

---

## QUICK REFERENCE — UI MEMORY HOOKS

| Topic | Memory Hook | One-Line Answer |
|-------|------------|----------------|
| UI architecture | Architecture → Library → State → Performance → A11y | Decide upfront, enforce consistently |
| React vs Angular | Team Skill → Ecosystem → Size → Existing Stack | Use what the team can ship reliably |
| State management | Local → Server → Global → Form | Right tool per bucket; not everything in Redux |
| Performance | Measure → Bundle → Render → Network → Perceived | RUM in prod, budgets enforced in CI |
| Testing | Unit → Component → Integration → E2E → Visual | Selective E2E; visual regression for design drift |
| API + errors | Loading → Success → Empty → Error → Retry | All four states designed, never skipped |
| Security | Auth → XSS → CSRF → Secrets → Headers | OAuth2 + HttpOnly cookies + framework escape |
| Accessibility | Semantic → Keyboard → ARIA → Contrast → Test | Semantic HTML first, ARIA last |
| Design system | Centralize → Version → Document → Adopt | Storybook + tokens + ergonomic API |
| UI migration | Strangler → Slice → Coexist → Migrate → Retire | Incremental wins; big-bang fails |

---

*File 5 of 8 — UI Development (React / Angular)*
