<div align="center">

<img src="todo-ui/icons/todo.png" alt="Logos Todo logo" width="128" height="128">

# Logos Todo

</div>

A todo app for [Logos Basecamp](https://logos.co), split into a C++ core module that stores todos in SQLite and a QML UI module that calls it.

## Quick start

Download `todo-core` and `todo-ui` `.lgx` files from [Releases](https://github.com/hackyguru/logos-todo/releases) and install them in Basecamp (core first) via **Modules → Install LGX Package**. To build them yourself, read on.

## Prerequisites

- [Nix](https://nixos.org/download/) with flakes enabled (`experimental-features = nix-command flakes` in `~/.config/nix/nix.conf`)
- [Logos Basecamp](https://github.com/logos-co/logos-basecamp/releases)

## What's here

- [`todo-core/`](todo-core/): `core` module (C++, Qt SQL / SQLite). Todos persist in `todo.db` under the app's data directory.
- [`todo-ui/`](todo-ui/): `ui_qml` module (QML) that depends on `todo`.

`core` modules are C++ Qt plugins that expose `Q_INVOKABLE` methods; they run in the background and have no UI. `ui_qml` modules are QML-only sidebar tabs that call into core modules via `logos.callModule(...)`.

## Build

```bash
cd todo-core
nix flake update        # first time only
nix build '.#lgx-portable' --out-link result-portable

cd ../todo-ui
nix build --override-input todo path:../todo-core '.#lgx-portable' --out-link result-portable
```

The UI's `todo` flake input is a placeholder, so always build it with `--override-input` pointing at the core.

## Install in Basecamp

Each build produces an `.lgx` file at `result-portable/*.lgx`. In [Logos Basecamp](https://github.com/logos-co/logos-basecamp/releases), open **Modules → Install LGX Package** and pick the file. Install the core first, then the UI.
