---
name: add-entity-module-view
description: Use this skill when the user wants to customize how a specific data type looks or behaves in the profile dashboard — requests like "customize the profile view for X", "add a custom list/detail screen for entity Y", or "add a settings screen for module Z". Covers adding a per-entity-type view folder that overrides the modules/default fallback. Consult this before hand-adding files under plugins/profile/html/views/modules, since the type-name-to-table linkage and fallback behavior are project-specific.
---

# Add an Entity Module View

Adds or customizes the profile-app view for one entity type (table).

## Step 1: Confirm the table exists

The folder name must match a table id under `plugins/catalog/html/data/fields` (e.g. `asset`,
`entityproduct`, `projectgoal`). If the table doesn't exist yet, create it first using the
`catalog-table-creator` skill in the `catalog` plugin — don't invent a profile view for a
nonexistent table.

## Step 2: Create the module view folder

Add `html/views/modules/<entitytype>/`. Until this folder exists, the type falls back to
`html/views/modules/default/` — only add the files you actually need to override (e.g. just a
custom detail page), everything else keeps falling back to `default`.

## Step 3: Add a settings screen if needed

If the type needs its own configuration screen (separate from viewing/editing records), add
`html/views/settings/modules/<entitytype>/`, following the pattern of existing type folders there
(e.g. `bankaccount`, `entityproduct`).

## Step 4: Wire shared chrome if relevant

If the new view needs a sidebar entry or top-menu link, add it under `html/components/sidebars/`
or `html/components/topmenu/` rather than hardcoding navigation inside the module view itself.

## Step 5: Validate

1. Clear the page cache (or restart).
2. Load the list and detail views for the entity type and confirm the override renders (and that
   types you didn't touch still correctly fall back to `modules/default`).
3. If a settings screen was added, confirm it saves and reloads correctly.
4. Check the view under both `defaulttheme` and `lighttheme` if it touches shared styling.
