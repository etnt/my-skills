# Flutter state management and data flow

Use this reference when a feature has shared state, asynchronous work, caching,
authentication, or data coming from a repository. The goal is predictable data
flow: widgets render state and send intents; controllers/providers coordinate state;
repositories own data access.

## Choose the smallest suitable scope

| State | Preferred owner |
| --- | --- |
| One field or animation used by one widget | `StatefulWidget` + `setState` |
| Form field validation within one screen | Local widget state or a form controller |
| Shared state across a feature | Feature provider/controller |
| Remote data, caching, retries, or auth session | Async provider/controller + repository |
| Cross-feature application state | App-level provider with a narrow public API |

Do not move every boolean into global state. Conversely, do not pass a remote model
through many widget constructors when a feature provider is the established pattern.

## Default: Riverpod for a new app

If no state library is already selected, `flutter_riverpod` is a reasonable default.
Wrap the app once with `ProviderScope`:

```dart
void main() {
  runApp(const ProviderScope(child: MyApp()));
}
```

Use providers for dependencies and controllers for mutable state. A small, complete
example:

```dart
final counterProvider = NotifierProvider<Counter, int>(Counter.new);

class Counter extends Notifier<int> {
  @override
  int build() => 0;

  void increment() => state++;
}

class CounterPage extends ConsumerWidget {
  const CounterPage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);

    return Scaffold(
      body: Center(child: Text('$count')),
      floatingActionButton: FloatingActionButton(
        onPressed: () => ref.read(counterProvider.notifier).increment(),
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

For asynchronous state, expose loading, data, and error states rather than adding
separate booleans that can contradict each other:

```dart
final tasksProvider = AsyncNotifierProvider<TasksController, List<Task>>(
  TasksController.new,
);

class TasksController extends AsyncNotifier<List<Task>> {
  @override
  Future<List<Task>> build() {
    return ref.read(taskRepositoryProvider).listTasks();
  }

  Future<void> add(Task task) async {
    state = const AsyncLoading<List<Task>>().copyWithPrevious(state);
    state = await AsyncValue.guard(() async {
      await ref.read(taskRepositoryProvider).add(task);
      return ref.read(taskRepositoryProvider).listTasks();
    });
  }
}
```

Render the states explicitly:

```dart
final tasks = ref.watch(tasksProvider);

return tasks.when(
  loading: () => const Center(child: CircularProgressIndicator()),
  error: (error, stackTrace) => ErrorView(
    message: 'Could not load tasks',
    onRetry: () => ref.invalidate(tasksProvider),
  ),
  data: (items) => items.isEmpty
      ? const EmptyState(message: 'No tasks yet')
      : TaskList(items: items),
);
```

Check the installed Riverpod version before copying an API example. If the project
uses an older API (`StateNotifierProvider`, for example), follow its existing style
unless upgrading is part of the request. Do not mix old and new patterns casually.

## Repository boundary

Keep transport and persistence details out of widgets and controllers:

```dart
abstract interface class TaskRepository {
  Future<List<Task>> listTasks();
  Future<void> add(Task task);
}

final taskRepositoryProvider = Provider<TaskRepository>((ref) {
  return ApiTaskRepository(ref.watch(apiClientProvider));
});
```

The repository should translate HTTP/database failures into app-level failures that
the presentation layer can explain or retry. Inject the repository through a
provider so tests can override it with a fake. Keep mutation behavior explicit:
decide whether the UI is optimistic, pessimistic, or invalidates/refetches after a
successful write, and test that decision.

## Forms, authentication, and lifecycle

- Keep draft form values local until submission unless drafts must survive routes
  or process death.
- Disable duplicate submissions while a request is in flight, but preserve enough
  prior data to avoid unnecessary blank-screen flicker.
- Treat authentication as state with loading, signed-out, signed-in, and failure
  conditions. Route guards must react when the state changes.
- Cancel or ignore stale requests when a screen is disposed or a query parameter
  changes. Do not update a disposed widget.
- Use `ref.listen` for one-off effects such as snackbars, dialogs, and navigation;
  use `ref.watch` for rendering.
- Keep errors useful for logs and safe for users. Never display tokens, raw server
  responses, or sensitive exception details.

## Alternatives

Use the existing library in an established app. For a new app, choose another
approach when its constraints fit better:

- **Bloc/Cubit:** useful for event-driven workflows, explicit transitions, and
  teams already standardized on Bloc.
- **Provider:** suitable for simple dependency/state trees or an existing Provider
  application; avoid introducing two global state systems.
- **ValueNotifier/ChangeNotifier:** useful for small local models, but keep
  lifecycle and listener ownership clear.
- **setState:** ideal for ephemeral widget state; it is not a replacement for a
  repository or shared async state.

Regardless of library, keep state transitions observable and testable, avoid hidden
global mutation, and represent failure and empty states explicitly.