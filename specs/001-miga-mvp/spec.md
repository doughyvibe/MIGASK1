# Feature Specification: Implement MIGA MVP - Mindful Gateway Timer and Content System

**Feature Branch**: `001-miga-mvp`
**Created**: 2025-11-20
**Status**: Draft
**Input**: User description: "Implement MIGA MVP - Mindful Gateway Timer and Content System"

## Clarifications

### Session 2025-11-20
- Q: Content Strategy: What should be the fallback strategy for first-time users without network access? → A: Option A - Bundle 20-30 curated mindfulness content items (quotes, affirmations) as app assets for immediate offline-first experience
- Q: Data Scale & Session Retention: How should the system handle data persistence scale for session tracking and statistics calculation? → A: Option B - Retain 30 days of raw session data, aggregate older data into daily/weekly summaries
- Q: URL Scheme Integration Strategy: How should the system handle custom or unknown social media apps and potential URL scheme conflicts? → A: Option A - Hardcode only predefined major social apps (Instagram, TikTok, Facebook, etc.) - no custom additions
- Q: MVP Social App Scope: Which social media platforms should be supported in the MVP (prioritized for development effort)? → A: Option A - Comprehensive: Instagram, TikTok, Facebook, Twitter/X, YouTube, Snapchat, LinkedIn, Reddit

## User Scenarios & Testing *(mandatory)*

<!--
  IMPORTANT: User stories should be PRIORITIZED as user journeys ordered by importance.
  Each user story/journey must be INDEPENDENTLY TESTABLE - meaning if you implement just ONE of them,
  you should still have a viable MVP (Minimum Viable Product) that delivers value.
  
  Assign priorities (P1, P2, P3, etc.) to each story, where P1 is the most critical.
  Think of each story as a standalone slice of functionality that can be:
  - Developed independently
  - Tested independently
  - Deployed independently
  - Demonstrated to users independently
-->

### User Story 1 - Experience Mindful Pause (Priority: P1)

A user taps a social media app icon on their MIGA Hub, experiences a calming 15-45 second pause with reflective content, and makes a conscious choice about whether to proceed to the app or exit mindfully.

**Why this priority**: This is the core value proposition of MIGA - the mindful gateway interaction creates the behavioral interceptor that transforms autopilot social media use into conscious decisions. Without this core functionality, MIGA has no unique value.

**Independent Test**: Can be fully tested independently by tapping any app icon and completing or interrupting a timer animation. Delivers immediate value by creating a pause for reflection before social media access.

**Acceptance Scenarios**:

1. **Given** user opens MIGA Hub, **When** they tap an app icon, **Then** a full-screen gateway screen appears with mindfulness content and a 15-second countdown timer
2. **Given** the gateway screen is active, **When** the timer reaches 0:00, **Then** "Proceed to [App Name]" button becomes enabled and leads to external app URL scheme
3. **Given** the gateway screen is active, **When** user taps "Exit to Focus" immediately (no timer wait), **Then** they return to home screen with a success celebration
4. **Given** user taps "Deepen the Pause" during timer, **Then** new content loads and timer resets to full duration
5. **Given** timer is active, **When** user force-quits the app, **Then** mindful pause resets when reopened

---

### User Story 2 - Personalize App Selection (Priority: P2)

A new user completes onboarding, selects which social media apps they want to monitor, manually hides those app icons at device level, and sees their selected apps in the MIGA Hub ready for mindful interception.

**Why this priority**: User personalization creates the foundation for the gateway system. Without selected apps in the Hub, there's no opportunity for mindful interception. This is essential for the app to have usable, meaningful functionality.

**Independent Test**: Can be fully tested independently by completing the app selection flow and verifying selected apps appear in MIGA Hub. Delivers clear value by showing users how their personalized mindfulness system works.

**Acceptance Scenarios**:

1. **Given** first app launch, **When** user advances through onboarding, **Then** introduction to MIGA purpose and value proposition is displayed
2. **Given** onboarding phase 1 complete, **When** user selects social apps (minimum 1), **Then** they proceed to phase 2 manual setup instructions
3. **Given** phase 2 instruction screen displays, **When** user taps "Ready to begin", **Then** MIGA closes allowing manual iOS app icon hiding
4. **Given** user returns after manual setup, **When** they reopen MIGA for first time, **Then** selected apps appear as tappable icons in Hub with gateway interception active
5. **Given** Hub is populated, **When** user taps Hub icon for configured app, **Then** gateway screen appears instead of direct app launch

---

### User Story 3 - Review Mindful Progress (Priority: P3)

A user views their statistics dashboard showing mindful pause experiences, exit rates, time reclaimed, and behavioral insights to understand the impact of their intentional digital habits.

**Why this priority**: While the core gateway delivers value, progress tracking enables users to see the effectiveness of their mindfulness practice, maintain motivation through quantified benefits, and validate the behavioral change hypothesis.

**Independent Test**: Can be fully tested independently by viewing analytics after multiple gateway interactions. Delivers value by providing data-driven insights into digital wellbeing improvements.

**Acceptance Scenarios**:

1. **Given** user has completed gateway interactions, **When** they open Statistics tab, **Then** they see lifetime metrics including "Hours Reclaimed" and "Total Conscious Exits"
2. **Given** multiple days of usage, **When** user views statistics, **Then** they see a 30-day trend chart showing "Exit vs. Attempts" ratio over time
3. **Given** streak of consecutive days, **When** user opens Hub, **Then** motivational banner shows "X-day Control Streak" in prominent top position
4. **Given** sufficient usage data, **When** user views stats, **Then** "Intentionality Score" displays as ratio of mindful exits to total pauses
5. **Given** stats show, **When** user views footer, **Then** completion message "Keep choosing wisely" appears as supportive sign-off

---

### User Story 4 - Customize Mindfulness Experience (Priority: P3)

A user adjusts gateway timer duration, content preferences, and app management through the Settings panel to personalize their mindful pause experience.

**Why this priority**: While the default 15-second timer works, user customization enables optimal pausing duration for different contexts and personalities. Settings provide essential configuration without compromising the core mindful intervention flow.

**Independent Test**: Can be fully tested independently by adjusting settings and verifying changes persist in subsequent gateway interactions. Delivers practical value through user autonomy and experience tailoring.

**Acceptance Scenarios**:

1. **Given** user accesses Settings tab, **When** they adjust pause duration slider, **Then** gateway timer reflects new value (15-45s range) in all future interactions
2. **Given** Settings are open, **When** user selects content categories, **Then** gateway screen prioritizes selected interests (Stoicism, Productivity, etc.) in content rotation
3. **Given** Settings are open, **When** user manages app list, **Then** they can add/remove social apps from Hub without losing existing statistics
4. **Given** initial trial period active, **When** user views Settings, **Then** purchase flow becomes available after trial expiration
5. **Given** user changes settings, **When** they immediately test gateway, **Then** all settings persist and take effect

### Edge Cases

- **What happens when user force-quits during active timer?** Timer resets and user must restart entire gateway sequence; state is not preserved across app terminations
- **How does system handle app not installed when URL scheme triggered?** System gracefully displays "App not found" message and returns to Hub; logged as analytics event
- **What occurs when network unavailable and no cached content exists?** System displays placeholder "Please connect to internet for mindful content" and continues with base timer functionality
- **How does system respond to extreme timer durations?** Timer validation prevents values outside 15-45s range; defaults to 15s if invalid value encountered
- **What happens when multiple gateway screens are triggered simultaneously?** Only one gateway screen active per app icon tap; subsequent taps during active gateway are ignored
- **How does user recover from incomplete onboarding?** Partial onboarding state is preserved; user can continue from where they left off or reset via Settings
- **What occurs when content service (Firestore) is unavailable?** Content falls back to offline cache; graceful degradation maintains core timer functionality
- **How does system handle devices without biometric authentication?** Manual app hiding instructions adapt to show device-appropriate steps for passcode-only security
- **What happens with insufficient usage data for statistics?** Statistics screen shows friendly placeholders like "Complete more mindful pauses to unlock insights"; proportional display adapts to data availability

## Requirements *(mandatory)*

<!--
  ACTION REQUIRED: The content in this section represents placeholders.
  Fill them out with the right functional requirements.
-->

### Functional Requirements

- **FR-001**: System MUST display welcoming onboarding screens explaining MIGA's purpose and guiding users through initial app personalization
- **FR-002**: System MUST allow users to select which social media apps to monitor through an intuitive selection interface
- **FR-003**: System MUST provide device-specific instructions for manually hiding selected app icons using biometric/passcode authentication
- **FR-004**: System MUST intercept attempts to launch hidden social media apps and present full-screen mindfulness gateway screen instead
- **FR-005**: System MUST display randomly selected mindfulness content (quotes, affirmations, articles) during the gateway timer
- **FR-005b**: System MUST bundle 20-30 curated mindfulness content items as app assets for immediate offline-first experience
- **FR-006**: System MUST enforce configurable 15-45 second timer countdown before enabling "proceed to app" action
- **FR-007**: System MUST provide three clear action choices: "Exit to Focus" (immediate), "Deepen the Pause" (reset timer), "Proceed to [App]" (timer-gated)
- **FR-008**: System MUST track and persist user interactions including session starts, exits, completion rates, and time spent
- **FR-009**: System MUST display lifetime progress metrics including hours reclaimed, conscious exits, and streak tracking
- **FR-010**: System MUST render 30-day trend visualizations showing mindful choice patterns over time
- **FR-011**: System MUST allow customization of timer duration and content category preferences through dedicated settings
- **FR-012**: System MUST gracefully handle offline operations using cached content and local data persistence
- **FR-013**: System MUST implement core functionality without requiring external app permissions or system-level access
- **FR-014**: System MUST maintain consistent mindful UX design language: neutral colors, calming animations, supportive copy
- **FR-015**: System MUST support complete data portability with local-only storage and transparent privacy practices

Integration Requirements:

- **FR-016**: System MUST successfully trigger external app URL schemes for comprehensive social media coverage: Instagram, TikTok, Facebook, Twitter/X, YouTube, Snapchat, LinkedIn, Reddit
- **FR-017**: System MUST integrate with Firebase for content delivery, remote configuration, and anonymized event analytics
- **FR-018**: System MUST prepare StoreKit integration for trial period management and one-time purchase monetization

### Key Entities *(include if feature involves data)*

- **Social App**: Represents a monitored social media application with display name, URL scheme identifier, icon asset name, and enabled status; relationship to user preferences and gateway sessions
- **Mindfulness Content**: Represents a content item with text content, category classification (Stoicism, Productivity, etc.), content length, and display priority; cache relationship with Firebase content source
- **Gateway Session**: Represents a single mindful pause interaction with start timestamp, duration setting, completion status, and user action outcome (exit/proceed); relationships to social app and content displayed
- **User Preferences**: Represents user configuration with timer duration setting (15-45s), selected content categories, managed social app list, and onboarding completion status
- **Usage Statistic**: Represents aggregated metrics with daily conscious exits count, total session time, streak length, and rolling average completion rate; derived from gateway sessions

## Success Criteria *(mandatory)*

<!--
  ACTION REQUIRED: Define measurable success criteria.
  These must be technology-agnostic and measurable.
-->

### Measurable Outcomes

- **SC-001**: Users can complete full onboarding flow and view functional MIGA Hub within 60 seconds of first app launch
- **SC-002**: System achieves 99% crash-free sessions with core gateway functionality operating within 2-second load time
- **SC-003**: Timer completion rate reaches 60%+ with users successfully experiencing mindful pause during 90%+ of gateway interactions
- **SC-004**: Exit rate achieves 30%+ threshold, demonstrating behavioral intervention effectiveness in reducing autopilot social media usage
- **SC-005**: System handles offline operation for 30 consecutive days while maintaining >80% of core functionality without network
- **SC-006**: Content loading succeeds in 95%+ of cases with graceful fallback to cached content when Firebase unavailable
- **SC-007**: All major social media platforms (Instagram, TikTok, Facebook, Twitter/X) successfully trigger through URL schemes
