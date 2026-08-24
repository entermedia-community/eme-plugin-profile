# AGENTS.md

## Profile Plugin Guide

### Purpose

`plugins/profile` is the end-user dashboard/profile app: the module-based UI for browsing and
editing entity records (assets, users, projects, products, transactions, etc.) that live in the
`catalog` data model. It's a pure HTML/xconf plugin — no `code/` folder, no `plugin.xml`; all
customization is done through view files.

### Folder Map

- `html/views/dashboard/` The landing dashboard
- `html/views/modules/<entitytype>/` Per-entity-type screens (list/detail/edit) — one folder per
  table defined in `plugins/catalog/html/data/fields` (e.g. `asset`, `user`, `entityproduct`,
  `projectgoal`, `library`, `channel`, ...); `modules/default/` is the fallback used when a type
  has no dedicated folder
- `html/views/settings/modules/<entitytype>/` Per-entity-type settings/configuration screens,
  mirroring the same type folders as `html/views/modules`
- `html/views/agentresponses/` Screens/snippets for surfacing AI agent output to the user
- `html/views/services/` JSON/HTML endpoints local to profile (e.g. product add/edit/save)
- `html/components/` Shared UI: sidebars, top menu, chat dashboard, javascript, services
- `html/theme/` Profile's own themes — `defaulttheme` and `lighttheme`, each with its own css/js

### What This Plugin Owns

- The generic "view/edit a record of type X" experience for every entity table
- The user dashboard and navigation chrome (sidebars, top menu)
- Presentation of AI agent responses to the end user

### Editing Rules

- To customize how a specific entity type looks/behaves, add or edit
  `html/views/modules/<entitytype>/` (falls back to `modules/default` if absent) — don't modify
  `modules/default` for a single-type change, add a type-specific folder instead.
- The entity type name must match a table id under `plugins/catalog/html/data/fields`; if the
  table doesn't exist yet, create it first (see the `catalog-table-creator` skill in the `catalog`
  plugin) before adding a profile view for it.
- Settings screens for a type go under `html/views/settings/modules/<entitytype>/`, kept separate
  from the type's regular view folder.
- Theme changes (colors, layout chrome) belong in `html/theme/<themename>/`, not scattered into
  individual module folders.

### Validation Checklist

1. Clear the page cache (or restart) after adding/removing view files.
2. Load the record type's list and detail views and confirm they render (falling back to
   `modules/default` correctly if no override exists).
3. If a settings screen was added, confirm it appears under the type's settings section and saves
   correctly.
4. Check both `defaulttheme` and `lighttheme` if the change touches shared theme assets.

### Notes For Agents

- Because this plugin has no Java layer, "add a field to the profile UI" almost always means (a) a
  schema/field change in `catalog` first, then (b) a view change here to surface it — check both.
