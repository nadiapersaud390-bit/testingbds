# BDS Dashboard — Firebase-Only Build

Firebase/Firestore is the only database used by this website.

## Firestore data

- `roster/{userId}` — agent profile, team, shift, lunch, break and status
- `attendance/{userId}_{YYYY-MM-DD}` — one daily attendance document per agent
- `sessions/{userId}_{YYYY-MM-DD}` — website login session information
- `reports/{YYYY-MM-DD}` — uploaded performance reports
- Existing Firestore collections for coaching, monitoring, targets, quotes, leads and settings remain unchanged

Attendance is created when an agent signs in through the website. Repeated logins update the same daily Firestore attendance document and increment its login count. Admin attendance changes also write directly to the same Firestore document.

The Monthly Attendance page reads the selected month directly from Firestore in real time. Administrators can select a month and team, then export the displayed Firebase attendance as a CSV file. The CSV is generated in the browser and is not stored in another service.

Firebase Anonymous Authentication must remain enabled, and Firestore rules must allow authenticated dashboard users to access the required collections.
