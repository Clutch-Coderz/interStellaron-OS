# Settings in the bootable edition

InterstellaronOS 1.0.1 has **1,287 settings**, and every one of them changes something you can see or measure.
`tests/test_settings_all.c` checks that for each setting, every time the tests run.

## Where to change them

- **Settings → All settings**: every setting, grouped into 23 categories, with a search box. Each row has the right
  control for its type: on/off switch, `−` / `+` for numbers, `<` / `>` for choices, a colour swatch (click to cycle,
  click the hex code to type one), or a text box. Changed settings show an accent mark and a **Reset** button;
  **Reset everything** is at the bottom of the category list.
- **Terminal**: `settings` (list categories), `settings WORDS` (search), `get ID`, `set ID VALUE`, `reset ID`,
  `reset all`. Values are written the same way the page shows them: `on`/`off`, a choice's name, `#1a2b3c`, a number.

Changes take effect immediately. Settings that differ from their default are saved a second later to
`System/settings.document.ssf` on the disk, as readable `id=value` lines, and loaded again at boot, before the boot
screens and the sign-in screen, so those use your settings too.

## What's in the 1,287

| Category | Count | Examples |
|---|---|---|
| Windows | 335 | size, position, title, colours and corners per app; minimise/maximise buttons; open at sign-in; Esc closes; snapping |
| Colours | 130 | every colour the desktop draws: palette, buttons, text boxes, Settings, Updates, Files, Notepad, Web, Calculator |
| Terminal | 87 | prompt, colours, cursor, layout, each command on/off, your own name for any command, 8 new commands |
| Start menu | 73 | each item: shown, name, position, icon colour, quick key; menu size and look |
| Games | 72 | Tetris/Snake/Minesweeper colours, speed, controls, next piece, best scores, walls, question marks |
| Keyboard & shortcuts | 72 | 34 shortcuts, each with its own on/off; sticky keys; Caps Lock |
| Wallpaper | 70 | every colour in the five painted scenes, how many petals/clouds/stars/birds, solid colours |
| Desktop & clock | 60 | clock and date style, desktop icons (per app: shown, label, colour), right-click menu |
| Stella | 43 | what she says (greetings, reactions to each app, time of day, events), walking, chatter, sleeping |
| Sound | 39 | each sound: on, pitch, length, loudness; the chime's notes and speed; wave shape |
| Boot & power | 37 | countdown, animation, boot terminal text and command, splash, crash and power-off screens |
| Notifications | 35 | position, size, time, sound, border; colour/sound/time for each app that sends them |
| Lock screen | 32 | clock, date, colours, card, passcode rules, retry limit, buttons |
| Files | 31 | sorting, columns, toolbar, stripes, file-type colours, hidden files, asking before delete |
| Taskbar | 27 | 8 app buttons, clock, icons, tooltips, height |
| System | 27 | device name, display filters, accessibility, idle dim / screen saver / auto-lock, About lines |
| Web browser | 18 | start page links, timeouts, layout |
| Notepad | 15 | syntax colours, line numbers, auto-indent, auto-save |
| Network | 11 | DHCP or a fixed address, DNS, ping, TTL, retries |
| Paint | 11 | brushes, palette, grid, buttons |
| Calculator | 10 | digits, separators, colours, remembering the result |
| Settings app | 10 | start tab, which tabs show, setting IDs, Reset everything |
| Mouse | 6 | speed, left-handed, double-click, invert, hide while typing |

By type: 439 on/off, 366 colours, 206 numbers, 146 text, 50 choices, 44 keys.

Many are fine-grained. A large share are per-app or per-element variants (the same window options for each of the
13 apps; the colour of one part of one screen). Each still has its own visible effect, but they are not 1,287
separate features.

## How each setting is proven

`make test-settings` (about two minutes) builds one desktop with a signed-in user and some files, then:

1. Runs 73 "scenes" — open each app, type in the terminal, play each game, the lock screen, the boot pages,
   sounds, a fake network card, two idle hours, and so on — in forked copies of that exact state, recording which
   settings each scene read and a hash of everything it produced: every pixel drawn plus a log of side effects
   (sounds and their samples, volume, reboot/power-off, network packets, terminal and notepad text, files on disk).
2. For every setting, changes it to another valid value and re-runs the scenes that read it (then all scenes, as a
   fallback). The setting passes only if the hash changes.

A setting fails as **never read** (no code looks at it) or **no effect** (read, but changing it changes nothing).
The current result is `1287 settings checked: 1287 change something, 0 never read, 0 read but no effect`.

1.0.1 added 2: Executor Mode and the screen timeout (the test runs with the timeout at Never, so the hours-long idle
scenes stay visible, and proves the timeout against that).

1.0 added 14, in a new Security & privacy category and Network > IPv6: IPv6 on/off, IPv6 first, private IPv6
addresses, a fallback IPv6 DNS server, IPv4 on/off, answering ping, allowing unverified sites after a warning, the
Global Privacy Control signal, the lock shortcut, password protection strength, refusing common passwords, the
wrong-password report, overwriting deleted files and CPU memory protection. Randomness (salts, IDs, privacy addresses)
is made repeatable for this test only, so two runs can be compared.

0.9 added 20: the taskbar look (Solid / Frosted glass / 3D glass), the taskbar facts and how often they change, the
layout picker and its delay, smooth window motion, its length, minimising into the taskbar button, the drag lean, the
photo wallpaper and your own picture, the lock screen style and its picture, and the App Library & Daily Talk category
(both buttons, space stocks, pointing to open, how often headlines rotate, and how often news and prices refresh).

Settings that only matter when another is on (a dim amount when dimming is off, a pitch when that sound is off) are
checked in scenes that turn the first one on.

## What the test can't see

The test runs the desktop, lock screen, boot pages, sound synthesis, keyboard translation, shell and network stack
as ordinary programs. The glue that only runs on real hardware — `kmain.c` handing those results to the screen,
sound card, PC speaker, PS/2 mouse and network card — is checked by reading it, not by the test. Boot it in
VirtualBox to see that part.

## Files

- `tools/settings_spec.py` — the list of every setting. Run it to regenerate `kernel/settings_gen.{h,c}`.
- `kernel/settings.c` — values, validation, the `id=value` file format, shortcut parsing.
- `kernel/screens.c` — boot terminal, splash, crash and power-off pages, and the boot plan.
- `tests/test_settings_all.c` — the proof test (`make test-settings`; `build/test_settings_all ID...` for some;
  `--shots DIR` for screenshots of the Settings page).
