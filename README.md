🎟️ RV.RA-ICP — Decentralized Event Tickets on Internet Computer

RV.RA-ICP is a decentralized platform for creating events and issuing verifiable tickets on the Internet Computer (ICP).
We combine Internet Identity (II), Plug Wallet, and Rust smart contracts so that tickets are:

🔒 Secure — bound to the user’s ICP principal, tamper-resistant

✅ Verifiable — every ticket includes a QR code for easy validation

🌍 Truly on-chain — core data and business logic live in canisters

🎨 Delightful — modern UI/UX, dark/light themes, animations, responsive design

The project evolved from the earlier “Tickets Partner” (a Telegram-first ticketing flow) into a fully on-chain ICP dapp. We keep Telegram as a parallel channel for growth and onboarding, while ICP is the primary product line.

✨ Key Features
👥 Auth & Wallets

Internet Identity (passwordless, privacy-preserving login)

Plug Wallet integration with ICP balance display

All tickets are bound to the holder’s principal (II/Plug)

🎫 Events

Create events (UI protected by password: 1298)

Edit/Delete your own events

Dual pricing: UAH and ICP (e8s)

Seeded demo events without owner (editable by anyone for onboarding)

🛒 Tickets

Purchase → instant ticket minting with QR code

My Tickets section lists all tickets owned by the current principal

Ticket deletion (simple moderation/demo model)

🎨 Design & UX

Modern event cards, fluid animations and micro-interactions

Ergonomic filters: search, category, date

Dark/Light theme, accessible controls, responsive layout

🤖 AI Helper (Roadmap)

Integrations (e.g., Cofein / coffee.ai) for:

auto-generating event descriptions,

personalized recommendations,

semantic search and ticket summaries.

🧱 Architecture
┌──────────── Frontend (Vite + Vanilla JS + Custom CSS) ────────────┐
│ Auth (II/Plug) · UI/UX · Filters · QR · Theme · Toasts · Modals    │
└──────────────▲──────────────────────────┬───────────────────────────┘
               │                          │
     HttpAgent/Plug Actor                 │ Candid
               │                          │
        ┌──────┴─────────┐        ┌───────▼─────────────────────────┐
        │ Backend Canister│        │ Stable Storage (ic-stable-structures) │
        │ Rust + ic-cdk   │        │ BTreeMap / Cell: events, tickets, ids │
        └─────────────────┘        └───────────────────────────────────────┘


Smart contract (Rust, ic-cdk)

Event: id, metadata, prices (price_uah, price_e8s), created_by

Ticket: event snapshot + qr_code, bound to caller principal

Access rules: only created_by can edit/delete (seeded events are globally editable)

Storage: StableBTreeMap for events/tickets (Candid-encoded), Cell for counters/flags

Frontend

Sign-in with II and/or Plug

Display ICP balance from Plug

Create/Edit/Delete events (password-gated creation)

Buy ticket → show in My Tickets with QR

🧪 Quick Start (Local)
Requirements

Rust with target wasm32-unknown-unknown

DFX SDK (recommended dfx 0.29+)

Node.js (v18/v20) and npm (10+)

Install
# repo root
npm install
cd frontend && npm install

Run locally
dfx start --clean --background
dfx deploy --network local
# the asset canister serves the frontend at http://127.0.0.1:4943

Vite dev server
cd frontend
npm run dev
# Vite will start a local port (typically 5173)


To create an event via UI, use password 1298.

🚀 Deployment

Mainnet: dfx deploy --network ic

Playground: sometimes restricts asset canisters; for stable demos prefer mainnet or local.

🔐 Security Model

Principal-bound: each ticket is linked to the holder’s ICP principal (II/Plug)

Safe auth: Internet Identity is passwordless; Plug signs requests/transactions

Access control

create_event is open in the canister, but the UI requires a password (1298)

update_event / delete_event restricted to created_by (unless seeded without owner)

buy_ticket requires an authenticated caller (II/Plug) — anonymous is rejected

💳 Payments (Roadmap)

The current purchase flow is a payment stub (for UX validation and ticket issuance).
Next steps: real ICP transfer via Plug:

compute e8s → prepare transfer/requestTransfer

store tx hash/receipt for provenance

optional canister-side verification & receipts

🧭 “Tickets Partner” (Telegram branch)

The project started with a Telegram-first approach: purchases, notifications, referrals, multi-crypto (NOWPayments), Monobank, admin tools.
Now ICP dapp is the primary product, while Telegram remains a parallel growth and onboarding channel (newsletter, mini-app, Web2-friendly entry point).

🧰 Tech Stack

Backend: Rust, ic-cdk, ic-stable-structures, Candid

Frontend: Vite, Vanilla JS, custom design system (no heavy UI frameworks)

Auth: Internet Identity, Plug Wallet

Deploy: ICP canisters, dfx

🧩 Candid API (Short)

get_events() -> vec Event

create_event(new: NewEvent) -> Result<Event, text>

update_event(id: u64, new: NewEvent) -> Result<Event, text>

delete_event(id: u64) -> Result<(), text>

buy_ticket(event_id: u64) -> Result<Ticket, text>

get_my_tickets() -> vec Ticket

Models
Event { id: u64, title, date, time, city, category, venue, image, description, price_uah: u64, price_e8s: u64, created_by: opt principal }
Ticket { id: text, event_id: u64, title, date, time, city, venue, category, price_uah: u64, price_e8s: u64, qr_code: text }

🗺️ Roadmap

Real ICP payments via Plug

Organizer roles & quotas (multi-org, moderation, issuance caps)

Secondary market (ticket transfer/listings)

AI features (description generation, similar events, personalized feed)

EN/UA localization, mobile-first refinements, offline caching

Verifier canister for scanning/validation on entry

🏆 Hackathon Alignment (WCHL25-style judging)

We tuned the project to typical Web3 judging dimensions (see DoraHacks/WCHL25 guidance):

Impact & Usefulness — on-chain, fraud-resistant tickets; transparent flows; low-friction II login and Plug wallet

Technical Depth — Rust canister with stable data structures, strict access control, e8s pricing, principal-bound assets, QR issuance

User Experience — polished UI/UX, theming, micro-interactions, accessible patterns (ARIA, keyboard focus), immediate feedback (toasts, skeletons)

Decentralization & Innovation — core logic on ICP, asset hosting in canisters; extensible toward real payments and a secondary market

Sustainability & Community — clear roadmap (roles, AI, payments), dual-channel growth (ICP + Telegram), production-minded architecture

We emphasize end-to-end completeness (from smart contract storage to professional UX) and a credible path to production: real payments, organizer tooling, and a verifiable entry flow.

📎 Testing Notes

Event creation UI requires the password 1298

Seeded events are editable by anyone (demo onboarding)

You must sign in with II or connect Plug to buy a ticket

Plug ICP balance is displayed in the header

After purchase, the ticket appears in My Tickets with a QR

📍 Demo Links (replace with real IDs)

Frontend canister: https://<frontend-canister-id>.icp1.io

Backend canister (Candid UI): https://a4gq6-oaaaa-aaaab-qaa4q-cai.icp0.io/?id=<backend-canister-id>

🤝 Contributing

PRs are welcome—especially around accessibility/UX polish, tests, and feature ideas (organizer roles, quotas, payment receipts, AI ranking).
Please open an issue with context and repro steps when applicable.

📄 License

Proprietary. All rights reserved.

📞 Contacts

Telegram (project): @TicketsPartners

Bot (Telegram branch): @TP_TicketsPartner_bot

Email: citointrues@gmail.com

Location: Kyiv, Ukraine
