# Tide.

**Pure thought. Instant connection. Zero noise.**

Tide is a fast, calm, text-first social network crafted from the ground up for people who value signal over noise. Designed with an unwavering focus on typography, fluid motion, and human connection, Tide strips away algorithmic feeds and video clutter to bring conversation back to center stage.

I built Tide as a modern, dual-platform experience: it runs effortlessly on the web, and compiles down to a lightning-fast native Android app built with SvelteKit 5 Runes, TypeScript, Material 3, and Capacitor 8—powered end-to-end by Supabase.

---

## Experience the App

### 🌊 Fluid, Real-Time Social Feed
* **Text-First Craftsmanship**: Clean typography and deep contrast built on curated Material 3 tonal palettes (Deep Teal, Quiet Slate, Warm Terracotta).
* **Live Interactions**: Posts, replies, and likes stream to your device instantly via Supabase Realtime websockets.
* **Smart Link Previews**: Automatically extracts open-graph metadata and renders beautiful preview cards (with built-in LAN/private network isolation for local development).

### 💬 Instant Direct Messaging
* **Real-Time Conversations**: Chat with anyone on the network with instant delivery and unread indicator badges.
* **Participant State Tracking**: Live read receipts and conversation updates managed without polling.

### 🔔 True Background Native Push Notifications
Never miss a message or interaction, whether your phone is in your hand or resting on your desk.
* **Foreground & Background Alerts**: High-importance native Android pop-ups (`importance: 5`) with sound, vibration, and haptic feedback.
* **Closed-App Delivery**: Integrated Firebase Cloud Messaging (FCM HTTP v1) combined with automated Supabase Edge Functions (`send-push`) and Postgres database webhooks (`pg_net`). When Tide is swiped away or completely asleep, background notifications arrive seamlessly with instant deep links into the exact post or chat.

### 🛡️ Social Control & Privacy
* **Public & Private Profiles**: Choose who sees your thoughts. Manage incoming follow requests with one tap.
* **Full Account Mastery**: Custom avatars stored in Supabase Storage, bio customization, and immediate account settings.

---

## Architecture at a Glance

Tide uses a unified codebase engineered to run flawlessly across web edge servers and native mobile silicon.

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Framework** | SvelteKit 2 (Svelte 5 Runes) | Reactive state management, fluid animations, and strict TypeScript compilation |
| **Design System** | Vanilla CSS + Material 3 Tokens | Accessible contrast ratios, dark mode elegance, and custom design tokens |
| **Native Shell** | Capacitor 8 (Android) | Hardware-accelerated WebView, native status bars, haptics, and system tray alerts |
| **Backend Core** | Supabase Postgres & Auth | Row Level Security (RLS) policies, database triggers, and real-time websockets |
| **Push Engine** | Firebase Cloud Messaging (FCM v1) | Secure background notification delivery orchestrated via Supabase Edge Functions |
| **Web Routing** | Vercel Edge (`adapter-vercel`) | Dynamic SSR server routes and SEO link preview endpoints |

---

## Repository Structure

```text
src/
├── lib/
│   ├── components/       # Reusable UI architecture (feed, layout, profile, messages, common)
│   ├── services/         # Domain-driven Supabase queries, mutations, and push managers
│   ├── stores/           # Svelte 5 rune-based global state (session, badges, snackbars)
│   ├── supabase/         # Typed database client and storage upload helpers
│   ├── types/            # Strict TypeScript interfaces and generated database schema types
│   └── utils/            # Native deep links, date formatters, and link preview guards
├── routes/
│   ├── (auth)/           # Authentication flows (sign-in, registration, password recovery)
│   ├── (app)/            # Core application layout (feed, messages, compose, profile, settings)
│   └── api/link-preview/ # Backend endpoint for fetching metadata from shared URLs
supabase/
├── migrations/           # Automated SQL schemas, RLS policies, real-time publications, & webhooks
└── functions/send-push/  # Deno Edge Function executing secure FCM v1 background push
android/                  # Native Android Capacitor studio project & build configuration
```

---

## Getting Started

### 1. Local Development
```bash
# Clone and install dependencies
pnpm install

# Start the local development server at http://localhost:5173
pnpm run dev
```

### 2. Database & Backend Setup
1. Create a Supabase project at [supabase.com](https://supabase.com).
2. Execute all SQL files located in `supabase/migrations/` inside your Supabase SQL Editor.
3. Copy `.env.example` to `.env` and insert your Supabase project URL and anon public key.
4. Deploy the background notification edge function:
   ```bash
   supabase functions deploy send-push --no-verify-jwt
   ```

### 3. Native Android Build
```bash
# Verify Svelte 5 compilation and type safety
pnpm run check

# Sync web build assets into the Android native shell
pnpm run cap:sync

# Launch on connected Android device or emulator
pnpm run cap:run
```

---

## Automated CI/CD & Security

Tide features a complete cloud release pipeline (`.github/workflows/build.yml`). Every commit to `main` automatically:
1. Validates strict TypeScript diagnostics via `svelte-check`.
2. Compiles production web bundles.
3. Securely injects encrypted build secrets (`GOOGLE_SERVICES_JSON`).
4. Compiles signed native Android **Debug** and **Release APKs**.
5. Publishes automated release tags ready for immediate installation.

*Crafted with precision.*
