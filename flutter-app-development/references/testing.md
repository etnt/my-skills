# Flutter testing and validation

Test user-visible behavior and important data-flow decisions. A test suite should
make it safe to refactor widget trees without rewriting assertions about private
implementation details.

## Test pyramid

| Test type | Use for | Typical location |
| --- | --- | --- |
| Unit | Pure logic, parsing, validation, repositories, state transitions | `test/**` |
| Widget | A widget's rendered states and user interactions | `test/**` |
| Integration | Critical end-to-end flows on a real/simulated platform | `integration_test/**` |
| Golden | Deliberately controlled visual regressions | `test/**` |

Most behavior should be covered with fast unit and widget tests. Add integration
tests for high-value flows such as sign-in, checkout, data creation, deep linking,
permissions, and offline recovery rather than trying to cover every screen this way.

## Commands

Run focused tests during development, then the full suite:

```bash
flutter test test/features/tasks/task_controller_test.dart
flutter test --name "shows retry action"
flutter test
flutter test --coverage
flutter analyze
```

Use the project's existing scripts and SDK constraints when available. Inspect
`coverage/lcov.info` with the project's configured tooling; do not treat a high line
coverage percentage as proof that important behavior is tested.

## Unit tests

Test domain behavior and state transitions without building widgets. Inject fakes
or mocks for repositories and verify success, empty, failure, retry, and duplicate
submission behavior.

```dart
class TaskValidator {
  String? validateTitle(String value) {
    if (value.trim().isEmpty) return 'Enter a title';
    if (value.trim().length > 80) return 'Use 80 characters or fewer';
    return null;
  }
}

void main() {
  test('rejects a blank task title', () {
    expect(TaskValidator().validateTitle('   '), 'Enter a title');
  });

  test('accepts a title within the limit', () {
    expect(TaskValidator().validateTitle('Read the Flutter guide'), isNull);
  });
}
```

For repository and controller tests, inject a hand-written fake or a mock into the
project's existing state-management API, then verify the resulting state and calls.
Do not make production classes more complicated solely to fit a mock framework.

## Widget tests

Pump the smallest meaningful widget tree, provide dependencies through the same
mechanism used in production, and interact as a user would:

```dart
testWidgets('submits a task', (tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [taskRepositoryProvider.overrideWithValue(FakeTaskRepository())],
      child: const MaterialApp(home: TaskPage()),
    ),
  );

  await tester.enterText(find.byType(TextField), 'Read');
  await tester.tap(find.byTooltip('Save'));
  await tester.pumpAndSettle();

  expect(find.text('Read'), findsOneWidget);
});
```

Use stable semantic labels and keys when a control cannot be found reliably by
role/text. Assert meaningful output: visible text, enabled/disabled behavior,
navigation, callbacks, and error/retry affordances. Avoid asserting the exact number
of internal `Container` widgets or private implementation classes.

For asynchronous providers, use `pump` deliberately and avoid unconditional
`pumpAndSettle` when animations, timers, or streams are intentionally ongoing. Test
loading, error, empty, and data states separately when they have different behavior.

## Integration tests

Put only critical workflows in `integration_test/`. Keep external systems
deterministic by using a test backend, local fixtures, or injectable adapters. Verify
on both iOS and Android when the flow touches permissions, platform channels,
notifications, storage, deep links, or rendering differences.

Run with the target device selected:

```bash
flutter test integration_test/app_test.dart
```

If a test requires a running backend, document setup, test data cleanup, credentials
handling, and whether the test is safe to run in CI. Never commit production tokens.

## Golden tests

Use goldens for stable, high-value visual contracts such as a design-system component
or a complex empty/error state. Control text scale, locale, theme, device size, and
platform differences. Review intentional golden changes; do not update snapshots
just to make a failing test green.

## Test quality checklist

- Test the behavior requested by the user, including failure and empty paths.
- Use deterministic clocks, IDs, network responses, and ordering where possible.
- Reset or isolate shared state between tests.
- Ensure semantics and accessibility labels exist for important controls.
- Re-run a focused test after each failure, then run `flutter test` and
  `flutter analyze` before declaring completion.
- If device, emulator, native SDK, or backend limitations prevent a test, report the
  exact command and limitation rather than silently omitting validation.