# HireAssist

An HR onboarding dashboard for People Ops teams and new hires. It's a single HTML file with no build step, no backend, and no dependencies.

**Live demo:** https://reyysc.github.io/HireAssist/

![HireAssist dashboard](docs/screenshot-dashboard.png)

> **Demo data only.** Every company, person, email, and document in this repo is fictional ("Acme Corp"). The legal forms are sample templates, not legal advice.

## Features

| Area | What it does |
|---|---|
| **Dashboard** | Headcount, open requisitions, pending documents, 401(k) enrollment, tasks, and announcements |
| **Employees and Onboarding** | Searchable directory and per-hire onboarding progress |
| **Orientation** | A slide deck with a full-screen presenter mode (← → to move, F for fullscreen, Esc to exit). You can upload, edit, and delete slides |
| **Dayforce Setup** | Seven step-by-step guides for new hires, with progress tracking |
| **Sign Forms** | Fill in and e-sign new-hire forms, view all submissions as HR, and print or save as PDF |
| **DocuSign** | Envelope tracking with status tabs, search, a webhook config view, and a simulated incoming event |
| **Payroll, Benefits, Documents** | Plan cards, an FAQ preview, and a document library |
| **Ada (AI assistant)** | A chat widget that answers from the Knowledge Base and logs unanswered questions for HR to fill in |

## Sign-in and roles

| Demo account | Email | Password |
|---|---|---|
| HR admin | `hr@example.com` | `Demo-HR-2026` |
| Candidate | `jordan@example.com` | `Demo-Jordan-2026` |
| Candidate | `sam@example.com` | `Demo-Sam-2026` |

You can also create a new candidate account from the sign-in screen.

- **Candidates** see their own portal: a checklist, Orientation, Dayforce Setup, Sign Forms, Payroll, Benefits, and the Knowledge Base.
- **HR** sees the full dashboard, including **Sign Forms → HR view**, which lists every candidate's submissions.
- **Encryption:** passwords are never stored. PBKDF2-SHA256 (210k rounds) turns each password into an AES-256-GCM key, and that key encrypts the account's own data. Each signed form is also encrypted to HR's public key (ECDH P-256), so only the HR account can read the shared copies.
- **Limits on guessing:** 5 wrong passwords lock that email for 60 seconds. Sessions end after 30 minutes idle or when the tab closes.
- **Phones:** the layout works on phones. The sidebar becomes a slide-out menu, and you can sign forms with your finger.

### Going to production
This is **demo-grade**. Data lives only in each browser's `localStorage`, so accounts don't sync between devices, and HR only sees submissions made on the same device. For real candidates, use server-side auth and a database with per-user access rules. Examples are Supabase Auth with Row Level Security, or Firebase Auth with Firestore rules. Also remove the demo accounts in `Auth.DEMO`.

## Run it locally

```bash
git clone https://github.com/reyySC/HireAssist.git
cd HireAssist
python3 -m http.server 8000   # then open http://localhost:8000
```

Sign-in needs a secure context (https or localhost), so use a local server rather than double-clicking the file.

## How it works

- **One file.** All HTML, CSS, and JavaScript live in `index.html`. Fonts load from Google Fonts.
- **No server.** Accounts and signed forms are saved, encrypted, in the browser's `localStorage` on that device only.
- **Ada** matches questions to Knowledge Base entries by keyword overlap. Nothing is sent to an AI API.
- **Integrations are mocked.** The DocuSign webhook URL and envelopes are sample data. The Dayforce guides contain instructions only.

## Customize for your organization

1. **Company name:** search and replace `Acme Corp` and `example.com`.
2. **Slides:** set `img:"assets/slides/01.png"` (or any image URL) in `orientationSlides`. Slots without an image show a generated placeholder.
3. **Dayforce screenshots:** set `"img"` and `"imgs[].src"` in `dayforceGuides` the same way.
4. **Knowledge Base:** edit the `kb` array. Ada uses the new answers right away.
5. **Legal forms:** replace the sample text in the `sfPrint*` functions with your organization's approved language.

⚠️ Never commit real employee data, internal slides, or login and password formats to a public repo.

## Roadmap ideas

- A real backend (for example Supabase or Firebase) in place of `localStorage`
- A live DocuSign Connect webhook receiver
- An LLM-backed Ada with retrieval over the Knowledge Base

## License

[MIT](LICENSE) © 2026 Shreya Chandra
