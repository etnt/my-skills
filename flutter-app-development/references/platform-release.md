# Flutter platform configuration and release preparation

Use this reference when a change touches build flavors, environment configuration,
localization, permissions, app identity (name/icon/splash), signing, or producing a
release build. Verify every command against the installed Flutter SDK and the
project's existing scripts; CLI flags and plugin setup steps change between releases.

## Environments and flavors

Most production apps run against more than one backend (dev/staging/prod). Keep the
selection out of Dart source so a build cannot ship pointing at the wrong API.

- Prefer compile-time configuration through `--dart-define` or, for multiple values,
  `--dart-define-from-file=config/prod.json`. Read them with
  `String.fromEnvironment` / `int.fromEnvironment` behind a small typed config class.
- Reserve native *flavors* (Android product flavors, iOS schemes/configurations) for
  when environments need different application IDs, icons, names, or signing so they
  can coexist on one device. Flavors touch Gradle and Xcode, so validate an actual
  build of each, not just the analyzer.
- Keep configuration files that contain real endpoints or keys out of version control
  when they are sensitive; commit only safe example templates.

```dart
class AppConfig {
  const AppConfig();

  static const apiBaseUrl = String.fromEnvironment(
    'API_BASE_URL',
    defaultValue: 'https://api.dev.example.com',
  );
}
```

## Localization

For anything beyond a throwaway prototype, route user-facing strings through a
localization mechanism instead of hardcoding them in widgets. Retrofitting i18n
later means touching every screen, so establish it early even if only one locale
ships at first.

- Add `flutter_localizations` (from the SDK) and `intl`, enable `generate: true` in
  `pubspec.yaml`, and keep translations in ARB files under `lib/l10n/`.
- Register the generated `AppLocalizations.delegate` and `supportedLocales` on
  `MaterialApp`, and read strings via `AppLocalizations.of(context)`.
- Localize dates, numbers, and currency with `intl` rather than manual string
  formatting; do not concatenate translated fragments, which breaks in many
  languages. Test at least one locale beyond the default, and verify text scaling
  and right-to-left layout if any target locale is RTL.

## Permissions

Declare only the permissions the app truly uses; unused sensitive permissions cause
store rejections and erode trust.

- iOS: add each usage-description key (for example `NSCameraUsageDescription`) to
  `ios/Runner/Info.plist` with an honest, human-readable reason.
- Android: declare permissions in the manifest and handle the runtime request flow
  for dangerous permissions.
- Request a permission at the point of use, explain why before prompting, and handle
  denied and permanently-denied states with a usable fallback rather than a dead end.

## App identity

Set these deliberately before a release; the defaults from `flutter create` are
placeholders.

- Display name, bundle/application identifier, and version/build number
  (`pubspec.yaml` `version:` maps to both the marketing version and build number).
- App icon and splash screen for both platforms, generated with the project's chosen
  tooling and checked on light and dark backgrounds and notched devices.

## Signing and secrets

- Android release builds must be signed with an upload/release key configured through
  `key.properties` and Gradle; never commit the keystore, `key.properties`, or
  passwords.
- iOS distribution relies on certificates and provisioning profiles managed in the
  Apple developer account or via the project's automation; never commit them.
- Store credentials in the project's approved secret mechanism (CI secrets, a secure
  vault), not in source, `pubspec.yaml`, or tracked platform files.

## Building a release

Match the artifact to the destination and build in release mode:

```bash
flutter build appbundle --release    # Google Play (preferred over APK)
flutter build apk --release          # direct distribution / testing
flutter build ipa --release          # App Store / TestFlight (on macOS + Xcode)
```

- Consider `--obfuscate --split-debug-info=build/symbols` for release builds, and keep
  the emitted symbol files so crash reports can be de-obfuscated.
- A release build exercises tree-shaking, minification, and native signing that debug
  runs skip, so a passing `flutter run` does not prove a release build works. Build the
  release artifact when release settings change.

## Pre-release checklist

- Correct environment/flavor, application ID, display name, and version/build number.
- App icon and splash render correctly on both platforms; no placeholder assets.
- Only necessary permissions are declared, each with a clear justification, and denial
  paths are handled.
- User-facing strings are localized; formatting is locale-aware.
- No secrets, keystores, or provisioning material are committed.
- `flutter analyze` and `flutter test` pass, and the release artifact builds for each
  target platform you can reach. Report any platform whose release build you could not
  produce, and why.
