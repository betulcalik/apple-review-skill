# macOS & Other Platforms — App-Type Checklist

**Covers:** Mac App Store apps, Apple TV (tvOS), Apple Watch (watchOS), visionOS, App Clips, widgets/extensions as surfaces, cross-platform (Catalyst) builds.

## Mac App Store (2.4.5)

| Requirement | Detail |
|---|---|
| Sandboxing | appropriate sandbox + macOS file-system rules; only proper APIs for modifying other apps' user data (bookmarks, Address Book, Calendar) |
| Packaging | Xcode-built, no third-party installers; single self-contained bundle; no shared-location installs |
| Startup | no auto-launch/login items without consent; no post-quit lingering processes; no Dock icon additions or desktop shortcuts |
| No code installation | no downloading kexts/standalone apps/resources that add functionality post-review |
| No privilege escalation | no root requests, no setuid |
| No license theater | no license screens/keys/copy protection at launch |
| Updates | only through the Mac App Store |
| OS currency | runs on current macOS; no deprecated/optionally-installed tech |
| Localization | all languages in the single bundle |

## Apple TV (2.4.3)

- Usable with **Siri Remote alone**; enhanced functionality with controllers is fine.
- Controller-required games must state it in metadata.

## Apple Watch

- Watch apps reviewed for standalone function where claimed independent; watch complications/widgets relate to app content (2.5.16).
- **No display ads on watchOS apps** (2.5.18).

## App Clips

- All Clip features exist in the main app binary too; **no advertising in App Clips** (2.5.16(a)).
- Clip experience focused; invocation (NFC/QR/maps) documented for reviewers.

## Widgets & extensions

- Content/functionality related to the app (2.5.16); no ads in widgets/notifications/keyboards (2.5.18).
- Keyboard extensions: functional without full access/network, next-keyboard progression, no repurposed keys (4.4.1).
- Safari extensions: current-Safari support, minimal host permissions, no UI interference (4.4.2).

## visionOS / spatial

- Standard rules apply; recording/passthrough capture needs explicit consent + indicator (2.5.14); persona/avatar usage per Apple guidelines.

## Cross-platform traps

- Catalyst builds: macOS sandbox still applies; touch-paradigm UI that macOS reviewers find broken = 2.4.5/4.0 rejections.
- iPad-not-optimized universal apps: 2.4.1 broken layouts are a top volume rejection — test iPad multitasking/orientation.
- Platform references: no other-platform names/icons in the app (2.3.10).
