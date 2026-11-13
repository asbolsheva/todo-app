# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A small todo app in plain HTML, CSS and JavaScript.
There is no framework, no build step, no package manager and no tests.
The folder is not a git repository.

## Running

Open `index.html` directly in a browser (e.g. `open index.html` on macOS).
No server is needed.
Changes show up on page reload.

## Structure

The repo holds two independent apps with the same layout:

- Root (`index.html`, `style.css`, `app.js`) — the main Todo app.
- `new-app/` — a separate, simpler copy of the same pattern. It has its own files and its own storage key.

Each app keeps one file per concern: markup in `index.html`, styles in `style.css`, logic in `app.js`.

## How `app.js` works

Both apps follow the same state → save → render loop:

- State is one in-memory array (`todos` in the root app, `tasks` in `new-app/`).
- It is loaded once from `localStorage` on startup. Bad or missing data falls back to `[]`.
- Every mutation (add, toggle, delete) changes the array, then calls the save function, then calls `render()`.
- `render()` clears the list and rebuilds every `<li>` from the array. There is no partial DOM update.
- Item IDs are random strings. Handlers find items by ID, not by index.
- Task text is set with `textContent`, never `innerHTML`. Keep it that way to avoid XSS.
- `localStorage` access is wrapped in `try/catch` so the app still works when storage is blocked.

Storage keys: `todos` (root app) and `new-app-tasks` (`new-app/`).
Changing the shape of stored items breaks existing saved data. Handle old data if you do.

## Do not

- Add any npm dependencies
- Use `eval()`
- Commit any secrets — no API keys, passwords, or tokens in the files
