# QA Case Study: Manual Testing for a Mobile Rewards Application
**Analyst:** Sunanda Thompson  
**Date:** March 2026  
**Scope:** Manual functional testing, UX validation, and ad placement verification for a reward-based mobile application  
**Tools Used:** Android & iOS devices, Chrome DevTools, JIRA (simulated), Google Sheets for test tracking  

---

## Overview

This case study documents a structured manual QA test cycle conducted on a fictional reward-based mobile application, **EarnApp Demo** — a platform where users earn rewards by completing everyday mobile activities such as streaming, gaming, and shopping.

The goal was to simulate the testing responsibilities of a Junior QA Analyst: executing structured test cases, validating user experience flows, verifying advertising integrations, and documenting defects with clear reproduction steps.

---

## Test Environment

| Parameter | Detail |
|-----------|--------|
| Platforms tested | Android 13 (Pixel 6), iOS 16 (iPhone 12), Chrome (desktop) |
| App version | v2.4.1 (simulated) |
| Test types | Functional, Regression, Smoke, Exploratory |
| Defect tracking | JIRA (simulated) |
| Test documentation | Google Sheets test log |

---

## Test Plan Summary

### Areas Covered
1. **User Onboarding Flow** — registration, permissions, first-run experience
2. **Core Earn Mechanics** — activity tracking, reward crediting, balance display
3. **Ad Placement & Monetization** — ad rendering, timing, click behavior, redirects
4. **UX & Responsiveness** — layout consistency, navigation, edge cases
5. **Cross-Device Validation** — behavior parity across Android, iOS, and mobile web

---

## Section 1: Functional Testing — Core Earn Flow

### Test Cases Executed

| Test ID | Test Case | Expected Result | Actual Result | Status |
|---------|-----------|-----------------|---------------|--------|
| TC-001 | User registers with valid email | Account created, confirmation email sent | ✅ As expected | PASS |
| TC-002 | User registers with already-used email | Error message displayed | ✅ As expected | PASS |
| TC-003 | Streaming activity tracked after 60 seconds | +5 points credited to balance | ❌ No points credited | **FAIL** |
| TC-004 | Balance updates in real time after activity | Balance reflects new points within 10 seconds | ❌ Delay of 45+ seconds observed | **FAIL** |
| TC-005 | Reward redemption with sufficient balance | Redemption confirmed, balance deducted | ✅ As expected | PASS |
| TC-006 | Redemption attempt with insufficient balance | Error message: "Insufficient balance" | ✅ As expected | PASS |
| TC-007 | Activity history log reflects completed tasks | All completed tasks listed with timestamps | ❌ Two tasks missing from log | **FAIL** |

---

## Section 2: Defect Reports

### BUG-001 — Streaming Activity Not Crediting Points

**Severity:** Critical  
**Priority:** P1  
**Environment:** Android 13, App v2.4.1  

**Summary:** Completing a 60-second streaming activity does not credit the expected 5 points to the user's balance.

**Steps to Reproduce:**
1. Log in to the app with a valid user account.
2. Navigate to the **Earn** tab.
3. Select **Stream & Earn** activity.
4. Allow the stream to play for 60+ seconds without interruption.
5. Navigate back to the **Balance** screen.

**Expected Result:** User balance increases by 5 points within 10 seconds of activity completion.

**Actual Result:** Balance remains unchanged. No points are credited. No error message is displayed.

**Evidence:** [Screenshot — balance unchanged after streaming]  [Screen recording — full activity flow]

**Notes:** Issue is consistent across 5 test runs. Not reproducible on iOS — points credit correctly on iPhone 12. Likely an Android-specific tracking event failure.

---

### BUG-002 — Balance Display Lag (45+ Seconds)

**Severity:** High  
**Priority:** P2  
**Environment:** Android 13 and iOS 16, App v2.4.1  

**Summary:** After completing an eligible earn activity, the balance display does not update for 45+ seconds, causing user confusion about whether the activity was successfully tracked.

**Steps to Reproduce:**
1. Log in with a valid user account. Note current balance.
2. Complete any eligible earn activity (shopping, gaming, or streaming).
3. Navigate immediately to the **Balance** screen.
4. Observe the balance and note the time until it updates.

**Expected Result:** Balance updates within 10 seconds of activity completion.

**Actual Result:** Balance update is delayed by 45–90 seconds across multiple test runs on both platforms.

**Impact:** Users may repeat activities thinking they weren't tracked, causing duplicate earn events or user frustration. High risk of negative app store reviews.

---

### BUG-003 — Two Activities Missing from History Log

**Severity:** Medium  
**Priority:** P2  
**Environment:** Android 13, App v2.4.1  

**Summary:** Activity history log is missing two completed tasks from a test session of seven activities.

**Steps to Reproduce:**
1. Complete 7 eligible earn activities in sequence.
2. Navigate to **Profile > Activity History**.
3. Count and verify logged entries.

**Expected Result:** All 7 completed activities appear in history with correct timestamps.

**Actual Result:** Only 5 of 7 activities are logged. Missing entries: "Stream & Earn" (run 3) and "Shop & Earn" (run 2).

**Notes:** Both missing entries correspond to activities completed within 30 seconds of each other. Possible race condition in the logging system.

---

## Section 3: Ad Placement & Monetization Testing

### Test Cases Executed

| Test ID | Test Case | Expected Result | Actual Result | Status |
|---------|-----------|-----------------|---------------|--------|
| AD-001 | Interstitial ad displays after activity completion | Ad renders fully before next screen loads | ✅ As expected | PASS |
| AD-002 | Banner ad renders without overlapping UI elements | Banner contained within designated zone | ❌ Banner overlaps bottom navigation bar | **FAIL** |
| AD-003 | Ad click redirects to correct destination URL | Opens correct advertiser page in browser | ✅ As expected | PASS |
| AD-004 | Ad closes correctly after countdown timer | Ad dismissible after 5-second timer | ✅ As expected | PASS |
| AD-005 | Ad does not display when user has no active session | No ad shown on logged-out state | ✅ As expected | PASS |
| AD-006 | Reward credited after rewarded video ad completion | +2 points credited after full ad view | ❌ Points not credited on iOS | **FAIL** |

---

### BUG-004 — Banner Ad Overlaps Bottom Navigation Bar

**Severity:** Medium  
**Priority:** P2  
**Environment:** Android 13 (Pixel 6), App v2.4.1  

**Summary:** The banner ad unit renders outside its designated display zone and overlaps the bottom navigation bar, obscuring the Home, Earn, and Profile tabs.

**Steps to Reproduce:**
1. Log in and navigate to the **Earn** tab.
2. Allow a banner ad to load (typically within 3–5 seconds).
3. Observe ad placement relative to the bottom navigation bar.

**Expected Result:** Banner ad renders within its designated zone above the navigation bar.

**Actual Result:** Banner ad renders 40px too low, overlapping the navigation bar and making bottom tabs unclickable while ad is active.

**Evidence:** [Screenshot — ad overlap on Pixel 6]

**Notes:** Not reproducible on iOS. May be related to Android safe area inset handling. Revenue impact: users cannot navigate the app while banner is active, reducing engagement and earn activity.

---

### BUG-005 — Rewarded Video Ad Not Crediting Points on iOS

**Severity:** High  
**Priority:** P1  
**Environment:** iOS 16 (iPhone 12), App v2.4.1  

**Summary:** After watching a full rewarded video ad on iOS, the promised +2 point reward is not credited to the user's balance.

**Steps to Reproduce:**
1. Log in on an iOS device.
2. Navigate to **Earn > Watch & Earn**.
3. Select a rewarded video ad and watch it to completion without skipping.
4. Observe balance after ad completion.

**Expected Result:** +2 points credited to balance within 10 seconds of ad completion.

**Actual Result:** Balance does not change. No error or confirmation message is shown.

**Impact:** Critical revenue and trust issue. Users completing rewarded ad interactions expect compensation. Failure to credit undermines the core earn mechanic and advertiser relationships.

---

## Section 4: UX Validation

### Observations

| Area | Finding | Severity |
|------|---------|----------|
| Onboarding | Permission request screen lacks explanation of why location is needed — users may decline unnecessarily | Medium |
| Balance screen | No loading indicator during balance refresh — users cannot tell if data is updating | Low |
| Activity history | No empty state messaging when history is blank — screen appears broken | Low |
| Navigation | Back button on Android exits the app from the Earn tab rather than returning to Home | Medium |
| Ad dismissal | No visual affordance indicating when a non-skippable ad will become dismissible | Low |

### UX Bug Report: Back Button Exits App from Earn Tab

**Severity:** Medium  
**Priority:** P3  
**Environment:** Android 13  

**Summary:** Pressing the Android hardware back button from the Earn tab exits the application entirely instead of returning to the Home tab.

**Steps to Reproduce:**
1. Launch app and navigate to the **Earn** tab via bottom navigation.
2. Press the Android hardware back button.

**Expected Result:** User is returned to the **Home** tab.

**Actual Result:** App closes entirely.

**Impact:** Disruptive UX pattern. Users losing their session mid-activity is likely to cause frustration and abandonment.

---

## Section 5: Cross-Device Regression Summary

| Test Area | Android 13 | iOS 16 | Mobile Web (Chrome) |
|-----------|-----------|--------|---------------------|
| Onboarding flow | ✅ Pass | ✅ Pass | ✅ Pass |
| Streaming earn tracking | ❌ Fail | ✅ Pass | N/A |
| Balance update timing | ❌ Fail | ❌ Fail | N/A |
| Activity history logging | ❌ Fail | ✅ Pass | N/A |
| Banner ad placement | ❌ Fail | ✅ Pass | ✅ Pass |
| Rewarded video crediting | ✅ Pass | ❌ Fail | N/A |
| Back navigation behavior | ❌ Fail | ✅ Pass | N/A |

---

## Defect Summary

| Bug ID | Description | Severity | Platform |
|--------|-------------|----------|----------|
| BUG-001 | Streaming activity not crediting points | Critical | Android |
| BUG-002 | Balance display lag 45+ seconds | High | Both |
| BUG-003 | Two activities missing from history log | Medium | Android |
| BUG-004 | Banner ad overlaps navigation bar | Medium | Android |
| BUG-005 | Rewarded video not crediting points | High | iOS |
| UX-001 | Back button exits app from Earn tab | Medium | Android |

**Total test cases executed:** 20  
**Pass:** 13 (65%)  
**Fail:** 7 (35%)  
**Critical defects:** 2  
**High defects:** 2  
**Medium defects:** 3  

---

## Key Takeaways & Recommendations

1. **Platform parity is a priority.** Several defects are platform-specific, suggesting insufficient cross-platform testing in the development cycle. A cross-device regression suite should be run on every release.

2. **Earn mechanic reliability is critical.** BUG-001 and BUG-005 directly impact the app's core value proposition. If users don't earn when they should, retention and trust collapse quickly. These are P1 and should block release.

3. **Ad monetization defects have revenue implications.** BUG-004 and BUG-005 both affect the monetization layer. Broken ad placements and failed reward credits reduce both advertiser confidence and user engagement.

4. **UX friction points compound over time.** Individually, low-severity UX issues seem minor. Collectively, they degrade the user experience and increase churn. A dedicated UX review pass should be part of each release cycle.

5. **Logging race conditions need investigation.** BUG-003 suggests a possible concurrency issue in the activity logging system. Engineering should investigate whether rapid successive activities cause event queuing failures.

---

## Appendix: Test Case Log (Sample)

| Test ID | Area | Description | Steps | Expected | Actual | Status | Notes |
|---------|------|-------------|-------|----------|--------|--------|-------|
| TC-001 | Onboarding | Register with valid email | 1. Open app 2. Tap Register 3. Enter valid email/password 4. Submit | Account created | Account created | PASS | |
| TC-003 | Earn | Stream activity credits points | 1. Login 2. Start stream 3. Wait 60s 4. Check balance | +5 pts | No change | FAIL | Android only |
| AD-002 | Ads | Banner ad placement | 1. Login 2. Navigate to Earn 3. Wait for banner | Banner above nav | Overlaps nav | FAIL | Android only |

---

*This case study was created as a portfolio project to demonstrate manual QA methodology, defect documentation practices, and analytical thinking as applied to a mobile rewards application. All application names, bugs, and data are fictional and created for demonstration purposes.*
