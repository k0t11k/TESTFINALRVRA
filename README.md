🎟️ RV.RA-ICP — Decentralized Tickets on Internet Computer (ICP)

RV.RA-ICP is an on-chain platform for creating and selling event tickets built on ICP.
We combine Internet Identity (II), Plug Wallet, and Rust smart contracts so tickets are:

🔒 Secure — bound to the user’s ICP principal; forgery-proof.

✅ Verifiable — each ticket carries a QR code for validation.

🌍 Truly on-chain — data and business logic live in ICP canisters.

🎨 Modern — refined UI/UX, dark/light themes, animations, responsive design.

The project evolved from the Telegram-based Tickets Partner MVP. The ICP dapp is now the primary product; the Telegram branch remains a high-velocity onboarding and growth channel.

✨ Features
👥 Auth & Wallets

Internet Identity — passwordless login.

Plug Wallet — one-click connection and ICP balance display.

All tickets are strictly tied to the user’s principal (II or Plug).

🎫 Events

Create events protected by a UI password 1298.

Edit/Delete your own events.

Dual pricing: UAH and ICP (e8s).

Seeded demo events (no owner) are editable by anyone for onboarding.

🛒 Tickets

Purchase → immediate ticket issuance with QR code.

My Tickets lists all tickets for the current principal.

Ticket deletion (simple moderation/demo model).

🎨 Design & UX

Clean cards, smooth micro-interactions, polished animations.

Smart filters: search, category, date.

Dark/Light theme, responsive layout, accessibility (ARIA, focus states).

🤖 AI Integrations (Roadmap)

Connect Cofein/coffee.ai: generate descriptions, personalize recommendations, enable semantic search.

🧱 Architecture
┌──────────── Frontend (Vite + Vanilla JS + Custom CSS) ────────────┐
│ Auth (II/Plug) · UI/UX · Filters · QR · Themes · Toasts · Modals  │
└──────────────▲──────────────────────────┬──────────────────────────┘
               │                          │
     HttpAgent/Plug Actor                 │ Candid
               │                          │
        ┌──────┴─────────┐        ┌───────▼─────────────────────────┐
        │ Canister (Rust) │        │ Stable Storage (BTreeMap / Cell)│
        │ ic-cdk + Candid │        │ Events, tickets, counters, flags │
        └─────────────────┘        └──────────────────────────────────┘


Smart Contract (Rust, ic-cdk)

Event: id, metadata, price_uah, price_e8s, created_by (opt principal).

Ticket: snapshot of event fields + qr_code, bound to the caller.

Access control: only created_by can update/delete; seeded events are open to all.

Storage: StableBTreeMap (Candid-encoded), Cell for counters/flags.

Frontend

Login via II and/or Plug, live ICP balance in header.

Create/Update/Delete events (UI password 1298 for creation).

Purchase → ticket appears in My Tickets with QR.

🧪 Quick Start (Local)

Requirements

Rust target: wasm32-unknown-unknown

DFX SDK (recommended dfx 0.29+)

Node.js (v18/v20), npm (10+)

Install

# in repo root
npm install
cd frontend && npm install


Run

dfx start --clean --background
dfx deploy --network local
# the asset canister serves the frontend at http://127.0.0.1:4943


Vite Dev

cd frontend
npm run dev
# typically on port 5173


To create an event in the UI, enter password 1298.

🚀 Deployment

Mainnet: dfx deploy --network ic

Note: some playgrounds restrict asset canisters. For stable demos, use mainnet or local.

🔐 Security Model

Principal-bound: each ticket is owned by a specific principal.

Secure Authentication: II (passwordless), Plug (signed requests/transfers).

Access Control

create_event is open at canister level (UI enforces password 1298).

update_event / delete_event — only the event’s created_by (seeded events are open).

buy_ticket rejects anonymous (requires II/Plug).

💳 Payments (Roadmap)

Current build uses a stub UX (purchase + ticket issuance). Next steps:

Real ICP transfers via Plug (requestTransfer) using price_e8s.

Persist tx hash/receipts and link them to tickets.

On-chain/contract-side verification of receipts.

🧭 Telegram Branch “Tickets Partner”

Legacy branch with purchases, notifications, referral program, NOWPayments/Monobank, and admin panel.
Primary product is the ICP dapp; Telegram remains an onboarding/retention channel (mini-app, broadcasts).

🧰 Tech Stack

Backend: Rust, ic-cdk, ic-stable-structures, Candid

Frontend: Vite, Vanilla JS, custom design system

Auth: Internet Identity, Plug Wallet

Deploy: ICP canisters, dfx

🧩 Compact API (Candid)

get_events() -> vec Event

create_event(new: NewEvent) -> Result<Event, text>

update_event(id: u64, new: NewEvent) -> Result<Event, text>

delete_event(id: u64) -> Result<(), text>

buy_ticket(event_id: u64) -> Result<Ticket, text>

get_my_tickets() -> vec Ticket

Event { id: u64, title, date, time, city, category, venue, image, description, price_uah: u64, price_e8s: u64, created_by: opt principal }
Ticket { id: text, event_id: u64, title, date, time, city, venue, category, price_uah: u64, price_e8s: u64, qr_code: text }

🗺️ Roadmap

Real ICP payments via Plug with receipt storage.

Organizer roles, capacities/quotas, moderation.

Secondary market / ticket transfers (with royalties).

AI recommendations/generation/search.

EN/UA localization, mobile-first hardening, offline cache.

Dedicated verifier canister (scan/offline list).

📽️ Presentation (10 Slides)

Slide 1 — Title
RV.RA-ICP: Decentralized Ticketing Platform on ICP
Subtitle: “Transforming ticketing with blockchain for security, transparency, and usability.”
Assets: logo, “Team Tickets Partner (RV.RA-ICP)”, “August 2025”, “ICP WCHL25 Hackathon”.
Visual: concert background with blockchain elements (chains/NFT tickets).
Speaker: greet, then state the core pain points we solve.

Slide 2 — Market Problems
Fraud/counterfeits, hidden fees (up to 30%), centralization, low organizer engagement.
Include stats/citations directly on the slide (e.g., industry reports).
Speaker: “traditional platforms are vulnerable — users lose trust, organizers lose revenue.”

Slide 3 — Our Solution
ICP dapp + Telegram integration; NFT tickets; low fees on ICP; AI personalization.
Diagram “Problem → Solution”, plus dapp/bot screenshots.
Speaker: “we started with Telegram MVP; ICP web app brings global scale.”

Slide 4 — Why ICP
Decentralization (canister hosting), scale/cost, security, integrations (II/Plug).
Table “ICP vs Cloud” (cost/speed/security).
Speaker: “ICP gives resilience and OPEX savings.”

Slide 5 — Plug Wallet & II
Plug: seamless ICP payments; II: passwordless auth and privacy.
Flow: user → wallet → ICP.
Speaker: “we reduce friction and boost conversion.”

Slide 6 — NFT Tickets
Unique NFT + QR; anti-fraud; secondary market + royalties (5–10%).
Speaker: “tickets become assets/souvenirs — stronger engagement and new revenue.”

Slide 7 — Ecosystem
Telegram → ICP Site → iOS/Android; multi-channel = better retention/reach.
Speaker: “Telegram for virality, ICP for scale/decentralization.”

Slide 8 — Business Model
Revenues: 5–10% fee, NFT royalties, premium AI analytics.
Low OPEX thanks to ICP. Target: UA + global Web3.
Speaker: “sustainable unit economics and scalable model.”

Slide 9 — Value & Roadmap
Users: security, convenience, AI. Organizers: analytics, low fees, global reach.
Roadmap: Q3’25 ICP site; Q4 mobile; 2026 partnerships/festivals.

Slide 10 — Close/CTA
“We solve ticketing with ICP/Plug/NFT.”
CTA: citointrues@gmail.com
 | Telegram: @TP_TicketsPartner_bot.
Speaker: “let’s build the future of ticketing together — Q&A.”

We included the key narrative in this README so judges can cross-check code, docs, presentation, and demo instantly.

🏆 Alignment with WCHL25 Judging Criteria (DoraHacks)

Uniqueness

ICP-native ticketing with principal-bound tickets, II/Plug, dual pricing (UAH + ICP), and fully on-chain storage.

Smooth evolution from Telegram MVP to a complete ICP product.

Revenue Model

Fees 5–10% (below market), NFT royalties, premium AI features.

Low OPEX via canister hosting.

Full-Stack Development

End-to-end: frontend (Vite/JS), canister (Rust), stable storage, auth, purchase → ticket issuance with QR.

Local and mainnet deployment via dfx.

Presentation Quality

Clear story: problems → solution → ICP → UX → business → roadmap → CTA.

Prepared speaker notes for each slide.

Utility & Value

Real industry pain: fraud, high fees, poor UX.

ICP provides security, transparency, portability, and new value (NFT).

Demo Video Quality (recording plan)

90–120s: II/Plug login → balance → filters → create event (password) → purchase → My Tickets + QR → (optional) Candid UI.

Short architecture bumper and CTA.

Code Quality

Rust canister: ic-cdk, ic-stable-structures, typed Candid, access control.

Frontend: clean modules, defensive UI, toasts/modals, accessibility.

Documentation (covered here)

Intro, architecture, local build/deploy, ICP features (II/Plug, canisters), demo links (placeholders), challenges & next steps.

Technical Difficulty

Stable storage, Candid serialization, principal-bound assets, II/Plug integration.

Next: verified payments, verifier canister, secondary market (higher complexity).

Bonus Points (we cover/plan)

Architecture diagram — included.

User-flow — covered in presentation/demo.

PocketIC tests — on the roadmap (unit/prop tests for business rules).

Frontend on ICP — yes.

Exceptional UX — custom design, themes, animations, accessibility.

We emphasize end-to-end completeness, UX polish, and a credible evolution path (Plug payments, roles, secondary market, AI) to maximize scoring and advance to the next round.

📍 Demo Links (replace with real ones)

Frontend canister: https://<frontend-canister-id>.icp1.io

Backend (Candid UI): https://a4gq6-oaaaa-aaaab-qaa4q-cai.icp0.io/?id=<backend-canister-id>

📞 Contacts

Telegram (project): (https://t.me/Top_Project001)



Location: Kyiv, Ukraine
