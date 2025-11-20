# Data Model: MIGA MVP

**Plan**: specs/001-miga-mvp/plan.md | **Date**: 2025-11-20

## Social App Entity

**Purpose**: Represents a monitored social media application

**Attributes:**
```swift
struct SocialApp {
    let id: String              // Unique identifier (e.g., "instagram", "tiktok")
    let displayName: String     // Human-readable name (e.g., "Instagram", "TikTok")
    let urlScheme: String       // Custom URL scheme (e.g., "instagram://", "tiktok://")
    let iconAssetName: String   // Asset name for app icon
    let isEnabled: Bool        // Whether user has selected this app
    let order: Int             // Display order in Hub
}
```

**Validation Rules:**
- `id` must be lowercase alphanumeric with hyphens only
- `urlScheme` must include "://" and be validated against known patterns
- `isEnabled` defaults to false for new apps

**Relationships:**
- One-to-many with Gateway Session (session.appId -> app.id)

---

## Mindfulness Content Entity

**Purpose**: Represents a mindful content item for display during pauses

**Attributes:**
```swift
struct MindfulnessContent {
    let id: String              // Unique content identifier
    let type: ContentType       // .quote, .excerpt, .prompt
    let text: String           // Content text (150-500 characters)
    let category: ContentCategory // .stoicism, .productivity, .mindfulness, etc.
    let author: String?        // Optional attribution source
    let length: Int            // Approximate reading time in seconds
    let priority: Int          // Display priority (higher = more frequent)
    let isBundled: Bool       // True for app-bundled, false for Firebase-sourced
    let lastUsed: Date?       // Track usage for rotation logic
}
```

**Validation Rules:**
- `text` length must be 50-750 characters
- `length` must be 10-60 seconds
- `priority` must be 1-10 (default: 5)
- Only bundled content available without Firebase

**Relationships:**
- One-to-many with Gateway Session (session.contentId -> content.id)

---

## Gateway Session Entity

**Purpose**: Records individual mindful pause interactions

**Attributes:**
```swift
struct GatewaySession {
    let id: UUID              // Unique session identifier
    let appId: String         // Referenced SocialApp.id
    let contentId: String     // Referenced MindfulnessContent.id
    let startTimestamp: Date  // Session start time
    let durationSetting: Int  // Configured pause duration (15-45)
    let completionStatus: CompletionType // .completed, .exited, .force_quit, .timeout
    let timeSpent: Int        // Actual seconds in mindful state
    let actionOutcome: ActionType // .proceed_to_app, .exit_to_home, .deepen_pause
    let platform: String      // iOS version for analytics
}
```

**Validation Rules:**
- `durationSetting` must be 15-45 seconds
- `timeSpent` cannot exceed `durationSetting` + 5 seconds (grace period)
- Session must reference valid app and content IDs

**Relationships:**
- Many-to-one with Social App (session.appId -> app.id)
- Many-to-one with Mindfulness Content (session.contentId -> content.id)

**Lifecycle Transitions:**
- Started: When GatewayScreen appears
- Completed: When timer reaches 0:00
- Exited: When user taps "Exit to Focus"
- Timeouted: When force-quit recovery occurs

---

## User Preferences Entity

**Purpose**: Stores user configuration and persistence state

**Attributes:**
```swift
struct UserPreferences {
    let id: String                    // Singleton identifier ("user_preferences")
    let timerDuration: Int           // Selected duration (15-45 seconds)
    let selectedCategories: Set<ContentCategory> // Enabled content types
    let enabledApps: Set<String>      // Selected social app IDs
    let onboardingCompleted: Bool     // First-run setup status
    let trialStartDate: Date?         // StoreKit trial tracking
    let lastContentSync: Date?       // Firebase sync timestamp
    let analyticsOptIn: Bool        // Privacy preference (defaults: false)
}
```

**Validation Rules:**
- `timerDuration` defaults to 15 seconds
- `selectedCategories` defaults to all categories
- `enabledApps` maximum of 10 concurrent selections
- Preferences must support offline mode

**Relationships:**
- One-to-many derived from enabled apps (preferences.enabledApps -> app.id)

---

## Usage Statistics Entity

**Purpose**: Aggregated metrics for insights and behavioral validation

**Attributes:**
```swift
struct UsageStatistics {
    let id: String              // Composite key (date_app format)
    let date: Date             // Aggregation date (reset at midnight)
    let appId: String          // Referenced SocialApp.id
    let totalSessions: Int     // Total mindful pauses started
    let completedSessions: Int // Sessions reaching timer completion
    let exitedSessions: Int    // Sessions with "Exit to Focus" action
    let avgTimeSpent: Double   // Average seconds in mindful state
    let streakCount: Int       // Consecutive days with >=1 completed session
}
```

**Validation Rules:**
- Statistics are calculated and stored daily at midnight
- Historical data retained for 30 days (auto-pruned)
- Metrics support behavioral validation (60%+ completion rate target)

**Relationships:**
- Many-to-one with Social App (stats.appId -> app.id)
- Used for trends and insights calculations

## Data Flow Architecture

### Create/Read/Update Patterns

**Onboarding Flow:**
1. Load bundled MindfulnessContent into Core Data
2. Create UserPreferences with defaults
3. Present app selection from hardcoded SocialApp options
4. Update preferences.onboardingCompleted when phase 2 finished

**Session Flow:**
1. GatewayScreen triggers → Create new GatewaySession
2. Load content by category preferences
3. Track timer state and user actions
4. On completion/exit → Update session record
5. Aggregate into UsageStatistics for insights

**Settings Flow:**
1. UserPreferences modification triggers validation
2. Timer duration validated against 15-45 range
3. App selections update enabledApps set
4. Categories filter available content

### Synchronization Strategy

**Firebase Integration:**
- Read-only content synchronization (no user data upload)
- Optional: Query current date content collections
- Fallback: Bundled content if sync fails
- Cache: TTL 24 hours, background refresh
