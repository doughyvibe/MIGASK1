# Tasks: Implement MIGA MVP - Mindful Gateway Timer and Content System

**Input**: Design documents from `/specs/001-miga-mvp/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Following Constitution III (Test-First iOS Development), unit and UI tests are integrated into each user story phase.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

Following MVVM iOS native architecture from plan.md:
- **Models**: `MIGASK1/Models/` (Core Data entities)
- **ViewModels**: `MIGASK1/ViewModels/` (Observable state management)
- **Views**: `MIGASK1/Views/` (SwiftUI views)
- **Services**: `MIGASK1/Services/` (Business logic, Firebase, URL schemes)
- **Tests**: `MIGASK1Tests/` (Unit tests), `MIGASK1UITests/` (UI tests)

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Xcode project configuration and basic project structure

- [ ] T001 Configure Core Data model file (MIGASK1.xcdatamodeld)
- [ ] T002 Add SwiftUI MVVM folder structure per plan.md specification
- [ ] T003 Configure Firebase iOS SDK (Podfile/Package.swift)
- [ ] T004 Setup StoreKit preparation (minimal entitlement configuration)
- [ ] T005 Initialize test targets with XCTest/XCUITest configuration

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T006 Implement Core Data persistent container setup (MIGASK1/CoreDataStack.swift)
- [ ] T007 Create base SocialApp entity model (MIGASK1/Models/SocialApp.swift)
- [ ] T008 Create base MindfulnessContent entity model (MIGASK1/Models/MindfulnessContent.swift)
- [ ] T009 Create base GatewaySession entity model (MIGASK1/Models/GatewaySession.swift)
- [ ] T010 Create base UserPreferences entity model (MIGASK1/Models/UserPreferences.swift)
- [ ] T011 Create base UsageStatistics entity model (MIGASK1/Models/UsageStatistics.swift)
- [ ] T012 [P] Configure Firebase initialization (MIGASK1/FirebaseConfig.swift)
- [ ] T013 [P] Implement URL scheme validation service (MIGASK1/Services/URLSchemeService.swift)
- [ ] T014 Create bundled mindfulness content JSON (MIGASK1/Resources/Content.bundle/)

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Experience Mindful Pause (Priority: P1) 🎯 MVP

**Goal**: Users can experience a 15-45 second mindful pause with reflective content before launching social media apps

**Independent Test**: Tap any configured social app and complete gateway timer interaction successfully

### Implementation for User Story 1

- [ ] T015 [US1] Create GatewayViewModel with timer logic (MIGASK1/ViewModels/GatewayViewModel.swift)
- [ ] T016 [P] [US1] Create MindfulnessContentService (MIGASK1/Services/ContentService.swift)
- [ ] T017 [P] [US1] Create ContentManager for cache/bundled logic (MIGASK1/Services/ContentManager.swift)
- [ ] T018 [US1] Implement GatewayScreen SwiftUI view (MIGASK1/Views/GatewayScreen.swift)
- [ ] T019 [US1] Add circular timer UI component (MIGASK1/Views/Components/CircularTimerView.swift)
- [ ] T020 [US1] Implement content display view (MIGASK1/Views/Components/ContentDisplayView.swift)
- [ ] T021 [US1] Add gateway dismissal actions (Exit, Deepen, Proceed) (MIGASK1/ViewModels/GatewayViewModel.swift)
- [ ] T022 [US1] Integrate URL scheme launching for proceed action (MIGASK1/Services/URLSchemeService.swift)
- [ ] T023 [US1] Add session analytics tracking (MIGASK1/Models/GatewaySession+Analytics.swift)

### Tests for User Story 1

- [ ] T024 [P] [US1] Unit tests for GatewayViewModel timer logic (MIGASK1Tests/GatewayViewModelTests.swift)
- [ ] T025 [P] [US1] Unit tests for ContentService operations (MIGASK1Tests/ContentServiceTests.swift)
- [ ] T026 [US1] UI tests for gateway timer completion (MIGASK1UITests/GatewayScreenTests.swift)

**Checkpoint**: Gateway pause experience fully functional and independently testable

---

## Phase 4: User Story 2 - Personalize App Selection (Priority: P2)

**Goal**: New users can select social media apps to monitor and complete manual iOS app hiding setup

**Independent Test**: Complete onboarding flow and see selected apps appear in MIGA Hub with hidden app intercept active

### Implementation for User Story 2

- [ ] T027 [US2] Create OnboardingViewModel with app selection logic (MIGASK1/ViewModels/OnboardingViewModel.swift)
- [ ] T028 [P] [US2] Create app selection UI with pill-style buttons (MIGASK1/Views/Components/AppSelectionView.swift)
- [ ] T029 [US2] Implement step-by-step manual setup instructions (MIGASK1/Views/OnboardingViews/SetupInstructionsView.swift)
- [ ] T030 [US2] Create OnboardingScreen container view (MIGASK1/Views/OnboardingScreen.swift)
- [ ] T031 [US2] Add onboarding state persistence (MIGASK1/Models/UserPreferences+Onboarding.swift)
- [ ] T032 [US2] Implement app hiding instruction UI for device types (MIGASK1/Views/OnboardingViews/AppHidingInstructions.swift)
- [ ] T033 [US2] Add onboarding completion flow to main hub transition

### Tests for User Story 2

- [ ] T034 [P] [US2] Unit tests for OnboardingViewModel state transitions (MIGASK1Tests/OnboardingViewModelTests.swift)
- [ ] T035 [US2] UI tests for onboarding completion flow (MIGASK1UITests/OnboardingFlowTests.swift)

**Checkpoint**: Complete onboarding experience functional and independently testable

---

## Phase 5: User Story 3 - Review Mindful Progress (Priority: P3)

**Goal**: Users can view statistics dashboard showing mindful pause metrics and behavioral insights

**Independent Test**: Complete gateway sessions and view lifetime statistics including streaks and intentionality score

### Implementation for User Story 3

- [ ] T036 [US3] Create StatisticsViewModel with metric calculations (MIGASK1/ViewModels/StatisticsViewModel.swift)
- [ ] T037 [P] [US3] Implement usage data aggregation service (MIGASK1/Services/StatisticsService.swift)
- [ ] T038 [P] [US3] Create StatisticsScreen with KPI cards (MIGASK1/Views/StatisticsScreen.swift)
- [ ] T039 [US3] Add 30-day trend chart component (MIGASK1/Views/Components/TrendChartView.swift)
- [ ] T040 [US3] Implement streak calculation logic (MIGASK1/Models/UsageStatistics+Streaks.swift)
- [ ] T041 [US3] Add intentionality score calculation (MIGASK1/ViewModels/StatisticsViewModel+Analytics.swift)
- [ ] T042 [US3] Create motivational banner view for streaks (MIGASK1/Views/Components/StreakBannerView.swift)
- [ ] T043 [US3] Add hub screen integration for banner display (MIGASK1/Views/HubScreen+Banners.swift)

### Tests for User Story 3

- [ ] T044 [P] [US3] Unit tests for StatisticsViewModel calculations (MIGASK1Tests/StatisticsViewModelTests.swift)
- [ ] T045 [US3] UI tests for statistics display accuracy (MIGASK1UITests/StatisticsScreenTests.swift)

**Checkpoint**: Progress tracking and insights fully functional and independently testable

---

## Phase 6: User Story 4 - Customize Mindfulness Experience (Priority: P3)

**Goal**: Users can adjust timer duration and content preferences through settings

**Independent Test**: Modify timer duration and content categories, verify changes persist in gateway experiences

### Implementation for User Story 4

- [ ] T046 [US4] Create SettingsViewModel with preference management (MIGASK1/ViewModels/SettingsViewModel.swift)
- [ ] T047 [US4] Implement SettingsScreen with configuration options (MIGASK1/Views/SettingsScreen.swift)
- [ ] T048 [US4] Add timer duration slider UI (MIGASK1/Views/Components/TimerDurationSlider.swift)
- [ ] T049 [US4] Create content category selection view (MIGASK1/Views/Components/ContentCategoryView.swift)
- [ ] T050 [US4] Implement app management toggle list (MIGASK1/Views/Components/AppManagementView.swift)
- [ ] T051 [US4] Add settings persistence integration (MIGASK1/Models/UserPreferences+Settings.swift)
- [ ] T052 [US4] Integrate StoreKit trial display logic (MIGASK1/ViewModels/SettingsViewModel+StoreKit.swift)

### Tests for User Story 4

- [ ] T053 [P] [US4] Unit tests for SettingsViewModel preference updates (MIGASK1Tests/SettingsViewModelTests.swift)
- [ ] T054 [US4] UI tests for settings persistence verification (MIGASK1UITests/SettingsScreenTests.swift)

**Checkpoint**: All user stories independently functional and testable

---

## Phase 7: Integration & Main Hub (Cross-Story Integration)

**Purpose**: Connect all user stories through main tab navigation and Firebase integration

- [ ] T055 Create main AppTabView navigation container (MIGASK1/Views/AppTabView.swift)
- [ ] T056 Implement HubScreen with app icon grid (MIGASK1/Views/HubScreen.swift)
- [ ] T057 Add gateway trigger from hub app taps (MIGASK1/Views/HubScreen+Gateway.swift)
- [ ] T058 [P] Implement Firebase content sync (optional) (MIGASK1/Services/FirebaseSyncService.swift)
- [ ] T059 [P] Add Firebase analytics event tracking (MIGASK1/Services/AnalyticsService.swift)
- [ ] T060 Create bundle resources for default content (MIGASK1/Resources/DefaultContent.json)
- [ ] T061 Add tab bar item configurations (MIGASK1/Views/TabBarItems.swift)

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Final improvements, testing, and production readiness

- [ ] T062 [P] Add offline connection state handling throughout app
- [ ] T063 [P] Implement comprehensive error recovery mechanisms
- [ ] T064 Add VoiceOver and Dynamic Type accessibility support
- [ ] T065 Implement mindful UX animations and transitions (per Constitution I)
- [ ] T066 Add Firebase performance monitoring integration
- [ ] T067 Create app icons and launch screen assets in asset catalog
- [ ] T068 [P] Performance optimization and memory management review
- [ ] T069 Add final crash reporting and error logging configuration
- [ ] T070 Execute TestFlight preparation checklist from quickstart.md

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-6)**: All depend on Foundational phase completion
  - Stories can proceed in parallel (if multiple developers)
  - Or sequentially: US1 → US2 → US3 → US4
- **Integration (Phase 7)**: Depends on all desired user stories
- **Polish (Phase 8)**: Depends on Integration completion

### User Story Dependencies

- **User Story 1 (P1)**: Foundation only - MVP core experience
- **User Story 2 (P2)**: Foundation only - Independent onboarding
- **User Story 3 (P3)**: Foundation only - Independent analytics (will enrich with US1+US2 data)
- **User Story 4 (P3)**: Foundation only - Independent settings management

### Within Each User Story

- Run tests first (if included) before implementation
- ViewModels/Models before Services
- Services before Views
- Core functionality before advanced features

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- Within each user story: UI tests and unit tests parallel
- Different user stories can be developed simultaneously by separate developers
- View creation and ViewModel logic can often proceed in parallel

---

## Parallel Example: User Story 1

```bash
# Launch all tests for User Story 1 together:
Task: "Unit tests for GatewayViewModel timer logic (MIGASK1Tests/GatewayViewModelTests.swift)"
Task: "Unit tests for ContentService operations (MIGASK1Tests/ContentServiceTests.swift)"
Task: "UI tests for gateway timer completion (MIGASK1UITests/GatewayScreenTests.swift)"

# Launch core implementation components:
Task: "Create GatewayViewModel with timer logic (MIGASK1/ViewModels/GatewayViewModel.swift)"
Task: "Create MindfulnessContentService (MIGASK1/Services/ContentService.swift)"
Task: "Implement GatewayScreen SwiftUI view (MIGASK1/Views/GatewayScreen.swift)"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test mindful pause independently - this is your MVP!
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Deployable foundation
2. Add User Story 1 → **MVP release ready** (pause experience)
3. Add User Story 2 → Complete onboarding experience
4. Add User Story 3 → Progress tracking and motivation
5. Add User Story 4 → Full customization and trial monetization

### Constitution Compliance Notes

- **Behavioral Validation**: Each story includes analytics tracking per Principle II
- **Test-First**: Unit tests and UI tests integrated per Principle III
- **Privacy-First**: Local-only data with transparent disclosures per Principle IV
- **Offline-First**: Bundled content + graceful degradation per VII
- **Performance Target**: <2s justified for comprehensive Firebase+content initialization

---

## Task Summary

- **Total Tasks**: 70 (T001-T070)
- **Setup Tasks**: 5 (foundational infrastructure)
- **Foundational Tasks**: 9 (blocking prerequisites)
- **User Story 1 (P1)**: 9 tasks (core MVP - mindful pause)
- **User Story 2 (P2)**: 9 tasks (onboarding flow)
- **User Story 3 (P3)**: 8 tasks (statistics dashboard)
- **User Story 4 (P3)**: 7 tasks (settings customization)
- **Integration Tasks**: 7 tasks (tab navigation, Firebase)
- **Polish Tasks**: 9 tasks (production readiness)

**Suggested MVP Scope**: User Stories 1 + 7 (Integration) to deliver complete mindful pause experience ready for initial testing.
