# Fork changes

- Removed all sound playback functionality (no more system beeps, OS sounds, or Linux sound file playback)
- Removed terminal bell (`BEL` / `\a`) and attention mode feature
- Removed `--notify-sound` and `--notify-attention` CLI flags
- Notifications only: desktop notifications (Windows toast, macOS osascript, Linux notify-send) and terminal notifications (Kitty OSC 99, OSC 777)
- Simplified `notifyOutcome()` function and related code

# pi-notify-agent

![pi-notify-agent preview](./assets/preview.png)

Cross-platform desktop notifications for [pi-coding-agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent).

When a pi run takes longer than a configurable threshold, this package notifies you when the agent:

- finishes successfully
- stops with an error / provider failure / connection-like failure

## Features

- **Windows:** native toast notifications
- **macOS:** native notification via `osascript`
- **Linux:** `notify-send` notifications
- **Terminal fallback:** Kitty `OSC 99`, otherwise `OSC 777`
- **Noise reduction:** default threshold is **3000ms**
- **pi commands:** `/notify-test`, `/notify-test error`, `/notify-status`
- **CLI flags:** configure threshold and on/off behavior without editing code

## Install

### From a local folder

```bash
pi install ./pi-notify-agent
```

### From GitHub

```bash
pi install https://github.com/AlyusLabs/pi-notify-agent
```

### From npm

```bash
pi install npm:pi-notify-agent
```

After installing, reload pi:

```text
/reload
```

## Quick test

```text
/notify-test
/notify-test error
/notify-status
```

## Default behavior

By default the package:

- waits until the run lasts at least **3000ms**
- sends notifications for **success**
- sends notifications for **error**
- ignores **aborted** runs

## CLI flags

The extension registers these pi flags:

- `--notify-min-ms <number>`
- `--notify-success <on|off>`
- `--notify-error <on|off>`

### Examples

```bash
# Only notify for long runs (5 seconds)
pi --notify-min-ms 5000

# Disable success notifications
pi --notify-success off

# Only notify on errors
pi --notify-success off --notify-error on
```

## Development

Run the extension directly without installing:

```bash
pi -e ./extensions/index.ts
```

Or load the package from its folder:

```bash
pi install .
```

## Package structure

```text
pi-notify-agent/
  extensions/
    index.ts
  package.json
  README.md
  LICENSE
```

## Publishing

### 1. Create a Git repo

```bash
git init
git add .
git commit -m "feat: initial pi notification package"
```

### 2. Push to GitHub

```bash
git remote add origin https://github.com/AlyusLabs/pi-notify-agent.git
git push -u origin main
```

Users can then install with:

```bash
pi install https://github.com/AlyusLabs/pi-notify-agent
```

### 3. Publish to npm

If the package name is already taken, rename the `name` field in `package.json` first.

```bash
npm login
npm publish --access public
```

Then users can install with:

```bash
pi install npm:pi-notify-agent
```

## Notes

- Linux desktop notifications require a GUI session and usually `notify-send`.

## License

MIT
