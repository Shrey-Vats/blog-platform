blog-platform/                         # Git repository root
├── .github/
│   └── workflows/
│       └── ci.yml                     # Lint → test → build → docker → deploy
├── apps/
│   ├── api/                           # Express 5 + Node 24
│   │   ├── src/
│   │   │   ├── app.ts                 # Express instance (helmet, cors, session, etc.)
│   │   │   ├── server.ts              # Entry point (cluster, graceful shutdown)
│   │   │   ├── config/
│   │   │   │   ├── database.ts
│   │   │   │   ├── env.ts             # zod schema
│   │   │   │   ├── redis.ts
│   │   │   │   └── logger.ts
│   │   │   ├── controllers/
│   │   │   │   ├── auth.controller.ts
│   │   │   │   ├── post.controller.ts
│   │   │   │   ├── page.controller.ts
│   │   │   │   ├── media.controller.ts
│   │   │   │   ├── setting.controller.ts
│   │   │   │   └── seo.controller.ts
│   │   │   ├── middlewares/
│   │   │   │   ├── auth.middleware.ts
│   │   │   │   ├── role.middleware.ts
│   │   │   │   ├── validate.ts
│   │   │   │   ├── errorHandler.ts
│   │   │   │   ├── rateLimiter.ts
│   │   │   │   └── csrf.ts
│   │   │   ├── models/
│   │   │   │   ├── user.model.ts
│   │   │   │   ├── post.model.ts
│   │   │   │   ├── page.model.ts
│   │   │   │   ├── media.model.ts
│   │   │   │   └── setting.model.ts
│   │   │   ├── routes/
│   │   │   │   ├── v1/
│   │   │   │   │   ├── index.ts
│   │   │   │   │   ├── auth.routes.ts
│   │   │   │   │   ├── post.routes.ts
│   │   │   │   │   ├── page.routes.ts
│   │   │   │   │   ├── media.routes.ts
│   │   │   │   │   └── seo.routes.ts
│   │   │   │   └── index.ts
│   │   │   ├── services/
│   │   │   │   ├── auth.service.ts
│   │   │   │   ├── post.service.ts
│   │   │   │   ├── seo.service.ts
│   │   │   │   └── sitemap.service.ts
│   │   │   ├── utils/
│   │   │   │   ├── generateSlug.ts
│   │   │   │   ├── sanitizeHtml.ts
│   │   │   │   └── seoScorer.ts
│   │   │   └── tests/                 # Unit & integration
│   │   │       ├── post.test.ts
│   │   │       └── auth.test.ts
│   │   ├── dist/                      # Compiled JS output
│   │   ├── Dockerfile
│   │   ├── nodemon.json
│   │   └── package.json               # "type": "module"
│   │
│   └── admin/                         # React 18 + Vite SPA (admin panel)
│       ├── index.html
│       ├── vite.config.ts
│       ├── tsconfig.json
│       ├── src/
│       │   ├── main.tsx
│       │   ├── App.tsx
│       │   ├── routes/
│       │   │   ├── index.tsx
│       │   │   ├── ProtectedRoute.tsx
│       │   │   └── lazyComponents.ts
│       │   ├── pages/
│       │   │   ├── Dashboard/
│       │   │   │   ├── Dashboard.tsx
│       │   │   │   └── components/
│       │   │   ├── Posts/
│       │   │   │   ├── PostsList/
│       │   │   │   │   ├── PostsList.tsx
│       │   │   │   │   └── PostTable.tsx
│       │   │   │   ├── PostEditor/
│       │   │   │   │   ├── PostEditor.tsx
│       │   │   │   │   ├── EditorToolbar.tsx
│       │   │   │   │   ├── HtmlEditorPane.tsx          # Monaco
│       │   │   │   │   └── PreviewPane.tsx             # iframe
│       │   │   │   └── hooks/
│       │   │   │       └── useAutoSave.ts
│       │   │   ├── Pages/
│       │   │   ├── MediaLibrary/
│       │   │   ├── Settings/
│       │   │   ├── SEO/
│       │   │   │   ├── GlobalSEO.tsx
│       │   │   │   ├── RedirectManager.tsx
│       │   │   │   └── StructuredDataBuilder.tsx
│       │   │   └── Profile/
│       │   ├── components/
│       │   │   ├── ui/                 # wrappers around packages/ui
│       │   │   ├── layout/
│       │   │   │   ├── Sidebar.tsx
│       │   │   │   ├── Header.tsx
│       │   │   │   └── Layout.tsx
│       │   │   └── providers/
│       │   │       ├── ThemeProvider.tsx
│       │   │       └── AuthProvider.tsx
│       │   ├── store/                  # Zustand or Redux Toolkit
│       │   ├── lib/
│       │   │   ├── api.ts              # axios + tRPC
│       │   │   └── motionVariants.ts
│       │   ├── styles/
│       │   │   └── globals.css
│       │   └── tests/
│       │       └── PostEditor.test.tsx
│       ├── .env.example
│       └── package.json
│
├── packages/
│   ├── ui/                             # Shared design-system
│   │   ├── src/
│   │   │   ├── components/
│   │   │   │   ├── Button/
│   │   │   │   ├── Input/
│   │   │   │   ├── Dialog/
│   │   │   │   └── index.ts
│   │   │   ├── hooks/
│   │   │   │   └── useMediaQuery.ts
│   │   │   ├── styles/
│   │   │   │   └── tailwind.config.ts
│   │   │   └── index.ts
│   │   ├── .storybook/
│   │   ├── tsconfig.json
│   │   └── package.json
│   │
│   ├── shared-types/                   # Zod schemas & TS types
│   │   ├── src/
│   │   │   ├── auth.ts
│   │   │   ├── post.ts
│   │   │   ├── seo.ts
│   │   │   └── index.ts
│   │   ├── tsconfig.json
│   │   └── package.json
│   │
│   ├── eslint-config-custom/
│   │   ├── index.js
│   │   └── package.json
│   │
│   └── tsconfig/
│       ├── base.json
│       ├── vite.json
│       └── node.json
│
├── docker-compose.yml                  # api + mongo + redis
├── Dockerfile                          # Multistage build (api & admin)
├── turbo.json                          # Turborepo pipelines
├── .gitignore
├── .env.example                        # Root env template
├── .husky/                             # Git hooks
├── .prettierrc
├── package.json                        # Workspaces root
├── README.md
└── LICENSE