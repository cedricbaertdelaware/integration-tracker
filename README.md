# Integration Tracker

**Integration Dev Portal** — A lightweight web application for managing SAP integration development projects with distributed teams.

Built as a single HTML file with zero build tools, zero dependencies on npm/node, and zero server-side code. Deploy anywhere that serves static files.

---

## What it does

Tracks the full lifecycle of integration interface development across multiple customer projects:

- **Interfaces** — Register, track status (spec → dev → review → done), assign architects and developers
- **Time Logging** — Log hours per person per interface per phase, with role-based visibility
- **Open Points** — Track decisions, blockers, and questions with priority, owner, and due dates
- **Validation Checklists** — Configurable per-interface sign-off checklist (customizable by admin)
- **dIF Templates** — Library of delaware Integration Framework templates (TF000–TF009.2)
- **User Management** — Role-based access control with admin, architect, developer, and viewer roles
- **Multi-project** — Switch between customer projects instantly; all data scoped per project

---

## Features

### Core
- Multi-project support with color-coded sidebar
- Role-based access control (Admin / Architect / Developer / Viewer / Custom)
- Granular per-view read/write permissions
- SHA-256 password hashing (never stored in plain text)
- Access request workflow (request → admin approval → account creation)

### Data Management
- Import interfaces, time logs, and open points from CSV/Excel (.xlsx/.xls)
- Export time logs to CSV (with active filters applied)
- Export full project summary report (CSV with all interfaces, hours, open points)
- Downloadable import templates with example data and valid value references
- Bulk select + delete on interfaces and time logs (with undo)

### UX
- Collapsible sidebar (icon-only mode for mobile)
- Mobile responsive layout
- Column-header sorting (click to cycle: none → ascending → descending)
- Filter bars on all tabs (status, priority, person, phase, interface, text search)
- @mention users in any description field (type `@` in rich text editors)
- User autocomplete on Owner, Spec Author, and Developer fields
- Rich text editor with bold, italic, lists, color highlights, and highlight removal
- Drag-and-drop interface reordering
- Toast notifications for all save/delete/approve actions
- Custom confirm dialogs (no browser popups)
- Animated dashboard counters and hours-per-person bar chart
- 5-second undo on all delete actions
- Global search across all projects (interfaces, open points, time entries)
- Session timeout after 1 hour of inactivity
- Loading bar during Firebase operations
- Smooth view transitions and modal animations

### Persistence
- **Online mode**: Firebase Firestore with real-time sync across all users/devices
- **Offline fallback**: Automatic localStorage fallback when Firebase is unavailable
- Dual-write: every save goes to both Firebase and localStorage for resilience

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML + CSS + JavaScript (single file) |
| Database | Firebase Firestore (real-time sync) |
| Fallback | localStorage |
| Hosting | GitHub Pages |
| Excel parsing | SheetJS (CDN) |
| Fonts | IBM Plex Sans + IBM Plex Mono (Google Fonts) |

No build step. No framework. No bundler. No npm.

---

## Deployment

### GitHub Pages (current)

1. Create a repository on GitHub
2. Upload `sap-integration-tracker.html` to the repo
3. Go to **Settings → Pages → Source: main branch**
4. Access at `https://{username}.github.io/{repo}/sap-integration-tracker.html`

### Any static hosting

The file is fully self-contained. Upload it to any web server, S3 bucket, Azure Blob Storage, or internal file share accessible via HTTP.

---

## Firebase Setup

The app uses Firebase Firestore for persistent, shared data. The Firebase project is already configured in the source code.

**Project**: `integration-tracker-90e23`
**Firestore document**: `appdata/main` (single document holds all state)

To use your own Firebase project:
1. Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable Firestore in the project
3. Replace the `firebaseConfig` object in the HTML file with your project's config
4. Update Firestore security rules as needed

---

## User Roles

| Role | Dashboard | Interfaces | Time Log | Open Points | Validation | Templates | Users |
|------|-----------|------------|----------|-------------|------------|-----------|-------|
| **Admin** | R/W | R/W | R/W | R/W | R/W | R/W | R/W |
| **Architect** | R/W | R/W | R/W | R/W | R/W | R | — |
| **Developer** | R | R | R/W | R/W | R | — | — |
| **Viewer** | R | R | R | R | R | — | — |
| **Custom** | Configurable per view | | | | | | |

Admin-only actions: delete project, delete interfaces/time/open points, manage users, edit validation checklist definition.

---

## Default Admin Account

On first run, the app creates an admin account:

- **Email**: `cedric.baert@delaware.pro`
- **Password**: `Tie2eiha3759`

Change this password after first login via Users → Edit.

---

## Validation Checklist (Defaults)

The admin-configurable validation checklist ships with these defaults:

1. Specification finalized by Architect
2. Spec reviewed by Functional / Customer
3. Development complete
4. Happy flow testing passed
5. Error handling flow tested
6. Error handling added (dead letter / alerts)
7. Monitoring keys configured in dIF dashboard
8. Performance / volume test OK
9. Customer sign-off received
10. Documentation added

Admins can add, edit, remove, and reorder checks via the Validation tab.

---

## Import Templates

Each importable entity has a downloadable CSV template:

| Entity | Template Columns |
|--------|-----------------|
| **Interfaces** | Interface ID, Interface Name, Source System, Target System, dIF Template, Status, Priority, Spec Author, Developer |
| **Time Logs** | Date, Person, Role, Interface ID, Phase, Hours, Description |
| **Open Points** | Interface ID, Priority, Description, Owner, Due Date |

Templates include example rows, valid value references, and the current project's interface list. Supports `.csv`, `.xlsx`, and `.xls` files.

---

## File Structure

```
sap-integration-tracker.html    # The entire application (~3,300 lines)
README.md                       # This file
```

That's it. One file.

---

## Browser Support

Tested on:
- Chrome (latest)
- Safari (latest)
- Firefox (latest)
- Mobile Safari (iOS)
- Chrome Mobile (Android)

Requires JavaScript enabled. Uses Web Crypto API for password hashing (supported in all modern browsers).

---

## License

Internal tool — delaware consulting. Not for external distribution.
