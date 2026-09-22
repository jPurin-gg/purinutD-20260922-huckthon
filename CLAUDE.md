# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Instruction priority
- ユーザーから明示的に指示されない限り、`docs/my_rule.md` を開いたり読んだり、その内容を考慮したりしないこと。

## Project

`utd_hack` — a mock of a mobile-first web app ("昼めし") for finding lunch spots near Nagaoka that are currently open, and voting on candidates as a group. No backend; the entire app is a single static HTML file (`lunch-list-mock.html`, ~600 lines: `<style>` then `<script>`, no build step, no dependencies besides Google Fonts).

## Running / testing

There is no build, lint, or test tooling in this repo. To view the app:

```sh
open lunch-list-mock.html
```

It's designed for a phone-width viewport (≤560px); use browser devtools device mode when checking on desktop. Since it's a single self-contained HTML file, verify changes simply by reloading it in a browser.

## Architecture (`lunch-list-mock.html`)

- **Store data**: `STORES` is generated from `CATS`/`HOURS`/`CAT_HOURS`/`CAT_PRICE` tables using a fixed-seed PRNG — all store info, hours, and photos are deterministic dummy data, not real. `STORE_BY_ID` indexes it.
- **Open/closed logic**: `statusOf(store, day, minutes)` derives 営業中 / まもなく閉店 / 閉店 from the store's hour pattern for a given day/time.
- **Board sync (`store` abstraction)**: the "board" (candidates + votes + memos) is written through a swappable `store` object with `add`/`update`/`remove`. `connectShared()` tries to detect a `window.claude` runtime (Claude Artifact `db`/`user` capabilities) at startup; if present it wires `store` to a shared Firestore-like collection with realtime `onSnapshot` sync across users, otherwise it silently falls back to the `local` in-memory stub (per-tab only, lost on reload). Any change to board persistence must go through this abstraction rather than mutating `items` directly, so both modes keep working.
- **Rendering**: three render functions (`renderList`, `renderSheet`, `renderBoard`) are all called from the single `onItems()` re-render entrypoint whenever `items` changes (from local mutation or a snapshot). There is no virtual DOM — each function re-does its own innerHTML diffing/rebuild.
- **Views**: 一覧 (list, with category/open-only/sort filters via `buildFilters()`), a detail bottom sheet (`openSheet`/`renderSheet`/`closeSheet`), and ボード (board, a freeform card layout using `randomPos()` to avoid overlapping new candidate cards). Navigation between 一覧/ボード is by top tabs or left/right swipe.
- Dark mode follows OS preference (`prefers-color-scheme`), no manual toggle.

