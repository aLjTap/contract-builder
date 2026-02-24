# DAML Contract Builder — Phase Plan

> Detailed implementation roadmap for the low-code Daml contract builder dashboard.

---

## 📋 Phase Plan

---

### Phase 1 — Foundation & Core Canvas (2-3 weeks)

> Set up the React Flow canvas, core node types, and basic project infrastructure.

#### 1.1 Project Setup
- [ ] Install `@xyflow/react`, `elkjs`, `zustand`, `@monaco-editor/react`
- [ ] Create new route `/builder` in `apps/web/src/routes/`
- [ ] Set up Zustand store for canvas state (`useFlowStore`)
- [ ] Configure React Flow dark mode with existing Tailwind theme
- [ ] Create `packages/daml-core` workspace package for Daml type definitions and codegen

#### 1.2 Core Node System
- [ ] **Template Node** — Represents a Daml Template
  - Fields: template name, signatories, observers
  - Data field definitions (name, type, optionality)
  - Visual: Card-style node with expandable sections
- [ ] **Choice Node** — Represents a Daml Choice on a Template
  - Fields: choice name, controller, consuming/non-consuming
  - Parameters with type definitions
  - Return type selector
  - Body expression builder (simple mode)
- [ ] **Party Node** — Represents a party reference
  - Fields: party alias, description
  - Connects to signatory/observer/controller ports
- [ ] **Data Type Node** — Custom data types (Record/Variant)
  - Fields: type name, constructors/fields
  - Used as field types in Templates

#### 1.3 Connection System (Edges)
- [ ] **Signatory Edge** — Party → Template (signatory role)
- [ ] **Observer Edge** — Party → Template (observer role)
- [ ] **Controller Edge** — Party → Choice (controller role)
- [ ] **Field Type Edge** — DataType → Template field
- [ ] **Choice Attachment Edge** — Choice → Template
- [ ] Custom edge styles with labeled arrows and color coding
- [ ] Connection validation rules (type-safe port matching)

#### 1.4 Sidebar & Toolbox
- [ ] Draggable node palette (Template, Choice, Party, DataType)
- [ ] Node search/filter functionality
- [ ] Minimap and zoom controls
- [ ] Canvas background grid (React Flow `<Background>`)

**Deliverable**: Interactive canvas where users can place, connect, and configure basic Daml model nodes.

---

### Phase 2 — Daml Code Generation Engine (2-3 weeks)

> Transform the visual graph into valid Daml source code.

#### 2.1 Graph-to-AST Compiler
- [ ] Create `packages/daml-codegen` workspace package
- [ ] Build a graph traversal engine that walks the React Flow node/edge graph
- [ ] Produce an intermediate AST (Abstract Syntax Tree) representation:
  ```
  DamlModule
  ├── ModuleName
  ├── Imports[]
  ├── DataTypes[]
  ├── Templates[]
  │   ├── Fields[]
  │   ├── Signatories[]
  │   ├── Observers[]
  │   └── Choices[]
  │       ├── Controller
  │       ├── Parameters[]
  │       ├── ReturnType
  │       └── Body (expression tree)
  └── Interfaces[] (Phase 4)
  ```

#### 2.2 AST-to-Daml Emitter
- [ ] Pretty-printer that emits valid Daml v3.4+ syntax
- [ ] Correct indentation and formatting
- [ ] Automatic import generation (`import Daml.Script`, `DA.List`, etc.)
- [ ] Template/Choice name collision detection

#### 2.3 Live Code Preview Panel
- [ ] Split-pane layout: Canvas (left) ↔ Code Preview (right)
- [ ] Monaco Editor with Daml syntax highlighting
- [ ] Real-time code generation as nodes are modified
- [ ] Click-to-highlight: clicking a node highlights corresponding Daml code
- [ ] Error indicators when graph produces invalid Daml

#### 2.4 Validation Engine
- [ ] Graph-level validation rules:
  - Every Template must have ≥ 1 signatory
  - Choice must be attached to exactly one Template
  - Controller must be a party in scope (signatory/observer or parameter)
  - No cyclic dependencies between templates
  - Field names must be unique per template
  - Type references must resolve to known types
- [ ] Real-time validation with error markers on nodes
- [ ] Validation panel listing all errors/warnings

**Deliverable**: Users see live Daml code generated from their visual graph, with validation feedback.

---

### Phase 3 — Advanced Node Types & Canton Network Integration (2-3 weeks)

> Add Splice package integration, Canton-specific nodes, and API connectivity.

#### 3.1 Canton Network Nodes
- [ ] **Interface Node** — Daml Interface definition
  - View-type functions
  - Interface choices
  - Implements relationship (Interface ← Template)
- [ ] **Transfer Node** — `AmuletRules_Transfer` integration
  - Connects to `splice-amulet` package
  - Amount, sender, receiver ports
- [ ] **CNS Node** — Canton Name Service lookup
  - Name resolution for party discovery
  - Links to `splice-amulet-name-service`
- [ ] **Wallet Node** — Wallet workflow triggers
  - Connects to `splice-wallet` automation

#### 3.2 Splice Package Templates
- [ ] Pre-built node groups for common Splice patterns:
  - "Payment Flow" (Transfer + Wallet + Party nodes)
  - "Governance Proposal" (DSO + Voting nodes)
  - "Name Registration" (CNS + Entry nodes)
- [ ] Template gallery in sidebar with preview
- [ ] One-click instantiation of node groups

#### 3.3 Canton API Integration Layer
- [ ] Create `packages/canton-client` workspace package
- [ ] Ledger API client (JSON API mode)
  - Party management endpoints
  - Command submission
  - Active contract set queries
  - Transaction stream subscriptions
- [ ] Scan API client
  - DSO party state queries
  - Network infrastructure status
- [ ] Validator API client
  - Wallet operations
  - Higher-level transaction abstractions
- [ ] Connection settings panel in builder UI
  - Validator Node URL configuration
  - Party authentication token management
  - Network selection (DevNet / TestNet / MainNet)

#### 3.4 Project & File Management
- [ ] DB schema for projects (`projects` table)
  - `id`, `userId`, `name`, `description`, `flowData` (JSON), `damlOutput`, `createdAt`, `updatedAt`
- [ ] DB schema for templates (`project_templates` table)
  - Community-shared template configurations
- [ ] ORPC routes for CRUD operations on projects
- [ ] Auto-save with debounced persistence
- [ ] Project list page with search/sort/filter
- [ ] Import/Export as `.daml` files

**Deliverable**: Canton-aware visual builder with API connectivity and persistent project storage.

---

### Phase 4 — AI-Assisted Contract Building (1-2 weeks)

> Leverage existing AI SDK integration for intelligent contract generation.

#### 4.1 AI Chat → Flow Generation
- [ ] Extend existing `/ai` chat interface with Daml-context system prompt
- [ ] "Describe your contract in plain language" → AI generates node graph JSON
- [ ] AI suggests corrections/improvements to existing graphs
- [ ] Natural language → Daml template generation

#### 4.2 Smart Autocomplete
- [ ] AI-powered field type suggestions based on template name
- [ ] Auto-suggest observers/controllers based on business logic patterns
- [ ] Choice body expression suggestions
- [ ] "What choices should this template have?" suggestions

#### 4.3 Daml-Aware AI Chat
- [ ] Context-aware chat that understands the current canvas state
- [ ] "Add a payment choice to the Asset template" → modifies flow
- [ ] Explain Daml concepts in chat with references to Canton docs
- [ ] Generate Daml Script test code from the visual model

**Deliverable**: AI co-pilot that understands Daml and can create/modify visual contract graphs via natural language.

---

### Phase 5 — Testing, Deployment & Production (1-2 weeks)

> Build deployment pipeline, testing infrastructure, and production polish.

#### 5.1 Daml Sandbox Integration
- [ ] One-click "Test in Sandbox" button
  - Spins up Daml Sandbox (local or hosted)
  - Compiles generated Daml code
  - Deploys DAR to sandbox
  - Opens Daml Navigator for interaction testing
- [ ] Test result visualization in builder UI
- [ ] Daml Script runner from generated test files

#### 5.2 Canton Network Deployment
- [ ] Deploy workflow:
  1. Validate graph → Generate Daml → Compile to DAR
  2. Connect to target Validator Node
  3. Upload DAR package
  4. Allocate parties
  5. Execute initialization script
- [ ] Deployment status tracking and logs
- [ ] Rollback capabilities

#### 5.3 Collaboration & Sharing
- [ ] Share project via URL (read-only view)
- [ ] Export as:
  - `.daml` source files
  - Full Daml project (with `daml.yaml`)
  - PDF/PNG of the visual graph
- [ ] Version history with diff view
- [ ] Fork/clone community templates

#### 5.4 UI/UX Polish
- [ ] Keyboard shortcuts (Ctrl+S save, Ctrl+Z undo, Delete remove)
- [ ] Undo/Redo system (command pattern on Zustand store)
- [ ] Node grouping (sub-flows)
- [ ] Auto-layout with `elkjs`
- [ ] Responsive layout for tablet screens
- [ ] Onboarding tutorial / interactive walkthrough
- [ ] Dark/Light mode toggle (leveraging existing `next-themes`)

**Deliverable**: Production-ready low-code Daml contract builder with deployment capabilities.

---

## 📊 Technology Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| **Graph Library** | `@xyflow/react` v12 | Best React node editor; supports SSR, dark mode, shadcn integration |
| **Canvas State** | Zustand | Lightweight, integrates cleanly with React Flow's controlled mode |
| **Code Editor** | Monaco Editor | Industry standard; Daml syntax can be registered as custom language |
| **Auto-layout** | ELK.js | Handles hierarchical graph layouts well; WASM-based, fast |
| **Daml Compilation** | Custom AST compiler | No existing JS-based Daml compiler; we build graph→AST→text pipeline |
| **Canton API** | JSON API (HTTP) | Simpler than gRPC for browser-based client; full Ledger API coverage |
| **AI Integration** | Existing AI SDK | Already integrated; extend with Daml-specific system prompts |

---

## 🔍 Key Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Daml syntax correctness | Generated code may not compile | Extensive validation rules + Sandbox testing integration |
| Canton API auth complexity | Token management for party-scoped APIs | Abstract behind connection manager with refresh logic |
| React Flow performance with large graphs | UI lag with 100+ nodes | Virtualization, sub-flow grouping, computed flows |
| Splice package version changes | Breaking API changes | Develop against stable Interfaces, not internal Templates |
| AI hallucination in Daml context | Invalid contract suggestions | Constrain AI with Daml grammar rules + validation gate |

---

## 🎯 Success Metrics

| Metric | Target |
|--------|--------|
| Time to create a basic contract | < 5 minutes (vs 30+ min manual coding) |
| Generated Daml compilation rate | > 95% valid on first generation |
| Canvas node types | ≥ 8 (Template, Choice, Party, DataType, Interface, Transfer, CNS, Wallet) |
| Canton API operations supported | Create, Exercise, Query, Stream |
| AI-assisted creation accuracy | > 80% correct structure from natural language |

---

## 📅 Milestone Summary

| Phase | Duration | Key Deliverable |
|-------|----------|----------------|
| **Phase 1** | 2-3 weeks | Interactive canvas with core Daml node types |
| **Phase 2** | 2-3 weeks | Live Daml code generation + validation |
| **Phase 3** | 2-3 weeks | Canton Network integration + project persistence |
| **Phase 4** | 1-2 weeks | AI-assisted contract building |
| **Phase 5** | 1-2 weeks | Testing, deployment, and production polish |
| **Total** | **8-13 weeks** | Full low-code Daml contract builder |
