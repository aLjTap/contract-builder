<p align="center">
  <img src="https://img.shields.io/badge/Canton_Network-Powered-6C5CE7?style=for-the-badge" alt="Canton Network" />
  <img src="https://img.shields.io/badge/Daml-v3.4+-00D2FF?style=for-the-badge" alt="Daml" />
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white" alt="React 19" />
  <img src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" />
</p>

# 🧱 DAML Contract Builder

A **visual low-code dashboard** for the [Canton Network](https://www.canton.network/) ecosystem. Design, compose, and deploy **Daml smart contracts** through an intuitive drag-and-drop node graph — no Daml coding required.

> Built with React Flow · Powered by the Global Synchronizer (Splice)

---

## ✨ What is this?

DAML Contract Builder lets you visually design smart contracts by dragging nodes onto a canvas, connecting them, and generating valid Daml source code in real-time. Think of it as **Figma for Daml contracts**.

### Key Capabilities

- 🎨 **Visual Contract Design** — Drag & drop Template, Choice, Party, and DataType nodes onto a React Flow canvas
- ⚡ **Real-time Code Generation** — See valid Daml v3.4+ code generated live as you build
- 🔗 **Canton Network Integration** — Native support for Splice packages (`splice-amulet`, `splice-wallet`, `splice-amulet-name-service`, `splice-dso-governance`)
- 🤖 **AI-Assisted Building** — Describe your contract in natural language, get a visual graph
- ✅ **Smart Validation** — Real-time validation ensures your graph produces compilable Daml
- 🚀 **One-Click Deploy** — Compile and deploy directly to a Canton Validator Node

---

## 🏛️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Web Application                    │
│  ┌─────────────┐  ┌──────────┐  ┌───────────────┐  │
│  │ React Flow  │  │  Monaco   │  │  AI Assistant  │  │
│  │   Canvas    │◄─┤  Editor   │  │   (Gemini)    │  │
│  └──────┬──────┘  └────▲─────┘  └───────┬───────┘  │
│         │              │                │           │
│         ▼              │                ▼           │
│  ┌─────────────────────┴────────────────────────┐   │
│  │           Daml Code Generation Engine         │   │
│  │     Graph → AST → Daml v3.4+ Source Code     │   │
│  └──────────────────────┬───────────────────────┘   │
│                         │                           │
│  ┌──────────────────────▼───────────────────────┐   │
│  │              Canton API Client                │   │
│  │   Ledger API · Scan API · Validator API      │   │
│  └──────────────────────┬───────────────────────┘   │
└─────────────────────────┼───────────────────────────┘
                          │
              ┌───────────▼───────────┐
              │   Canton Network      │
              │  (Global Synchronizer)│
              └───────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | [TanStack Start](https://tanstack.com/start) (SSR) + React 19 |
| **Graph Editor** | [@xyflow/react](https://reactflow.dev/) v12 (React Flow) |
| **Styling** | Tailwind CSS v4 + [shadcn/ui](https://ui.shadcn.com/) |
| **Code Editor** | Monaco Editor (Daml syntax) |
| **API** | [ORPC](https://orpc.dev/) (end-to-end type-safe) |
| **Database** | Drizzle ORM + SQLite/Turso |
| **Auth** | [Better-Auth](https://www.better-auth.com/) |
| **AI** | [AI SDK](https://sdk.vercel.ai/) (Google Gemini) |
| **State** | Zustand (canvas state) |
| **Auto-layout** | ELK.js |
| **Build** | Turborepo + pnpm workspaces |
| **Linting** | Biome |
| **Smart Contracts** | [Daml](https://docs.digitalasset.com/build/3.4/) v3.4+ |
| **Blockchain** | [Canton Network](https://www.canton.network/) / [Splice](https://github.com/hyperledger-labs/splice) |

---

## 📁 Project Structure

```
contract-builder/
├── apps/
│   ├── web/                    # Main web application (TanStack Start)
│   │   └── src/
│   │       ├── routes/         # File-based routing
│   │       │   ├── builder.tsx         # Visual contract builder
│   │       │   ├── builder/$projectId  # Project editor
│   │       │   ├── projects.tsx        # Project management
│   │       │   ├── ai.tsx              # AI chat assistant
│   │       │   ├── dashboard.tsx       # User dashboard
│   │       │   └── login.tsx           # Authentication
│   │       ├── components/
│   │       │   ├── builder/            # Builder canvas, sidebar, nodes
│   │       │   └── ui/                 # shadcn/ui components
│   │       └── stores/                 # Zustand stores
│   └── docs/                   # Documentation (Astro Starlight)
├── packages/
│   ├── daml-core/              # Daml type definitions & AST
│   ├── daml-codegen/           # Graph → Daml compiler & validator
│   ├── canton-client/          # Canton Network API client
│   ├── api/                    # ORPC API routes & business logic
│   ├── auth/                   # Authentication (Better-Auth)
│   ├── db/                     # Database schema (Drizzle + SQLite)
│   ├── env/                    # Environment variable validation
│   └── config/                 # Shared TypeScript config
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 20
- **pnpm** ≥ 10.26

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/contract-builder.git
cd contract-builder

# Install dependencies
pnpm install
```

### Database Setup

```bash
# Start local SQLite database (optional — for local dev)
pnpm run db:local

# Apply schema
pnpm run db:push
```

### Environment Variables

Copy the example env file and fill in your values:

```bash
cp apps/web/.env.example apps/web/.env
```

| Variable | Description |
|----------|-------------|
| `DATABASE_URL` | SQLite/Turso database URL |
| `DATABASE_AUTH_TOKEN` | Turso auth token (production) |
| `BETTER_AUTH_SECRET` | Authentication secret key |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Gemini API key for AI features |

### Development

```bash
# Start all apps in dev mode
pnpm run dev

# Start only the web app
pnpm run dev:web
```

Open [http://localhost:3001](http://localhost:3001) to start building contracts.

---

## 📜 Available Scripts

| Command | Description |
|---------|-------------|
| `pnpm run dev` | Start all apps in development mode |
| `pnpm run build` | Production build for all apps |
| `pnpm run check` | Run Biome linting & formatting |
| `pnpm run check-types` | TypeScript type checking across all packages |
| `pnpm run db:push` | Push schema changes to database |
| `pnpm run db:generate` | Generate Drizzle client/types |
| `pnpm run db:migrate` | Run database migrations |
| `pnpm run db:studio` | Open Drizzle Studio (database GUI) |
| `pnpm run db:local` | Start local SQLite instance |

---

## 🗺️ Roadmap

See [PHASE_PLAN.md](./PHASE_PLAN.md) for the detailed 5-phase implementation roadmap.

| Phase | Status | Description |
|-------|--------|-------------|
| **Phase 1** | 🔜 | Foundation & Core Canvas |
| **Phase 2** | 📋 | Daml Code Generation Engine |
| **Phase 3** | 📋 | Canton Network Integration |
| **Phase 4** | 📋 | AI-Assisted Contract Building |
| **Phase 5** | 📋 | Testing, Deployment & Production |

---

## 🌐 Canton Network Resources

- **Daml Documentation**: https://docs.digitalasset.com/build/3.4/
- **Splice Documentation**: https://docs.sync.global/
- **Splice Repository**: https://github.com/hyperledger-labs/splice
- **Canton Network**: https://www.canton.network/
- **Featured Apps**: https://sync.global/featured-apps/
- **App Dev Slack**: `#gsf-global-synchronizer-appdev` (request access via operations@sync.global)

---

## 📄 License

This project is licensed under the [MIT License](./LICENSE).
