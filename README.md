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

## Agent rebuttal screen behavior

- The complete Firebase rebuttal library is always visible at the bottom and every rebuttal can be opened directly.
- Smart Match begins scoring as the agent types, previews the closest answer, and keeps the full library visible.
- Quick Search filters suggestions while the full sectioned library remains available.

## New lead alert (r7)

- Replaces the thin transfer banner with a centered animated square alert.
- Blurs the dashboard behind the alert.
- Uses a Firestore real-time listener to detect each increase in an agent's lead/transfer count while the dashboard tab remains open.
- Displays the total number of new leads and rotates through the agents who gained them.
- Keeps the full list of affected agents visible as chips.
- Every new lead update restarts a full 30-second popup countdown and animates the browser-tab title with `NEW LEAD ON BOARD`.
- The alert can be dismissed with the close button, **LET'S GO**, Escape, or by clicking the blurred background.

Deploy version: `2026-08-02-new-lead-live-30s-r7`
