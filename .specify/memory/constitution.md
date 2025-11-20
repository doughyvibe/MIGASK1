# MIGA Constitution
<!-- MIGA: Mindful Gateway to Social Media - iOS App Constitution -->

## Core Principles

### I. Mindful by Design (NON-NEGOTIABLE)
Every component must embody the mindful UX philosophy: Neutral palette, calming animations, non-punitive messaging; All UI decisions must support intentional technology use; Copy tone always supportive and empowering; No red/orange colors except for critical warnings; Generous whitespace and slow, deliberate interactions

### II. Behavioural Validation Over Velocity
Development prioritizes behavioral impact validation over rapid feature delivery; Every feature must include analytics tracking and retrospective evaluation; MVP features (Gateway Timer, Content, Analytics) must be functionally complete before enhancement; Performance targets: <1s main screen load, 99% crash-free sessions, 60%+ timer completion rate

### III. Test-First iOS Development (NON-NEGOTIABLE)
Unit testing mandatory for all ViewModels, Models, and Core Logic; UI testing for critical user journeys (Onboarding flow, Gateway interaction); Analytics event tracking must be testably correct; Red-Green-Refactor cycle strictly enforced; Integration testing required for Firebase/Firestore interactions

### IV. Privacy-First Implementation
No remote user data storage except analytics aggregates; Local-only identifiers for tracking; Minimal Firebase Analytics usage; All data mirrors locally in Core Data; User's personal information never leaves device except for anonymous behavioral patterns; Transparent privacy disclosures during onboarding

### V. Modular MVVM Architecture
Each feature implemented as standalone module with clear MVVM separation; ViewModels per feature with observable state management; Clean data flow: View → ViewModel → (Local/Firestore) → Response; Dependencies properly injected; Easy to test and maintain; Scalable beyond MVP scope

### VI. Intention Over Restriction
MIGA serves as behavioral guide, never gatekeeper; All interventions voluntary and mindful; Exit paths always available; Analytics track only positive behavioral choices; App Store compliance: Emphasize voluntary mindfulness, not app blocking; User agency paramount in all interactions

### VII. Firebase Integration Standards
Firestore content sync optional, must work offline; Firebase Remote Config for content category selection; StoreKit integration for monetization; Crashlytics for reliability monitoring; All third-party integrations must have offline fallbacks; Performance monitoring for Firebase operations

## Additional Constraints

### Technology Stack (MANDATORY)
- **Frontend**: SwiftUI + MVVM + Combine (iOS 17.0+, Xcode 15.0+)
- **Local Storage**: Core Data + UserDefaults (no custom databases)
- **Backend**: Firebase Firestore + Firebase Remote Config + Crashlytics
- **Monetization**: StoreKit 2 (one-time purchase model)
- **Integration**: Custom URL Schemes only (no UIScene/UIApplication shared linkages)
- **Deployment**: Apple Developer Program + TestFlight + App Store Connect

### Security & Privacy Standards
- **Data Minimization**: Analytics events contain no PII, only behavioral aggregates
- **Local Encryption**: All Core Data stores encrypted at-rest
- **Sandbox Compliance**: All iOS sandbox restrictions respected for app hiding mechanisms
- **GDPR Compliance**: Transparent data practices, user data never sold or shared
- **App Store Requirements**: Voluntary usage interventions only, clear recovery pathways

### Performance Standards
- **Cold Launch**: < 2s to main screen (with cached content)
- **Timer Animation**: Smooth 60fps circular progress bar
- **Memory Usage**: < 50MB resident memory on iPhone 13+
- **Offline Grace Period**: Full functionality for 30 days without network
- **Battery Impact**: < 5% additional daily drain when active

### App Store Compliance
- **No App Blocking**: MIGA positions as "mindfulness assistant" not "app blocker"
- **Supervision Avoidance**: Clear education that app hiding requires user agency and biometric authentication
- **Reviews Process**: All submissions highlight voluntary nature, never mandatory restrictions

## Development Workflow

### Specify Framework Implementation
- **Feature Management**: All new features created using `.specify/scripts/bash/create-new-feature.sh`
- **Branch Naming**: `###-descriptive-name` format with auto-number assignment
- **Specification First**: Every feature starts with comprehensive spec.md in `/specs/###-feature-name/`
- **Memory Integration**: All documents stored in `.specify/memory/` with project context

### Code Review Process
- **Pre-merge Requirements**: Unit tests written and passing; UI tests added for new screens
- **Constitution Compliance**: Constitution reviewed against each PR; Behavioral validation required
- **Performance Gate**: 99% crash-free sessions maintained; Main screen load <1s
- **Privacy Review**: All Firebase/analytics usage approved; No new PII exposure introduced

### Quality Gates by Phase
- **Development Phase**: Constitution compliance confirmed; Tests failing initially but written
- **Implementation Phase**: Red-Green cycle completion; All acceptance criteria met via tests
- **Integration Phase**: Firebase testing passed; Offline functionality confirmed; Performance benchmarks met
- **TestFlight Release**: Full user journey QA complete; Analytics events validated; App Store compliance confirmed

### Testing Standards
- **Unit Test Coverage**: All ViewModels and Models 100%; Core logic >90%
- **Integration Tests**: Firebase/Firestore interactions; URL scheme launching; Core Data operations
- **UI Tests**: Onboarding flow completion; Gateway timer functionality; App exit scenarios
- **Performance Tests**: Launch time validation; Memory usage monitoring; Animation smoothness

## Governance

### Constitution Authority
This Constitution supersedes all other development practices and frameworks. When conflicts arise between this Constitution and external guidelines, the Constitution prevails. Constitution compliance is mandatory for all code contributions and architecture decisions.

### Review & Enforcement Rules
- **All PRs must demonstrate Constitution compliance**; Code reviews explicitly verify adherence to Core Principles
- **Complexity must be justified** through Constitution principle documentation; YAGNI principle strictly enforced
- **Behavioral validation mandatory** for all features; Retrospective analysis required for user-facing changes
- **Use PRD.md and UX-Design.md** for runtime development guidance during implementation

### Amendment Process
- **Amendments require**: Constitution change proposal → Behavioral impact assessment → PRD/UX alignment verification → Implementation planning
- **Approval requires**: Technical feasibility confirmed, retrospective evaluation framework established, migration plan documented
- **Breaking changes**: Full TestFlight cycle required for validation, App Store compliance reassessed

### Runtime Guidance Hierarchy
1. **Primary**: This Constitution (governing principles)
2. **Secondary**: PRD.md (detailed requirements and MVP scope)
3. **Tertiary**: UX-Design.md (implementation specifications and UI details)
4. **Reference**: Generated specifications in `/specs/` directory for specific feature implementations

**Version**: 1.0.0 | **Ratified**: 2025-11-20 | **Last Amended**: 2025-11-20
