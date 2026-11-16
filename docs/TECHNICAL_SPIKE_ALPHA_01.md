# Technical Spike Alpha 0.1

**Date:** 2026-11-16  
**Branch:** prototype/alpha-01  
**Scope:** PWA shell, workspace/journal interaction proof, browser-local persistence concept

## 1. Purpose

Create the first runnable front-end prototype before committing to the production React/editor/backend architecture. This spike checks the visual shell and the fastest capture path using browser-native capabilities, with no package dependencies or cloud credentials.

## 2. Prototype delivered

The source bundle is supplied as `Mission-OS-Alpha-0.1.zip` in the conversation. It contains:

- Responsive Midnight Workshop app shell.
- Today dashboard concept with XP panel, briefing, Main Mission placeholder, habits placeholder, quick actions and recent local records.
- Workspace page list and lightweight contenteditable writing experiment.
- Journal quick capture with note, idea and reflection types.
- Local search across pages and journal entries.
- IndexedDB stores for pages, journal entries and metadata.
- Page title/content autosave.
- PWA manifest and static app-shell service worker.
- Service worker caches static shell files only; personal records are not placed in Cache Storage.
- User-controlled Recovery Mode presentation toggle.

## 3. Test and verification status

### Passed static checks
- JavaScript syntax validated with `node --check app.js`.
- PWA manifest parses as valid JSON.
- Required prototype files exist and are non-empty.
- The app root responded to a local HTTP GET during the initial server check.

### Not verified in this execution environment
- Browser automation could launch Chromium, but navigation to localhost and file URLs was blocked with `ERR_BLOCKED_BY_ADMINISTRATOR`.
- A headless Chromium screenshot attempt stalled in the environment.
- The package registry request timed out, so React, BlockNote, Tiptap, Dexie, and the Vite PWA plugin were not installed or built here.
- IndexedDB persistence, service-worker installation/offline behavior, real mobile layout, and editor interactions still require a browser/device test.

Do not interpret an unverified test as passed.

## 4. Deliberate limitations

This prototype is local-only:
- No authentication.
- No cloud database or synchronization.
- No account/workspace sharing.
- No ChatGPT connector.
- No production-grade editor dependency.
- No production task, habit, tracker, database-view, relation, or rollup engine.

The writing canvas is a small contenteditable experiment, not a selected production editor. It demonstrates the intended workflow only. It lacks the advanced structured blocks, robust sanitization, revision history, collaborative editing, and schema migration required for the final product.

The browser stores prototype records in IndexedDB. Clearing site data can delete them. Do not store sensitive personal information or rely on this prototype as a backup.

## 5. Technical findings

1. A zero-dependency shell is sufficient to test page hierarchy and capture flow, but it is not a substitute for the production React app.
2. The block editor remains an open decision. BlockNote is the first candidate; compare it against Tiptap in a dependency-enabled environment and on the target Android phone.
3. The service worker source caches static shell files only. The browser may stop service workers, so foreground/reconnect sync remains the reliable baseline for the real app.
4. IndexedDB is the planned local structured store. Real persistence and quota/recovery behavior must be tested in a browser.
5. Supabase/authentication were intentionally not wired up. A private backend and RLS proof must happen before real personal data is stored remotely.

## 6. Next validation checklist

- [ ] Open the prototype in a current Chromium-based Android browser over localhost or HTTPS.
- [ ] Create a page, type content, reload, and verify restoration.
- [ ] Create a journal entry, reload, and verify restoration.
- [ ] Test editor input, toolbar formatting, paste, and long content.
- [ ] Install the PWA after a successful online load.
- [ ] Test app-shell access offline.
- [ ] Compare BlockNote and Tiptap using the same sample document.
- [ ] Test the chosen editor's structured JSON round-trip and Markdown export.
- [ ] Create a development backend and prove RLS allow/deny behavior with separate test identities.
- [ ] Only then choose the production stack and begin the secure app foundation.

## 7. Decision

**Status: Prototype scaffold created; technical spike is partially verified.**

Proceed to the editor comparison and real-browser test when package installation and target-device testing are available. Do not yet treat React, BlockNote, Supabase, offline sync, or PWA installation as proven in this spike.
