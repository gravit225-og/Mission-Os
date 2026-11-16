# Mission OS Alpha 0.1 Prototype

This folder documents the first dependency-free prototype source bundle.

The source archive is delivered separately as `Mission-OS-Alpha-0.1.zip`. Extract it and serve the `mission-os-alpha-01` folder over localhost or HTTPS.

## Contents
- Responsive app shell and Today dashboard.
- Workspace page creation and local writing experiment.
- Journal quick capture and timeline.
- Search across local records.
- IndexedDB persistence and page autosave.
- PWA manifest and app-shell service worker.

## Run
```bash
python -m http.server 4173
```
Open `http://localhost:4173`.

## Important
This is a local-only prototype, not the production application. It has no authentication, cloud database, synchronization, or ChatGPT connector. The contenteditable editor is an experiment, not a final editor choice. See [Technical Spike Alpha 0.1](../../docs/TECHNICAL_SPIKE_ALPHA_01.md) for verified checks and remaining tests.
