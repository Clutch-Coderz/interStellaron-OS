# InterstellaronOS (isOS) — bootable edition 1.1.1

A small operating system written from scratch: a 512-byte bootloader, a 4 KB second-stage loader, and a 32-bit kernel in
C and assembly. No Linux, no Windows underneath. It boots on its own in VirtualBox — animated boot intro, lock screen
with real accounts, Settings, a web browser that speaks **HTTPS / TLS 1.3 with CA-verified certificates**, live news and
stock prices, a built-in **Software Update** system, three games, a paint app, photo wallpapers, and a Device Manager
that identifies the hardware it finds.

**Setup steps for VirtualBox are in `VIRTUALBOX-GUIDE.txt`.** If you already have the VM, just swap in the new ISO
(Settings > Storage > the CD > choose `InterstellaronOS.iso`). 128 MB of memory is recommended, and tick
**Enable PAE/NX** (Settings > System > Processor) for the full memory protection.

## What's new in 1.1.1 — a fix for starting in VirtualBox
- 1.1.0 stopped right after "isOS boot" with **"FATAL: ata-detect: Failed to detect ATA device"** (a VirtualBox
  BIOS message). The kernel had grown, so the boot sector loaded it all the way up to 0x89200 - past 0x80000, into
  memory a BIOS may keep for itself (1.0.1's ended at 0x76600). That is the most likely cause. Now
  the boot sector loads only the 4 KB stage 2, and stage 2 loads the kernel to 6 MB through a small buffer, the same
  way it already loaded the photo pack. Nothing is loaded at or above 0x80000 any more, and the image check
  (`tools/check.py`) refuses an image that would.
- Nothing else changed: same features and settings as 1.1.0.

## From 1.1 — the Electron edition's 0.14.2 to 0.16.2, in the bootable OS
This release brings the bootable OS level with the Electron edition (0.16.2). Electron itself is unchanged at 0.16.2.

**The Web app works like Chrome**
- **Tabs in the title bar** (up to 8): **+** opens one, each has an **x**, a middle click closes one, Ctrl+T / Ctrl+W /
  Ctrl+Tab. Back and forward are kept for each tab.
- **Links you can click.** Pointing at one shows its address in the corner. Links that ask for a new window
  (`target=_blank`) open a new tab (Settings > Apps > Web app > Pop-ups).
- **Right-click menus:** on a link (Open in new tab, Save link as..., Save link to Downloads, Copy link address), on a
  picture (Open image in new tab, Save image as..., Copy image address), on selected text (Copy, Search for "..."), and
  on the page (Back, Forward, Reload, Bookmark this page, Select all). Zoom is in the ⋮ menu.
- **Select text with the mouse** and copy it with Ctrl+C. Paste works in the address bar, Notepad and the Terminal.
- **Downloads.** "Save as..." asks which folder; files arrive in the background, one at a time, with a notification.
- **Bookmarks:** the star (or Ctrl+D) adds one; a bookmarks bar under the address bar; right-click to rename or
  remove; All Bookmarks. Saved in your (encrypted) home.
- The address bar shows a **padlock** or **"Not secure"**; typing words searches with your **search engine**
  (DuckDuckGo by default; Google, Bing, Brave or Ecosia). A **Home button** and **home page** are optional. Your
  picture sits at the top right, with its own menu.

**Taskbar**
- A **search button**: type and press Enter, and the Web app searches.
- **Weather** next to it, with a panel: now, feels-like, wind, humidity and the next days. Real readings from
  Open-Meteo, for where your internet address says you are (GeoJS, then ipapi.co) or for a city you type
  (Settings > Personalization > Taskbar). Units: automatic (°F in the US), °F or °C. Refreshed every 20 minutes. No
  connection: it says so, it never makes numbers up.
- **A slimmer taskbar while an app fills the screen** (on by default).
- The **logo button is off by default**, like the Electron edition (turn it on in Settings > Personalization >
  Taskbar). **Restart and Shut down** are now in the App Library too.

**Settings looks like Windows 11**
- Your picture and name at the top, **"Find a setting"**, 11 sections with icons (Home, System, Network & internet,
  Personalization, Apps, Accounts, Time & language, Accessibility, Privacy & security, Stella, About), a back button and
  breadcrumbs (Personalization > Taskbar), and switches that slide (they stretch a little with soothing animations).
- **Home**: your PC (with Rename), the network, the weather, recommended pages, quick settings, your account, themes,
  storage and how many settings you've changed.
- **Your picture and name** (Accounts): pick any picture on the disk; it shows in Settings, the Web app and **on the
  lock screen**, which now shows your picture and name, with each account as a chip (your picture, or an initial).
- **About**: device specifications, the Product Unlocker and Software update.
- All 1,290 settings are still in **All settings**, with search.

**Look and feel**
- **Glass colour** (Match theme / Dark / Light) for the frosted and 3D glass taskbar, menus and panels.
- **Soothing animations** (on by default; reduce-motion turns them off): jelly buttons, soft ripples where you click,
  menus that fade in, and windows that melt in and out.
- **Stella keeps busy**: when she's free she reads, has a coffee, naps under a blanket, walks over to poke the app
  buttons, looks things up with a magnifier while you search, and texts (blushing) when a chat site is open.

**The Product Unlocker and the System Log**
- The Unlocker is no longer on the taskbar, the start menu or the desktop: open it from **Settings > About**. Codes
  aren't shown anywhere (no hints); 10 wrong tries a minute is the limit. Executor Mode's `isos_help/apps` lists 11
  apps now.
- The **System Log** (unlocked with a code) now records activity: apps opened and closed (and how long they were
  used), searches, sites visited, commands run, sign-ins (and failed ones), locking, account and picture changes,
  and updates. **Settings > Privacy & security > Log activity** turns this off. Kept in your encrypted home (the last
  2,000 entries).

**1,290 settings** (13 new, the Unlocker's 10 removed with it), and `make test-settings` still changes every one.

### Honest limits of 1.1
- **Not boot-tested in an emulator** (none could be run where it was built), and 1.1.0 didn't start in VirtualBox (fixed
  in 1.1.1, which also hasn't been booted here). Everything was tested natively with
  the real kernel code (see Status and the screenshots), but the first boot is the real test — if anything looks
  wrong, send a screenshot.
- The Web app is still a **text browser**: no JavaScript, no page styles, and pictures show as `[image: ...]` (click
  one to open it on its own if it's a JPEG). Tabs show a **letter badge** instead of the site's icon. A download can be
  up to 1.5 MB, and isOS uses **one connection at a time** (pages, downloads, weather and news take turns).
- Automatic updates from the update website aren't wired in yet; that's the next step (see Software Update).

## From 1.0.1
- **Executor Mode** (Settings > All settings > Desktop & clock > Executor Mode): no app buttons, start menu, App
  Library or desktop icons. A command bar sits on the taskbar instead, in the code font. Type `isos_ajar/browser` (or
  `isos_ajar/files`, `isos_ajar/calc`... `isos_ajar/web` works too) to open an app or bring it back,
  `isos_help/apps` to list all 12 apps with their commands, and `isos_help` for the commands. When no window is
  focused, typing goes straight to the bar; click the bar to type there anytime; Esc leaves it. The commands also work
  in the Terminal.
- **Screen timeout** (Settings > All settings > Lock screen; Never, 30 seconds, 1, 2, 5, 10, 15 or 30 minutes;
  5 minutes by default): when nobody has used the computer for that long, the whole screen goes black (brightness 0,
  no pointer). Moving the mouse or pressing a key turns it back on, and that first move or key does nothing else. If it
  stayed black for 30 seconds or more, you go to the lock screen and need your password.
- Checked: clicking each taskbar app opens it and leaves the taskbar fact alone (the bug reported in the Electron
  edition, where centred apps sat under the fact ticker; the bootable taskbar didn't have it, and a test now proves it).
- **1,287 settings** (2 new); `make test-settings` still changes every one.

## What's new in 1.0 — security
- **Nobody can open another account's files without its password.** Each account has its own home folder, encrypted
  with a key that only that account's password unlocks (scrypt, 16 MB per guess, or 64 MB with "Strong"; files are
  sealed with ChaCha20-Poly1305, and even the file names are encrypted). Signed out, the files are unreadable bytes on
  the disk — the Terminal can't get at them either. Changing your password keeps your files.
- **Sign-in rules that hold:** 8+ characters, common passwords refused; after 5 wrong tries a 30-second wait that
  doubles up to an hour, **kept on the disk** so a restart doesn't reset it (and a clock moved back can't stretch it).
  A new account needs an existing account's password; the owner can delete accounts. After you sign in, isOS says how
  many wrong tries there were since last time. Ctrl+Alt+L locks the screen; it also locks after 10 idle minutes.
- **HTTPS done carefully:** TLS 1.3 only; the whole certificate chain is checked — RSA (2048-bit and up) and ECDSA
  P-256/P-384 signatures, dates on every certificate, what each certificate may be used for, name constraints, and
  the site's name (wildcards only in the leftmost label) — against **116 root authorities from Mozilla's list**. A
  site that can't be verified is **refused, with the reason** (an optional "Continue anyway" exists in Settings,
  off by default). The Web app and the news fetching use **https only** (http is upgraded). Random numbers come from
  the CPU's generator plus timing jitter, so keys can't be guessed.
- **IPv6 first.** Automatic addresses (SLAAC) with **private random addresses**, duplicate-address checks, the
  router's DNS server, AAAA lookups, and a quick fallback to IPv4 when a site's IPv6 doesn't answer. IPv4 still works
  (and can be switched off). A **firewall** answers nothing unsolicited (ping replies are off by default) and counts
  what it blocked; TCP uses random ports and sequence numbers and checks resets.
- **A checked kernel:** the kernel is stored compressed with its SHA-256; a small stub unpacks it and refuses to start
  it if the hash doesn't match. The picture pack is checked against a hash built into the kernel too.
- **CPU memory protection** (Settings > Security; new page tables at start): kernel code is read-only, data can't be
  run as code (**NX**, with PAE/NX ticked), nothing is both writable and executable, page 0 and the page under the
  kernel stack are unmapped (null pointers and stack overflows stop with a clear message instead of corrupting
  memory), write-protect in ring 0, SMEP/SMAP/UMIP when the CPU has them, ring-3 segments and a TSS with no port
  access, a separate double-fault task, a handler for all 256 interrupt vectors, and **random stack canaries** in 232
  kernel functions. If a start ever stops while the protection switches on, the next start turns it off by itself.
- **Settings > Security** shows what protects you right now. 14 new settings (Security & privacy, Network > IPv6).
- **1,285 settings**, and `make test-settings` still changes every one: `1285 settings checked: 1285 change something`.

### Honest limits of 1.0
- **Not boot-tested in an emulator** (none could be installed where it was built). Everything that can run outside a
  real machine was tested natively (see Status) — but the moment the CPU switches to the new page tables, the TSS and
  NX is only checked by testing the tables byte by byte and by reasoning. The automatic turn-off (above) is the safety
  net; if the boot log says `[--] memory protection: turned off`, send me the serial log.
- Desktop apps still run in ring 0: the account separation is enforced by the encryption and the file system, not by
  the CPU. There is no ring-3 sandbox yet.
- No certificate revocation checks; no full address-space randomization; the heap has basic checks only.
- BIOS boot has no Secure Boot or TPM, so someone who can rewrite the whole disk image can rewrite the hash too.

## From 0.9 — the Electron edition's 0.13 features, in the bootable OS
- **Layout picker.** Rest the pointer on a window's maximise button: a panel of **8 layouts** drops down (Halves,
  Thirds, Wide + side, Main + stack, Quarters, Top + bottom, Centre stage, Full screen). Click a part to put the window
  there.
- **Smooth motion.** Windows grow out of their taskbar button when they open, shrink back into it when minimised,
  fade away when closed, glide when maximised, snapped or arranged, and lean a little while you drag them.
  (Settings > All settings > Windows; with "Smooth..." off you get the classic rise-in.)
- **Taskbar looks:** Solid, **Frosted glass** (a blur of the wallpaper behind it) and **3D glass** — a thick glass shelf
  with every app standing on it as a little glass box. Settings > Appearance > Taskbar look.
- **App Library** (taskbar, next to the status area): recent apps, all apps with **type-to-search**, recent files,
  recommendations from what you actually use, **today's special day** (Pi Day, Halloween, Mole Day... with Easter and
  the US/Canada holidays worked out for any year) and what's coming up, and **3 space-industry stocks**.
- **Daily Talk** (next to the App Library; opens when you point at it or click it): the **top 20 real headlines** with a
  picture and summary, a new one every **15 seconds** (with a progress bar, < > and Read the story), then **10 market
  prices** with sparklines, **gaming news** with pictures, and **5 websites worth a visit** (a different five each day).
  Clicking a story opens it in the Web app. Headlines refresh every **5 hours**, prices every 15 minutes.
- **Facts on the taskbar.** When there's room, one of **695 facts** shows beside the apps and changes every
  **10 seconds**; click it for another. A fact is only shown if it fits, so you never see one cut off. Stella walks
  beside it.
- **Photo wallpapers:** **Fjord sunset** (the new default), **Forest light** and **Cherry blossom** — plus **your own
  picture**. Settings > Appearance.
- **Lock screens:** **Colour flow** (the new default), **Northern lights** and **Prism** move and change colour; or one of
  the three photos, or your own picture. Settings > Appearance > Lock screen.
- **Your own pictures, for real:** Paint has a **Save** button (it saves to `Pictures`), the Web app now **shows JPEG
  pictures** and has **Save to Pictures**, and Files can **View** a picture and **Set as wallpaper**.
- **Also new:** a JPEG decoder (baseline + progressive, written from scratch), mouse-wheel scrolling (panels, Web,
  Files, All settings, Terminal), the Web app follows redirects, and a bigger Settings window.
- **1,271 settings** (20 new, in a new "App Library & Daily Talk" category among others). A test still changes every
  one of them and checks that something changes: `1271 settings checked: 1271 change something`.

### Where the live data comes from
The same sources as the Electron edition, fetched by the kernel's own TCP/IP and TLS 1.3 code:
- **News:** the RSS feeds of BBC News (two feeds, one share), NPR, The Guardian and Al Jazeera. Titles and summaries are
  the outlets' own words; pictures come from the feed or the article's own preview image (`og:image`).
- **Gaming news:** Polygon, GameSpot, Eurogamer and Rock Paper Shotgun.
- **Prices:** Yahoo Finance's chart data (two hosts), with Stooq as a fallback. Prices are delayed and not for trading.
- If a source can't be reached it's simply left out; if none can, the **last real copy** stays with its time and a note;
  if there has never been a copy, the panel says so. **Nothing is made up.** Times like "17 min ago" use the time the
  web servers report, so they're right even if the VM's clock isn't.
- Nothing is fetched while you're signed out, and switching the Daily Talk button off stops the news fetching.

### Honest limits of 0.9 (still true unless 1.0 says otherwise)
- **I couldn't boot-test 0.9 in an emulator this time** (QEMU couldn't be installed where it was built). Everything that
  can run outside a real machine was tested natively (see Status). The part that only a real boot can prove is the
  loader: stage 2 now also copies the **picture pack** (the three photos and the facts, ~460 KB) to 4 MB using the BIOS.
  If that step fails, isOS still boots — the photos fall back to the painted scenes, facts are off, and the boot log
  says `[--] picture pack`. If you see that, send me a screenshot.
- The network stack has **one connection at a time**, shared with the Web app, so Daily Talk fetches one thing after
  another in the background (about 40 requests the first time — give it a minute or two) and waits while a web page
  loads. Opening a page mid-fetch can make one item miss its picture until the next refresh.
- Sites whose certificates use **ECDSA** still connect encrypted but unverified (as in 0.7/0.8); the data is used either
  way.
- Pictures: **JPEG** (baseline and progressive) and Paint's own pictures. PNG, WebP and GIF aren't decoded yet; a
  headline without a JPEG picture shows a calm placeholder with the outlet's name.
- The text uses the built-in 8x16 font, so accents in headlines are written without them (café → cafe).

## From 0.8
- **1,251 settings that all do something.** Settings → **All settings** lists every one, with search, the right
  control for each, and Reset. The Terminal can do the same (`settings`, `get`, `set`, `reset`). A test changes each
  setting and checks the screen or behaviour changes — see `SETTINGS-ENGINE-README.md` for what's in there and how
  it's checked.
- **New on the desktop:** minimise and maximise buttons, drag a window to an edge to fill half the screen, desktop
  icons, a right-click menu, four more taskbar buttons with names on hover, an idle screen saver / dimming /
  auto-lock, display filters (brightness, night light, greyscale, invert), sticky keys and high contrast.
- **New in the apps:** Notepad line numbers and auto-save; Files sorting, dates, New folder and per-type colours;
  eight new Terminal commands (`date`, `whoami`, `hostname`, `history`, `calc`, `uname`, `fortune`, `tree`);
  remappable game controls, the next Tetris piece, best scores, Snake walls; Stella reacts to what you do.
- **Smoother boot.** The boot animation and countdown draw off-screen and show whole frames, so they no longer
  flicker.
- **Fixes:** every sound except the beep was playing the start-up chime; Tetris rotation read the wrong memory and a
  rotated piece could freeze the system on hard drop; calculator results showed `%ld`; a mouse release after
  pressing a title bar was lost; a wide About window wrote past a buffer; saved settings weren't loaded until after
  the boot screens and sign-in.
- The kernel's zero-filled memory (stack and buffers) now lives at 1 MB, so the larger kernel still fits below
  the 512 KB the boot loader can load.

## From 0.7
- **CA certificate verification — from scratch.** TLS now *verifies who it's talking to*. A complete X.509 path:
  **ASN.1 / DER** parsing, **RSA** signature verification (PKCS#1 v1.5 **and** PSS, with MGF1) built on a from-scratch
  **bignum** doing **Montgomery** modular exponentiation, **certificate-chain** walking, **hostname / SAN** matching
  (wildcards included), and validity-date checks. The TLS `CertificateVerify` signature is checked, then the chain is
  walked to an embedded **trusted root store** (ISRG Root X1, DigiCert Global Root G2, USERTrust RSA). Every piece is
  checked against **OpenSSL-generated** vectors, and the full chain verifies **end-to-end against a live OpenSSL
  server** in the test suite (`cert_status = VERIFIED`). Honest scope: RSA chains verify; **ECDSA** chains complete the
  encrypted handshake but show as *encrypted, identity unverified* (ECDSA cert verification isn't implemented yet). The
  browser tells you which you got.
- **Software Update — like Windows Update, but honest about scope.** Settings now has an **Updates** tab showing your
  installed build and whether one is available. A team unlocks the **Update Team Controller Panel** with the product
  key **`UPD.TEAM`** (then a team password). From there they pick an update package, set the new version + release
  notes, and **Publish** — which notifies every account **on that machine**: *"Update time! Update now, In 30 minutes,
  or When I close the laptop?"* Pick **Update now** for an animated install; choose **when I close the laptop** and it
  applies on the next sleep/boot. The version is persisted to the system config on disk. Honest scope: this is a
  **single-machine** update channel — isOS has no internet fleet server, and it swaps the version/record rather than
  self-flashing a new bootloader. See *Honest notes*.
- **Animated boot intro.** A starfield, the isOS logo igniting with orbiting sparks, and the title + maker credit fading
  in (~2 s; press any key to skip) — then the countdown. Shows **"Built by InterStellaron Mechanical Dynamics."**

## From 0.6
- **HTTPS / TLS 1.3 — from scratch.** The browser does `https://`. A complete TLS 1.3 stack:
  **X25519** key exchange, **ChaCha20-Poly1305** AEAD, **SHA-256 / HMAC / HKDF** key schedule, and the handshake state
  machine — all written from nothing. Every primitive is checked against official test vectors (RFC 8439, RFC 7748,
  RFC 5869, FIPS, and the **RFC 8448** TLS-1.3 key-schedule reference), and the whole handshake **completes against a
  real OpenSSL TLS 1.3 server** in the test suite. (As of 0.7 the certificate identity is also verified — see above.)

## From 0.5
- **Wallpapers.** Five hand-painted scenes drawn by the kernel — **Cherry blossom, Sunny beach, Calm forest, Busy city,
  Japanese sunset** — plus a **solid-colour** picker, in Settings → Appearance (saved to disk). Open **Paint**, draw
  anything, and press **Set as background** to use your own art (applies for the session).
- **Device Manager** (Settings → Devices). Reads the **CPU** via CPUID (brand, vendor, family/model, feature flags:
  SSE/SSE2/AVX…), the RAM, and enumerates the **PCI bus** with vendor/device names (chipset, IDE, VGA, the e1000 NIC,
  AC'97 audio, USB…). This is isOS identifying the parts inside the machine.
- **Cleaner animations.** Windows now rise into place when opened, and Stella has a gentle walk-bob.
- **PC speaker driver.** A universal tone generator (PIT channel 2) — the chime/beep now work even with no AC'97 card.

> Honest notes: **HTTPS/TLS with CA verification is now in** (TLS 1.3 + X.509 RSA chain validation, verified end-to-end
> against OpenSSL). It resists both eavesdropping and an active MITM on RSA chains; **ECDSA** certificate chains aren't
> verified yet (they connect encrypted but show as *identity unverified*). The **Software Update** system is a real,
> working update channel **for one machine** — the Controller Panel publishes a new build and every account on that
> machine is notified and updated — but there is **no internet fleet server** (isOS can't phone home to push updates to
> other people's computers), and applying an update swaps the stored version/record rather than re-flashing the
> bootloader from scratch. **Real GPU acceleration** still isn't feasible from scratch (the VESA framebuffer is already
> the GPU's output). "Every driver" is unbounded — isOS ships drivers for what VirtualBox emulates and *identifies*
> everything else in the Device Manager.

## First boot: what you'll see
1. **Boot screen (text).** The ASCII banner, then a 15-second countdown. Type
   `stellaron.boot.graphics(open_graphics_boot)` (or just `g`) to launch immediately, or wait — the desktop starts on
   its own when the timer runs out.
2. **Lock screen.** First time, click **New Account**, pick a username and a passcode (any characters — e.g.
   `hiMyNaM31s&^#*@`), and it's saved to the disk. Next boot you sign in with it. You can make several accounts and
   switch between them; log out any time from **Settings → Account**.
3. **Desktop.** Everything below.

## From 0.4
- **Boot countdown** with the `stellaron.boot.graphics(...)` command, shown in graphics mode with a live timer.
- **Lock screen + accounts.** Create accounts with passcodes, stored on disk (salted + hashed), multi-account, logout.
- **Settings app** — a real sidebar: **Account** (create/delete/switch, log out), **Appearance** (6 wallpaper themes,
  saved to disk), **Sound** (volume), **Network** (status), **System** (RAM, graphics, disk, uptime, kernel size).
- **Web browser.** A from-scratch **TCP** stack on top of the existing IP layer, an **HTTP** client and a small **HTML**
  renderer. Type a URL, it does DNS → TCP → HTTP and shows the page as text. (http:// only — no HTTPS/TLS yet.)
- **Games:** **Tetris** (rotate/drop, line clears, levels), **Snake**, **Minesweeper** (left reveal, right flag,
  flood-fill, win/lose). Press **R** to restart after game over.
- **Paint** — 10 colours, three brush sizes, freehand drawing, Clear.
- **Notepad syntax highlighting** for code files (`.c .h .js .py .sh .json …`): keywords, strings, comments, numbers.
- **Files** gains **Copy**, **Rename** and **Delete**.

## Also included (from 0.3)
- **Graphics mode.** 32-bit VESA framebuffer, a composited desktop with a dirty-rectangle renderer, draggable windows,
  a start menu, taskbar, clock, status icons, notifications, and Stella walking the taskbar (she blinks, chats, and
  sulks to the corner if poked too much). Falls back to text mode if there's no framebuffer.
- **Memory management.** BIOS memory map, a 4 KB-page bitmap allocator, a merging kernel heap, and paging.
- **Disk + file system.** IDE driver and ISFS: folders, files, and `name.type.class` naming. A blank disk is set up
  automatically; a disk with other data is never touched.
- **Network.** Intel PRO/1000 (e1000) driver, DHCP, ARP, IP, ICMP ping, DNS — and now TCP/HTTP. In VirtualBox this
  rides on your PC's Wi-Fi (the guest never sees a Wi-Fi card; real Wi-Fi chips aren't supported yet).
- **Sound.** Intel AC'97 driver, a synthesizer, a startup chime, `beep`, `chime`, `volume`.

## Files
| File | Use it for |
|---|---|
| `InterstellaronOS.iso` | **VirtualBox (easiest).** Attach it as a CD. |
| `InterstellaronOS-disk.vmdk` + `InterstellaronOS-disk.img` | VirtualBox as a boot hard disk (keep both together). |
| `InterstellaronOS.img` | Floppy image, or for writing to a USB stick. |
| `source/` | Source code, build tools and tests. |

## Status: built and checked
**1.1:** compiles clean (warnings are errors); the kernel is 478 KB compressed (the boot stub's limit is 576 KB); image, ISO
and VMDK checks pass; `make test` passes, now including a **1.1 desktop test** (65 checks with screenshots: tabs,
links, right-click menus, text selection and copy, downloads into the folder you pick, bookmarks, the slim taskbar,
Settings' sections, back button, Find a setting, your picture and name, the Unlocker and the System Log, Stella),
**weather** parsing and fetching over the kernel's own network stack against stand-in sites (in the 85 live-data
checks), the HTML links test, the lock screen with your picture, and `make test-settings`:
`1290 settings checked: 1290 change something`.

**1.0:** compiles clean (warnings are errors); image, ISO and VMDK checks pass; `make test` passes, now including:
**accounts** (59 checks: encrypted homes on a RAM disk, isolation between accounts, lockout kept across restarts, a 0.9
disk upgraded), **crypto** (185 checks against OpenSSL and the RFC/NIST vectors: SHA-384/512, ECDSA P-256/P-384,
AES-GCM, PBKDF2, scrypt), **31 real TLS 1.3 connections** to a local OpenSSL server (12 good chains verified, 19 bad ones
refused with the right reason), **all 117 Mozilla roots** parsed and fuzzed (4,298 checks), **IPv6/IPv4** (57 checks with
a fake dual-stack router, including spoofed packets), the **boot stub** (7), the **page tables and GDT/IDT bytes** (38),
and `make test-settings` (all 1,285 settings).

**0.9:** compiles clean (warnings are errors); the image, ISO and VMDK checks pass (including the new picture-pack
layout); `make test` passes, now including a from-scratch **JPEG decoder test** (42 pictures compared with libjpeg — the
worst match is 49.9 dB — plus 2,400 deliberately broken files that must fail cleanly under AddressSanitizer), a
**live-data test** (62 checks: real RSS/Atom/JSON/CSV formats, then the whole fetch queue over the kernel's TCP/IP stack
against a fake network with redirects, chunked replies, outages and retry timing), a **0.9 desktop test** (17 checks with
screenshots at 1024x768), the **lock screen styles** test, and `make test-settings` (all 1,271 settings). Not done:
booting 0.9 in an emulator (see Honest limits).

Earlier: versions 0.2 and 0.3 booted in VirtualBox. 0.7 compiled clean (warnings are errors), the image/ISO/VMDK checks passed, and
~200 automated checks across 14 suites pass: memory allocator, file system, network stack **including a full TCP + HTTP
GET + HTML render against a fake NAT router**, **26 crypto vectors (SHA-256/ChaCha20/Poly1305/X25519/HKDF/TLS-1.3 key
schedule, plus bignum + RSA PKCS#1-v1.5/PSS verify)**, **X.509 parse + RSA chain + hostname checks vs real OpenSSL
certs**, a **live TLS 1.3 handshake with CA verification against a real OpenSSL server** (`cert_status = VERIFIED`),
shell, desktop rendering, the **Software Update flow** (product-key gate, Controller Panel, notification, install), and
20,000 random clicks/keys. The wallpapers, Device Manager, games, Settings, browser, paint, boot-intro, Update-panel and
syntax-highlight screenshots in `build/` are rendered by the real kernel code — the Device Manager one shows CPUID
reading the real host CPU, and the TLS one is a real OpenSSL interop. The loader stage and hardware drivers run on the
real (virtual) machine, so if the first boot of 0.7 misbehaves, send a screenshot and the serial log (see the guide) for
a quick fix.

### Using the Software Update system (the team side)
Open **Settings → Updates → Enter product key**, type **`UPD.TEAM`**, then the team password. That opens the **Update
Team Controller Panel**: pick an update package (any file on the isOS disk stands in for the package), set the new build
number and the "what's new" note, and press **Publish Update**. Every account on the machine then gets the *"Update
time!"* notification with **Update now / In 30 minutes / When I close the laptop**. (The product key and team password
gate the publisher side — they're in the panel's own code, not shipped here in plain text.)



## Terminal commands
`help` · `about` · `info` · `uptime` · `mem` · `video` · `ls [folder]` · `cat FILE` · `write FILE TEXT` · `mkdir NAME` ·
`rm PATH` · `rename PATH NEWNAME` · `df` · `disk` · `format yes` · `net [renew]` · `dns NAME` · `ping HOST` · `beep` ·
`chime` · `sound` · `volume 0-100` · `open files|terminal|notepad|about` · `stella` · `echo` · `clear` · `reboot` · `poweroff`

The desktop apps (Web, Settings, Paint, Tetris, Snake, Minesweeper, plus Terminal/Files/Notepad/About) open from the
**start menu** (the logo at the bottom-left of the taskbar).

## Build it yourself (optional)
Needs gcc, GNU binutils and Python 3 (on Windows: WSL Ubuntu or MSYS2). In `source/`: `make` builds the images,
`make test` runs the tests. `tools/mkart.py` regenerates `kernel/art.c` (fonts, logo, Stella, icons; needs Pillow).

## How it starts
BIOS → `boot/boot.s` (loads the rest to 0x10000) → `boot/stage2.s` (picture pack to 4 MB, memory map, VESA graphics mode, protected mode) →
`boot/stub.c` at 0x11000 (unpacks the kernel to 1 MB and checks its SHA-256) → `kernel/entry.s` → `kmain`: CPU tables,
interrupts, memory + paging, random numbers, mouse, disk, memory protection, sound, network →
**boot countdown** → **lock screen** (sign in / create account) → desktop.

## Source layout
1.0: `kernel/users.c` accounts + key store · `kernel/fs.c` encrypted homes · `kernel/net.c` IPv6 + IPv4 + firewall ·
`kernel/tls.c`, `x509.c`, `certverify.c`, `roots_gen.c` (from `tools/mkroots.py`) HTTPS · `kernel/ecc.c`, `aes.c`, `sha512.c`,
`kdf.c`, `random.c` crypto · `kernel/pagetables.c` (the protection policy, tested) + `kernel/cpuprot.c` (switching it on) ·
`kernel/idt.c` GDT/TSS/IDT · `boot/stub.c` + `tools/lzpack.py` the checked, compressed kernel.
0.9: `kernel/jpeg.c` JPEG decoder + picture scaling · `kernel/feeds.c` news/gaming/price fetching and parsing ·
`kernel/pack.c` the picture pack · `kernel/desk09.inc` the App Library, Daily Talk, facts, layout picker, window motion,
glass taskbars, photo wallpapers and picture chooser · `kernel/lock.c` the lock screen styles · `tools/mkpack.py` builds
the pack from `assets/`.
`kernel/tls.c` TLS 1.3 handshake/records · `kernel/sha256.c` SHA-256/HMAC/HKDF · `kernel/chacha.c`
ChaCha20-Poly1305 · `kernel/x25519.c` X25519 · `kernel/bootanim.c` boot intro · `kernel/net.c` TCP + HTTP(S) ·
`kernel/hw.c` CPUID + PCI naming · `kernel/pcspk.c` PC speaker · `kernel/users.c`/`lock.c` accounts + lock screen ·
`kernel/games.c` games · `kernel/html.c` HTML→text · Settings, Device Manager, Paint, browser UI, wallpaper scenes and
syntax highlighting in `kernel/desktop.c`. Crypto is proven by `tests/test_crypto.c` + the OpenSSL interop in
`tests/tls_server.py`.

## Next milestones
ECDSA certificate checks · PNG pictures · clickable browser links · several connections at once · more PCI drivers
(PCnet, HD Audio, AHCI) · multitasking · USB input · 64-bit.
