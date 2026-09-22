# Repository Guidelines

## Instruction priority
- ユーザーから明示的に指示されない限り、`docs/my_rule.md` を開いたり読んだり、その内容を考慮したりしないこと。

## Project Structure & Module Organization

- `lunch-list-mock.html` is the complete mobile-first web app: CSS, markup, mock data, state, and rendering logic live in one file.
- `README.md` documents the product behavior and how to open the mock.


## Build, Test, and Development Commands

There is no build step or dependency installation. Open the app directly:

```sh
open lunch-list-mock.html
```

For development, edit `lunch-list-mock.html` and reload it in a browser. Use a device-emulation viewport of 560px or less to match the target layout. A local static server may be used when needed, for example `python3 -m http.server`, but no server is required for normal use.

## Coding Style & Naming Conventions

Keep the existing self-contained HTML structure and use 2-space indentation for JavaScript, CSS, and markup. Use `camelCase` for functions and local variables (`renderList`, `openSheet`), `UPPER_SNAKE_CASE` for constant data tables (`STORES`, `CAT_HOURS`), and descriptive lowercase kebab-case for HTML IDs/classes. Keep mock data deterministic and route board persistence through the existing `store` abstraction rather than mutating board items directly.

No formatter or linter is configured. Keep changes focused and preserve the existing plain JavaScript style.

## Testing Guidelines

No automated test framework or coverage requirement exists. Manually verify list filters, open/closing status, detail-sheet actions, board add/vote/delete/memo behavior, tab switching/swipes, dark mode, and reload behavior in a browser. Check both a phone-width viewport and a desktop device-emulation view.

## Commit & Pull Request Guidelines

Existing commits use short, task-focused subjects, including Japanese descriptions (for example, `昼めしモック（lunch-list-mock.html）を初期コミット`). Keep commits similarly concise and scoped. Keep each PR under roughly 200 changed lines, explain the behavior changed, and include manual verification notes plus screenshots for visual/UI changes. Discuss each change and obtain explicit approval before merging; reviewers are expected to understand the code directly.

## Security & Configuration Tips

This is a mock with dummy store data and no secrets. Do not commit `.env` files, credentials, or real customer/store data. Google Fonts are the only documented external dependency. If shared persistence is changed, preserve the fallback local mode and avoid exposing runtime credentials in the client.
