# QA Case Study: Full-Cycle Manual Testing for a Mobile Rewards Application

---

**Analyst:** Sunanda Thompson

**Date:** March 2026

**Application:** EarnFlow *(fictional mobile rewards app)*

**Scope:** Full manual QA cycle covering application testing, UX validation, advertising & monetization, defect reporting, and release support

**Tools Used:**
- Android 14 (Samsung Galaxy S23)
- iOS 17 (iPhone 14)
- Android 10 (older test device)
- Chrome DevTools
- JIRA (simulated)
- Google Sheets (test log)
- Loom (screen recording)

---

## 📖 Who This Document Is For

This case study is written for anyone who is new to software quality assurance (QA) and wants to understand what manual testing actually looks like in practice — from the first smoke test all the way through to a hotfix verification after release.

If you have never worked in QA before, don't worry. Every term will be explained before it is used. By the end of this document, you will understand:

- What manual QA testing is and why it matters
- The three core test types every QA analyst uses
- How to write a proper defect report
- What happens before, during, and after a software release

---

## 🧠 Part 1: Understanding Manual QA Testing

### What Is Manual QA Testing?

**Manual testing** is the process of a human tester systematically using a software product — clicking through screens, triggering features, and entering data — to find defects, validate expected behavior, and evaluate the user experience.

It is different from **automated testing**, which uses code scripts to run checks automatically. Automated tests are fast and great for repetitive checks, but they have a significant limitation: they can only check what they were programmed to look for.

Manual testing fills the gaps that automation cannot. It applies human judgment, curiosity, and real-world thinking to surfaces problems that no script would ever find.

---

### Why Can't Automation Do Everything?

Here are four things that require a human eye:

**Visual and layout validation**
A script can confirm that a button exists on a page. It cannot tell you that the button is partially hidden behind an ad, or that the text on it is cut off on a smaller screen. A human notices that immediately.

**Ad rendering and creative quality**
Automated checks can verify that an ad loaded. They cannot tell you that the ad is displaying a blank white screen instead of the actual creative, or that the ad is overlapping the navigation bar and blocking the user from tapping anything.

**Edge-case behaviors**
An edge case is an unusual but realistic situation — like a user who taps a button twice very quickly, or who loses internet connection mid-transaction. These scenarios are hard to anticipate when writing automated scripts. Exploratory manual testing finds them naturally.

**UX friction**
Something can work perfectly from a technical standpoint and still be confusing, frustrating, or unintuitive for a real user. A manual tester experiences the app the way a user would, and can flag when something feels wrong even if it isn't technically broken.

---

## 🔬 Part 2: The Three Core Test Types

Before any testing begins, it helps to understand the three primary types of manual testing. Each one has a different purpose and is used at a different point in the release cycle.

---

### 🔁 Regression Testing

**What it is:**
Regression testing means re-running previously passing tests after a code change — to confirm that nothing which worked before has broken as a result of the update.

**Why it matters:**
Every time engineers change the code, there is a risk of unintended side effects. A fix applied to one feature can silently break something in a completely unrelated area. This is called a **regression** — something that used to work, no longer does.

Regression testing acts as a safety net. It ensures that the application as a whole still behaves correctly after every update, not just the area that was changed.

**When it is used:**
Regression testing is run before every release, after every significant code change, and after every bug fix.

**Real example from this case study:**
After engineering fixed the balance calculation bug (BUG-006), regression testing was used to verify that the fix did not accidentally break the earn crediting system, the redemption flow, or the activity history log — none of which were directly touched by the fix.

---

### 💨 Smoke Testing

**What it is:**
Smoke testing is a quick, high-level check of the most critical application functions. It is designed to answer one simple question: *"Is this build stable enough to test further?"*

**Why it matters:**
Imagine spending four hours running a full test suite on a build — only to discover at the end that the login screen was broken the entire time and none of your tests were reflecting real behavior. Smoke testing prevents this. It takes 15–30 minutes and covers only the core flows. If a smoke test fails, the build goes straight back to engineering. No time wasted.

**When it is used:**
Smoke testing is always the first thing that happens when a new build arrives for testing.

**Real example from this case study:**
Before testing EarnFlow v3.1.0, a six-point smoke test confirmed the app launched, users could log in, the Earn tab loaded, balances displayed, redemptions initiated, and the new Referral tab was visible. All six passed — so full testing proceeded.

---

### 🔍 Exploratory Testing

**What it is:**
Exploratory testing is unscripted, curiosity-driven testing. Unlike structured test cases — which follow a pre-written script — exploratory testing lets the tester simultaneously design and execute tests in real time, guided by their intuition and their understanding of how the application works.

**Why it matters:**
No test plan can anticipate every way a real user will interact with an application. Exploratory testing is where the most interesting and unexpected bugs are found — the ones nobody thought to write a test case for.

**When it is used:**
Exploratory testing is most valuable after structured testing is complete, or when testing a new feature where behavior is not yet fully defined. It is also used when a tester has a "hunch" — a feeling that something might be wrong in a particular area.

**Real example from this case study:**
During exploratory testing of EarnFlow, the following questions were investigated:
- *What happens if I switch apps mid-stream and come back?* → Activity was not tracked or prompted (BUG-002)
- *What if I complete two activities within five seconds of each other?* → Only the first was logged (BUG-005)
- *What if I rotate my phone while an ad is playing?* → Ad layout broke on Android
- *What if I redeem exactly my full balance, leaving zero?* → Balance displayed as "-2" (BUG-006)

None of these scenarios were in the structured test plan. All of them revealed real defects.

---

## 📱 Part 3: The Application Under Test

**EarnFlow** is a fictional mobile rewards application. Users earn points by completing everyday mobile activities — streaming content, gaming, shopping, and watching ads. Points can be redeemed for gift cards, cash, or stock ownership in the platform.

This case study covers two consecutive release stages:

**v3.1.0 — Feature Release**
New features added: a referral program and rewarded video ads. This version required full pre-release validation before going live.

**v3.1.1 — Hotfix Release**
A balance calculation regression was introduced by a fix in v3.1.0. A targeted hotfix was released 48 hours later and required verification before going live.

---

## 🚦 Part 4: Pre-Release Validation — v3.1.0

Pre-release validation is the full QA cycle that happens before a build is released to the public. The goal is to find every significant defect before real users encounter it.

The cycle follows this order:
1. Smoke test — is the build stable enough to test?
2. Functional testing — do all features work as designed?
3. Edge-case testing — what happens at the boundaries of expected behavior?
4. UX validation — does the experience feel right for a real user?
5. Ad & monetization testing — is the revenue layer working correctly?
6. Defect triage — which bugs block release, and which can wait?

---

### Step 1: Smoke Test

The smoke test is always first. If it fails, testing stops and the build returns to engineering.

| Smoke Test ID | Check | Result |
|---------------|-------|--------|
| SMK-001 | App launches without crash | ✅ PASS |
| SMK-002 | User can log in with valid credentials | ✅ PASS |
| SMK-003 | Earn tab loads and displays activities | ✅ PASS |
| SMK-004 | Balance displays correctly on Home screen | ✅ PASS |
| SMK-005 | Reward redemption flow initiates | ✅ PASS |
| SMK-006 | New referral tab visible in bottom navigation | ✅ PASS |

**Result: ✅ PASS — build cleared for full testing.**

---

### Step 2: Functional Testing

Functional testing verifies that each feature in the application behaves exactly as specified. Every test case follows the same structure:

- **Steps** — the exact sequence of actions to perform
- **Expected result** — what should happen
- **Actual result** — what actually happened
- **Status** — PASS or FAIL

This structure is important because it makes every test **reproducible** — any other tester can follow the same steps and get the same result.

| Test ID | Feature | Steps | Expected | Actual | Status |
|---------|---------|-------|----------|--------|--------|
| TC-001 | Login — valid credentials | Enter correct email/password → tap Login | Home screen loads | Home screen loads | ✅ PASS |
| TC-002 | Login — invalid password | Enter wrong password → tap Login | Error: "Incorrect password" displayed | Error displayed | ✅ PASS |
| TC-003 | Login — account lockout after 5 failed attempts | Enter wrong password 5 times | Account locked message shown | App freezes, no message | ❌ FAIL |
| TC-004 | Stream & Earn — complete full activity | Stream for 60 seconds uninterrupted | +5 pts credited to balance | +5 pts credited | ✅ PASS |
| TC-005 | Stream & Earn — interrupted mid-activity | Begin stream → switch apps → return | Partial credit or restart prompt shown | No credit, no prompt | ❌ FAIL |
| TC-006 | Referral — generate share link | Tap Refer a Friend → Generate link | Unique referral link created | Link generated | ✅ PASS |
| TC-007 | Referral — bonus credited after new user signup | New user signs up via referral link | Referrer receives +50 pts | No bonus credited | ❌ FAIL |
| TC-008 | Balance — real-time update after activity | Complete earn activity → view balance | Balance updates within 10 seconds | Updates in ~8 seconds | ✅ PASS |
| TC-009 | Redeem — with sufficient balance | Redeem $5 gift card with 500 pts | Redemption confirmed, balance deducted | Confirmed, deducted | ✅ PASS |
| TC-010 | Redeem — with insufficient balance | Attempt redemption with only 100 pts | Error: "Insufficient balance" | Error displayed | ✅ PASS |

**Functional test result: 7 of 10 PASS — 3 failures logged as defects.**

---

### Step 3: Edge-Case Testing

Edge cases sit at the boundaries of normal behavior. They represent situations that are unusual but realistic — the kind of thing that happens when a real user does something unexpected, uses an older device, or encounters an unusual network condition.

Edge cases are important because they often reveal bugs that functional testing misses entirely. A feature can pass all of its standard test cases and still fail under edge-case conditions.

| Test ID | Edge Case | What Was Tested | Expected | Actual | Status |
|---------|-----------|-----------------|----------|--------|--------|
| EC-001 | Earn activity during low battery mode | Enable battery saver → complete stream activity | Points credited normally | Points not credited | ❌ FAIL |
| EC-002 | Two activities completed within 5 seconds | Open stream and shopping tasks back-to-back | Both tracked independently | Only first activity logged | ❌ FAIL |
| EC-003 | Redeem exact balance (zero remaining) | Redeem all points so balance = 0 exactly | Balance shows 0 | Balance shows "-2" | ❌ FAIL |
| EC-004 | App used in airplane mode | Enable airplane mode → attempt earn activity | Friendly offline message shown | App crashes entirely | ❌ FAIL |
| EC-005 | Very long username in UI | Create account with 50-character username | Username truncated gracefully in UI | Username overflows its container | ❌ FAIL |

**Edge-case result: 0 of 5 PASS — all edge cases failed.**

> ⚠️ This result was escalated to engineering immediately. A 0% pass rate on edge-case testing is a significant quality signal. It suggests the application was not tested against real-world usage patterns during development.

---

## 🎨 Part 5: UX Validation

UX stands for **User Experience** — the overall feeling a person has when using an application. UX validation goes beyond asking "does it work?" to ask "does it work *well* for a real person?"

This distinction matters because a feature can be technically functional and still be confusing, frustrating, or easy to misuse. A good QA analyst thinks like a user, not just like a tester.

---

### What UX Validation Covers

During UX validation, every screen and flow is evaluated across six dimensions:

**Usability** — Can a new user figure out what to do without instructions?

**Navigation** — Can users find what they need quickly and intuitively?

**Clarity** — Is the language plain and easy to understand? Are labels and buttons obviously named?

**Interaction flows** — Do sequences of actions feel logical? Are there dead ends, confusion, or unnecessary steps?

**Visual consistency** — Does the design feel cohesive and polished across all screens?

**Responsiveness** — Does the layout adapt correctly to different screen sizes and orientations?

---

### UX Findings

| UX ID | Screen | Finding | Severity | Recommendation |
|-------|--------|---------|----------|----------------|
| UX-001 | Onboarding | Location permission is requested with no explanation of why the app needs it | Medium | Add a brief explanation of the benefit to the user before the prompt appears |
| UX-002 | Balance screen | No loading indicator shown — screen appears blank for 2–3 seconds on slower connections | Medium | Add a skeleton loader or spinner so users know data is on its way |
| UX-003 | Earn tab | Three activity cards are visible but there is no indication that more exist below — users may not scroll | Low | Add a scroll affordance or "See all activities" label |
| UX-004 | Referral screen | "Link copied" confirmation disappears in under one second — too fast to read | Low | Extend the confirmation message to at least 3 seconds |
| UX-005 | Redemption flow | After a successful redemption, the screen has no button to return to the Home tab — navigation dead end | High | Add a clear "Return to Home" button on the success screen |
| UX-006 | Error messages | Raw technical error codes (e.g. "HTTP 403") are shown directly to end users | High | Replace all technical error codes with plain, user-friendly language |
| UX-007 | Settings | Account deletion is buried 4 taps deep with no way to search for it | Low | Surface account deletion in the main settings list |

---

### Cross-Device Consistency

One of the most important UX checks is verifying that the application looks and behaves consistently across different devices and operating systems. What works perfectly on one device may break on another.

| Screen | Android 14 (S23) | iOS 17 (iPhone 14) | Android 10 (older device) | Notes |
|--------|------------------|--------------------|--------------------------|-------|
| Home screen | ✅ | ✅ | ⚠️ Balance text clipped | Font size issue on older Android |
| Earn tab | ✅ | ✅ | ✅ | Consistent across all devices |
| Ad placement | ❌ Overlaps nav bar | ✅ | ❌ Same overlap | Android-specific layout issue |
| Referral screen | ✅ | ✅ | ⚠️ Layout shifted slightly | Minor alignment issue |
| Redemption flow | ✅ | ❌ Dead-end navigation | ✅ | iOS-specific — UX-005 |

> **Note on cross-device testing:** It is never safe to assume that because something works on one device, it works on all devices. Android and iOS handle layout, safe area insets, fonts, and system UI differently. Testing on at least one device per platform — and ideally on both a recent and an older device — is essential.

---

## 📢 Part 6: Advertising & Monetization Testing

For applications like EarnFlow, advertising is the primary source of revenue. If the ad layer breaks, the business loses money — often silently, without any visible error to the user.

This makes ad testing one of the highest-stakes areas of QA. Defects here are not just user experience issues. They are financial issues.

---

### Ad Types Tested

**Banner ads** — Small display units that appear at the top or bottom of the screen while the user navigates the app.

**Interstitial ads** — Full-screen ads that appear between activities. They typically include a countdown timer before the user can dismiss them.

**Rewarded video ads** — The user chooses to watch a short video advertisement in full, and receives points in exchange for completing it.

---

### Ad Test Cases

| Test ID | Ad Type | What Was Tested | Expected | Actual | Status |
|---------|---------|-----------------|----------|--------|--------|
| AD-001 | Banner | Ad renders within its designated display zone | Banner contained above the navigation bar | Overlaps the navigation bar on Android | ❌ FAIL |
| AD-002 | Banner | Interactive UI elements remain tappable while ad is displayed | All navigation buttons remain accessible | Navigation bar buttons blocked by ad | ❌ FAIL |
| AD-003 | Interstitial | Ad displays correctly after activity completion | Full-screen ad renders | Ad renders correctly | ✅ PASS |
| AD-004 | Interstitial | Skip button appears after the 5-second countdown | Skip button visible at exactly 5 seconds | Skip button never appears — user is trapped | ❌ FAIL |
| AD-005 | Interstitial | Tapping outside the ad does not dismiss it | Ad remains until user taps Skip | Any tap anywhere on screen dismisses ad | ❌ FAIL |
| AD-006 | Rewarded video | Points credited after watching the full video | +2 pts credited after full view on Android | Points credited correctly | ✅ PASS |
| AD-007 | Rewarded video | Points are NOT credited if the user skips early | No credit when ad is skipped | Points credited even when skipped immediately | ❌ FAIL |
| AD-008 | Rewarded video | Tapping the ad redirects to the correct destination | Opens correct advertiser URL in browser | Correct URL opened | ✅ PASS |
| AD-009 | Rewarded video | Points credited correctly on iOS | +2 pts after full view on iOS | No credit on iOS — reward never triggers | ❌ FAIL |
| AD-010 | All types | Ad creative renders with actual content | Ad image or video displays | 1 of 8 ads displayed a blank white screen | ❌ FAIL |

**Ad test result: 3 of 10 PASS — 7 failures, multiple with direct revenue impact.**

---

### Revenue Impact Assessment

Some bugs are inconvenient. Others cost the business money. The table below maps each ad defect to its financial consequence — because QA's job is not just to find bugs, but to communicate their business impact clearly.

| Bug | What It Means for Revenue |
|-----|--------------------------|
| AD-007 — Points credited when ad is skipped | Users can farm points by repeatedly skipping ads. Advertisers pay for completed views they never receive. |
| AD-009 — iOS rewarded video not crediting points | iOS users stop watching rewarded ads. Ad inventory goes unsold on the entire iOS platform. |
| AD-004 — Interstitial skip button never appears | Users are trapped inside the ad. This creates a severely negative user experience and risks bad app store reviews and uninstalls. |
| AD-005 — Tap dismisses interstitial | Advertisers are charged for impressions their ads never delivered. This can violate ad contracts. |
| AD-010 — Blank ad creative | A blank impression is delivered to the advertiser. CPM revenue is lost and advertisers may dispute charges. |

---

## 🐛 Part 7: Defect Reporting & Documentation

Finding bugs is only half the job. The other half is documenting them clearly enough that an engineer can reproduce, understand, and fix them — without needing to ask follow-up questions.

A vague bug report like *"the app sometimes crashes"* is nearly useless. A precise bug report with exact steps, environment details, and evidence is a gift to the engineering team.

---

### The Anatomy of a Good Bug Report

Every defect logged during this cycle followed this structure:

---

```
Bug ID:           [Unique identifier, e.g. BUG-007]
Title:            [One-line summary of what is broken]
Severity:         [Critical / High / Medium / Low]
Priority:         [P1 (fix before release) / P2 / P3 (can wait)]
Environment:      [Device model, OS version, App version]
Summary:          [What is broken, and why it matters]

Steps to Reproduce:
  1. [First action]
  2. [Second action]
  3. [Continue until the bug appears]

Expected Result:  [What should happen]
Actual Result:    [What actually happens]
Evidence:         [Screenshots, screen recordings, log output]
Notes:            [Is it platform-specific? How often does it occur? Related bugs?]
```

---

**Severity vs. Priority — what is the difference?**

These two terms are often confused, but they mean different things:

**Severity** describes how bad the bug is from a technical or user impact standpoint. A Critical severity bug breaks a core feature entirely. A Low severity bug is a minor visual issue that most users won't notice.

**Priority** describes how urgently the bug needs to be fixed. A P1 bug must be fixed before the next release. A P3 bug can be scheduled for a future update.

A bug can be High severity but Low priority — for example, a crash that only occurs on a device that represents 0.1% of users. Conversely, a bug can be Low severity but High priority — for example, a typo on the app's login screen that looks unprofessional.

---

### Sample Full Defect Report

Below is a complete, real-format defect report for one of the bugs found during this cycle.

---

**Bug ID:** BUG-007

**Title:** Rewarded video ad credits points when ad is skipped early

**Severity:** Critical

**Priority:** P1 — must be resolved before release

**Environment:**
- Device: Samsung Galaxy S23
- OS: Android 14
- App version: EarnFlow v3.1.0
- Also reproducible on: iOS 17 (iPhone 14)

**Reported by:** Sunanda Thompson

**Date:** March 5, 2026

---

**Summary:**

Users receive the full +2 point reward for a rewarded video ad even when they skip the ad within the first 1–2 seconds. This means users can repeatedly initiate and immediately skip rewarded ads to accumulate points without watching any content — directly undermining the monetization model and defrauding advertisers.

---

**Steps to Reproduce:**

1. Log in to EarnFlow with any valid user account.
2. Note the current point balance on the Home screen.
3. Navigate to **Earn** → **Watch & Earn**.
4. Tap on any available rewarded video ad to begin watching.
5. Immediately tap **Skip** within 1–2 seconds of the ad starting.
6. Return to the Home screen and observe the balance.

---

**Expected Result:**

No points are credited when the ad is skipped before completion. The reward should only trigger after the user watches the full video.

**Actual Result:**

+2 points are credited to the user's balance even when the ad is skipped immediately. The reward appears to fire when the ad *starts*, not when it *ends*.

---

**Evidence:**
- Screen recording: user skips immediately, balance increases from 100 to 102 pts
- Screenshot: balance before (100 pts) and after skip (102 pts)

---

**Impact:**

This defect has three direct consequences:

1. Users can exploit this to earn unlimited points without watching any ads
2. Advertisers are charged for completed ad views they never received — a potential contract violation
3. If not caught before release, this bug could be widely discovered and shared, causing rapid exploitation at scale

---

**Notes:**

- Reproducible on 10 of 10 test runs
- Reproducible on both Android and iOS
- Likely caused by the reward event firing on the `adStarted` callback instead of the `adCompleted` callback in the ad SDK integration
- Related to BUG-009 — iOS rewarded video not crediting on completion — suggesting the same event handler may be misconfigured differently per platform

---

### Complete Defect Log — v3.1.0

| Bug ID | Title | Severity | Priority | Platform |
|--------|-------|----------|----------|----------|
| BUG-001 | Account lockout — app freezes instead of showing message | High | P2 | Both |
| BUG-002 | Interrupted stream activity not tracked or prompted | Medium | P3 | Both |
| BUG-003 | Referral bonus not credited to referrer on signup | High | P2 | Both |
| BUG-004 | Earn activity fails when low battery mode is enabled | Medium | P3 | Android |
| BUG-005 | Two rapid activities — only first logged | High | P2 | Both |
| BUG-006 | Balance shows negative value after full redemption | Critical | P1 | Both |
| BUG-007 | Rewarded video points credited when ad is skipped | Critical | P1 | Both |
| BUG-008 | Banner ad overlaps and blocks bottom navigation bar | Medium | P2 | Android |
| BUG-009 | Rewarded video does not credit points on iOS | High | P1 | iOS |
| BUG-010 | Interstitial skip button never appears | High | P1 | Both |
| BUG-011 | Tapping outside interstitial dismisses ad early | High | P2 | Both |
| BUG-012 | One of eight ad creatives renders as blank white screen | High | P2 | Both |
| BUG-013 | App crashes when opened in airplane mode | High | P1 | Both |
| UX-005 | Redemption success screen has no navigation button | High | P2 | iOS |
| UX-006 | Raw HTTP error codes displayed to end users | High | P2 | Both |

**Total defects logged: 15**

| Severity | Count |
|----------|-------|
| Critical | 2 |
| High | 9 |
| Medium | 3 |
| Low | 1 |

**Release recommendation: HOLD**

> A release should be held whenever there are unresolved Critical defects or P1 bugs. In this case, BUG-006, BUG-007, BUG-009, BUG-010, and BUG-013 all qualify as release blockers. Shipping with these defects would mean releasing an application that crashes offline, traps users inside unskippable ads, defrauds advertisers, and shows negative balances to users.

---

## 🚀 Part 8: Release Support

QA work does not end when the bugs are fixed. Release support covers three distinct stages — and each one has a specific purpose.

---

### Stage 1: Pre-Release Validation Checklist

Before any build is approved for release, a final checklist confirms that all blocking issues have been resolved and the application is ready for real users.

| Check | Area | Result |
|-------|------|--------|
| All P1 defects resolved and re-verified | Critical bugs | ❌ BUG-006, BUG-007, BUG-009, BUG-010, BUG-013 outstanding — release blocked |
| Smoke test passing on all target devices | Stability | ✅ PASS |
| Full regression suite completed | Core features | ✅ PASS (known defects logged separately) |
| Ad serving verified on both platforms | Monetization | ❌ AD-009 outstanding |
| UX review completed | User experience | ⚠️ Conditional — UX-005 and UX-006 flagged for next release |

**Initial pre-release decision: HOLD — blocked by 5 unresolved P1 defects.**

After engineering resolved BUG-006, BUG-007, BUG-009, BUG-010, and BUG-013, each fix was individually re-verified before the build was cleared.

**Re-verification result: ✅ All P1 defects confirmed resolved — build cleared for release.**

---

### Stage 2: Post-Release Checks

Once a build goes live on the app store, post-release checks confirm that the production version behaves as expected with real users. This is important because production environments can differ from test environments in subtle ways — different server load, different network conditions, different device configurations.

Post-release checks are run within the first two hours of a release going live.

| Check | Result | Notes |
|-------|--------|-------|
| App store build launches without crash | ✅ PASS | Verified on Android and iOS |
| Core earn flows functional | ✅ PASS | Stream, game, and shop activities all tracking |
| Rewarded video crediting correctly | ✅ PASS | Fix verified on both platforms |
| Balance calculation correct after redemption | ✅ PASS | Fix verified — no negative values |
| Interstitial skip button appears at 5 seconds | ✅ PASS | Fix verified |
| No new crashes in first 2 hours of live traffic | ✅ PASS | Monitoring active |
| Banner ad overlap on Android nav bar | ❌ Still present | Known issue — P2 — scheduled for v3.1.2 |

**Post-release assessment: Stable. One known P2 issue remains and is tracked for the next update.**

---

### Stage 3: Hotfix Verification — v3.1.1

A **hotfix** is an emergency release designed to fix a single, specific problem as quickly as possible — without going through a full development cycle. Hotfixes are typically released within hours or days of a critical issue being discovered in production.

In this case, engineering discovered that the fix for BUG-006 (the negative balance bug) introduced a new regression — point balances were now being displayed as decimal values (e.g. "102.5 pts") instead of whole numbers.

A hotfix — v3.1.1 — was released 48 hours after v3.1.0 to correct the rounding logic.

**Hotfix scope:** Balance rounding corrected to always display whole numbers.

**Verification steps performed:**

1. Confirmed balance displays as a whole number in all scenarios tested
2. Ran the full redemption flow with multiple balance amounts — exact amounts, amounts requiring rounding, fractional edge cases
3. Confirmed no regression introduced in earn crediting, activity tracking, or referral bonuses
4. Verified the fix on Android 14, iOS 17, and Android 10 (older device)

**Hotfix result: ✅ PASS — v3.1.1 cleared for release.**

---

## 📊 Part 9: Test Metrics Summary

At the end of a test cycle, it is useful to summarize what was done, what was found, and what it means for the quality of the release. This gives product and engineering teams a clear picture of where the application stands.

| Metric | Value |
|--------|-------|
| Total test cases executed | 45 |
| Test cases passed | 28 (62%) |
| Test cases failed | 17 (38%) |
| Total defects logged | 15 |
| Critical defects | 2 |
| High defects | 9 |
| Medium defects | 3 |
| P1 defects resolved before release | 5 of 5 |
| Releases supported | 2 (v3.1.0 and v3.1.1) |
| Post-release regressions caught | 1 (balance rounding — resolved in v3.1.1) |

---

## 💡 Part 10: Key Lessons

These are the five most important things this test cycle demonstrated — principles that apply to QA work on any application, not just EarnFlow.

---

**1. Smoke tests save hours.**

Running a focused smoke test before the full suite means the team never spends hours testing a build that was broken from the start. If a build fails smoke, it goes back to engineering in 30 minutes rather than after a full day of wasted effort.

---

**2. Edge cases reveal the most impactful bugs.**

All five edge-case tests failed in v3.1.0 — including the balance calculation error and the airplane mode crash. These bugs were not in any structured test plan. They were only found because someone asked *"what happens if...?"* That question is at the heart of good QA work.

---

**3. Monetization defects require immediate escalation.**

BUG-007 — points credited when an ad is skipped — was a silent exploit. If it had shipped to production, it could have been discovered and shared widely before anyone noticed. Understanding the business impact of a defect is just as important as documenting the technical details.

---

**4. Cross-platform parity is never guaranteed.**

Several bugs in this cycle were platform-specific. The iOS rewarded video failure, the Android banner overlap, and the Android airplane mode crash all passed on one platform and failed on another. Testing on a single platform creates a false sense of confidence. Real coverage requires real devices on both platforms.

---

**5. A clear bug report is a gift to engineering.**

Every defect report in this cycle included exact reproduction steps, full environment details, visual evidence, a plain-language impact assessment, and notes on potential root causes. Engineers should never have to ask *"can you tell me more?"* after reading a bug report. The faster a defect is understood, the faster it gets fixed — and the faster the product improves.

---

*This case study is a portfolio project created to demonstrate manual QA methodology across a full release cycle. All application names, defects, devices, and data are entirely fictional and were created for educational and demonstration purposes only.*
