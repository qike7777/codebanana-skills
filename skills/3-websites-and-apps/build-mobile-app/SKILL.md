---
name: build-mobile-app
description: |
  Combined skill for designing and building production-ready mobile apps.
  Part 1 (Design): Research styles, generate mobile design systems, implement UIs — backed by 67+ styles,
  161 color palettes, 57 font pairings, 99 UX guidelines across React Native, Flutter, and SwiftUI.
  Part 2 (Engineering): Structured development workflow — brainstorm → plan → TDD → code review → verify → Expo preview.
  Triggers:
  - "build a mobile app"
  - "design a mobile app"
  - "build mobile app"
  - "react native"
  - "flutter app"
  - "ios app"
  - "android app"
  - "做一个App"
  - "移动端开发"
  - "手机应用"
---

# SKILL.md — Build a Mobile App

> From idea to Expo-previewed mobile app: mobile design system first, then clean production code.
> Combines a searchable design database (67+ styles, 161 palettes, 57 font pairings, 99 UX guidelines)
> with a rigorous engineering workflow (brainstorm → plan → TDD → code review → verify → Expo preview).

---

## Overview

This skill combines two capabilities into one pipeline:

**Part 1 — Design (Phases 1–6)**
Research mobile styles → generate mobile design system (tokens + touch targets + safe areas + platform conventions) → implement UI → brand polish → mobile accessibility review → deliver

**Part 2 — Engineering (Phases 7–12)**
Brainstorm requirements → write implementation plan → build with TDD → code review → verify → Expo Go preview handoff

### Supported Mobile Tech Stacks

| Stack | Support Level |
|-------|--------------|
| React Native + Expo | Full (primary) |
| Flutter | Full |
| SwiftUI | Full |

> **Mobile-only.** This skill covers mobile stacks only. For React, Next.js, Vue, or web → use Build a Web App.

### Platform Awareness

Always distinguish iOS and Android conventions. Key differences:

| Aspect | iOS | Android |
|--------|-----|---------|
| Navigation | Stack (back button top-left) | Back gesture / hardware back |
| Bottom bar | Tab Bar (translucent) | Bottom Navigation Bar |
| Typography | SF Pro | Roboto |
| Touch target | ≥ 44×44pt | ≥ 48×48dp |
| Status bar | Dynamic (light/dark) | Transparent or colored |
| Safe areas | Notch + home indicator | Status bar + gesture nav bar |

### When to Start from Which Phase

| Scenario | Start From |
|----------|------------|
| "Build me a mobile app from scratch" | Phase 1 (Design Research) |
| "I have designs, just code them" | Phase 7 (Brainstorm) → Phase 9 (Execute) |
| "Quick screen tweak" | Phase 8 (Plan) → Phase 9 (Execute, lightweight) |
| "Review my code" | Phase 10 (Code Review) |
| "Fix this bug" | Phase 9 (Execute, lightweight TDD) |

### Design Philosophy — Anti-AI-Slop Manifesto

Every design must be **distinctive, intentional, and memorable** for mobile. The #1 enemy is "AI slop" — the generic, indistinguishable aesthetic.

#### Commit to a Bold Aesthetic Direction

Before touching code, pick a **specific** aesthetic and execute it with conviction:
- **Platform fit**: iOS vs Android vs cross-platform — each has its own design language
- **Tone**: Clean iOS-native, Material You expressive, playful/game-like, luxury/premium, dark/editorial, neon/cyberpunk... Don't default to "clean and modern."
- **Differentiation**: What's the ONE thing someone will remember about this app?

#### The "Never Do" List (Mobile AI Slop Indicators)

| ❌ Never Do This | ✅ Do This Instead |
|---|---|
| Default system fonts everywhere | SF Pro for iOS, Roboto for Android, distinctive display fonts for branding |
| Blue-on-white flat cards | Commit to a visual style with depth and character |
| Generic list-based layout for everything | Cards, grids, horizontal scrolls, custom layouts that fit the content |
| Ignoring safe areas | `SafeAreaView` / `SafeAreaProvider` from line 1 |
| Web-style navigation (top nav) | Bottom tab bar or drawer for mobile navigation |
| Same layout for iOS and Android | Adapt to each platform's conventions where it matters |

---

## Phase 1 — Design Research

### 1.1 Purpose

Understand the project requirements and search the design database for matching mobile styles, colors, typography, and UX patterns. Produces a **Mobile Design Brief**.

### 1.2 The Design Database

| Domain | Records | What It Contains |
|--------|---------|-----------------|
| `style` | 67+ | UI style categories with keywords, colors, effects, best-for/avoid, framework compatibility |
| `color` | 161 | Color palettes with hex codes, mood, industry fit, accessibility notes |
| `typography` | 57 | Font pairings with Google Fonts links, CSS imports, use cases |
| `product` | 161 | Product-type rules: style priority, color mood, typography mood, key effects, anti-patterns |
| `ux` | 99 | UX guidelines including mobile navigation, touch, gestures, loading patterns |
| `google-fonts` | 1900+ | Full Google Fonts catalog |

### 1.3 Search Commands

```bash
# Domain search
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain> --max-results <N>

# Mobile stack-specific snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack reactnative
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack flutter
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack swiftui

# Design system generation
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<Project Name>"

# Mobile-relevant UX search
python3 src/ui-ux-pro-max/scripts/search.py "mobile navigation" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "touch gestures" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "mobile forms" --domain ux
```

### 1.4 Research Process

**Step A — Understand the Project**

Gather from the user:
- What are you building? (app type, core features, target audience)
- What's the vibe? (clean iOS feel, dark & bold, playful, premium...)
- Framework preference? (React Native+Expo / Flutter / SwiftUI / "you pick")
- Target platform? (iOS only / Android only / both)
- Any existing brand assets? (colors, fonts, logos)
- Any design references?

Then **commit to a bold aesthetic direction**:
- Platform fit: does this feel more iOS-native, Material You, or brand-first cross-platform?
- What single visual element will make users remember this app?

**Step B — Search the Database**

1. **Product type search**: `python3 src/ui-ux-pro-max/scripts/search.py "<app type>" --domain product`
2. **Style search**: `python3 src/ui-ux-pro-max/scripts/search.py "<keywords>" --domain style --max-results 5`
3. **Color search**: `python3 src/ui-ux-pro-max/scripts/search.py "<mood/industry>" --domain color --max-results 5`
4. **Typography search**: `python3 src/ui-ux-pro-max/scripts/search.py "<style/mood>" --domain typography --max-results 3`
5. **Mobile UX patterns**: `python3 src/ui-ux-pro-max/scripts/search.py "mobile navigation" --domain ux`

**Step C — Compose the Mobile Design Brief**

```markdown
## Mobile Design Brief: <App Name>

### Project
- **Type:** <e.g., Fitness Tracker, Social App, E-commerce>
- **Framework:** <React Native+Expo / Flutter / SwiftUI>
- **Target platform:** <iOS / Android / Both>
- **Target audience:** <who>

### Style Direction
- **Primary style:** <from search results>
- **Platform feel:** <iOS-native / Material / Brand-first>

### Color Palette
- **Primary / Secondary / Accent / Neutrals / Semantic colors**

### Typography
- **Display font / Body font** — platform-appropriate defaults + brand fonts

### Mobile UX Patterns
- Navigation: <Bottom Tab / Drawer / Stack-only>
- Key interactions: <gestures, transitions>
- Platform notes: <iOS vs Android differences>

### Anti-Patterns to Avoid
```

### 1.5 Quality Criteria

- [ ] App type identified and searched
- [ ] At least 3 style options considered
- [ ] Color palette complete (primary, secondary, accent, neutrals, semantic)
- [ ] Font pairing selected, platform-appropriate
- [ ] Mobile UX patterns identified (navigation, touch, gestures)
- [ ] Platform notes documented (iOS vs Android differences)
- [ ] User approved the Mobile Design Brief

---

## Phase 2 — Mobile Design System

### 2.1 Purpose

Generate a complete token-based design system adapted for mobile. Three-layer architecture plus mobile-specific extensions.

### 2.2 Token Architecture

#### Layer 1 — Primitive Tokens

```js
// React Native example
const primitives = {
  colors: {
    blue500: '#3b82f6',
    gray900: '#111827',
    white: '#ffffff',
  },
  spacing: {
    1: 4, 2: 8, 3: 12, 4: 16, 6: 24, 8: 32,
  },
  fontSize: {
    xs: 12, sm: 14, base: 16, lg: 18, xl: 20, '2xl': 24, '3xl': 30,
  },
  radius: {
    sm: 4, md: 8, lg: 12, xl: 16, full: 9999,
  },
}
```

#### Layer 2 — Semantic Tokens

```js
const semantic = {
  colors: {
    primary: primitives.colors.blue500,
    textPrimary: primitives.colors.gray900,
    surface: primitives.colors.white,
    success: '#22c55e',
    error: '#ef4444',
  },
  spacing: {
    section: primitives.spacing[8],
    component: primitives.spacing[6],
    element: primitives.spacing[4],
  },
}
// Dark mode variants
const dark = {
  colors: {
    textPrimary: primitives.colors.white,
    surface: '#1a1a2e',
  },
}
```

#### Layer 3 — Component Tokens

```js
const components = {
  button: {
    heightSm: 36, heightMd: 44, heightLg: 52,  // iOS minimum = 44pt
    paddingX: primitives.spacing[4],
    radius: primitives.radius.md,
  },
  card: {
    padding: primitives.spacing[4],
    radius: primitives.radius.lg,
  },
  input: {
    height: 48,  // Android minimum = 48dp
    paddingX: primitives.spacing[3],
    radius: primitives.radius.md,
  },
}
```

### 2.3 Mobile-Specific Additions

**Touch Targets (non-negotiable)**

| Platform | Minimum | Recommended |
|----------|---------|-------------|
| iOS | 44×44pt | 48×48pt |
| Android | 48×48dp | 56×56dp |

Every tappable element must meet the minimum. No exceptions.

**Safe Area Insets**

Always use `SafeAreaProvider` + `SafeAreaView` from `react-native-safe-area-context`:
```jsx
import { SafeAreaProvider, SafeAreaView } from 'react-native-safe-area-context'

// Wrap app root
<SafeAreaProvider>
  <SafeAreaView edges={['top', 'bottom']}>
    ...
  </SafeAreaView>
</SafeAreaProvider>
```

**Platform-Specific Typography**

```js
const fontFamily = {
  iOS: { system: 'SF Pro', display: '-apple-system' },
  android: { system: 'Roboto', display: 'Roboto' },
  // Override with brand font from design brief
}
```

**Navigation Patterns**

| Pattern | When to Use |
|---------|-------------|
| Bottom Tab Bar | Primary navigation (2-5 tabs), content-focused apps |
| Stack Navigator | Drill-down flows (list → detail, checkout) |
| Drawer | Secondary navigation, settings-heavy apps |
| Modal Stack | Full-screen overlays, onboarding |

### 2.4 Quality Criteria

- [ ] All three token layers defined
- [ ] Touch targets meet platform minimums (44pt iOS / 48dp Android)
- [ ] Safe area tokens/usage defined
- [ ] Platform-appropriate typography specified
- [ ] Navigation pattern chosen and justified
- [ ] Dark mode variants included

---

## Phase 3 — Mobile UI Implementation

### 3.1 Purpose

Build each screen using the mobile design system, following the style direction from the Design Brief and mobile-first UX patterns.

### 3.2 Setup: React Native + Expo (default)

```bash
npx create-expo-app@latest <project-name> --template blank-typescript
cd <project-name>
npx expo install react-native-safe-area-context react-native-screens
npx expo install @react-navigation/native @react-navigation/bottom-tabs @react-navigation/stack
```

### 3.3 Setup: Flutter

```bash
flutter create <project_name>
cd <project_name>
# Add dependencies to pubspec.yaml
```

### 3.4 Implementation Rules

| Rule | Details |
|------|---------|
| **SafeAreaView from line 1** | Never skip safe area handling |
| **No hardcoded colors** | Always use design tokens |
| **No hardcoded spacing** | Use spacing scale from tokens |
| **Touch targets ≥ 44pt/48dp** | Every button, list item, icon tap area |
| **Platform.OS checks** | Handle iOS vs Android differences explicitly |
| **Keyboard handling** | `KeyboardAvoidingView` + scroll into view for all inputs |
| **Loading states** | Skeleton screens or activity indicators — never empty screens |
| **Error states** | Every network/async operation has an error UI |

### 3.5 Navigation Implementation (React Native)

```jsx
import { NavigationContainer } from '@react-navigation/native'
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs'

const Tab = createBottomTabNavigator()

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="Home" component={HomeScreen} />
        <Tab.Screen name="Profile" component={ProfileScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  )
}
```

### 3.6 Performance Rules

- Use `FlatList` or `SectionList` for all lists > 20 items (never `ScrollView` + `map`)
- `useCallback` for event handlers passed to list items
- `React.memo` for pure list item components
- Images: use `expo-image` (better caching than built-in `Image`)
- Animations: `react-native-reanimated` for 60fps (never JS-thread animations for UI)

### 3.7 Quality Criteria

- [ ] All screens use `SafeAreaView` correctly
- [ ] All colors use design tokens
- [ ] All touch targets meet platform minimums
- [ ] Platform differences handled (iOS vs Android)
- [ ] Keyboard handling implemented for all input screens
- [ ] Loading and error states for all async operations
- [ ] Lists use `FlatList` (never `ScrollView` + map)
- [ ] Animations use `react-native-reanimated` (not `Animated` API)

---

## Phase 4 — Brand & Polish

### 4.1 App Icon & Splash Screen

```bash
# Expo: set in app.json
{
  "expo": {
    "icon": "./assets/icon.png",          // 1024x1024
    "splash": {
      "image": "./assets/splash.png",     // 1284x2778 (iPhone 14 Pro Max)
      "backgroundColor": "<primary color from design tokens>"
    }
  }
}
```

Design icon using design system colors and typography — must be recognizable at 60×60pt.

### 4.2 Platform-Specific Polish

**iOS:**
- Use `SF Symbols` for system icons (matches iOS language)
- Haptic feedback for key interactions: `expo-haptics`
- `blur` effects for bottom sheets/modals

**Android:**
- Material You color system integration (Android 12+)
- Ripple feedback on touchables: `android_ripple` prop
- Proper status bar color: `expo-status-bar`

### 4.3 Consistency Checklist

- [ ] App icon matches design system colors
- [ ] Splash screen uses primary color
- [ ] Typography hierarchy consistent across all screens
- [ ] All buttons have the same shape and size system
- [ ] Navigation chrome (tab bar, header) styled consistently
- [ ] Dark mode consistent across all screens

---

## Phase 5 — Review

### 5.1 Mobile Accessibility Review

**Touch & Interaction:**
- [ ] All touch targets ≥ 44pt (iOS) / 48dp (Android)
- [ ] Touch targets have adequate spacing (≥ 8pt between targets)
- [ ] Buttons respond to tap (not just press-and-release)

**Visual:**
- [ ] Color contrast ≥ 4.5:1 for normal text, ≥ 3:1 for large text
- [ ] Color is NOT the only indicator of state
- [ ] Text readable without zooming (≥ 14pt body, ≥ 11pt caption)

**Screen Reader:**
- [ ] All interactive elements have `accessibilityLabel`
- [ ] Images have `accessibilityLabel` (or `accessibilityElementsHidden` if decorative)
- [ ] Custom components have correct `accessibilityRole`

**Keyboard/Input:**
- [ ] All inputs have `accessibilityLabel`
- [ ] Correct `keyboardType` and `returnKeyType` for each input
- [ ] `autocomplete` / `textContentType` set for common inputs (email, password, etc.)

### 5.2 UX Guidelines Review

```bash
python3 src/ui-ux-pro-max/scripts/search.py "mobile navigation" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "mobile forms" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "loading states" --domain ux
python3 src/ui-ux-pro-max/scripts/search.py "gesture interactions" --domain ux
```

### 5.3 Platform Convention Review

**iOS:**
- [ ] Navigation follows iOS conventions (back swipe gesture works)
- [ ] Large Title mode used appropriately
- [ ] Action sheets use iOS style (bottom modal)
- [ ] Haptics on key interactions

**Android:**
- [ ] Back button/gesture behavior correct
- [ ] Bottom sheet for actions (not alert dialogs for lists)
- [ ] Ripple feedback on all touchables
- [ ] Status bar color appropriate

### 5.4 Performance Review

| Area | Target |
|------|--------|
| App launch time | < 2s cold start |
| Screen transitions | 60fps |
| List scroll | No dropped frames |
| Image loading | Cached after first load |

### 5.5 Output

Review report categorized by:
- **Must Fix** — accessibility violations, crashes, platform convention violations
- **Should Fix** — UX guideline violations, performance issues, missing states
- **Nice to Have** — polish items, micro-interactions

---

## Phase 6 — Deliver (Design)

### 6.1 Deliverables

1. **Working app** — All screens implemented with design system
2. **Design System docs** (`DESIGN-SYSTEM.md`) — Tokens, components, platform notes
3. **Mobile Design Brief** — Research findings and decisions
4. **Review Report** — Accessibility and platform audit results

### 6.2 Completion Summary Format

```markdown
## ✅ Mobile Design Complete

**App:** <name>
**Framework:** <React Native+Expo / Flutter / SwiftUI>
**Platforms:** <iOS / Android / Both>
**Style:** <primary style>

### What Was Built
- <screen list>
- Design system: N tokens, touch targets compliant
- Platform adaptations: iOS + Android ✅

### Design Decisions
- **Style / Palette / Typography / Navigation:** <why>

### Quality
- Touch targets: ≥ 44pt iOS / ≥ 48dp Android ✅
- Safe areas: all screens ✅
- Accessibility: screen reader labels ✅
- Dark mode: ✅
```

### 6.3 Next Steps to Offer

1. Iterate — colors, typography, layout adjustments
2. Extend — add more screens
3. Continue to Phase 7 — build the full app with TDD + Expo preview

---

## Phase 7 — Brainstorm & Requirements

### 7.1 Purpose

Transform a vague idea or design into a clear, actionable specification for the engineering build phase.

### 7.2 Process

**Step A — Understand the Problem**
- What problem are you solving? (the "why")
- Who is the user? (the "who")
- What does success look like? (the "what")

Do NOT propose solutions yet. Listen first.

**Step B — Explore the Solution Space**

Propose 2-3 distinct approaches with trade-offs. For each:
- **How it works** (1-2 sentences)
- **Pros / Cons**
- **Effort estimate** (S/M/L)

**Step C — Nail Down the Spec**

- **Functional requirements** — numbered list
- **Non-functional requirements** — performance, offline behavior, platform targets
- **Out of scope** — what we explicitly will NOT build
- **Open questions**

**Step D — Confirm**

Get explicit user approval before moving on.

### 7.3 Quality Criteria

- [ ] Problem statement is clear and specific
- [ ] At least 2 approaches were considered
- [ ] Functional requirements are numbered and testable
- [ ] Platform targets confirmed (iOS / Android / Both)
- [ ] User explicitly approved the spec

---

## Phase 8 — Write Implementation Plan

### 8.1 Plan Structure

```markdown
# Implementation Plan: <App/Feature Name>

## Context
<what we're building and why>

## Architecture
<Screen structure, navigation hierarchy, state management approach, API/data layer>

## Tasks

### Task 1: <Name>
**Files:** `src/screens/HomeScreen.tsx`, `src/components/Card.tsx`
**Tests:** `src/__tests__/HomeScreen.test.tsx`
**Description:** <what to do>
**Acceptance criteria:**
- [ ] Screen renders without error
- [ ] Navigation to detail screen works

## Task Dependencies
## Risks & Mitigations
```

### 8.2 Mobile-Specific Plan Elements

Every plan for a mobile app should include:
- **Screen list** with navigation structure (which navigator: Stack/Tab/Drawer)
- **State management decision** (useState / Zustand / Redux — justified for project scope)
- **API/data layer** (mock data first, real API later)
- **Platform-specific tasks** (if iOS+Android, note which tasks need Platform.OS)
- **Expo setup tasks** (dependencies, app.json config)

### 8.3 Quality Criteria

- [ ] Every requirement maps to at least one task
- [ ] Every task has files, tests, and acceptance criteria
- [ ] Navigation architecture is explicit (navigator types chosen)
- [ ] State management approach decided
- [ ] Task order respects dependencies
- [ ] User approved the plan

---

## Phase 9 — Execute with TDD

### 9.1 The TDD Iron Law

```
NEVER write implementation code without a failing test that demands it.
```

Cycle: **Red → Green → Refactor**, no exceptions.

### 9.2 Task Execution Protocol

```
1. Read task description and acceptance criteria
2. Examine existing code referenced by the task
3. Write test(s) for the first acceptance criterion
4. Run tests → confirm RED
5. Write minimal implementation → confirm GREEN
6. Refactor if needed → confirm still GREEN
7. Repeat for remaining acceptance criteria
8. Mark task complete in plan.md
9. Git commit: "Task N: <description>"
```

### 9.3 Mobile Testing Strategy

| What to Test | How |
|-------------|-----|
| Screen renders | React Native Testing Library: `render(<Screen />)` |
| Navigation | Mock navigation object: `jest.fn()` |
| API calls | Mock fetch/axios at boundary |
| State changes | Test state via rendered output |
| Platform behavior | Mock `Platform.OS` in tests |

```bash
# Run tests
npx jest
npx jest --watch   # watch mode

# React Native Testing Library
npm install --save-dev @testing-library/react-native @testing-library/jest-native
```

### 9.4 Commit Discipline

- Commit after each task: `git commit -m "Task N: <description>"`
- Never commit failing tests
- One task per commit

### 9.5 Quality Criteria

- [ ] Every task has at least one test before implementation
- [ ] All tests pass after each task
- [ ] Navigation flows tested
- [ ] Platform-specific behavior tested
- [ ] Commits are per-task

---

## Phase 10 — Code Review

### 10.1 Review Process

**Step A — Plan Compliance**: All acceptance criteria met, tests pass, no unplanned changes

**Step B — Code Quality**

| Area | What to Check |
|------|---------------|
| **Correctness** | Does it do what the plan says? Edge cases handled? |
| **Mobile-specific** | SafeAreaView, touch targets, Platform.OS handling correct? |
| **Performance** | FlatList used for lists, no JS-thread animations, no excessive re-renders |
| **Error handling** | Network errors, empty states, loading states all handled |
| **Testing** | Tests meaningful, not just smoke tests |
| **Accessibility** | accessibilityLabel on interactive elements |
| **Security** | No secrets hardcoded, sensitive data stored in SecureStore |

**Step C — Issue Classification**

| Severity | Action |
|----------|--------|
| **Critical** | Crash, data loss, accessibility violation, missing safe area → must fix |
| **Important** | Performance issue, missing error state, platform bug → should fix |
| **Minor** | Style preference, naming → note but don't block |

### 10.2 Output

Review summary in chat: tasks reviewed, issues by severity, actions taken.

---

## Phase 11 — Verification

### 11.1 Verification Protocol

**Test Suite**
```bash
npx jest --coverage
# Confirm: ALL tests pass, zero failures
```

**Plan Checklist**: Each task marked complete, acceptance criteria verified

**Integration Sanity**: Start Expo dev server, load on device/simulator, exercise happy path and key error cases

**Platform Check** (if cross-platform):
- Test on iOS simulator (or device)
- Test on Android emulator (or device)
- Verify navigation, safe areas, platform-specific UI on both

### 11.2 Completion Report

```markdown
## ✅ Implementation Complete

**App:** <name>
**Tasks:** N/N completed
**Tests:** X passing, 0 failing

### What was built
### What to try
### Known limitations
```

---

## Phase 12 — Handoff & Preview

### 12.1 Expo Preview

```bash
# Start Expo dev server
npx expo start

# QR code will appear — scan with Expo Go app (iOS App Store / Google Play)
# Or use start_mobile_service tool for a hosted preview URL
```

Use `start_mobile_service` tool to generate a hosted preview URL + QR code.

### 12.2 Handoff Checklist

- [ ] Completion report delivered
- [ ] `plan.md` updated with all tasks marked done
- [ ] Code committed, no uncommitted changes
- [ ] Expo Go QR code / mobile_url delivered
- [ ] Design system summary delivered (tokens, palette, fonts, platform notes)
- [ ] Build guide offered (TestFlight / Play Store if requested)

### 12.3 Next Steps to Offer

1. **Iterate** — Adjust design, add animations, polish platform feel
2. **Extend** — Add more screens or features (restart from Phase 7)
3. **Deploy** — TestFlight (iOS) / Play Store (Android) build guide
4. **Review together** — Walk through the codebase
5. **Done** — App delivered ✅

---

## Technical Reference

### Design Tool Quick Reference

```bash
# Style / color / typography / product / ux search
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain>

# Mobile stack-specific snippets
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack reactnative
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack flutter
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --stack swiftui

# Full design system generation
python3 src/ui-ux-pro-max/scripts/search.py "<query>" --design-system -p "<Project>"
```

### Database Files

All CSV data lives in `src/ui-ux-pro-max/data/`:

| File | Records |
|------|---------|
| `styles.csv` | 67+ |
| `colors.csv` | 161 |
| `typography.csv` | 57 |
| `products.csv` | 161 |
| `ux-guidelines.csv` | 99 |
| `google-fonts.csv` | 1900+ |

### Engineering Workflow Decision Tree

```
User request arrives
  │
  ├─ Vague idea? ──────────── Phase 7 (Brainstorm)
  ├─ Clear requirements? ──── Phase 8 (Plan)
  ├─ Plan exists? ──────────── Phase 9 (Execute)
  ├─ Code needs review? ───── Phase 10 (Review)
  ├─ Bug fix / small tweak? ─ Phase 9 (lightweight TDD)
  └─ Quick question? ──────── Answer directly
```

### Scaling Rules

| Task Size | Brainstorm | Plan | TDD | Review | Verify |
|-----------|-----------|------|-----|--------|--------|
| Tiny (< 5 min) | Skip | Skip | Regression test only | Skip | Run tests |
| Small (5-30 min) | Verbal | Mental | Full TDD | Self-review | Run tests |
| Medium (30min–2hr) | Brief | plan.md | Full TDD | Full review | Full verification |
| Large (> 2 hr) | Full spec.md | Detailed plan.md | Full TDD + per-task commits | Full review | Full verification + platform test |

### Git Workflow

```bash
git checkout -b feature/<feature-name>
# After each task:
git add -A && git commit -m "Task N: <description>"
git push origin feature/<feature-name>
```

### Debugging Protocol

1. **Stop and observe** — Read the error completely. Don't guess.
2. **Form a hypothesis** — "I think the failure is because X."
3. **Find evidence** — Add logging, read the code path. Don't change code yet.
4. **Confirm or revise** — Update hypothesis if evidence doesn't fit.
5. **Fix the root cause** — Not the symptom.
6. **Write a regression test** — Prove the bug is fixed.
7. **Check for siblings** — Does the same pattern exist elsewhere?

### Common Mobile Gotchas

| Gotcha | Solution |
|--------|----------|
| Content hidden behind notch | Use `SafeAreaView` with `edges` prop |
| Keyboard covers inputs | `KeyboardAvoidingView` + `behavior="padding"` |
| List performance | `FlatList` with `keyExtractor`, never `ScrollView` + map |
| Image flicker | `expo-image` with `cachePolicy="memory-disk"` |
| Android back button exits app | Handle with `BackHandler` or navigation |
| Platform font mismatch | Explicitly set `fontFamily` per platform |
| Touch target too small | Increase `hitSlop` or pad the container |
| 60fps animation drops | Use `react-native-reanimated` worklets |

### Quality Maxims

**Design:**
1. "Search, don't guess." — Use the design database before making design decisions.
2. "Tokens, not values." — Never hardcode a color, spacing, or font.
3. "Platform-first, brand-second." — Make it feel native first, branded second.
4. "Touch is not click." — Every interaction must be designed for a finger, not a cursor.

**Engineering:**
1. "If you can't test it, you don't understand it." — Go back to requirements.
2. "Make it work, make it right, make it fast." — In that order.
3. "The plan is not the territory." — If reality diverges, update the plan.
4. "Every bug is a missing test." — Write the test first, then fix.
5. "Done means verified on device." — Not in the simulator only.
