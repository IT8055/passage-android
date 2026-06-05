# Publishing "Passage" to Google Play

Everything below is what *you* need to do in the browser. The app itself is built, signed,
and tested — nothing more to code.

---

## 0. What's already done (artifacts on disk)

| Item | Location |
|---|---|
| **Signed release AAB** (upload this) | `android/app/build/outputs/bundle/release/app-release.aab` |
| Upload keystore (BACK THIS UP) | `android/app/upload-keystore.jks` |
| Keystore password | in your password manager (shown once during the build) |
| App icon (512×512) | `store-assets/icon-512.png` |
| Phone screenshots (1080×2400) | `store-assets/screenshots/` |
| Privacy policy page | `privacy-policy.html` (host it — see step 1) |

- **App ID:** `eu.walkerjones.passage` (permanent)
- **Version:** versionCode 1, versionName 1.0
- **Target SDK:** 35 (meets Play's current requirement)

> ⚠️ **Back up `upload-keystore.jks` and its password.** You need both to ship every future
> update. (If lost, Google can reset the upload key, but it takes days — avoid the hassle.)

---

## 1. Host the privacy policy (required)

Play requires a public privacy-policy URL for any app, and especially health apps.

**Upload the updated `privacy-policy.html`** (it now says "Passage") to:
```
https://walker-jones.eu/androidapps/passage/privacy-policy.html
```
Paste this exact URL into the store listing (step 4) and the Data Safety / App content forms (step 6).

---

## 2. Create your Google Play Developer account (one-time, $25)

1. Go to <https://play.google.com/console> and sign in with the Google account you want to own
   the app (likely garyleewalkerjones@gmail.com).
2. Pay the **one-time $25** registration fee.
3. Choose account type **Personal** (unless you have a registered business).
4. Google now requires ID verification for new personal accounts — have a photo ID ready. This
   can take a day or two to clear, so start it early.

---

## 3. Create the app

In Play Console → **Create app**:
- **App name:** `Passage`
- **Default language:** English (United Kingdom)
- **App or game:** App
- **Free or paid:** Free
- Tick the developer-program-policy and US-export declarations.

---

## 4. Store listing copy (paste these in)

**App name (30 chars max):**
```
Passage
```

**Short description (80 chars max):**
```
Private bowel-health diary. Track Bristol type & symptoms for your doctor.
```

**Full description (paste as-is):**
```
Passage is a simple, private diary for tracking your bowel health — built for people
managing IBD, Crohn's, IBS, or anyone who wants to understand their gut better.

Log an entry in seconds using the clinically recognised Bristol Stool Scale, then add as much
or as little detail as you like: stool colour, urgency, pain, symptoms (blood, mucus, bloating,
cramping and more), notes, optional photos, and the date and time.

See your patterns at a glance with a clean dashboard, a monthly calendar, and clear charts —
daily frequency, stool type distribution, pain and urgency trends, time of day, and your most
common symptoms.

Review your history any time: browse by calendar or list, filter by type, colour, symptom or
pain level, and edit or delete past entries whenever you need to.

When it's time for a doctor's appointment, export a CSV your GP, gastroenterologist or dietitian
can open in any spreadsheet, or back up everything as a JSON file.

• Bristol Stool Scale (Types 1–7) with clear descriptions
• Colour, urgency, pain, symptoms, notes and photos
• Monthly calendar and filterable history — edit or delete any entry
• Charts: frequency, type distribution, pain & urgency, time of day, symptoms
• CSV export for your healthcare team
• Full JSON backup and restore
• Three themes, including a dark mode
• Works completely offline

YOUR PRIVACY COMES FIRST
All your data stays on your device. Nothing is ever uploaded, and there are no accounts, no ads,
and no tracking. We never see your entries.

Passage is a personal tracking tool and does not provide medical advice. Always consult a
qualified healthcare professional about your symptoms.
```

**App category:** Health & Fitness
(Medical is also valid; Health & Fitness usually attracts less aggressive policy review.)

**Tags:** health, IBD, IBS, symptom tracker, diary

**Contact email:** dev@walker-jones.eu

**Privacy policy URL:** `https://walker-jones.eu/androidapps/passage/privacy-policy.html`

---

## 5. Graphics

All prepared in `store-assets/` — nothing left to create:
- **App icon:** `icon-512.png` (512×512) ✅
- **Phone screenshots:** four 1080×2400 PNGs in `store-assets/screenshots/` ✅ (Play needs 2–8)
- **Feature graphic:** `feature-graphic.png` (1024×500) ✅

---

## 6. Required questionnaires (in "App content")

**Privacy policy:** paste your URL.

**Data safety** — answer as follows (this is the honest, simple case):
- Does your app collect or share any user data? → **No**
  (The app stores health data only on-device and never transmits it. On-device-only storage is
  not "collection" under Google's definition. The CSV/JSON export is user-initiated sharing the
  user performs themselves, not data your app collects.)
- Is all data encrypted in transit? → N/A (no data leaves the device)
- Result: your Data Safety section will say **"No data collected or shared."**

**Content rating:** complete the IARC questionnaire. It's a utility/health app with no violence,
sexual, or gambling content → expect **PEGI 3 / Everyone**.

**Ads:** declare **No ads**.

**Target audience:** 18+ (or 13+). Do **not** mark it as appealing to children.

**Health apps declaration:** if prompted, confirm it's a personal health-tracking tool that
does not provide diagnosis or medical advice (matches the in-app disclaimer).

---

## 7. Upload the build (use a test track first)

Strongly recommended: release to **Internal testing** before Production.

1. Play Console → **Testing → Internal testing → Create new release**.
2. Because you're using **Play App Signing** (the default), accept it when prompted — Google
   generates and holds the real signing key; your AAB is signed with your upload key.
3. Upload `android/app/build/outputs/bundle/release/app-release.aab`.
4. Add release notes (e.g. "First release of Passage.").
5. Add yourself as a tester (your email), save, review, and roll out.
6. Install via the opt-in link on your own phone and try it end-to-end.

When happy: **Production → Create new release**, reuse the same AAB, fill the rollout details,
and submit for review. First reviews typically take a few hours to a few days.

---

## 8. Shipping future updates

1. Make your changes in `www/`.
2. Bump `versionCode` (to 2, 3, …) and `versionName` in `android/app/build.gradle`.
3. From `android/`: `./gradlew bundleRelease` (env: set JAVA_HOME to Android Studio's `jbr`).
4. Upload the new AAB to a release. Same upload key = Google accepts it as an update.

(Helper not strictly needed, but `npx cap copy android` before building ensures `www/` changes
are picked up.)
