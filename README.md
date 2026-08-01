# BDS Dashboard — Firebase Rebuttal Library

This build keeps the existing dashboard and adds a fully Firebase-synchronized rebuttal system.

## Firestore collections

- `rebuttal_sections` — section/category records shown in the Rebuttals page and Admin Rebuttal Manager.
- `rebuttals` — all rebuttal content, search phrases, keywords, display order and active/critical settings.
- `rebuttal_clicks` — existing agent usage tracking remains unchanged.

On the first successful login, if the two rebuttal collections are empty, the built-in original rebuttals are copied into Firebase automatically. Existing edited Firebase records are not overwritten by the **Sync Original Rebuttals** button; it only restores missing originals.

## Admin use

Open **Admin Panel → Rebuttal Manager** to:

- Add, rename or delete empty sections.
- Add a rebuttal under any section.
- Edit the button label, heading, customer phrases, search keywords, order, active status and critical status.
- Edit the visible rebuttal content directly in the rich content area.
- Add response blocks, duplicate or delete rebuttals.

All changes are stored in Firestore and appear on open agent dashboards through real-time listeners.
