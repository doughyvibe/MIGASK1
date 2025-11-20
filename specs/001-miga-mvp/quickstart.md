# MIGA MVP Implementation Quickstart

**Plan**: specs/001-miga-mvp/plan.md | **Date**: 2025-11-20

## Prerequisites

- macOS 14.0+ with Xcode 15.0+
- Swift 5.9 compatible environment
- Firebase Console project configured
- Physical iOS 17.0+ device for app icon hiding testing

## Phase 1: Foundation Setup (Week 1)

### 1. Project Structure Setup
```bash
# In MIGASK1/ directory
mkdir -p Sources/Models Sources/Services Sources/ViewModels Sources/Views Tests/Unit Tests/UI Tests/Integration
mkdir -p Resources/BundledContent Resources/AppIcons
```

### 2. Core Data Models Implementation
- Create `SocialApp`, `GatewaySession`, `MindfulnessContent`, `UserPreferences`, `UsageStatistics`
- Implement Core Data migrations for schema evolution
- Add validation rules from contracts

### 3. Firebase Integration
```bash
# Add to Podfile or Package.swift
pod 'Firebase/Firestore'
pod 'Firebase/Crashlytics'
pod 'Firebase/RemoteConfig'
```

### 4. Test-First Development Setup
- Create XCTest target with 100% coverage configuration
- Implement first test: `GatewayViewModelInitializationTests`
- Setup CI pipeline for test execution

## Phase 2: Core Functionality (Week 2)

### 1. MVVM Architecture Implementation
```swift
// GatewayViewModel primary interface
class GatewayViewModel: ObservableObject {
    @Published var content: MindfulnessContent?
    @Published var timeRemaining: Int
    @Published var isActive: Bool

    func startSession(appId: String, duration: Int)
    func handleExit() // Core user action
    func handleProceed() // Timer-gated action
    func handleDeepen() // Reset timer, new content
}
```

### 2. Timer Management System
- Implement Combine-based timer with proper cleanup
- Handle force-quit scenarios with state restoration
- Add background execution handling

### 3. Content Delivery System
- Bundle 25 mindfulness quotes in JSON format
- Implement rotation algorithm (no repeats until all consumed)
- Add Firebase content sync as optional enhancement

## Phase 3: UI Implementation (Week 3)

### 1. SwiftUI Screen Hierarchy
```
HubView (TabView root)
├── Grid of enabled SocialApp icons
├── Motivational banner (if streak active)
└── Settings navigation

GatewayScreen (Modal)
├── App context header
├── Mindful content display
├── Circular progress timer
├── Action buttons (Exit, Deepen, Proceed)
└── Celebration animation (exit feedback)

StatsView (TabView secondary)
├── Daily metrics cards
├── 30-day trend chart
└── Insight callouts
```

### 2. Onboarding Flow Implementation
- Phase 1: Content introduction + app selection (pill-style UI)
- Phase 2: Manual iOS app hiding instructions (device-aware copy)
- State persistence across app termination

### 3. Settings Management
- Timer duration slider (15-45s range)
- Content category toggle rows
- App management (enable/disable from selected set)

## Phase 4: Integration & Testing (Week 4)

### 1. URL Scheme Integration
```swift
// App launch implementation
let urlString = "instagram://" // Dynamic per app
if let url = URL(string: urlString), UIApplication.shared.canOpenURL(url) {
    UIApplication.shared.open(url)
} else {
    // Handle failure: app not installed
    showErrorMessage("Instagram is not installed on this device")
}
```

### 2. Analytics Implementation
- Track: session_start, gateway_completed, exit_selected, open_selected, content_read
- Aggregate daily metrics at midnight
- Implement behavioral validation targets

### 3. StoreKit Preparation
- Trial period management (3-day free)
- One-time purchase setup
- Purchase receipt validation

## Critical Implementation Notes

### Performance Requirements
- Cold launch: <2s (documented variation from 1s constitution target)
- Timer accuracy: ±0.1 seconds
- Memory usage: <50MB resident
- Offline functionality: 30-day grace period

### Data Privacy Constraints
- All session data stored locally only
- No PII collection or transmission
- Analytics aggregates only (no individual user tracking)

### Error Handling Priorities
- Network unavailability → fallback to bundled content
- App not installed → user-friendly error messages
- Force-quit recovery → graceful state restoration
- Firebase failure → fail-silently with offline operation

## Testing Strategy

### Unit Tests (Critical Path)
```swift
GatewayViewModelTests.testTimerCompletionTriggersProceedButton
ContentServiceTests.testBundledContentLoading
AnalyticsServiceTests.testSessionAggregationToStatistics
```

### UI Tests (User Journey Validation)
```swift
OnboardingFlowUITests.testCompleteAppSelectionAndSetup
GatewayFlowUITests.testExitActionTriggersCelebration
StatsFlowUITests.testDailyMetricsDisplay
```

## Success Validation

**Pre-TestFlight Checklist:**
- [ ] Onboarding completion rate >95%
- [ ] Gateway timer accuracy: 99% within 1-second tolerance
- [ ] Content loading success rate >98%
- [ ] App URL scheme success rate >90% (for installed apps)
- [ ] Offline 30-day functionality confirmed
- [ ] Memory usage <50MB sustained
- [ ] Privacy: zero network calls containing user identifiers

**Behavioral Validation Targets:**
- Timer completion rate target: >60%
- Exit rate target: >30%
- Daily active session reliability: >99%
