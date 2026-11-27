---
name: browser-use
description: Browse logged-in sites through the dedicated automation Chrome, one profile per site, using the browser-use CLI. Use when a task needs a site that requires sign-in.
---

# Browser Use

Use the dedicated automation Chrome, never my regular Chrome. Browser-use commands connect to it at `BU_CDP_URL=http://127.0.0.1:9223`.

Data directory: `~/Library/Application Support/Google/Chrome-BrowserUse/`

## Profiles

Profile names are case-sensitive. Each site gets one profile, and profiles are reused. Don't create a new profile for a site that already has one. The Chrome profile label matches its folder name. When you create a profile, use the same name for both.

Example rows. Add one row per site:

| Profile | Use for | Start URL | Check after sign-in |
|---|---|---|---|
| `Example-Site` | Example site | https://example.com/ | Signed in as the expected user |
| `Example-Env` | Example app, production | https://app.example.com/ | Account `<ACCOUNT_ID>`, environment PRODUCTION |
| `Example-Env-Sandbox` | Example app, sandbox | https://sandbox.example.com/ | Account `<ACCOUNT_ID>`, environment SANDBOX |

Never share one profile across unrelated sites. Browser sign-in is separate from any MCP server sign-in. Never store passwords in this skill.

## Connect

1. Check the browser: `curl -s --max-time 2 http://127.0.0.1:9223/json/version`. If it responds, reuse it. Don't start a second one.
2. If it doesn't respond, start it with the profile you need:

   ```sh
   open -na "Google Chrome" --args \
     --user-data-dir="$HOME/Library/Application Support/Google/Chrome-BrowserUse" \
     --profile-directory=<Profile> \
     --remote-debugging-address=127.0.0.1 \
     --remote-debugging-port=9223 \
     --no-first-run --no-default-browser-check \
     "<Start URL>"
   ```

   Other profiles open as windows in this same browser. Run the command again with another `--profile-directory` to add one.
3. Run commands with `BU_CDP_URL=http://127.0.0.1:9223 browser-use`. Run them one at a time. Don't set `BU_NAME`.

## Rules

- Read-only. Navigate, search, and view. Create, edit, submit, send, or delete only when I explicitly ask. Signing in is allowed.
- Confirm the account and environment in the UI before acting, using the table above.
- Close tabs you opened when you're done, unless I still need them.
