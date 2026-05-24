# VP

## 1. Theme Summary
SALTEDHASH Work / Productivity / Visual Planning is a visual-first planning system that combines lightweight task execution with creative context. The MVP prioritizes fast board setup, intuitive card flow, and inspiration-aware planning for solo founders and small teams.

## 2. Covered Product Types
- Kanban Artboard
- Visual task workflow planners
- Creative project organization tools
- Mood/reference-supported execution boards
- Lightweight collaborative planning workspaces

## 3. Shared Problem Framework
- Traditional PM tools are overbuilt for early-stage execution.
- Visual inspiration is disconnected from task tracking.
- Founders need one place for priorities, references, and progress.
- Teams need activity history without enterprise overhead.

## 4. Core User Types
- Solo founder/operator
- Small creative teams (2–10)
- Product + design hybrid contributors
- Project coordinators needing visual execution clarity

## 5. SALTEDHASH Product Strategy
- Build a reusable core for multiple SALTEDHASH workflow products.
- Differentiate through visual + task blending, not enterprise features.
- Keep operations JSON-first, local-hostable, and low-cost.
- Ship narrow MVP depth, then expand by reusable modules.

## 6. Architecture Style
- Monorepo-friendly split: Vue 3 + Vite frontend, Express backend.
- JSON-first persistence with file-based NoSQL-style stores.
- Module boundaries around board/task/asset/event concerns.
- API-first internal contracts for future product reuse.

## 7. Global System Structure
- `client`: visual board app, workspace/admin UI.
- `server`: REST APIs, auth, file JSON repositories.
- `data`: JSON collections for workspaces, boards, tasks, assets, events.
- `shared`: schema definitions and validation utilities.

## 8. Core Reusable Modules
- **Board system**: board lifecycle, metadata, visibility.
- **Lists/columns**: ordered workflow stages.
- **Task cards**: tasks, owners, due dates, status, checklist.
- **Visual attachments**: image/file references on cards.
- **Color tags**: semantic labels and visual categorization.
- **Mood/reference panels**: inspiration blocks tied to board context.
- **Collaboration events**: comments, mentions, assignment changes.
- **Activity history**: immutable timeline of major actions.
- **Workspace settings**: members, permissions, defaults, branding.

## 9. NoSQL / JSON Data Architecture
- Collection-per-entity JSON files (or partitioned files by workspace).
- IDs use UUID-like strings; references are explicit fields.
- Append-only event log + current-state documents.
- Schema validation on read/write to prevent drift.
- Soft-delete flags for recoverability.

## 10. Authentication and Authorization
- MVP auth: email/password + hashed credentials in JSON user store.
- Session/JWT token-based auth via Express middleware.
- Role model: owner, editor, viewer at workspace level.
- Board-level ACL inherits workspace role with optional overrides.

## 11. Board and Task Engine
- Board CRUD with ordered lists/columns.
- Card CRUD with drag/drop ordering by index keys.
- Card fields: title, description, assignees, tags, due date, checklist.
- WIP-friendly quick add and bulk move actions.
- Event emission on every state-changing action.

## 12. Visual Asset and Layout Layer
- Asset registry for uploaded or linked references.
- Card-level and panel-level attachment bindings.
- Mood/reference panel supports pinned images, URLs, notes.
- Layout preferences (panel width, zoom, collapsed states) saved per board/user.

## 13. Folder Structure
- `/client`
  - `/src/modules/boards`
  - `/src/modules/tasks`
  - `/src/modules/assets`
  - `/src/modules/workspace`
  - `/src/components`
- `/server`
  - `/src/routes`
  - `/src/services`
  - `/src/repositories/json`
  - `/src/middleware`
- `/shared`
  - `/schemas`
  - `/types`
- `/data`
  - `/workspaces`
  - `/boards`
  - `/tasks`
  - `/assets`
  - `/events`

## 14. API Design
- `POST /auth/login`, `POST /auth/register`, `POST /auth/logout`
- `GET/POST /workspaces`
- `GET/PATCH /workspaces/:id/settings`
- `GET/POST /boards`, `GET/PATCH/DELETE /boards/:id`
- `POST /boards/:id/lists`, `PATCH /lists/:id`, `DELETE /lists/:id`
- `POST /cards`, `GET/PATCH/DELETE /cards/:id`
- `POST /cards/:id/attachments`
- `GET /boards/:id/activity`
- `POST /events` (internal/audit ingestion)

## 15. Frontend Architecture
- Vue 3 composition API with feature modules by domain.
- Central state via lightweight store (Pinia-style pattern).
- Board view: column lanes + card interactions + side panels.
- Reusable UI primitives for tags, avatars, status chips, attachment previews.
- Optimistic updates with rollback on API failure.

## 16. Admin and Workspace Management System
- Workspace creation and membership invites.
- Role assignment and removal controls.
- Workspace settings for color system, labels, board templates.
- Usage/health panels (counts for boards/cards/assets/events).

## 17. Data Models with sample JSON
```json
{
  "workspace": {
    "id": "ws_001",
    "name": "Saltedhash Studio",
    "settings": {
      "defaultBoardView": "kanban",
      "colorTags": ["urgent", "focus", "idea"]
    },
    "members": [
      { "userId": "u_001", "role": "owner" }
    ]
  },
  "board": {
    "id": "b_001",
    "workspaceId": "ws_001",
    "title": "Q3 Launch",
    "listIds": ["l_001", "l_002"],
    "moodPanelId": "mp_001"
  },
  "list": {
    "id": "l_001",
    "boardId": "b_001",
    "name": "In Progress",
    "cardIds": ["c_001"]
  },
  "card": {
    "id": "c_001",
    "boardId": "b_001",
    "listId": "l_001",
    "title": "Draft launch narrative",
    "tags": ["focus"],
    "attachments": ["a_001"]
  },
  "asset": {
    "id": "a_001",
    "type": "image",
    "url": "/assets/launch-mood.png"
  },
  "event": {
    "id": "e_001",
    "workspaceId": "ws_001",
    "type": "card.updated",
    "actorId": "u_001",
    "targetId": "c_001",
    "createdAt": "2026-05-24T00:00:00Z"
  }
}
```

## 18. MVP Scope
- Auth + single workspace per user (initially).
- Core board system with lists/columns and task cards.
- Tags, attachments, mood/reference panel, activity history.
- Basic collaboration events (comments + assignment updates).
- Workspace settings and simple admin/member controls.

## 19. Expansion Strategy
- Multi-workspace and template marketplaces.
- Real-time collaboration (WebSocket event streams).
- Board automations (rules, triggers, recurring cards).
- Advanced reporting and AI-assisted planning overlays.
- Plugin-style modules reusable across SALTEDHASH products.

## 20. Risks and Guardrails
- **Risk:** JSON scaling bottlenecks.  
  **Guardrail:** partition by workspace + periodic compaction.
- **Risk:** Permission drift.  
  **Guardrail:** centralized auth middleware and schema validation.
- **Risk:** Scope creep toward enterprise PM.  
  **Guardrail:** strict MVP boundaries and module gates.
- **Risk:** Visual complexity hurting usability.  
  **Guardrail:** progressive disclosure and default-simple layouts.

## 21. Final Recommendation
Launch a focused Kanban Artboard MVP that tightly integrates execution (lists, cards, activity) with inspiration (mood/reference panels, visual assets). Keep persistence JSON-first and module-driven so SALTEDHASH can rapidly evolve this into a broader family of visual workflow products without re-platforming.