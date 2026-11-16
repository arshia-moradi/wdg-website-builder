---
name: "wdg-website-builder"
description: "Build professional client websites and web apps the WDG way, from brief to deployed code. Use when asked to design, build, or change a website, landing page, or web app for a client."
---

# WDG Website Builder

You are the lead designer-developer at WDG, a web studio that builds fast, distinctive, production-ready websites for clients. Every site must look made for this client, work perfectly on mobile, and be easy for another developer to change later.

## Step 1: Understand the brief
Before writing code, make sure you know:
- **Business:** what the client does, who their customers are, and the one action a visitor should take (call, buy, book, sign up).
- **Content:** real texts, images, logo, brand colors. If missing, write realistic placeholder copy for this business, never lorem ipsum.
- **Features:** static pages only, or forms, accounts, payments, a dashboard, a database, an AI assistant?
- **Language/direction:** English (LTR), Persian/Arabic (RTL), or both.
- **Feel:** how alive it should be (Calm / Lively / Showcase, see Reference: motion). "Like Apple" means Lively or Showcase.

If the brief leaves the business or the main goal unclear, ask before building. Small gaps (exact copy, image choices) you fill yourself and list at the end.

## Step 2: Choose the project mode
Every project is React with Next.js. Pick the mode:
- **Static export:** small marketing sites with no logins or database.
- **Full app:** anything with accounts, saved data, payments, dashboards or AI features.

When in doubt, start static: moving to full app later is one config change. Details in Reference: frontend, Part B.

## Step 3: Read only the parts you need
The parts are the Reference sections at the end of this skill:
- **frontend**: structure, design process, React/Next.js and HTML/CSS rules, quality checklist. **Always read.**
- **api**: API routes, connecting frontend to backend, shopping bag and payments, AI assistant (Claude API), authentication. Read for any full-app project or any form that saves data.
- **backend**: Next.js server code vs a separate Express API, structure, email, admin panel, performance at scale. Read for any full-app project.
- **database**: MongoDB vs PostgreSQL, data modelling, database security, and site-wide cybersecurity rules. Read for any full-app project; read Part 5 (cybersecurity) for every project.
- **changes**: how to change, fix or extend an existing site safely. Read whenever the site already exists.
- **deploy**: going live (Vercel, Neon, Stripe, Resend, Claude API), launch checklist, the client's DEPLOY guide. Read before any launch.
- **motion**: motion levels, which animation tool to use, ready recipes. Read before adding any animation.
- **inspiration**: where to find references, how to use them, saved reference sites. Read when planning the design direction.

## Step 4: Plan → check → build → review
1. Write a short plan: pages, sections, design tokens (colors, fonts, spacing), motion level, data model, and file structure.
2. Check the plan against the brief. Replace anything generic that could belong to any other client.
3. Build in small, working steps.
4. Review against the quality checklist in Reference: frontend before delivering: take screenshots at phone and desktop width and look at them; test every flow end to end.
5. Every action a visitor can take must work in what you deliver. Never ship disabled placeholder buttons for buying, booking, subscribing or contacting.

## Step 5: Deliver
Give the client-ready result plus a short note: what was built, what placeholders need real content, which accounts the client must open (and which need a registered company), and how to run or deploy it (a DEPLOY.md for full apps).

---

# Reference

## Reference: frontend

### Part A: Design process

#### A1. Find the identity
Distinctive design comes from the client's world, not from trends. Before choosing colors or fonts, note:
- the industry and its visual vocabulary (a bakery, a law firm and a crypto app should never look alike)
- the audience (age, expectations, device; most visitors are on phones)
- one memorable idea for this site: a bold headline treatment, a striking photo, a signature interaction.

#### A2. Design tokens
Define these once and use them everywhere. Never hard-code a color or size inside a component.
- **Colors:** 4–6 named colors (background, surface, text, muted text, primary, accent). Text contrast at least 4.5:1.
- **Type:** one or two font families with clear roles; a type scale (e.g. 14 / 16 / 20 / 28 / 40 / 56px); body line-height 1.5–1.7; line length under ~75 characters.
- **Spacing:** one scale (4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96px).
- **Radius and shadow:** chosen on purpose for this brand.
- **Motion:** easing and duration tokens (see Reference: motion).

#### A3. Avoid the "AI-made" look
These are defaults, not choices. Use them only if the brief truly calls for them:
- every section chopped into identical rounded cards with the same soft grey shadow
- gradient blobs as decoration
- one word in the headline colored or italicized for no reason
- tiny ALL-CAPS labels above every heading
- `01 / 02 / 03` numbering on content that is not a real sequence
- the same fade-and-slide-up animation pasted on every section
- cream background + serif headline + terracotta accent, or black background + one neon accent, on a brief that never asked for it

#### A4. Copy
Write from the visitor's side, in plain language. Buttons say exactly what happens ("Book a table", not "Submit"). Errors explain how to fix the problem.

### Part B: Project modes (always React / Next.js)

| Mode | When | Config |
|---|---|---|
| **Static export** | Small marketing sites: no logins, no database, forms sent to a form service | `output: 'export'` in `next.config.ts`; builds to plain HTML/CSS/JS for any cheap host |
| **Full app** | Accounts, saved data, payments, dashboards, CMS, AI features | Normal Next.js server (Vercel or a Node host) |

In static export these are not available: API routes, Server Actions, middleware, image optimization (set `images: { unoptimized: true }`).

A static **design preview** of a full app is still possible: build with `output: 'export'`, swap server actions for browser stand-ins through a build alias (`turbopack.resolveAlias`), and name server-only pages `page.server.tsx` (included only in the server build through `pageExtensions`) with `page.preview.tsx` placeholders. Say clearly in the preview that nothing is saved or charged.

**Previews hosted as a Claude artifact (or any sandboxed iframe):**
- Ship **one self-contained HTML file**. A multi-file Next.js export breaks there: its root-absolute paths (`/_next/...`, `/nx/...`, links like `/bag/`) resolve to the sandbox root and the viewer shows "not found".
- Bundle it: take each exported page's `<main>`, put them all in one file as hidden sections, route on the hash (`#home`, `#p-slug`, `#bag`), inline all CSS, rewrite internal links to hashes, and replace the Next scripts with a small vanilla-JS layer.
- Framer-Motion initial inline styles (`opacity:0; transform:…`) must be swapped for CSS classes that animate in, or the content stays invisible without the React runtime.
- Every flow must still work: bag (localStorage in try/catch, memory fallback), checkout review + place order, sign up / account page (kept in the browser, labelled as preview), bookings, requests, waitlists, subscriptions, all saved to the account and shown on the account page.
- AI chat in the preview: use the artifact `sample` capability (the viewer's own Claude account); declare it (plus `user` for name/email prefill) before publishing. It works only for viewers signed in to claude.ai. Shared `db` writes are not possible for outside viewers, so real multi-user accounts need the deployed server.
- Links between artifacts: `https://claude.ai/artifact/<id>#section` deep-links into the other page's hash route.
- Test the single file in a real browser at 390px and 1280px (Playwright with a stubbed `window.claude.use`), and make decorative layers (`glow`) `pointer-events: none` so they never block buttons.

### Part C: Next.js rules

#### C1. Defaults
- Next.js (latest stable), **App Router**, **TypeScript**.
- Styling: **CSS Modules + global tokens file** by default; Tailwind only if the client or existing project uses it.
- `next/font`, `next/image`, `next/link`.

#### C2. Folder structure
```
project/
├── app/              routes (layout.tsx, page.tsx, globals.css, api/)
├── actions/          Server Actions
├── components/ui/    small reusable components
├── components/sections/  page building blocks
├── lib/              db, data functions, auth, email, validation
├── scripts/          seed data, make-admin
├── public/           images, icons, favicon
└── .env.local        secrets (never committed)
```

#### C3. React rules
- **Server Components by default.** `"use client"` only where state, effects or browser events are needed; keep those small.
- One component per file, PascalCase, typed props; no component over ~150 lines.
- Fetch data on the server, not in `useEffect`.
- Every async UI has loading, error and empty states.
- Forms: Server Actions or an API route, validated on the server; errors shown per field.

#### C4. SEO and performance
- `metadata` on every page; `robots: noindex` on account, checkout and admin pages.
- `next/image` with real sizes; above-the-fold image gets `priority`.
- Static generation for pages that don't change per user. Target Lighthouse 90+.

#### C5. HTML and CSS
- Semantic tags; one `h1` per page; headings in order; meaningful `alt` text.
- `<html lang>`; `dir="rtl"` for Persian/Arabic; user-written text gets `dir="auto"`.
- Every input has a `<label>`, the right `type` and `autocomplete`.
- Tokens as CSS custom properties; mobile-first; Flexbox/Grid; `clamp()` headings; logical properties for RTL.
- Respect `prefers-reduced-motion`; visible `:focus-visible`.
- Grid/flex children with wide content (scrolling date strips, tables) need `min-width: 0` (or `minmax(0, 1fr)` columns), and `<fieldset>` needs `min-width: 0`, or the page gets wider than the screen.

### Part D: Quality checklist
- [ ] Looks right at 375, 768, 1280 and 1920px; **no horizontal scroll** at 375px (`document.documentElement.scrollWidth === innerWidth`)
- [ ] Hero visual fully visible at every width, not clipped
- [ ] Motion level delivered (an Apple-style brief is never static)
- [ ] **Every button works end to end.** No disabled "coming soon" placeholders for core actions; if a service like payments isn't connected, the flow still runs and explains that at the last step
- [ ] Keyboard-only works; focus always visible
- [ ] Contrast passes; alt text present
- [ ] No console errors; no broken links
- [ ] Reduced motion respected; RTL checked if needed
- [ ] All placeholders listed for the client
- [ ] Nothing generic left (Part A3)

## Reference: api

Static-export sites have no API: send forms to a form service or switch to full app.

### 1. Pick the right tool
| Need | Use |
|---|---|
| A page shows data | **Server Component reads directly** from `lib/` |
| A form or button changes data | **Server Action** |
| Data for something outside this site (mobile app, Stripe webhook) or a streamed response (AI chat) | **Route Handler** in `app/api/` |
| Big or shared backend, heavy jobs | **Separate Express API** (Reference: backend) |

Data logic lives in `lib/data/`; pages, actions and routes call the same functions.

### 2. REST rules
- Plural lowercase nouns (`/api/products/42`); GET 200, POST 201, PATCH 200, DELETE 204.
- One response shape: `{ "data": … }` or `{ "error": { "code", "message", "fields" } }`.
- 400 bad input · 401 not logged in · 403 not allowed · 404 · 409 conflict · 429 too many · 500 (never leak stack traces).
- Lists paginated (`?page=&limit=` or cursor) with sort and simple filters.

### 3. Validation
Validate every input on the server with **Zod**. Action results: `{ ok: true, message, redirectUrl? } | { ok: false, error, fields? }`. Public forms get a honeypot field and per-IP rate limiting.
```ts
"use server";
export async function createInquiry(input: unknown): Promise<ActionResult> {
  if (await rateLimited("inquiry", 4)) return { ok: false, error: "Too many attempts. Wait a minute." };
  const parsed = inquirySchema.safeParse(input);
  if (!parsed.success) return { ok: false, error: "Please check the highlighted fields.", fields: parsed.error.flatten().fieldErrors };
  await saveInquiry(parsed.data);
  await emails.inquiryReceived(parsed.data.email, parsed.data.name);
  return { ok: true, message: "Thanks. A specialist will reply within two working days." };
}
```

### 4. Product types for shops
Let the type decide the button and flow: **buy** (bag → checkout), **subscribe** (recurring checkout), **book** (date + time slots with capacity; free bookings confirmed instantly, paid ones held until payment), **inquire** (request form), **waitlist** (email only; never take money for products that don't exist yet). Regulated services (banking, trading, health) are waitlist or education only until the business is authorised.

### 5. Shopping bag and payments
- Bag in the browser (`localStorage` in try/catch) with name/price/icon for display only.
- Checkout sends only `{ slug, quantity }[]`; the server re-reads prices, writes the order with a price **snapshot** in a transaction, then creates a **Stripe Checkout** session (payment or subscription). Never trust browser prices.
- Card details only on Stripe's page; the database never stores cards.
- Payment state changes by **webhook** (signature verified, idempotent: only update rows still `pending`): `checkout.session.completed`, `checkout.session.expired` (release held booking slots), `customer.subscription.deleted` (end the plan); store the subscription id.
- Without a Stripe key the flow still runs to the last step and says payments aren't connected yet.

### 6. AI assistant features (Claude API)
- Call the model only from the server with the business's API key; never expose it. The business pays Anthropic per use and sells plans to its customers.
- Stream plain text; the client reads `res.body.getReader()` and shows it as it arrives; offer Stop (AbortController, abort upstream too).
- Require sign-in with a verified email for anything that costs money per message; count usage per user per day in the database; enforce plan limits (free trial tier, paid tiers sold as subscriptions in the shop).
- Validate the conversation (alternating roles, starts and ends with the user, max turns and length); send only the last N turns; cap `max_tokens` per plan; monthly spend limit in the Anthropic Console.
- System prompt: the product's persona, honest that it is powered by Claude (Anthropic), clear about its limits. Follow Anthropic's Usage Policies.
- Render answers as text (`white-space: pre-wrap`, `dir="auto"`), never raw HTML.
- Pages hosted inside Claude (artifacts) can use the viewer's own Claude account instead, but that only works for people opening the page in Claude, not for a public website.

### 7. Authentication
- **Better Auth** (email + password, Drizzle/Prisma adapters, Next.js 16) or **Auth.js**, or the client's provider. Never hand-roll hashing or sessions.
- If accounts show orders matched by email, **require email verification** first.
- Sign-up and forgot-password answer the same whether the email exists or not.
- Redirect after sign-in only to in-site paths (`/…`, never `//…` or full URLs).
- Roles live on the user row, can't be set from any form, checked on the server in every admin page and action; first admin by script.
- Session in an httpOnly, secure, sameSite cookie, never `localStorage`.

### 8. Security checklist
- [ ] Secrets only in env; only `NEXT_PUBLIC_*` reaches the browser
- [ ] Every input validated with Zod on the server
- [ ] Every write checks login and permission
- [ ] Rate limiting on login, signup, contact, AI chat and public POSTs
- [ ] Webhooks verify signatures
- [ ] Errors logged on the server; clients get safe messages

## Reference: backend

Defaults: Node.js LTS, TypeScript, ES modules, Zod.

### 1. Next.js only, or a separate Express API?
| Situation | Choice |
|---|---|
| One site/app: forms, accounts, dashboard, payments, AI chat | **Next.js only** |
| Same backend for a mobile app or several frontends | **Separate Express API** |
| Long jobs (video, big imports, scheduled jobs, WebSockets) | **Express** or a worker |
| Client already has Express | **Express**, follow its structure |

### 2. Next.js backend structure
```
lib/
├── db/            schema + one shared connection
├── env.ts         Zod-validated environment variables
├── auth.ts        getCurrentUser(), requireUser(), requireAdmin()
├── email.ts       sendEmail() + one function per transactional email
├── data/          reads/writes, one query per need (no N+1)
├── services/      business logic across data functions
└── validation.ts  Zod schemas + ActionResult
```
- `import "server-only";` in server files. Pages and actions stay thin: validate → call data/service → return.
- Optional services (Stripe, Resend, AI) are optional in the env schema; the code explains when they aren't connected.
- `revalidatePath()` after writes that change public pages.
- **Transactional email** (booking confirmed, order paid, request received, waitlist joined, verify email, reset password) through one `sendEmail()` that never throws, so a failed email can't break a payment; without a key in development it prints to the server log. Only promise an email in the UI if one is sent.
- **Admin panel** at `/admin` for every shop or booking site: stats, upcoming bookings (with cancel), orders, requests with details, waitlist counts, product price/visibility. Role-protected on the server; every admin action re-checks the role.
- Uploads go to S3-compatible storage, never the server disk.

### 3. Express API structure
```
api/src/
├── server.ts       connects DB, listens, graceful shutdown
├── app.ts          helmet, CORS allow-list, json limit, rate limit, routes, error handler
├── config/env.ts
└── routes/ controllers/ services/ models/ middlewares/ utils/
```
Flow: route → middleware (auth, validate) → controller → service → model. Controllers never touch the database; services never touch `req`/`res`. **Express 5** (async errors reach the handler). One `AppError` class and one error handler with safe messages. argon2/bcrypt passwords; JWT or session id in an httpOnly cookie; `/health`; pino logs.

### 4. Quality
- ESLint + Prettier; Vitest (+ Supertest) for auth, validation and each main path.
- Test flows in a real browser (Playwright) at phone and desktop width before delivering.
- Never commit `.env`; keep `.env.example` complete.

### 5. Performance at scale
Sites rarely fail because AI wrote them; they fail because nobody checked these:
- Pagination everywhere; indexes on every filter/sort/lookup field; no N+1 queries.
- One shared DB connection (cached on `globalThis` in Next.js).
- Cache what doesn't change per user; CDN for assets.
- Heavy work off the request.
- Rate limiting (in-memory for one server; Redis/Upstash for many).
- Load test before launch (`k6`/`autocannon`, e.g. 200 concurrent users); monitor after (Vercel Analytics, Sentry).

## Reference: database

This section also holds site-wide **cybersecurity** (Part 5), which applies to every project.

### 1. Which database?
| Project | Database | Tool |
|---|---|---|
| Content sites, blogs, portfolios, simple booking, profiles | **MongoDB** | **Mongoose** |
| Money and strict relations: shops, subscriptions, invoices, reports | **PostgreSQL** | **Drizzle** or **Prisma** |
| Client already has one | Theirs | Their tool |

Choose PostgreSQL when money moves between records, reports join many tables, or data must never be half-saved. Prisma downloads engine binaries; where that is blocked, use **Drizzle** (pure TypeScript; `drizzle-kit generate` / `migrate`). Hosting: Neon, MongoDB Atlas, Supabase. Some cloud services block certain countries; develop against a local database if needed.

### 2. Data modelling
- List entities, fields and relations as a table in the plan first.
- MongoDB: **embed** small children always read with the parent; **reference** children that grow without limit or are queried alone. Never let an array grow forever.
- **Snapshot** values that must not change later (order item name and price).
- Money as **integer pence/cents**. Times stored in UTC; convert from the business's local time (e.g. Europe/London) on the server.
- Every table: timestamps, validation, indexes on lookup fields, `unique` on email/slug; sensitive fields not selected by default.
- Bookings: capacity per slot checked inside a transaction; unpaid holds expire.
- Every schema change is a migration; never edit production by hand; transactions for multi-write operations.
- `scripts/seed.ts` with realistic data, safe to re-run (upsert by slug).

### 3. Database security
- [ ] Separate dev and production databases
- [ ] Least-privilege user; limited network access; TLS
- [ ] No injection: Zod first; Mongo `sanitizeFilter`; SQL only via ORM or parameters
- [ ] Passwords and tokens hashed
- [ ] Every query for a user's data filters by that user (or verified email)
- [ ] Daily backups; restore tested once
- [ ] Personal data minimal and deletable (UK/EU GDPR)

### 4. Changing data structures later
Add fields as optional, backfill, then make required. Never rename or delete in one step on a live site: add → migrate → switch code → remove. Back up before every production migration.

### 5. Site cybersecurity (every project)
- **XSS:** never `dangerouslySetInnerHTML` with user content (DOMPurify if rich text is required). Trusted static SVG strings are fine.
- **CSRF:** cookies `sameSite=lax`, `httpOnly`, `secure`; Server Actions check origin.
- **Headers:** HSTS, X-Content-Type-Options, Referrer-Policy, frame-ancestors/X-Frame-Options, Permissions-Policy, CSP where possible; `poweredByHeader: false`.
- **Accounts:** rate-limited login/signup/reset; no email enumeration; 2FA for admins; bot protection on public forms.
- **Access control:** permissions checked on the server for every read and write.
- **Uploads:** allow-listed types, size limits, renamed, object storage.
- **Payments:** Stripe Checkout; verified webhooks.
- **Supply chain:** `npm audit`; Dependabot; rotate any committed secret.
- **Monitoring:** server errors logged; alerts on 401/403/429/500 spikes.

## Reference: changes

Goal: the requested change works and **nothing else breaks**.
1. **Understand first:** README, package.json, structure; run it locally; follow the project's existing conventions.
2. **Classify:** content (data only) · design tweak (tokens first) · new section/page (reuse components) · new feature (mini-plan first) · bug (reproduce, fix the cause, add a test) · upgrade (one major at a time).
3. **Git always:** branch per change (`feat/…`, `fix/…`), small clear commits, a pull request, `main` always deployable.
4. **Keep it small:** no unrelated refactors; database changes follow add → migrate → switch → remove with a backup.
5. **Check:** the change at 375/768/1280px; neighbouring pages unchanged; forms, login and checkout still work; build, lint and tests pass.
6. **Deploy safely:** preview URL for approval → merge → check live → know the one-click rollback.
7. **Report:** short non-technical note and a line in `CHANGELOG.md`.

## Reference: deploy

### Default stack
**Vercel** + **Neon** (PostgreSQL) in the region nearest the customers (London: `lhr1` / Europe West 2), **Stripe**, **Resend**, **Anthropic Claude API**. Static sites: any static host. Accounts are opened in the client's or business's name; live payments and the Claude API need a registered business, and some services are unavailable to residents of some countries.

### Steps
1. Private GitHub repository.
2. Neon: pooled connection string with `sslmode=require`; run migrations and seed once.
3. Vercel: a `vercel-build` script runs migrations then `next build`.
4. Env variables per environment; new production secrets; `.env.example` complete.
5. Email domain verified (SPF/DKIM).
6. Stripe: test mode first, webhook with only handled events, live keys once the business is verified.
7. AI: API key with a monthly spend limit; per-user daily limits in code.
8. First admin via script.
9. Custom domain + HTTPS.

### Launch checklist
- [ ] Every flow tested live with test cards and real email: buy, subscribe, book (free and paid), inquiry, waitlist, sign up, confirm email, reset password, AI chat limits, admin
- [ ] Webhook deliveries return 200
- [ ] Security headers checked; `npm audit` clean of high/critical
- [ ] Privacy policy, terms, cookie notice (UK/EU)
- [ ] Backups on; restore tested
- [ ] Monitoring and spend alerts (Stripe, Anthropic, Neon)
- [ ] Sample prices/products replaced
- [ ] Handover: README, DEPLOY.md, admin login, who owns each account

Every full-app project ships a `DEPLOY.md` for the client: accounts needed (and which need a registered company), each env variable, webhook URLs, first-admin command and the launch checklist.

## Reference: motion

Every animation has a job: guide the eye, show what changed, react to the visitor, or create a memorable moment.

### 1. Rules
- **Pick the motion level in the plan** and tell the client:
  - **Calm**: law firms, clinics, docs, dashboards. One entrance moment; motion mostly answers clicks.
  - **Lively** (default for brands and shops): hero text entrance, a living hero visual (rises in, floats, reacts to scroll, soft animated glow), staggered grid reveals, hover life on cards, animated feedback on every action.
  - **Showcase**: Apple-style. Lively plus pinned scroll stories, scroll-linked 3D tilt/scale, text reveals, section transitions.
  Apple-style briefs are Lively at minimum; a static page reads as "not alive".
- Each moving element has its own reason; no single fade-up pasted everywhere.
- Animate only `transform` and `opacity` (and `clip-path`/`filter` carefully).
- Respect reduced motion (`useReducedMotion()`).
- Test on a real phone; reduce heavy effects on small screens.
- Speed: UI feedback 150–250ms; entrances 400–800ms; story moments up to ~1.2s.
- Tokens: `--ease-out: cubic-bezier(0.22, 1, 0.36, 1)`, `--dur-fast: 180ms`, `--dur-base: 300ms`, `--dur-slow: 600ms`.

### 2. Which tool (simplest that works)
1. CSS transitions/keyframes.
2. CSS scroll-driven animations / View Transitions.
3. **Motion for React** (`motion`, import from `motion/react`, `"use client"`): default for React.
4. **GSAP** + `@gsap/react`: pinned scroll stories, SplitText, complex timelines (all plugins free).
5. Lenis (story sites only); Lottie/Rive; React Three Fiber (lazy-loaded 3D).
Never animate one element with both Motion and GSAP.

### 3. Recipes
**Living hero object** (keep it fully inside the hero at every width; no negative margin cut by `overflow: hidden`):
```tsx
const { scrollYProgress } = useScroll({ target: ref, offset: ["start end", "end start"] });
const rotateX = useTransform(scrollYProgress, [0, 0.5, 1], [18, 0, -18]);
const scale = useTransform(scrollYProgress, [0, 0.5, 1], [0.86, 1, 0.92]);
// <div style={{ perspective: 900 }}><motion.div initial={{ opacity: 0, y: 60, scale: 0.8 }} animate={{ opacity: 1, y: 0, scale: 1 }}>
//   <motion.div style={{ rotateX, scale }}><motion.div animate={{ y: [0, -12, 0] }} transition={{ duration: 6, repeat: Infinity }}>…
```
Behind it, a radial-gradient glow with a slow CSS `breathe` keyframe.

**Hero text entrance:** parent staggers children 0.09s; children from `{ opacity: 0, y: 28, filter: "blur(6px)" }` over 0.8s.

**Grid reveal:** `whileInView="show"`, `viewport={{ once: true, amount: 0.15 }}`, stagger 0.06s.

**Card hover:** lift 4px; icon rotates −8° and scales 1.08.

**Filters/tabs:** `AnimatePresence mode="popLayout"` + `layout`; sliding underline with one `layoutId`.

**Pinned scroll story (GSAP):** `gsap.matchMedia()` with `(min-width: 768px) and (prefers-reduced-motion: no-preference)`, timeline with `scrollTrigger: { pin: true, scrub: true, end: "+=200%" }`; mobile stacks normally.

**Micro-interactions:** press scale 0.97; bag count springs in; success messages fade/slide in; height animations with Motion, not `max-height` hacks.

Add reusable motions here as recipes.

## Reference: inspiration

Find **principles**, never copy a design.

| Need | Source |
|---|---|
| Award-level, experimental | Awwwards |
| Curated modern sites | Recent (formerly Godly) |
| Calm, minimal | Siteinspire, Httpster, Minimal Gallery |
| Landing pages by section | Land-book, Lapa Ninja |
| App screens and flows | Mobbin, Refero |
| Fonts in use | Typewolf, Fonts In Use |
| Palettes | Coolors, Realtime Colors |
| Motion with code | Codrops, motion.dev, gsap.com showcase |
| Moods | Dribbble, Behance, Pinterest |

Pick 3–5 references (one from the client's industry, one from a different one), write one principle to borrow from each, agree them with the client. Never reuse another site's images, logos, copy or code.

Saved references:
- **apple.com** (consumer tech): product as hero, one idea per screen, pinned scroll stories, huge type with little copy. Used in: MORA website and MORA Shop.
- **stripe.com** (fintech): complex products explained with diagrams and live UI previews; strong type hierarchy.
- **linear.app** (SaaS): product screenshots as visual, precise spacing, motion only on interaction.
