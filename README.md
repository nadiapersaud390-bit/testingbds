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

## Bulk attendance update

Open **Admin Panel → Daily Attendance** to:

- Choose any attendance date.
- Filter by all agents, team or current attendance status.
- Select individual agents or use **Select Visible**.
- Mark all selected agents as Present, Late, Absent, Sick, Vacation, Personal Out, Day Off or Holiday.
- Add an optional note and save every selected record to Firestore in one action.

Each agent/date is stored in `attendance/{agentId}_{YYYY-MM-DD}`. A summary audit of every bulk action is stored in `attendance_bulk_updates`.

## Firebase administrator account

This build seeds a Firebase-backed administrator document at `admin_accounts/rose` the first time the dashboard connects. The password is verified using a SHA-256 hash; the plain password is not stored in Firestore. Successful Rose logins update `admin_accounts/rose` and create an audit record in `admin_login_history`.

## Rose-only missing-from-stats review (r11)

Open **Admin Panel → Missing From Stats** while logged in as Rose.

- Each Agent Stats CSV upload is saved in Firestore as the normal `reports/{date}` record and a compact `stats_presence/{date}` snapshot.
- The snapshot stores the agent IDs and normalized names found in that upload.
- Rose can choose how many consecutive uploads an agent must be absent from before the profile is flagged: 2, 3, 5, 7 or 10 uploads.
- The list shows the missing-upload streak, last report in which the agent appeared, and how many saved uploads contained the agent.
- Existing report history is also read directly, so reports uploaded before r11 are included where available.
- Rose can review, mark inactive, reactivate, or archive and delete a flagged profile. Deletions are backed up in `deleted_agent_profiles`.

Deploy version: `2026-08-02-rose-missing-stats-r11`

## Rose archive manager and dark-only interface (r13)

The Rose-only **Missing From Stats** area now includes two internal views: **Missing Profiles** and **Archive**.

- Missing profiles can be selected individually, by visible page, or all matching filters.
- Page size can be set to 10, 25, 50 or all profiles.
- Bulk actions include **Mark Inactive** and **Archive Selected**.
- Archiving writes a complete profile copy to `archived_agent_profiles/{agentId}` and removes only the live `roster/{agentId}` document.
- Attendance, reports, coaching, rebuttal use and other history are not deleted.
- The Archive view supports search, team filtering, pagination, multi-select, bulk restore and optional permanent deletion of archive copies.
- Legacy backups in `deleted_agent_profiles` also appear in the Archive view and can be restored.
- Archive, restore and permanent-delete actions are audited in `unused_profile_actions`.

The light-theme toggle, saved appearance preference and light-theme CSS overrides have been removed. The login page, agent dashboard and both admin panels now use the dark interface only. Native form controls are also forced to the dark browser color scheme.

Deploy version: `2026-08-03-rose-archive-dark-r13`
