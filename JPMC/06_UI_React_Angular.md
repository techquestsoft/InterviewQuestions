# UI Development – React & Angular
**Role:** Senior Manager of Software Engineering – JPMorgan Chase (BBAO)

> Note: As an Engineering Manager, my role here is to guide architecture, quality, and delivery — not to write production UI code daily. These answers reflect that lens.

---

## Q1: How do you guide your team in building modern UI applications using React or Angular?

**Memory Trick:** Architecture → Component Library → State → Performance → Accessibility

- **Clear architecture** – Decide upfront on routing, state management, API layer, and folder structure. Document it so all engineers follow the same pattern.
- **Reusable component library** – Build or adopt a shared design system (Material, Ant Design, or in-house) so we don't rebuild buttons, forms, modals.
- **State management strategy** – Redux/Zustand for React, NgRx for Angular when needed. Avoid Redux for everything — local state where possible.
- **Performance discipline** – Bundle size budgets, code splitting, lazy loading routes, memoization where it matters.
- **Accessibility (a11y)** – WCAG 2.1 AA minimum. Critical for customer-facing banking journeys where compliance and inclusion matter.

---

## Q2: How do you decide between React and Angular for a project?

**Memory Trick:** Team Skill → Ecosystem → Project Size → Existing Stack

- **Team skill** – Use what the team is strong in. Forcing a switch costs months of productivity.
- **Ecosystem** – React for flexibility and rich ecosystem of libraries; Angular for batteries-included opinions and enterprise apps.
- **Project size and complexity** – Angular's structure (modules, services, DI) shines for larger enterprise apps. React fits faster, lighter projects.
- **Existing stack** – If the org has a React platform or Angular platform, align rather than fragment.
- **Long-term maintenance** – Hiring and onboarding ease matters. Don't pick the trendier tool if it leaves you with a maintenance burden.

---

## Q3: How do you ensure UI performance for customer-facing journeys?

**Memory Trick:** Measure → Bundle → Render → Network → Perceived Performance

- **Measure with real data** – Lighthouse, Web Vitals (LCP, FID, CLS), real user monitoring (RUM) in production.
- **Bundle optimization** – Code splitting per route, tree-shaking, lazy loading, removing unused dependencies.
- **Render optimization** – `React.memo`/`useMemo`/`useCallback` where they help (not blanket); `OnPush` change detection in Angular.
- **Network discipline** – API call batching, caching with React Query / RTK Query / Angular signals + HTTP cache.
- **Perceived performance** – Skeleton screens, optimistic UI updates, progress indicators. Speed is felt, not just measured.

---

## Q4: How do you manage state in a complex UI application?

**Memory Trick:** Local → Server → Global → Form

- **Local state** – Component state for UI concerns (modal open, input focus). Don't promote everything to global.
- **Server state** – React Query, RTK Query, or NgRx Data. Cache, refetch, and sync with backend automatically.
- **Global app state** – Redux/Zustand/NgRx only for truly cross-cutting concerns (auth, current user, theme, feature flags).
- **Form state** – Dedicated tools (React Hook Form, Formik, Angular Reactive Forms). Forms have unique needs.
- **Avoid one-size-fits-all** – Different state needs different tools. Putting everything in Redux is a common antipattern.

---

## Q5: How do you ensure UI quality through testing?

**Memory Trick:** Unit → Component → Integration → E2E → Visual

- **Unit tests** – Pure logic and utilities (Jest).
- **Component tests** – React Testing Library / Angular TestBed. Test behavior, not implementation.
- **Integration tests** – Test components with their data layer mocked at the API boundary.
- **End-to-end tests** – Cypress or Playwright for critical user journeys (login, account origination flow). Selective — they're slow and brittle.
- **Visual regression** – Chromatic, Percy, or Storybook+visual diff for design-critical components.

---

## Q6: How do you secure customer-facing UI applications?

**Memory Trick:** Auth → XSS → CSRF → Secrets → Headers

- **Authentication** – OAuth2/OIDC with PKCE flow for SPAs. Tokens in HttpOnly secure cookies, not local storage.
- **XSS prevention** – Framework escaping (React/Angular default escape), avoid `dangerouslySetInnerHTML`/`innerHTML`. Sanitize user input.
- **CSRF protection** – SameSite cookies, anti-CSRF tokens for sensitive POST endpoints.
- **No secrets in frontend** – Frontend is public. API keys, credentials, business rules belong on the server.
- **Security headers** – CSP, X-Frame-Options, HSTS, Referrer-Policy. Configured at the gateway/CDN.

---

## Q7: How do you handle accessibility (a11y) in customer-facing UIs?

**Memory Trick:** Semantic → Keyboard → ARIA → Contrast → Test

- **Semantic HTML first** – `<button>`, `<nav>`, `<form>`, proper headings. Most a11y is solved by using HTML correctly.
- **Keyboard navigation** – Every interactive element reachable and operable by keyboard. Focus order is logical and visible.
- **ARIA where needed** – For custom components only. Wrong ARIA is worse than no ARIA.
- **Color contrast** – WCAG AA minimum (4.5:1 for normal text). Don't rely on color alone for state.
- **Test with tools and users** – axe, Lighthouse, screen readers (NVDA/VoiceOver). Real user testing for critical flows.

---

## Q8: How do you handle API integration and error states in the UI?

**Memory Trick:** Loading → Success → Empty → Error → Retry

- **Always design four states** – Loading, success, empty (no data), error. Skipping any of them creates a poor UX.
- **Centralize API calls** – Single HTTP client (Axios, Angular HttpClient) with interceptors for auth, retries, logging.
- **Handle errors gracefully** – User-friendly messages, retry options, fallback to cached data where possible.
- **Optimistic updates with rollback** – For actions where the user expects instant feedback (likes, toggles).
- **Correlation IDs in headers** – Sent on every request, surfaced in error UI for support traceability.

---

## Q9: How do you ensure a consistent design system across multiple UI applications?

**Memory Trick:** Centralize → Versioned → Documented → Adopted

- **Centralized component library** – One repo, published as an npm package, used by all UI apps.
- **Semantic versioning** – Breaking changes follow major version bumps; consumers upgrade on their schedule.
- **Documented with Storybook** – Every component has examples, props, accessibility notes, do's and don'ts.
- **Design tokens** – Colors, spacing, typography in a shared tokens file consumed by both code and Figma.
- **Drive adoption** – Make the right thing the easy thing. Hard to use library = teams build their own.

---

## Q10: How do you guide a team through a major UI migration (e.g., legacy → React/Angular)?

**Memory Trick:** Strangler → Slice → Coexist → Migrate → Retire

- **Strangler fig pattern** – New UI replaces legacy piece by piece, not big-bang. Reduces risk significantly.
- **Slice by route or feature** – Identify self-contained features (e.g., document upload page) to migrate first.
- **Coexistence strategy** – Iframe, micro-frontend, or shared shell so old and new run side-by-side during migration.
- **Migrate in order of value** – Highest-traffic, most-buggy, or most-complained-about screens first to show ROI early.
- **Retire deliberately** – Once legacy traffic drops below threshold, formally retire and clean up. Don't leave zombies.

> Strong closing line: *"As a manager, my job on UI is to set architecture and quality standards, then trust the team. I stay close enough to spot problems early, far enough to let engineers own their craft."*

---
