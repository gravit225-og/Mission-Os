# Mission OS Alpha 0.1 prototype

A dependency-free, mobile-first interface prototype for validating the Mission OS workspace experience.

## GitHub Pages preview

The workflow at `.github/workflows/deploy-mission-os-preview.yml` publishes this folder when `prototype/alpha-01` is updated.

Expected preview URL: https://gravit225-og.github.io/Mission-Os/

**One-time repository setting:** open **Settings → Pages → Build and deployment → Source**, choose **GitHub Actions**, and save. Then open **Actions**, select **Deploy Mission OS prototype**, and run it with **Run workflow** (or push another commit to this branch). GitHub Pages is not yet confirmed active until the first deployment succeeds.

## Included experiments

- Today dashboard and navigation
- Workspace page creation and local autosave
- Unified journal capture (notes, ideas, reflections, learning, decisions)
- Local JSON backup export
- Recovery Mode preference
- PWA manifest and static app-shell service worker

## Run locally

Serve this folder over localhost or HTTPS. For example:

```bash
python -m http.server 4173
```

Then open `http://localhost:4173`.

## Important limitations

This is a browser prototype, not production software. Pages and journal entries are stored in the current browser using IndexedDB. There is no authentication, cloud database, cross-device sync, production editor, task engine, habit engine, or MCP integration. The service worker caches static app assets only. Browser interaction and offline behaviour must be verified on real devices before relying on them.

Do not enter sensitive personal information into this public preview. The source repository and preview site are public; this prototype has no account or server-side privacy controls.
