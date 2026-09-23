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

## Version badge and tag-driven releases

Show the app version in small print next to the app name in the AppBar, fed from a
build-time define so the version comes from the git tag rather than a hardcoded
string. Local/debug builds without the define fall back to `dev`.

1. Version constant (e.g. `lib/src/config/app_version.dart`):

   ```dart
   /// App version injected at build time via `--dart-define=APP_VERSION=...`.
   /// Falls back to 'dev' for local/debug builds.
   const appVersion = String.fromEnvironment('APP_VERSION', defaultValue: 'dev');
   ```

2. Small print beside the app name in the AppBar title:

   ```dart
   appBar: AppBar(
     title: const Text.rich(
       TextSpan(
         text: '<App Name>',
         children: [
           TextSpan(
             text: '  $appVersion',
             style: TextStyle(fontSize: 12, fontWeight: FontWeight.w300),
           ),
         ],
       ),
     ),
   ),
   ```

3. Release workflow at `.github/workflows/release.yml`. It triggers when a `v*` tag
   is pushed, builds release APKs with `--dart-define=APP_VERSION=${{ github.ref_name }}`
   (so the tag name becomes the displayed version), and publishes them to a GitHub
   Release. Signing material comes from repository secrets:

   ```yaml
   name: Release

   on:
     push:
       tags:
         - "v*"
     workflow_dispatch:

   permissions:
     contents: write

   jobs:
     build-android:
       name: Build & release Android APKs
       runs-on: ubuntu-latest

       steps:
         - name: Checkout
           uses: actions/checkout@v5

         - name: Set up JDK 17
           uses: actions/setup-java@v5
           with:
             distribution: temurin
             java-version: "17"

         - name: Set up Flutter
           uses: subosito/flutter-action@v2
           with:
             channel: stable
             cache: true

         - name: Install dependencies
           run: flutter pub get

         - name: Analyze
           run: flutter analyze

         - name: Run tests
           run: flutter test

         - name: Decode release keystore
           env:
             KEYSTORE_BASE64: ${{ secrets.KEYSTORE_BASE64 }}
           run: echo "$KEYSTORE_BASE64" | base64 --decode > android/release-keystore.jks

         - name: Create key.properties
           env:
             KEYSTORE_PASSWORD: ${{ secrets.KEYSTORE_PASSWORD }}
           run: |
             cat > android/key.properties <<EOF
             storePassword=$KEYSTORE_PASSWORD
             keyPassword=$KEYSTORE_PASSWORD
             keyAlias=release
             storeFile=../release-keystore.jks
             EOF

         - name: Build split-per-ABI APKs
           run: flutter build apk --release --split-per-abi --dart-define=APP_VERSION=${{ github.ref_name }}

         - name: Build universal APK
           run: flutter build apk --release --dart-define=APP_VERSION=${{ github.ref_name }}

         - name: Publish GitHub Release
           uses: softprops/action-gh-release@v2
           with:
             files: |
               build/app/outputs/flutter-apk/app-release.apk
               build/app/outputs/flutter-apk/app-armeabi-v7a-release.apk
               build/app/outputs/flutter-apk/app-arm64-v8a-release.apk
               build/app/outputs/flutter-apk/app-x86_64-release.apk
             fail_on_unmatched_files: true
             generate_release_notes: true
   ```

   The keystore decode/`key.properties` steps (and the matching Gradle signing
   configuration) are only needed for signed release builds; skip them when the
   project has no signing setup yet and keep the unsigned artifacts.

4. Cut a release by pushing a tag: `git tag v1.0.0 && git push origin v1.0.0`.
   Verify the badge shows the tag (not `dev`) in the published APK.

## Pre-release checklist

- Correct environment/flavor, application ID, display name, and version/build number.
- Version badge next to the app name shows the injected tag version in release
  builds and `dev` in local builds.
- App icon and splash render correctly on both platforms; no placeholder assets.
- Only necessary permissions are declared, each with a clear justification, and denial
  paths are handled.
- User-facing strings are localized; formatting is locale-aware.
- No secrets, keystores, or provisioning material are committed.
- `flutter analyze` and `flutter test` pass, and the release artifact builds for each
  target platform you can reach. Report any platform whose release build you could not
  produce, and why.
