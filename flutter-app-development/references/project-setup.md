# Flutter project setup and architecture

Use this reference when creating a Flutter app, organizing a new feature, or
setting up theme and navigation. Verify commands against the installed Flutter
SDK and the project's SDK constraint; Flutter command flags and package APIs can
change between releases.

## Inspect the toolchain

Run these before scaffolding or changing platform configuration:

```bash
flutter --version
flutter doctor -v
flutter devices
```

Resolve actionable `flutter doctor` failures first. A working Flutter SDK does not
guarantee that Xcode, CocoaPods, Android SDK licenses, an emulator, or a physical
device is ready.

## Create a project

For a new application, use a reverse-domain organization and a stable project name:

```bash
flutter create --org com.example --project-name my_app my_app
cd my_app
flutter pub get
```

Replace `com.example` with the real organization identifier. Confirm the generated
bundle/application identifiers in `android/` and `ios/`; do not assume changing
the Dart project name changes every native identifier.

If the user requests a specific platform, use the installed Flutter CLI's platform
options or remove unneeded platform directories only after confirming the release
plan. Keep both iOS and Android folders for a normal cross-platform mobile app.

## Feature-first structure

Start small, but establish boundaries that can grow:

```text
lib/
  main.dart
  app/
    app.dart
    router.dart
    theme.dart
  core/
    error/
    network/
    widgets/
  features/
    todos/
      data/
        todo_repository.dart
        todo_remote_data_source.dart
      domain/
        todo.dart
      presentation/
        todo_list_page.dart
        todo_controller.dart
```

Keep `core/` limited to genuinely shared code. A feature's repository, model, and
controller should stay inside that feature until multiple features truly share them.
Avoid a dumping-ground `helpers.dart`, global mutable singletons, and UI widgets
that know the details of an API response.

For a small prototype, a simpler layout is acceptable:

```text
lib/
  main.dart
  app.dart
  features/<feature>/...
```

Do not create empty abstraction layers merely to satisfy a template. Introduce a
domain layer when business rules, multiple data sources, or meaningful model
transformations justify it.

## Dependencies and SDK compatibility

Inspect the existing `environment.sdk` constraint and lockfile before adding a
package. Prefer the project's established choices. For a new app, common choices
may include:

- `flutter_riverpod` for application state and dependency injection
- `go_router` for declarative navigation and deep links
- the project's selected HTTP client and serialization tooling
- `flutter_test` and `integration_test`, which are provided by Flutter

Use `flutter pub add <package>` where possible so the constraint is resolved by
Pub. Do not blindly upgrade all packages while implementing an unrelated feature.
After dependency changes, run `flutter pub get`, `dart format .`, and
`flutter analyze`.

## Material 3 theme

Centralize design tokens in `app/theme.dart`. Prefer a `ColorScheme` and theme
extensions over hardcoded colors in individual widgets:

```dart
import 'package:flutter/material.dart';

ThemeData buildTheme({required Brightness brightness}) {
  final scheme = ColorScheme.fromSeed(
    seedColor: const Color(0xFF1565C0),
    brightness: brightness,
  );

  return ThemeData(
    colorScheme: scheme,
    brightness: brightness,
    useMaterial3: true,
    inputDecorationTheme: const InputDecorationTheme(
      border: OutlineInputBorder(),
    ),
  );
}
```

Configure both `theme` and `darkTheme` when dark mode is supported. Use
`Theme.of(context).colorScheme`, `textTheme`, and spacing constants in widgets.
Check contrast, text scaling, touch target sizes, and focus indicators rather than
judging the UI only at the default simulator settings.

## Navigation with `go_router`

Keep route configuration close to the app root. Define path parameters explicitly,
use `extra` only for non-URL state, and make authentication redirects depend on an
observable auth state rather than a one-time startup boolean:

```dart
final router = GoRouter(
  initialLocation: '/tasks',
  routes: [
    GoRoute(
      path: '/tasks',
      builder: (context, state) => const TaskListPage(),
      routes: [
        GoRoute(
          path: ':taskId',
          builder: (context, state) => TaskPage(
            taskId: state.pathParameters['taskId']!,
          ),
        ),
      ],
    ),
  ],
);
```

Test route transitions, unknown routes, back behavior, and protected routes. For
deep links, configure the corresponding iOS Associated Domains or Android intent
filters and test a cold launch, not only an in-app navigation.

## Run and configure platforms

Use `flutter run -d <device-id>` and hot reload for Dart/UI iteration. Hot reload
does not replace a full restart when changing app initialization, native plugin
registration, platform manifests, or some static state.

When a change touches native configuration:

- iOS: inspect `ios/Runner/Info.plist`, entitlements, deployment target, and
  CocoaPods state; use the project's specified Xcode/CocoaPods workflow.
- Android: inspect the manifest, Gradle files, namespace/application ID, minimum
  SDK, permissions, and signing configuration.
- Plugins: read the plugin's platform setup instructions and verify both platforms;
  a successful Dart analyzer run is not sufficient.

Never commit local keystores, provisioning profiles, certificates, or credentials.