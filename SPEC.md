# SHIELD OS — Master Specification v1.0
**Institution:** Lunara Society
**Classification:** Founder Priority
**Authored by:** Rosario (CEI)
**Date:** 2026-06-30
**Status:** Phase 1 Built & Deployed

---

## WHAT SHIELD OS IS

Shield OS is the private institutional mobile headquarters of Lunara Society.
It is a Progressive Web App (PWA) — not a native APK — for deliberate strategic reasons.

### Why PWA over APK
- Installs to Android home screen, launches fullscreen — indistinguishable from a native app
- No sideloading required — Google's September 2026 restrictions do not apply
- Single codebase, zero build pipeline, deploys via GitHub Pages
- All institutional capabilities (Drive, Gmail, task management, knowledge) are web-native
- Can be upgraded in real time without app store approval

---

## PHASE 1 — DEPLOYED (2026-06-30)

**URL:** https://lunara-society.github.io/shield-os/
**Status:** Live

What exists:
- Sanctum background (same as lunarasociety.com mobile)
- Cinematic boot screen with institutional crest
- Gold wordmark + inscription
- 10-icon engraved bottom navigation
- All 10 nav items link to corresponding lunarasociety.com pages
- Language auto-detection (10 languages)
- Offline capability via service worker
- Install prompt (Android Chrome shows "Add to Home Screen")
- Touch feedback and curtain transition

---

## PHASE 2 — INSTITUTIONAL DASHBOARD

**Objective:** Transform Shield OS from a launcher into a live operational headquarters.

**Screens to build:**
1. Dashboard Home — live feed of institutional activity
2. Drive Browser — read/search institutional knowledge
3. Intelligence Viewer — read Operation BLACK ICE-class reports
4. Team Status — which agents are active, last report, last action
5. Task Board — create/assign/track tasks across team

**Dependencies:**
- Google OAuth integration (Drive API read scope)
- Backend function or edge function to proxy API calls securely
- Session token storage (never localStorage for auth tokens)

**Complexity:** Medium. 3–4 weeks of focused build time.

---

## PHASE 3 — COMMAND LAYER

**Objective:** Full institutional command from the app.

**Features:**
- Secure compose — draft/send institutional emails via Gmail API
- Knowledge search — full-text search across Drive
- Push notifications — when agents complete tasks or new intelligence arrives
- Encrypted notes — device-local, never synced unless explicitly exported
- Incident log — record and classify security or operational events
- Settings — language, display, notification preferences, session management

**Dependencies:**
- Gmail API write scope
- Push notification infrastructure (Firebase Cloud Messaging or Web Push API)
- End-to-end encryption for secure notes (WebCrypto API — native to browser)

**Complexity:** High. 6–8 weeks.

---

## TECHNICAL BLOCKERS (pre-Phase 2)

### Blocker 1: Google OAuth in a PWA
**Why it exists:** Drive and Gmail require user authorization. OAuth redirects need a registered origin.
**Solution:** Register https://lunara-society.github.io/shield-os as an authorized redirect URI in Google Cloud Console.
**Recommendation:** Create a Lunara Society Google Cloud project. Register the OAuth app. Scope: Drive read-only initially, Gmail read-only, then expand.

### Blocker 2: CORS on Google APIs
**Why it exists:** Direct API calls from a PWA work for Drive/Gmail with a valid token, but token exchange must not expose client secrets.
**Solution:** Deploy a lightweight backend function (Base44 backend or Cloudflare Worker) to handle token exchange.
**Recommendation:** Use Base44 backend function — already available, no new infrastructure.

### Blocker 3: Icons (192px and 512px PNG)
**Why it exists:** PWA install prompt requires proper PNG icons at those exact sizes.
**Status:** Currently using JPEG. Needs PNG conversion.
**Solution:** Convert shield-logo.jpg to PNG at required sizes.

### Blocker 4: HTTPS
**Why it exists:** PWAs require HTTPS. Service workers only register on HTTPS.
**Status:** RESOLVED. GitHub Pages provides HTTPS automatically.

---

## ENGINEERING PACKAGE — Phase 2 Entry Requirements

Before Phase 2 development begins, the following must exist:
1. Google Cloud project for Lunara Society (lunarasociety.com verified domain)
2. OAuth 2.0 client ID with authorized redirect to shield-os URL
3. Drive API and Gmail API enabled on the project
4. Base44 backend function: /token-exchange — handles auth code → access token
5. PNG icons at 192px and 512px (converted from shield-logo.jpg)

---

*This document is maintained by Rosario (CEI). Updates require Founder authorization for scope changes.*
