# @amritk/niv

## 0.8.0

The first release as `@amritk/niv`. Releases before it, and their changelog, are
in the [`amritk/mini` repository](https://github.com/amritk/mini/blob/main/packages/mini/CHANGELOG.md).

### Renamed

- The package is `@amritk/niv`. Coming from the previous name, replace it in
  imports and in `jsxImportSource`, rename the JSX types to `NivChild`,
  `NivChildren` and `NivElementProps`, and change the reactivity guard's opt-out
  comment to `// niv-static-ok`.
- The route helpers and `schemaToValidator` now live inside the package instead
  of a separate helpers dependency, and are still exported from
  `@amritk/niv/router` and `@amritk/niv/forms`. The only runtime dependency is
  `alien-signals`.

### Minor Changes

- 37e5042: Faster row creation in `template()` and `list`, plus guidance on long lists.

  - `template()` now finds `data-ref` nodes once, when it parses the HTML, and
    each clone just walks to them. It used to run a selector query on every
    clone. Building 10,000 benchmark rows takes about a quarter less script time.
    One edge case changed: when the root and a descendant use the same
    `data-ref` name, the descendant now wins. The rule is now simply that the
    last node in document order wins.
  - `list` spots duplicate keys by stamping each row with the pass that claimed
    it, instead of building a `Set` of every key on each update. A row that is
    already on screen now costs one map lookup instead of three hash operations.
  - A reactive JSX text child writes its Text node's `data` instead of
    `textContent`, which skips a generic replace-children path.
  - JSX accepts an `onScroll` handler.
  - The README has a new Performance section: where the time actually goes, a
    recipe for rendering only the visible rows of a long list, and why sharing
    memory with WebAssembly does not help.
