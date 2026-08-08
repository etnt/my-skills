# Flutter performance and rebuild cost

Use this reference when a screen janks, a list scrolls poorly, the app rebuilds
more than it should, or a profile shows dropped frames. Optimize against measured
evidence, not guesses: most Flutter jank comes from a small number of hotspots, and
premature micro-optimization usually just makes widget trees harder to read.

Flutter targets 60fps (16ms/frame) or 120fps on capable displays. Two threads
matter: the **UI thread** builds and lays out widgets, and the **raster thread**
paints them. Knowing which one is over budget tells you whether to fix build/layout
cost or paint cost.

## Measure first

- Run in profile mode (`flutter run --profile`) on a real device. Debug mode is
  much slower and its timings are not representative; release/profile is.
- Use DevTools: the Performance view shows per-frame UI vs. raster time; the CPU
  profiler finds hot methods; "Track widget builds" (or `debugProfileBuildsEnabled`)
  reveals widgets rebuilding too often.
- Reproduce the problem with realistic data volume. A list that is smooth with 10
  items can collapse at 10,000.

## Keep rebuilds small and cheap

The framework rebuilds a widget's subtree when its state changes; the fix is almost
always to shrink *what* rebuilds, not to rebuild less often.

- Mark widgets `const` wherever inputs are compile-time constant so the framework can
  skip rebuilding and reuse the element.
- Extract stable subtrees into their own widgets so a state change repaints a leaf
  instead of a whole page. A large `build` method rebuilds as one unit.
- Watch the narrowest slice of state. With Riverpod use `ref.watch(p.select(...))`;
  with listenables use `ValueListenableBuilder` or a scoped `Consumer`/`builder` so
  only the dependent widget rebuilds, not its parent.
- Do not create controllers, `Future`s, or expensive objects inside `build`; it runs
  often. Create them in `initState`/providers and reuse them.
- Keep genuinely expensive pure computation out of `build` by memoizing the result
  and recomputing only when inputs change.

## Long and infinite lists

- Use `ListView.builder`/`GridView.builder` or slivers so only visible items build.
  A plain `ListView(children: [...])` or a `Column` in a `SingleChildScrollView`
  builds every child up front and does not scale.
- Avoid `shrinkWrap: true` and non-scrolling parents around large lists; they force
  full-content layout. Provide `itemExtent`/`prototypeItem` when rows are uniform so
  the framework can lay out without measuring every item.
- Give moving/reorderable items stable keys so the framework reuses elements instead
  of rebuilding them.

## Paint and raster cost

When the raster thread is the bottleneck, reduce painting work rather than rebuilds.

- Wrap a subtree that repaints independently (an animation, a spinner, a video) in a
  `RepaintBoundary` so it does not force the rest of the screen to repaint.
- Prefer driving animations with `AnimatedBuilder`/`ValueListenableBuilder` scoped to
  the animating widget instead of calling `setState` on a large tree each frame.
- Use `Opacity`, `ClipRRect`, `ShaderMask`, and large `BackdropFilter`/blur
  sparingly; they are comparatively expensive. Prefer cheaper alternatives (for
  example, a pre-faded asset or `Color.withOpacity` on a paint) where possible.

## Images and assets

- Decode images at display size with `cacheWidth`/`cacheHeight` (or
  `ResizeImage`); decoding a full-resolution photo into a thumbnail wastes memory
  and raster time.
- Cache and reuse network images with the project's image-caching approach instead
  of refetching and re-decoding on every rebuild.

## Startup and jank checklist

- Confirm the fix on a real device in profile mode; verify the frame budget is met,
  not just that the code "feels" faster.
- Keep heavy work (parsing large payloads, crypto, image processing) off the UI
  thread with `compute`/isolates so frames are not blocked.
- Defer non-critical work after first frame; do not block `main`/first paint on
  network or disk.
- Re-check after changes: an optimization that complicates the widget tree without a
  measured frame-time improvement is not worth keeping.
