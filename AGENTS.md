# TabStack Agent Notes

- This is a Sublime Text package, not a standalone app.
- Runtime target is Sublime Text's Python 3.14 plugin host.
- `main.py` is the only entry file. It clears cached `plugin.*` modules before star-importing `plugin`.
- Keep exported plugin symbols in `plugin/__init__.py`; real implementation lives under `plugin/`.
- Per-window state is in-memory only, keyed by `window.id()` in `plugin/state.py`.
- MRU order is per window. `TabStackListener.on_activated` updates history, except during an active quick-panel session.
- `TabStackListener.on_close` prunes closed sheets from history and removes empty window state.
- Tab captions come from `plugin/captions.py`. Widgets, panels, and transient/non-tab views are excluded from the MRU list.
- Ctrl-release detection lives in `plugin/ctrl_release/`. Linux needs X11 or XWayland; native Wayland is unsupported.
- `show_tab_stack` opens the quick panel, starts a Ctrl-release poller, and commits when Ctrl is released.
- The quick-panel context key is `tab_stack.quick_panel`.
- When `tab_stack.quick_panel` is `false`, `ctrl+tab`, `ctrl+shift+tab`, `ctrl+up`, and `ctrl+down` start `show_tab_stack`.
- When `tab_stack.quick_panel` is `true`, those keys call `move`, and `ctrl+escape` calls `tab_stack_cancel`.

## Verification

- Use `uv run ruff check .` for linting.
- Use `uv run ruff format .` for formatting.
- `pyproject.toml` only defines Ruff settings; there is no repo-local test runner.
