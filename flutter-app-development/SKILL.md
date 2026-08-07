---
name: flutter-app-development
description: "Build, extend, and maintain production-quality Flutter mobile applications with Dart. Covers project setup, feature-first architecture, Material 3 UI, navigation, responsive layouts, Riverpod state management, repositories and API integration, platform configuration, testing, debugging, and release preparation. TRIGGER when the user asks to create or modify a Flutter app, mentions Flutter or Dart UI/mobile code, has a pubspec.yaml with a Flutter dependency, or asks for an iOS and Android app using Flutter. ALSO trigger for Flutter-specific build, widget, navigation, state, or testing problems. SKIP for React Native, native Swift/UIKit/SwiftUI, native Android/Kotlin/Compose, pure Dart CLI/server packages, or backend-only work unless the request also includes a Flutter client."
---

# Flutter application development

Build maintainable Flutter applications rather than isolated demo screens. Prefer
small, testable features, explicit data flow, accessible UI, and platform-aware
validation. Match the existing project's conventions when modifying an existing
app; do not replace its architecture or state-management library without a clear
reason.

## First inspect the project

Before writing code, inspect:

- `pubspec.yaml` and `pubspec.lock` for the Flutter/Dart SDK constraint, dependencies,
  scripts, and existing state-management, routing, networking, and testing choices.
- `lib/`, `test/`, `integration_test/`, `android/`, `ios/`, and any flavor or build
  configuration directories.
- `analysis_options.yaml`, generated-code configuration, and existing test commands.
- Git status and the current app entry point (`lib/main.dart` or its equivalent).

For a new app, clarify the target platforms, app identifier/organization, minimum
OS versions, backend/API, authentication, offline requirements, and whether the
user expects a prototype or production-ready implementation. If details are not
provided, use the defaults below and state them briefly.

## Defaults for a new Flutter app

- **UI:** Material 3 with a centralized `ThemeData` and `ColorScheme`; support dark
  mode when the product requirements do not explicitly exclude it.
- **Structure:** feature-first organization. Keep feature-specific UI, state,
  models, repositories, and services together rather than creating a large global
  `screens/` or `utils/` directory.
- **State:** use Riverpod for shared and asynchronous application state when the
  project has no existing choice. Use `setState` for genuinely local, ephemeral
  widget state. Do not introduce Riverpod into an existing app already standardized
  on another library unless migration is requested.
- **Navigation:** use `go_router` for named routes, nested navigation, redirects,
  and deep links when the project has no existing router. Keep route definitions
  centralized and typed through route data or well-defined path parameters.
- **Networking:** keep HTTP calls behind repositories/services. Do not call a
  client directly from a widget. Reuse the project's existing HTTP client and
  serialization approach; otherwise choose the smallest suitable dependency.
- **Quality:** enable `flutter_lints`, use null safety, prefer `const` widgets, and
  keep `flutter analyze` clean. Add tests for behavior, not implementation details.
- **Responsive UI:** design for phones first, then handle larger widths, text
  scaling, safe areas, keyboard insets, orientation, and accessibility semantics.

## Core workflow

1. **Understand the product behavior.** List screens, user actions, loading/empty/
   error/success states, persistence needs, navigation paths, and platform-specific
   behavior before choosing widgets.
2. **Choose the project shape.** For a new app, read
   [references/project-setup.md](references/project-setup.md). For an existing app,
   preserve its structure unless it prevents the requested change.
3. **Model data flow.** Define domain models and repository boundaries before
   connecting UI to a remote API or local database. Represent loading, errors, and
   retry behavior explicitly. Read
   [references/state-management.md](references/state-management.md) when adding or
   changing state.
4. **Build the UI as composable widgets.** Keep build methods readable, extract
   widgets around meaningful behavior, use theme values instead of repeated magic
   numbers/colors, and provide semantics and useful labels for interactive controls.
5. **Implement navigation and platform behavior.** Handle back navigation, deep links,
   authentication redirects, permissions, keyboard/focus behavior, and lifecycle
   changes deliberately. Never assume iOS and Android behave identically.
6. **Test the important paths.** Read [references/testing.md](references/testing.md)
   and add focused unit/widget/integration tests appropriate to the change.
7. **Validate before finishing.** Run, as applicable:

   ```bash
   flutter pub get
   dart format --output=none --set-exit-if-changed .
   flutter analyze
   flutter test
   ```

   Run the app on at least one target platform when available. If native files,
   permissions, plugins, deep links, push notifications, or release settings
   changed, validate the affected iOS/Android build path as well. Report commands
   that could not run and why; do not claim a build is verified when it was not run.

## Reference routing

Read only the references relevant to the current task:

| Task | Reference |
| --- | --- |
| Create a project, choose structure, configure theme or navigation | [project-setup.md](references/project-setup.md) |
| Add or change state, async flows, repositories, or dependency injection | [state-management.md](references/state-management.md) |
| Add unit, widget, golden, or integration tests | [testing.md](references/testing.md) |

## Non-negotiable implementation rules

- Do not put secrets, API keys, or private signing material in Dart source,
  `pubspec.yaml`, or committed platform files. Use the project's approved secret
  and build-configuration mechanism.
- Do not use `BuildContext` after an `await` without checking that the context is
  still mounted. Prefer moving async work out of widgets when possible.
- Do not use `!` to hide nullable-state problems. Model absent data and failures
  explicitly and give users a recoverable error state.
- Do not perform network, database, file, or expensive computation work inside
  `build`; providers, repositories, and lifecycle methods should own that work.
- Dispose controllers, focus nodes, animation controllers, streams, and listeners
  that are owned by a `StatefulWidget`.
- Avoid unbounded `ListView`/`Column` nesting, fixed dimensions that break text
  scaling, and layout code that only works on one simulator size.
- Use stable keys where list identity matters, especially for reorderable or
  stateful lists. Avoid using list indexes as keys when items can move.
- Preserve generated files and regenerate them with the project's documented
  command instead of editing generated output by hand.
- When adding a package, explain why it is needed, use a version compatible with
  the SDK constraint, and avoid adding overlapping libraries for the same concern.

## Definition of done

A Flutter change is complete when the requested behavior works, loading/empty/error
states are handled, the UI remains usable with accessibility and different screen
sizes, relevant tests cover the behavior, formatting and analysis pass, and any
platform-specific or build limitations are clearly reported.