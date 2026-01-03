# Powerlifting App — MVP PRD

**Owner:** Dave Richardson  
**Version:** 1.0  
**Last Updated:** 2026-01-03 12:01 UTC

---

## 1. Product Summary
Mobile powerlifting training app with pre-made, %1RM-based templates that mirror the cycles that built Dave’s total. Users select a template, log sessions quickly, and see progress trends. Monetized via subscription.

**Mission:** Get lifters stronger with proven templates and a clean, gym‑friendly UX.

---

## 2. Users & Personas
- **Novice–Intermediate Lifters:** Need structure and clear loading.
- **Competitors:** Need reliable peaking and tapering.
- **Masters Lifters:** Appreciate joint-friendly accessories and predictable volume.
- **Specialists:** Bench-only and Deadlift-only athletes.

---

## 3. Goals & Non-Goals
**Goals**
- Ship 5 launch templates (%1RM-first with fixed accessories).
- Fast, offline-first logging.
- Clear progress visuals: PRs, est. 1RM trends, weekly volume/tonnage.
- Solid subscription entitlements at launch.

**Non-Goals (MVP)**
- Community/chat, custom program builder, live coaching.

---

## 4. Core MVP Features
1. **Template Library** — browse, preview, start block.
2. **Onboarding** — collect 1RMs for S/B/D; allow N/A for single-lift templates.
3. **Auto Load Calculation** — percent of 1RM with rounding: **ceil to nearest 2.5 kg**.
4. **Training Flow** — Today’s session → set logging (load, reps, RPE, notes).
5. **Progress** — PR timeline, est. 1RM trend, weekly volume/tonnage.
6. **Subscription** — Paywall + entitlements; Premium unlocks all templates & analytics.
7. **Offline-first** — queue logs locally; sync when online (last-write-wins).

---

## 5. Non-Functional Requirements
- **Performance:** Session screen loads <300 ms cached; set log <100 ms.
- **Reliability:** Conflict resolution via last-write-wins; audit trail for overwrites.
- **Accessibility:** Dark mode, high contrast, large tap targets, haptics on set complete.
- **Privacy:** GDPR-compliant; export/delete account.

---

## 6. Success Metrics
- Onboarding completion ≥ 70%
- Day-7 retention ≥ 35% (Premium ≥ 45%)
- Paywall conversion ≥ 3–6%
- Crash-free sessions ≥ 99.5%

---

## 7. Navigation & Screens
**Bottom Tabs**
- **Home** — Readiness check-in, today’s session, quick start.
- **Train** — Block → Week → Session → Log screen.
- **Templates** — Browse, filter, preview, start.
- **Progress** — PRs, est. 1RM, volume charts.
- **Profile** — 1RMs, preferences (kg/lb), subscription.

**Key Flows**
- Onboarding → Suggested templates → Preview → Start block.
- Session logging → PR detection → Progress updates.
- Paywall exposure on template gating & analytics tabs.

---

## 8. Data Model (high level)
- **User:** id, email, preferences, one_rms, created_at
- **Subscription:** provider, product_id, status, renews_at
- **Template:** metadata, rules (rounding, %1RM), weeks/days/exercises
- **Block:** user_id, template_id, start/end, scaling overrides
- **Session:** planned sets per day
- **ExerciseSet:** planned vs actual, RPE, notes
- **LiftLog:** readiness, metrics, timestamps

---

## 9. API Endpoints (MVP)
- `POST /auth/signup` | `POST /auth/login`
- `GET /templates` — list metadata
- `GET /templates/{id}` — full JSON
- `POST /blocks` — assign template to user
- `GET /blocks/{id}/sessions` — retrieve planned sessions
- `POST /logs` — persist session/set logs
- `GET /progress/{user_id}` — PRs and trends
- `POST /subscriptions/entitlements` — webhook to update entitlement state

---

## 10. Analytics Events
- `onboarding_complete`, `template_preview`, `start_block`
- `session_start`, `set_logged`, `session_complete`
- `pr_achieved`, `paywall_view`, `purchase`, `trial_start`

Funnels: onboarding → start_block → first log → day 7 retention → purchase.

---

## 11. Load Calculation & Rounding
```
load_kg = ceil((user_1rm_kg * percent_1rm) / 2.5) * 2.5
```
- Display rounding badge if rounded up.
- Internally store in kg; UI can convert to lb.

---

## 12. Roadmap (12 Weeks)
- **W1–2:** Implement data model, import templates, build onboarding & template preview.
- **W3–6:** Train/Log flows, offline cache, rounding & plate math helper (optional).
- **W7–8:** Subscriptions (RevenueCat), analytics, progress charts.
- **W9–10:** Closed beta (20–50 lifters), polish UX & performance.
- **W11–12:** App Store/Play Store submissions, marketing assets, launch & monitor.

---

## 13. Risks & Mitigations
- **App Store review delays:** Submit early with clean privacy policy.
- **Data loss offline:** Robust queue + retries; conflict banner on overwrite.
- **Plate rounding confusion:** Show computed load and optional plate breakdown (phase 2).

---

## 14. Out of Scope (for MVP)
- Custom program builder, messaging/community, coach dashboards.

