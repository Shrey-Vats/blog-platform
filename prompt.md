
“Generate the complete source code for a production-grade, premium blog platform called [xS] built on the latest MERN stack (MongoDB 7, Express 5, React 18 with Vite, Node 24 LTS) and state-of-the-art tooling. The deliverable must be a single mono-repo (Turborepo or Nx) with two clearly separated apps:

1. client-admin (React 18 + TypeScript + Vite + SWC)  
2. server-api (Node 24 + Express 5 + TypeScript + ts-node-dev)

No Next.js. No SSR. Public site is served by the same Vite React bundle with pre-rendering if needed (e.g. react-snapshot or prerender-spa-plugin). Everything runs on Node 24 LTS.

--------------------------------------------------------------------------------------------------------------------
GLOBAL REQUIREMENTS  
• 100 % TypeScript (strict).  
• ESLint 9 (flat config) + Prettier + Husky + lint-staged + commitlint.  
• 100 % test coverage on critical paths (Vitest + React Testing Library + Supertest).  
• Docker & docker-compose for dev, CI, and prod.  
• GitHub Actions: lint → test → build → Docker push → deploy (Railway / Render).  
• Environment variables validated with zod.  
• Security: Helmet, rate-limit, mongo-sanitize, express-validator, JWT (access + refresh) in HttpOnly cookies, CSRF double-submit, CSP nonce-based.  
• Logging: pino-http + pino-pretty in dev, Loki in prod.  
• Internationalization (react-i18next).  
• Dark / light mode with next-themes.  
• Accessibility WCAG 2.2 AA, keyboard navigation.

--------------------------------------------------------------------------------------------------------------------
DATABASE  
MongoDB 7 with native driver + Mongoose 8.  
Schema design:  
- User (role: admin | editor | reader)  
- BlogPost (slug, title, metaTitle, metaDescription, metaKeywords, canonicalUrl, ogImage, twitterImage, structuredData, content JSON AST, tags[], status: draft | published | archived, scheduledAt, publishedAt, updatedAt, authorId, seoScore)  
- Page (custom landing pages, same fields as BlogPost)  
- Media (GridFS)  
- Settings (global SEO, social links, GA, GTM, 404 text, etc.)

Indexes: text index on title + content, unique on slug, TTL on tokens.

--------------------------------------------------------------------------------------------------------------------
SERVER (Express 5)  
RESTful + tRPC router for type-safe admin calls.  
Routes:  
GET    /api/v1/posts?limit&page&tag&search  
GET    /api/v1/posts/:slug  
POST   /api/v1/posts (admin)  
PATCH  /api/v1/posts/:id (admin)  
DELETE /api/v1/posts/:id (admin)  
Same for /pages, /media, /settings.  
Admin-only middleware checks role from JWT.  
Image upload: Sharp for WebP/AVIF, responsive sizes (320,640,768,1024,1440), lazy blur placeholder.  
Caching: Redis in-memory cache, ETag, 304 Not Modified.  
Sitemap.xml & robots.txt generated nightly via cron.  
RSS & JSON-Feed at /feed.xml, /feed.json.

--------------------------------------------------------------------------------------------------------------------
CLIENT ADMIN (React 18 + Vite)  
SPA living at https://admin.xS.com  
Protected by React Router 6 + role-based routes.  
Layout:  
- Sidebar (collapsible, keyboard accessible)  
- Top bar (search, notifications, profile)  
Pages:  
1. Dashboard (analytics widgets using Recharts)  
2. Posts → List (table with TanStack Table v8, bulk actions)  
3. Posts → Create / Edit (see dedicated section below)  
4. Pages (identical to posts but routed as pages)  
5. Media Library (Masonry grid, drag-drop, bulk delete)  
6. Settings (global SEO, theme colors, social cards preview)  
7. Profile  
All forms react-hook-form + zod validation.  
Real-time collaboration cursor via Yjs + WebRTC.  
Auto-save every 5 s with debounced optimistic updates.

--------------------------------------------------------------------------------------------------------------------
SEO SUPER ADMIN FEATURES  
A dedicated “SEO Studio” page at /admin/seo  
- Global meta tags editor (title template, description template, default OG image, twitter card type, favicon variants, manifest.json, theme-color).  
- Per-post / per-page overrides: meta title, meta description, meta keywords, canonical URL, robots (index, follow, noarchive, nosnippet etc.), hreflang alternates, JSON-LD schema (FAQ, HowTo, Product, etc.).  
- Live preview of SERP snippet (mobile & desktop).  
- Bulk import/export via CSV.  
- Validation: missing meta, duplicate titles, oversized OG images.  
- Automatic generation of structured data (Article, BreadcrumbList, WebSite, Person).  
- Real-time SEO score (0-100) with actionable tips.  
- Integration with Google Search Console & Bing Webmaster Tools APIs (fetch queries, submit URLs).  
- 301/302 redirect manager (source → target, regex support).  
- Custom <head> injection (extra meta, links, scripts).  
- Custom 404/500 page editor with HTML.

--------------------------------------------------------------------------------------------------------------------
WYSIWYG + RAW-HTML EDITOR (POST / PAGE CREATION)  
Route: /admin/posts/new and /admin/posts/:id/edit  
Editor stack:  
- TipTap 2 headless editor (React)  
- Extensions: Bold, Italic, Underline, Strikethrough, Code, CodeBlock, FontSize, FontFamily, TextColor, BackgroundColor, Highlight, Subscript, Superscript, Link, Image (with alt, caption, lazy, lightbox), Video (embed), Table, HorizontalRule, Blockquote, OrderedList, BulletList, TaskList, Indent, Align, LineHeight, Emoji, Mention, Math (KaTeX).  
- Toggle between WYSIWYG, Markdown, and RAW-HTML modes (Monaco Editor with HTML language server).  
- Real-time preview pane (iframe sandbox) that renders the same HTML/CSS/JS as the public site.  
- “Insert HTML block” node for arbitrary HTML snippets (sanitized by DOMPurify server-side).  
- Drag-and-drop image/video upload with automatic compression & lazy-loading markup.  
- Table of contents auto-generated from headings.  
- Reading time & word count.  
- Revision history (diff viewer with Monaco).  
- Keyboard shortcuts & slash commands.  
- Accessibility: aria-labels, keyboard navigation, high-contrast mode toggle.  
- Export: Copy HTML, Download .html, Download .md, Print-friendly CSS.

--------------------------------------------------------------------------------------------------------------------
PUBLIC FRONT-END (SAME VITE REACT BUNDLE)  
Entry at http://localhost:3000
Routes:  
- / (home with paginated posts)  
- /blog/:slug (single post)  
- /tag/:tag  
- /page/:slug (custom pages)  
- /about, /contact, etc.  
Pre-rendering: react-snapshot (or vite-plugin-ssr) to generate static HTML at build time for SEO.  
Meta tags injected via react-helmet-async from admin SEO data.  
Code syntax highlighting via rehype-pretty-code (Shiki).  
Reading progress bar (framer-motion).  
Table of contents with active scroll spy.  
Comments: Giscus lazy-loaded.  
Newsletter form (Buttondown API).  
PWA: vite-plugin-pwa, offline fallback page.

--------------------------------------------------------------------------------------------------------------------
UI & ANIMATION SYSTEM  
Design tokens in packages/ui: color, spacing, typography, shadows.  
Component library: Radix UI primitives + Tailwind CSS + clsx + tailwind-merge.  
Dark mode CSS variables auto-generated from tokens.  
Animation library: framer-motion 11 (latest) with spring presets.  
Motion patterns:  
- Staggered list (useAnimate + stagger)  
- Page transitions (AnimatePresence mode="wait")  
- Micro-interactions (hover, tap, focus-visible)  
- Parallax hero images (useScroll, useTransform)  
- Admin sidebar slide-in (layoutGroup)  
- Skeleton loaders (motion.div variants)  
- Confetti on successful publish (react-confetti)  
All animations respect prefers-reduced-motion.

--------------------------------------------------------------------------------------------------------------------
SEO & PERFORMANCE CHECKLIST (BUILT-IN)  
✓ Semantic HTML5 (article, nav, main, section)  
✓ 100 Lighthouse score on all four metrics (desktop & mobile)  
✓ Critical CSS inlined, fonts preloaded  
✓ Images: lazy, WebP/AVIF, responsive srcset  
✓ Hreflang alternate links for i18n  
✓ 404 & 500 error pages with custom illustrations  
✓ Security headers (CSP, HSTS, X-Frame-Options)  
✓ Storybook 8 for component docs (packages/ui/.storybook)  
✓ Chromatic visual regression tests on PR.

--------------------------------------------------------------------------------------------------------------------
PROJECT STRUCTURE (Turborepo example)
repo-root  
├─ apps  
│  ├─ admin (React 18 + Vite)  
│  └─ api (Express 5)  
├─ packages  
│  ├─ ui (Radix + Tailwind + Storybook)  
│  ├─ shared-types (zod schemas)  
│  ├─ eslint-config-custom  
│  └─ tsconfig  
├─ docker-compose.yml  
├─ turbo.json  
└─ .github/workflows/ci.yml

--------------------------------------------------------------------------------------------------------------------
PACKAGE MANAGER CLARIFICATION  
npm is the default Node package manager. The project uses npm workspaces (not pnpm). All commands below use npm.

--------------------------------------------------------------------------------------------------------------------
DEV COMMANDS  
npm install     → installs root + all workspaces  
npm run dev     → concurrently starts api, admin, web with hot reload  
npm run build   → builds all apps  
npm run test    → runs tests in parallel  
npm run lint    → lints everything  
npm run storybook → starts Storybook on 6006

--------------------------------------------------------------------------------------------------------------------
DELIVERABLES  
1. Complete mono-repo source code.  
2. README with: quick start, env template, deployment guide, tech decisions.  
3. OpenAPI 3.1 spec (swagger-ui at /docs).  
4. Postman collection.  
5. Figma file link (UI kit already designed).  
6. Architecture diagram (C4 model).  
7. 5-minute loom video demo.

--------------------------------------------------------------------------------------------------------------------
END OF PROMPT