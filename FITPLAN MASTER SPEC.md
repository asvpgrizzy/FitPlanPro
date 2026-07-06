# FITPLAN PRO — MASTER SPEC & PROJECT STATE
**Owner:** Wizzop (GitHub: asvpgrizzy) · **Updated:** July 6, 2026 · **STATUS: v3.0 BUILT — ready to deploy**

---

## 1. USER PROFILE (drives all app logic)
- 5'11", 265 lbs → goal 185–200 lbs
- Gym Mon–Fri 10:00–11:10 AM (flexible-day engine will adapt around misses)
- 16:8 intermittent fasting: eat 12–8 PM (meals ~12:00 / 3:30 / 7:00)
- Nutrition targets: ~1,900–2,100 kcal · 190–220g protein
- Fish: salmon + tilapia only
- Health context: lowering LDL, managing type 2 diabetes
- Starting lifts: UNKNOWN → app estimates from bodyweight + experience level
- Monetization path: app + guides tied to YouTube fitness content under Wizzop AI Academy brand

## 2. DEPLOYMENT STATE
- **Live:** v2.1 single-file PWA → GitHub Pages, repo `asvpgrizzy/FitPlanPro`, URL asvpgrizzy.github.io/FitPlanPro, on iPhone 15 Pro Max home screen
- **Update workflow:** delete old index.html in repo → Upload files → Commit → wait ~2 min → hard refresh
- **v2.1 features:** 115 exercises (equipment-tagged), equipment-aware swaps, YouTube search-link videos (embed fix), 12 meals, rest timer, streaks, phases, localStorage ("fp21"), no service worker (prevents stale-cache bug)
- **Companion files in outputs:** health_plan.docx, meal_plan.docx, meal_plan_full.docx, workout_plan.docx, supplements_guide.docx, food_is_medicine.docx, fitplan_app.jsx (React version, parked)

## 3. LOCKED v3 DECISIONS (confirmed July 5)
1. **Platform:** Stay single-file PWA on GitHub Pages until all kinks worked out → THEN port to React Native + Expo + Supabase + Apple Health (aligns with Jarvis Phase 13 App Store PWA plan)
2. **Scheduling:** Flexible days — no fixed Mon–Fri split. Recovery engine calculates the right workout for whatever day you show up
3. **Session length:** Onboarding choice of 45 / 60 / 70+ minutes → controls exercise count (~5–6 / 7–8 / 9–10)
4. **Meals:** Cleaner UI + more built-in meals + USER CUSTOM RECIPE INPUT (name, macros, recipe steps, saved to localStorage)
5. **Starting weights:** Estimated from bodyweight (265) + experience level via standard strength-standard ratios; refined by actual logs after that

## 4. v3 ARCHITECTURE — FIVE PILLARS
**P1 Onboarding (first launch):** goal → experience → equipment → session length → days/week. Editable in Settings.
**P2 Muscle Recovery Engine (core Fitbod mechanic):** per-muscle freshness 0–100%; logged work drops primary muscles to ~20%, secondaries to ~50%; recovery over 48–72h based on volume; generator builds each session from the FRESHEST muscle groups; flexible-day aware (missed days = more recovery = fuller workout).
**P3 Exercise Library ~180:** every exercise gets primary + secondary muscle mapping (e.g., bench: chest primary; triceps, front delts secondary). New tracked groups: calves, forearms, lower back. Keeps equipment tags + cues + YouTube search links.
**P4 Set Logging + Progression:** tap set → log weight + actual reps; last-weights pre-filled; hit top of rep range on all sets → suggest +5 lbs (upper) / +10 lbs (lower).
**P5 Adaptive Starting Weights:** experience-level ratios of bodyweight per lift pattern, then learn from logs.

**Deferred to native port:** Apple Health/Google Fit, Supabase sync, in-app Claude AI coach.

## 5. BUILD SEQUENCE — ALL COMPLETE (July 6, 2026)
1. ☑ Exercise DB: **736 movements** (open-source public-domain dataset, GitHub) + 60 warm-up stretches — full primary/secondary muscle mapping, difficulty levels, 10 equipment types
2. ☑ Recovery engine: 13 muscle groups, freshness 0–100%, per-set fatigue (primary −16/set, secondary −7/set), full recovery in ~60h, flexible-day template scorer (Push/Pull/Legs/Upper/Full auto-selected by freshness)
3. ☑ Onboarding: goal → experience → session length (45/60/70) → days/week; editable via Settings
4. ☑ Set logging: weight+reps per set, last-weight prefill, auto progression (+5 upper / +10 lower when all sets hit top of rep range), starting weights estimated from 265 lbs bodyweight × experience ratios
5. ☑ Meals v2: 20 built-in meals, custom recipe creator (name/macros/recipe, saved locally), My Recipes filter, cleaner compact UI
6. ☑ Assembled + node syntax check + full smoke test (generate → log → finish → fatigue → re-generate avoids tired muscles → 30h recovery tick → equipment-constraint PASS). **283 KB single file at outputs/index.html**

## 5b. v3.0 SMOKE TEST RESULTS
- Push Day generated (8 ex, 2 warmups, weight suggestions e.g. DB bench 75 lbs @ intermediate/265)
- After finish: chest/shoulders 20%, triceps/core 10%, all others 100% — correct
- Immediate second generate chose Pull Day, zero exercise repeats — correct
- +30h: chest 20%→70% — correct
- Dumbbell+bodyweight-only constraint honored — PASS
- v2.1 localStorage (fp21) migrates streak/logs/meals into fp3 automatically

## 5d. KNOWN FIX — HOME SCREEN ICON (July 6)
Issue: home-screen icon showed text on black (iOS screenshots the page when no icon file exists). Fix shipped: index.html now references apple-touch-icon; icon-180.png + icon-512.png (dumbbell logo) must be uploaded to the repo ROOT alongside index.html. Then delete + re-add to home screen.

## 5c. DEPLOY (same workflow)
GitHub repo asvpgrizzy/FitPlanPro → delete old index.html → Upload files → Commit → ~2 min → hard-refresh https://asvpgrizzy.github.io/FitPlanPro (first launch shows onboarding)

## 6. KNOWN TECHNICAL PATTERNS (environment)
- Large files: bash heredoc to temp files → Node build script assembling final HTML (worked for v2.1: gen.js → build_html.js → index.html)
- create_file tool intermittently fails validation → use bash heredoc
- No service worker in PWA (caused v1 "loading but not updating" bug)
- localStorage key: migrate "fp21" → "fp3" with import of old logs/streak
