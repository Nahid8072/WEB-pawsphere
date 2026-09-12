# PawSphere — Project Structure Case Study

## 1. Executive Summary

**PawSphere** is a front-end-only prototype for a pet adoption and veterinary care platform aimed at Bangladesh. It simulates a multi-role SaaS product (Pet Owner, Shelter, Veterinarian, Administrator) using **React 18**, **TypeScript**, **Vite 6**, and **Tailwind CSS v4**.

The project originated from a **Figma Make** export (evident from `@figma/my-make-file` in `package.json`, shadcn/ui scaffolding, and Figma-specific Vite plugins). The live application logic is intentionally consolidated into a single large file — `src/app/App.tsx` (~5,900 lines) — while a full **shadcn/ui component library** sits alongside it, ready for future refactoring but not yet wired into the main app.

There is **no backend**, **no database**, and **no real authentication**. All data is mock; navigation is handled by React state, not React Router.

---

## 2. High-Level Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                        index.html                           │
│              (HTML shell + #root mount point)               │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                      src/main.tsx                           │
│         createRoot → renders <App /> + imports CSS          │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                    src/app/App.tsx                          │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────┐ │
│  │ Mock Data   │  │ Screen       │  │ State-driven Router │ │
│  │ (PETS, VETS)│  │ Components   │  │ role + view strings │ │
│  └─────────────┘  └──────────────┘  └─────────────────────┘ │
└──────────────────────────┬──────────────────────────────────┘
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
   Tailwind CSS     Lucide Icons      Recharts
   (theme tokens)   (inline UI)       (dashboards)
```

**How navigation works:** Instead of URL-based routing, the root `App` component maintains two pieces of state:

| State | Purpose |
|---|---|
| `role` | `"guest" \| "owner" \| "shelter" \| "vet" \| "admin"` |
| `view` | String identifier for the current screen (e.g. `"dashboard"`, `"browse-pets"`) |

The `navigate(view, extra?)` function updates these states. When `role === "guest"`, public screens render full-page without the dashboard sidebar. When authenticated, `DashboardLayout` wraps all screens with a collapsible sidebar driven by the `NAV` config object.

---

## 3. Complete Directory Tree

```text
WEB-pawsphere/
├── dist/                          # Production build output (generated)
├── guidelines/                    # AI/design guidelines template
├── node_modules/                  # Installed dependencies (generated)
├── src/
│   ├── app/
│   │   ├── App.tsx                # ★ Entire application lives here
│   │   └── components/
│   │       ├── figma/             # Figma Make utility components
│   │       └── ui/                # shadcn/ui component library (scaffold)
│   ├── imports/
│   │   └── pasted_text/           # Original project brief / prompt
│   ├── main.tsx                   # React entry point
│   └── styles/                    # Global CSS pipeline
├── ATTRIBUTIONS.md
├── default_shadcn_theme.css       # Reference shadcn theme (unused in app)
├── index.html                     # Vite HTML entry
├── package.json                   # Dependencies & scripts
├── pnpm-lock.yaml                 # Locked dependency tree
├── pnpm-workspace.yaml            # pnpm monorepo/build config
├── postcss.config.mjs             # PostCSS (empty — Tailwind handles it)
├── README.md                      # Project documentation
├── tsconfig.json                  # TypeScript configuration
└── vite.config.ts                 # Vite bundler configuration
```

---

## 4. Root-Level Files (Detailed)

### `index.html`
**Purpose:** The single HTML document Vite serves as the application shell.

**Why it exists:** Vite requires an HTML entry point that references the JavaScript module (`/src/main.tsx`). It defines the `#root` div where React mounts.

**How it works:**
- Sets viewport, charset, and SEO meta tags
- Inline CSS ensures `html`, `body`, and `#root` fill 100% height (prevents layout collapse)
- Loads `main.tsx` as an ES module
- References a favicon from the `dist/assets/` folder (note: this path is unusual for dev — typically favicons live in `public/`)

---

### `package.json`
**Purpose:** Defines project metadata, npm scripts, and all third-party dependencies.

**Why it exists:** Standard Node.js project manifest. The name `@figma/my-make-file` confirms this was exported from **Figma Make**.

**Key scripts:**

| Script | Command | Purpose |
|---|---|---|
| `dev` | `vite` | Starts local dev server on port 5173 |
| `build` | `vite build` | Produces optimized static files in `dist/` |

**Notable dependency groups:**
- **Core:** `react`, `react-dom`, `typescript`
- **UI primitives:** All `@radix-ui/react-*` packages (headless accessible components)
- **Styling:** `tailwindcss`, `tailwind-merge`, `clsx`, `class-variance-authority`
- **Charts:** `recharts` (used directly in `App.tsx`)
- **Icons:** `lucide-react` (used directly in `App.tsx`)
- **Forms (scaffolded, unused in App):** `react-hook-form`
- **Animation:** `motion` (Framer Motion successor)
- **Carousels:** `embla-carousel-react`, `react-slick`
- **Notifications (scaffolded):** `sonner`
- **MUI (scaffolded, unused):** `@mui/material`, `@mui/icons-material`

---

### `vite.config.ts`
**Purpose:** Configures the Vite build tool and dev server.

**Why it exists:** Vite needs plugins for React JSX transformation, Tailwind CSS v4 integration, and path aliases.

**How it works:**

```typescript
import { defineConfig } from 'vite'
import path from 'path'
import tailwindcss from '@tailwindcss/vite'
import react from '@vitejs/plugin-react'

function figmaAssetResolver() {
  return {
    name: 'figma-asset-resolver',
    resolveId(id) {
      if (id.startsWith('figma:asset/')) {
        const filename = id.replace('figma:asset/', '')
        return path.resolve(__dirname, 'src/assets', filename)
      }
    },
  }
}

export default defineConfig({
  plugins: [
    figmaAssetResolver(),
    react(),
    tailwindcss(),
  ],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  assetsInclude: ['**/*.svg', '**/*.csv'],
})
```

- **`figmaAssetResolver()`** — Custom plugin left over from Figma Make. Resolves `figma:asset/...` import paths to local files under `src/assets/` (that folder doesn't currently exist in the project).
- **`@` alias** — Allows imports like `@/app/components/ui/button` (configured but not used in `App.tsx` yet).
- **`assetsInclude`** — Treats `.svg` and `.csv` as static assets.

---

### `tsconfig.json`
**Purpose:** TypeScript compiler configuration.

**Why it exists:** Enables type-checking for `.tsx` files, JSX transform (`react-jsx`), strict mode, and the `@/*` path alias matching Vite's config.

**How it works:** `"noEmit": true` means TypeScript only type-checks — Vite handles actual compilation. `"include": ["src"]` scopes checking to the source folder.

---

### `pnpm-workspace.yaml`
**Purpose:** pnpm workspace and build-policy configuration.

**Why it exists:** Originally exported from Figma Make with Linux-only architecture support. Was updated to support Windows (`win32`), macOS (`darwin`), and MSVC libc so native binaries (`esbuild`, `@tailwindcss/oxide`, `rollup`) install correctly on all platforms.

**Key settings:**
- `allowBuilds` — Whitelists packages allowed to run postinstall scripts
- `supportedArchitectures` — Controls which platform-specific optional dependencies get installed

---

### `pnpm-lock.yaml`
**Purpose:** Locks exact dependency versions for reproducible installs.

**Why it exists:** Ensures every developer gets identical `node_modules` when running `pnpm install`.

---

### `postcss.config.mjs`
**Purpose:** PostCSS plugin configuration.

**Why it exists:** Placeholder file. With Tailwind CSS v4 via `@tailwindcss/vite`, PostCSS is handled automatically. The file is empty (`export default {}`) and exists only for future custom PostCSS plugins.

---

### `default_shadcn_theme.css`
**Purpose:** Reference copy of the default shadcn/ui CSS variable theme.

**Why it exists:** Figma Make ships this as a baseline theme. The project uses a **customized version** in `src/styles/theme.css` (PawSphere green `#16A34A` instead of shadcn's default dark primary). This file is **not imported** anywhere — it's a reference/backup.

---

### `README.md`
**Purpose:** Comprehensive project documentation covering features, tech stack, installation, architecture diagrams, design system colors, limitations, and future roadmap.

**Why it exists:** Serves as the primary onboarding document for developers and evaluators (e.g., course instructors).

---

### `ATTRIBUTIONS.md`
**Purpose:** Legal attribution for third-party assets.

**Why it exists:** Credits **shadcn/ui** (MIT license) and **Unsplash** photos used in mock data image URLs.

---

### `.gitignore`
**Purpose:** Tells Git to ignore `node_modules/`.

**Note:** Minimal — does not ignore `dist/`, `.env`, or IDE files. Only `node_modules` is excluded.

---

### `dist/` (generated)
**Purpose:** Output folder from `pnpm build`.

**Contents:**
- `index.html` — Processed HTML with hashed asset references
- `assets/index-*.js` — Bundled JavaScript (~743 KB)
- `assets/index-*.css` — Bundled CSS (~110 KB)

**Why it exists:** Production-ready static files that can be deployed to any static host (Netlify, Vercel, GitHub Pages).

---

## 5. `guidelines/` Folder

### `guidelines/Guidelines.md`
**Purpose:** Template for AI assistant design rules (Cursor/Figma Make context).

**Why it exists:** Figma Make generates this so AI tools know the project's design constraints. Currently contains placeholder comments with example rules (button variants, layout guidelines, font sizes).

**Status:** Empty template — no project-specific rules have been filled in yet.

---

## 6. `src/` Folder — Application Source

### `src/main.tsx`
**Purpose:** The JavaScript entry point that bootstraps React.

**Why it exists:** Vite's `index.html` references this file. It is the first code that runs in the browser.

**How it works:**

```typescript
import { createRoot } from "react-dom/client";
import App from "./app/App.tsx";
import "./styles/index.css";

createRoot(document.getElementById("root")!).render(<App />);
```

1. Finds the `#root` DOM element
2. Creates a React 18 root
3. Renders the `App` component
4. Imports global styles (which cascade into Tailwind + theme)

---

### `src/styles/` — CSS Pipeline

This folder implements a layered CSS architecture:

#### `index.css` — CSS Entry Point
**Purpose:** Aggregates all style imports in the correct order.

```css
@import './fonts.css';
@import './tailwind.css';
@import './theme.css';
```

Import order matters: fonts first, then Tailwind framework, then design tokens.

#### `fonts.css` — Typography
**Purpose:** Loads the **Inter** Google Font and sets it as the body font family.

**Note:** README mentions **Manrope** as the design font, but the actual implementation uses **Inter**. This is a minor inconsistency between docs and code.

#### `tailwind.css` — Tailwind v4 Bootstrap
**Purpose:** Initializes Tailwind CSS v4 with content scanning.

```css
@import 'tailwindcss' source(none);
@source '../**/*.{js,ts,jsx,tsx}';

@import 'tw-animate-css';
```

- `source(none)` — Disables default content detection
- `@source '../**/*.{js,ts,jsx,tsx}'` — Scans all source files for Tailwind class usage
- Imports `tw-animate-css` for animation utility classes

#### `theme.css` — Design System Tokens
**Purpose:** Defines PawSphere's complete visual identity as CSS custom properties.

**Why it exists:** Centralizes all colors, radii, chart colors, sidebar tokens, and typography defaults. Supports both light and dark mode via `.dark` class.

**Key tokens:**

| Token | Value | Usage |
|---|---|---|
| `--primary` | `#16A34A` | Brand green — buttons, accents, active nav |
| `--background` | `#F8FAFC` | Page background |
| `--foreground` | `#111827` | Primary text |
| `--border` | `#E5E7EB` | Card and input borders |
| `--radius` | `1.125rem` | Border radius for cards, buttons |

The `@theme inline` block maps CSS variables to Tailwind utility classes (e.g., `bg-primary`, `text-muted-foreground`), and `@layer base` sets default typography for HTML elements.

**Important:** While these tokens exist, `App.tsx` mostly uses **hardcoded hex values** in Tailwind classes (e.g., `bg-[#16A34A]`) rather than semantic tokens (e.g., `bg-primary`). The theme is set up for the shadcn/ui library but not fully adopted by the main app.

---

### `src/imports/` — Reference Material

#### `src/imports/pasted_text/pawsphere-app.tsx`
**Purpose:** The original AI prompt / product specification used to generate the application.

**Why it exists:** Figma Make stores the generation prompt for traceability. This 460+ line document describes the intended tech stack (originally Next.js 15), design language, all screens, role permissions, and UX requirements.

**Status:** Not imported or executed — purely archival reference. The actual implementation diverged (uses Vite instead of Next.js, consolidated into one file).

---

## 7. `src/app/App.tsx` — The Entire Application

This is the heart of the project. At ~5,943 lines, it contains **everything**: data, types, shared UI, all screens, and routing logic.

### Internal Structure (Section by Section)

| Section | Lines (approx.) | Purpose |
|---|---|---|
| **Types** | 85–87 | `Role` union type |
| **Mock Data** | 88–739 | All fake data arrays |
| **Navigation Config** | 740–962 | Sidebar menus per role |
| **Shared UI** | 964–1201 | Reusable inline components |
| **Dashboard Layout** | 1202–1321 | Sidebar + header shell |
| **Auth Screens** | 1323–1535 | Login, Register, Forgot Password |
| **Public Screens** | 1536–1798 | Browse Pets, Pet Detail |
| **Shared Screens** | 1799–2147 | Messages, Settings |
| **Owner Screens** | 2148–2848 | 8 owner-specific views |
| **Shelter Screens** | 2849–3716 | 7 shelter-specific views |
| **Vet Screens** | 3717–4254 | 5 vet-specific views |
| **Admin Screens** | 4255–5142 | 9 admin-specific views |
| **Landing Page** | 5143–5735 | Marketing homepage |
| **Main App** | 5736–5943 | Root component + routing |

### Mock Data Objects

| Constant | Records | Simulates |
|---|---|---|
| `PETS` | 6 pets | Adoption listings with photos, personality tags, health info |
| `VETS` | 4 vets | Veterinarian profiles with specialties, ratings, locations |
| `APPOINTMENTS` | 5 | Scheduled vet visits with status tracking |
| `MESSAGES_DATA` | 6 threads | Chat conversations between roles |
| `ADOPTION_APPS` | 3 | Owner-side adoption application tracking |
| `SHELTER_APPS` | 4 | Shelter-side application review queue |
| `PATIENT_RECORDS` | 3 | Vet patient medical records |
| `ADMIN_USERS` | 6 | Platform user management table |
| `NOTIFICATIONS` | 5 | In-app notification feed |
| `GUIDE_ARTICLES` | 4 | Pet care educational content |
| `MEET_GREETS` | 3 | Scheduled meet-and-greet sessions |
| `SYS_LOGS` | 5 | Admin system activity logs |
| `adoptionTrend`, `speciesBreakdown`, `adminStats` | Chart data | Recharts dashboard visualizations |

All data uses realistic Bangladesh locations (Mirpur, Chittagong, Sylhet) and Bengali-context names.

### Shared UI Components (defined inline)

| Component | Purpose |
|---|---|
| `Badge` | Colored status pills (Available, Pending, Adopted) |
| `StatCard` | Dashboard metric cards with icon, value, trend |
| `PageTitle` | Consistent page heading with optional subtitle and action button |
| `EmptyState` | Friendly "nothing here yet" messages with icons |
| `VerificationBadge` | Green verified checkmark for shelters/vets |
| `SuccessNotice` | Toast-style success banner (auto-dismissible) |
| `WelcomeModal` | First-login greeting per role (state exists but trigger not wired) |
| `ReminderWidget` | Upcoming appointment card on owner dashboard |
| `LoadingCards` / `Skeleton` / `*Skeleton` | Loading state placeholders |
| `ConfirmationDialog` | Modal for destructive action confirmation |
| `Breadcrumbs` | Navigation breadcrumb trail |
| `DocumentPreviewModal` | Mock document viewer |
| `ErrorPage` | 403/404/500 error screens |
| `AuthShell` | Wrapper layout for auth pages |
| `DashboardLayout` | Full authenticated app shell with sidebar, header, search, notifications |

### Screen Inventory by Role

**Guest (Public):** Landing Page, Browse Pets, Pet Detail, Login, Register, Forgot Password, Reset Sent, Error Pages

**Pet Owner (11 views):** Dashboard, Browse Pets, Pet Detail, Find Vet, Adoption Tracking, My Pets, Appointments, Favorites, Pet Care Guide, Messages, Notifications, Settings

**Shelter (9 views):** Dashboard, Add Pet, Pet Listings, Applications, Meet & Greet, Certificates, Analytics, Messages, Settings

**Veterinarian (7 views):** Dashboard, Appointments, Patient Records, Prescriptions, Medical History, Messages, Settings

**Administrator (10 views):** Dashboard, Users, Verification, Pets & Listings, Adoptions, Analytics, Broadcasts, Moderation, Feedback, System Logs, Settings

### State Management Pattern

```typescript
const [role, setRole] = useState<Role>("guest");
const [view, setView] = useState("landing");
const [selectedPetId, setSelectedPetId] = useState<number>(1);
const [showWelcome, setShowWelcome] = useState(false);
```

No Redux, Zustand, or Context API. Each screen manages its own local state with `useState`. Data mutations (favoriting a pet, canceling an appointment) happen in local component state and reset on page refresh.

---

## 8. `src/app/components/` — Component Library (Scaffold)

This folder contains a full **shadcn/ui** component library exported from Figma Make. **None of these components are currently imported by `App.tsx`**. The main app builds UI inline with Tailwind utility classes.

### `components/figma/`

#### `ImageWithFallback.tsx`
**Purpose:** Resilient image component for Figma Make exports.

**How it works:** Wraps `<img>` with an `onError` handler. If the image URL fails to load, it swaps to an inline SVG placeholder (broken-image icon). Prevents broken image icons from appearing in the UI.

**Status:** Not used in `App.tsx` (which uses raw `<img>` tags with Unsplash URLs).

---

### `components/ui/` — shadcn/ui Library (47 files)

Each file follows the shadcn/ui pattern: **Radix UI primitive + Tailwind styling + `cn()` utility**. They are pre-built for when the app is refactored from the monolith.

| File | Built On | Purpose |
|---|---|---|
| **`utils.ts`** | `clsx` + `tailwind-merge` | `cn()` helper — merges Tailwind classes without conflicts |
| **`use-mobile.ts`** | `window.matchMedia` | Hook returning `true` when viewport < 768px |
| **`button.tsx`** | Radix Slot + CVA | Button with 6 variants (default, destructive, outline, secondary, ghost, link) and 4 sizes |
| **`input.tsx`** | Native `<input>` | Styled text input with focus ring |
| **`textarea.tsx`** | Native `<textarea>` | Multi-line text input |
| **`label.tsx`** | Radix Label | Accessible form labels |
| **`checkbox.tsx`** | Radix Checkbox | Toggle checkbox with check indicator |
| **`radio-group.tsx`** | Radix Radio Group | Mutually exclusive option selector |
| **`switch.tsx`** | Radix Switch | On/off toggle switch |
| **`select.tsx`** | Radix Select | Dropdown select with keyboard navigation |
| **`slider.tsx`** | Radix Slider | Range slider input |
| **`toggle.tsx`** | Radix Toggle | Pressable toggle button |
| **`toggle-group.tsx`** | Radix Toggle Group | Group of toggle buttons |
| **`tabs.tsx`** | Radix Tabs | Tabbed content panels |
| **`accordion.tsx`** | Radix Accordion | Expandable/collapsible sections |
| **`collapsible.tsx`** | Radix Collapsible | Single expand/collapse region |
| **`dialog.tsx`** | Radix Dialog | Modal dialog overlay |
| **`alert-dialog.tsx`** | Radix Alert Dialog | Confirmation modal (requires explicit action) |
| **`sheet.tsx`** | Radix Dialog (side) | Slide-in panel from screen edge |
| **`drawer.tsx`** | Vaul | Bottom/side drawer (mobile-friendly) |
| **`popover.tsx`** | Radix Popover | Floating content anchored to trigger |
| **`tooltip.tsx`** | Radix Tooltip | Hover/focus tooltip |
| **`hover-card.tsx`** | Radix Hover Card | Rich hover preview card |
| **`dropdown-menu.tsx`** | Radix Dropdown Menu | Context/action menu |
| **`context-menu.tsx`** | Radix Context Menu | Right-click menu |
| **`menubar.tsx`** | Radix Menubar | Horizontal menu bar |
| **`navigation-menu.tsx`** | Radix Navigation Menu | Site navigation with dropdowns |
| **`command.tsx`** | cmdk | Command palette / search dialog (⌘K style) |
| **`calendar.tsx`** | react-day-picker | Date picker calendar grid |
| **`input-otp.tsx`** | input-otp | One-time password input boxes |
| **`card.tsx`** | Native divs | Card container with header, content, footer |
| **`badge.tsx`** | Native span | Small status/label badge |
| **`avatar.tsx`** | Radix Avatar | User avatar with image fallback |
| **`separator.tsx`** | Radix Separator | Horizontal/vertical divider line |
| **`aspect-ratio.tsx`** | Radix Aspect Ratio | Container maintaining fixed aspect ratio |
| **`scroll-area.tsx`** | Radix Scroll Area | Custom styled scrollbar region |
| **`progress.tsx`** | Radix Progress | Progress bar indicator |
| **`skeleton.tsx`** | Native div | Loading placeholder shimmer |
| **`table.tsx`** | Native table | Styled data table with header/body/footer |
| **`breadcrumb.tsx`** | Native nav | Breadcrumb navigation trail |
| **`pagination.tsx`** | Native buttons | Page number navigation |
| **`alert.tsx`** | Native div | Inline alert banner (info, warning, error) |
| **`carousel.tsx`** | embla-carousel-react | Image/content carousel with prev/next |
| **`chart.tsx`** | Recharts | Themed chart wrapper with CSS variable colors |
| **`form.tsx`** | react-hook-form | Form field wrapper with validation context |
| **`resizable.tsx`** | react-resizable-panels | Drag-to-resize panel layouts |
| **`sidebar.tsx`** | Multiple ui imports | Full sidebar system with collapse, mobile sheet, cookies (~727 lines) |
| **`sonner.tsx`** | sonner | Toast notification provider wrapper |

**Why this entire library exists:** Figma Make auto-generates the complete shadcn/ui suite when you select it as the design system. It provides a production-ready component foundation. The current prototype phase skipped integrating these in favor of rapid inline Tailwind development inside `App.tsx`.

---

## 9. Data Flow Diagram

```text
User Action (click "Sign in as Pet Owner")
        │
        ▼
  navigate("dashboard", "owner")
        │
        ├── setRole("owner")
        └── setView("dashboard")
        │
        ▼
  role !== "guest" → skip public screens
        │
        ▼
  <DashboardLayout role="owner" view="dashboard">
        │
        ├── Sidebar reads NAV["owner"] → 11 menu items
        ├── Header shows search, bell, messages, avatar
        └── Main content: renderContent()
                │
                └── role === "owner" && view === "dashboard"
                        → <OwnerDashboard />
                                │
                                ├── Reads PETS, APPOINTMENTS (mock)
                                ├── Renders StatCards, ReminderWidget
                                └── Quick action buttons → setView("browse-pets")
```

---

## 10. Build & Development Pipeline

```text
Developer runs: pnpm dev
        │
        ▼
    Vite Dev Server (port 5173)
        │
        ├── @vitejs/plugin-react → JSX → JS
        ├── @tailwindcss/vite → scans classes → CSS
        ├── figmaAssetResolver → resolves figma: imports
        └── Hot Module Replacement (instant updates)
        │
        ▼
    Browser loads index.html → main.tsx → App.tsx

Developer runs: pnpm build
        │
        ▼
    Vite Production Build
        │
        ├── Tree-shaking (unused shadcn components excluded)
        ├── Minification (JS: 743KB → 194KB gzipped)
        └── Output → dist/
```

---

## 11. Key Architectural Observations

### Strengths
- **Complete UX prototype** covering 40+ screens across 4 roles
- **Consistent design language** with green health-care aesthetic
- **Realistic mock data** with Bangladesh-specific context
- **Responsive sidebar** with collapse and mobile breakpoints
- **Rich dashboard analytics** using Recharts
- **Full shadcn/ui library** ready for component extraction

### Current Limitations

| Area | Status |
|---|---|
| Routing | State-based strings, no URL changes, no browser back/forward |
| Authentication | Demo buttons only — no validation, sessions, or JWT |
| Data persistence | All mock — resets on refresh |
| Component architecture | Monolithic 5,900-line file |
| shadcn/ui integration | 47 components scaffolded but unused |
| Theme consistency | Hardcoded hex in App vs. CSS variables in theme |
| Font | README says Manrope, code uses Inter |
| Welcome modal | State exists but never triggered on login |
| React Router | Installed in dependencies but not used |

### Recommended Evolution Path
1. **Extract screens** from `App.tsx` into `src/app/screens/` folder
2. **Adopt shadcn/ui components** to replace inline Tailwind UI
3. **Add React Router** for URL-based navigation
4. **Replace mock data** with API calls to a backend
5. **Use theme tokens** (`bg-primary`) instead of hardcoded hex values
6. **Wire up WelcomeModal** on first login per role

---

## 12. Summary Table — Every File at a Glance

| File / Folder | Category | Active? | Purpose |
|---|---|---|---|
| `index.html` | Config | Yes | HTML entry shell |
| `package.json` | Config | Yes | Dependencies & scripts |
| `vite.config.ts` | Config | Yes | Bundler + Figma plugin |
| `tsconfig.json` | Config | Yes | TypeScript settings |
| `pnpm-workspace.yaml` | Config | Yes | pnpm build policies |
| `pnpm-lock.yaml` | Config | Yes | Locked versions |
| `postcss.config.mjs` | Config | Empty | PostCSS placeholder |
| `default_shadcn_theme.css` | Styles | No | Reference theme |
| `README.md` | Docs | Yes | Project documentation |
| `ATTRIBUTIONS.md` | Docs | Yes | License credits |
| `.gitignore` | Config | Yes | Git exclusions |
| `guidelines/Guidelines.md` | Docs | Template | AI design rules |
| `dist/` | Build | Generated | Production output |
| `src/main.tsx` | Entry | Yes | React bootstrap |
| `src/styles/index.css` | Styles | Yes | CSS import chain |
| `src/styles/fonts.css` | Styles | Yes | Inter font |
| `src/styles/tailwind.css` | Styles | Yes | Tailwind v4 init |
| `src/styles/theme.css` | Styles | Yes | Design tokens |
| `src/app/App.tsx` | App | Yes | **Entire application** |
| `src/app/components/ui/*` | Library | Scaffolded | shadcn/ui (47 files) |
| `src/app/components/figma/*` | Utility | Scaffolded | Image fallback |
| `src/imports/pasted_text/*` | Archive | No | Original AI prompt |

---

This case study reflects the project as it exists today: a feature-rich front-end prototype with a monolithic architecture, a complete but unused component library, and mock-data-driven role-based navigation — ready for backend integration and structural refactoring.
