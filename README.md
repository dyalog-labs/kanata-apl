# Kanata APL Layouts

<div align="center"><img
    alt="Image of a keycap with the APL logo on it in green tones"
    title="Kanata APL Layouts"
    align="center"
    height="220"
    src="assets/kanata-apl-icon.svg"
  />
  <p>International keyboard layouts for APL.</p>
</div>

[![GitHub Licence](https://img.shields.io/github/license/rikedyp/kanata-apl)](LICENSE)

- [About](#about)
- [Installing kanata](#installing-kanata)
- [Running a layout](#running-a-layout)
- [Available layouts](#available-layouts)
  - [Shifting key layouts](#shifting-key-layouts)
  - [Prefix key layouts](#prefix-key-layouts)
- [Typing APL](#typing-apl)
- [Optional modules](#optional-modules)
  - [Box drawing on the keypad](#box-drawing-on-the-keypad)
  - [AltGr compositions](#altgr-compositions)
- [Customising](#customising)
- [Troubleshooting](#troubleshooting)

## About

[Kanata](https://github.com/jtroo/kanata) is cross-platform software to modify the behaviour of keyboard input. This repository contains Kanata configuration files to enable [typing APL symbols](https://aplwiki.com/wiki/Typing_glyphs). The configurations ship as **.kbd** files that you point Kanata at.

## Installing kanata

Getting Kanata itself installed and starting at login is different on every platform, and may change over time. Rather than repeat those steps here and let them drift out of date, follow Kanata's own documentation, which its authors keep current:

- **Windows and macOS:** the [Kanata releases page](https://github.com/jtroo/kanata/releases) for binaries, and the [Kanata configuration guide](https://github.com/jtroo/kanata/blob/main/docs/config.adoc) for details.
- **Linux:** [setup-linux.md](https://github.com/jtroo/kanata/blob/main/docs/setup-linux.md), which covers permissions, systemd, and Wayland.
- **Starting at login:** the [discussion on running Kanata in the background](https://github.com/jtroo/kanata/discussions/122). System administrators and others with particular security concerns should also see [Yvan-Masson's suggested configuration](https://github.com/jtroo/kanata/discussions/130#discussioncomment-11377658).

On Windows, the `winiov2` binary is the easiest to start with because it needs no driver. The `wintercept` binary works in more applications, including elevated ones, but requires the Interception driver. The `gui` variants add a system-tray icon; the `tty` variants run in a terminal window that stays open. If you use `wintercept`, copy `interception.dll` next to the executable rather than relying on `system32\drivers`, where loading it is unreliable.

## Running a layout

1. Download a **.kbd** file from this repository. If your layout uses the optional modules below, keep every file it needs in the same directory.
2. Point Kanata at it:

   ```
   kanata --cfg en-GB_APL.kbd
   ```

   Add `--check` to validate the configuration without starting anything.

To stop Kanata at any time, hold <kbd>Left Ctrl</kbd> + <kbd>Space</kbd> + <kbd>Esc</kbd>.

The key names in the configurations come from the `str_to_oscode` function in [the Kanata source](https://github.com/jtroo/kanata/blob/main/parser/src/keys/mod.rs) and follow the US English layout. These are often the same physical keys as on other layouts. Where they differ, the Kanata documentation covers [configuration for non-US keyboards](https://github.com/jtroo/kanata/blob/main/docs/config.adoc#non-us-keyboards).

## Available layouts

### Shifting key layouts

These configurations define one or more keys that, while held, output APL symbols. The defaults are <kbd>Caps Lock</kbd> and <kbd>Right Ctrl</kbd>. Double-tap the key to perform its original action; the double-tap timeout is 200ms, which you can change in the `defalias apl-mode` section. To choose different keys, edit the section beginning `deflayermap base`.

| Layout (link to diagram)                       | Kanata configuration file      |
| ---------------------------------------------- | ------------------------------ |
| [English (United Kingdom)](./layouts.md#en-gb) | [en-GB_APL.kbd](en-GB_APL.kbd) |

### Prefix key layouts

A prefix key is entered immediately before the corresponding key rather than held. These configurations use <kbd>Backquote</kbd> (also known as <kbd>backtick</kbd> or <kbd>grave</kbd>, <kbd>\`</kbd>) and <kbd>Backslash</kbd> (<kbd>\\</kbd>) as prefix keys. Configure them by editing the section beginning `deflayermap base`. Double-tap a prefix key for its original action. The double-tap timeout is 200ms and set in `defalias apl-mode`.

## Typing APL

Hold <kbd>Caps Lock</kbd> or <kbd>Right Ctrl</kbd> and press a key:

```
Caps+E   ∊        Caps+I   ⍳        Caps+[   ←
Caps+A   ⍺        Caps+W   ⍵        Caps+J   ∘
```

Add <kbd>Shift</kbd> for the second set. Press them in either order, so <kbd>Caps</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd> and <kbd>Shift</kbd>+<kbd>Caps</kbd>+<kbd>E</kbd> both give `⍷`.

```
Caps+Shift+E   ⍷        Caps+Shift+F   ⍛        Caps+Shift+Z   ⊆
```

**Double-tap <kbd>Caps Lock</kbd>** for actual Caps Lock. Everything the layout does not map types normally, including <kbd>Shift</kbd> for capitals and shifted punctuation.

## Optional modules

The en-GB layout ships with two add-ons already included: Dyalog-style box drawing on the numeric keypad, and [abrudz-style](https://github.com/abrudz/Kbd/) dead keys for accents, subscripts, and composed glyphs. Each is one extra `.kbd` file that `en-GB_APL.kbd` pulls in.

| File                  | What it adds                                                     |
| --------------------- | ---------------------------------------------------------------- |
| `apl-numpad-box.kbd`  | Box-drawing characters on the keypad.                            |
| `altgruk-compose.kbd` | AltGr dead keys: accents, sub/superscripts, composed APL glyphs. |

### Box drawing on the keypad

Hold an APL key and use the keypad. The characters are arranged the way they look on screen:

```
    7 ┌    8 ┬    9 ┐
    4 ├    5 ┼    6 ┤
    1 └    2 ┴    3 ┘
      0 ─         . │
```

So <kbd>Caps</kbd>+<kbd>Numpad6</kbd> gives ┤. NumLock makes no difference, and the keypad behaves normally when no APL key is held.

This is enabled by two lines in `en-GB_APL.kbd`:

```
(deflayermap APL
  ...
  (t! numpad-box)               ;; inside the APL layer
```

```
(include apl-numpad-box.kbd)    ;; near the bottom, above (defsrc)
```

To disable it, delete both lines or do not include the file next to the layout.

### AltGr compositions

Press <kbd>AltGr</kbd> with one of seven keys to start a dead key sequence. The subsequent keystroke produces the composed character.

| Start with                        | Dead key | Gives you                                         |
| ---------------------------------- | -------- | ------------------------------------------------- |
| <kbd>AltGr</kbd>+<kbd>W</kbd>      | `^`      | superscripts, circumflex accents, the quad family |
| <kbd>AltGr</kbd>+<kbd>R</kbd>      | `¨`      | umlauts, diaeresis operators                      |
| <kbd>AltGr</kbd>+<kbd>Y</kbd>      | `~`      | tildes, fractions, currency                       |
| <kbd>AltGr</kbd>+<kbd>U</kbd>      | `_`      | subscripts, underbar glyphs, circled letters      |
| <kbd>AltGr</kbd>+<kbd>A</kbd>      | `´`      | acute accents                                     |
| <kbd>AltGr</kbd>+<kbd>S</kbd>      | `` ` ``  | grave accents, box drawing on the number row      |
| <kbd>AltGr</kbd>+<kbd>Space</kbd>  |          | typographic spaces and dashes                     |

Shift is optional when starting a sequence, so <kbd>AltGr</kbd>+<kbd>Y</kbd> and <kbd>AltGr</kbd>+<kbd>Shift</kbd>+<kbd>Y</kbd> both begin the `~` dead key. Once started, the next keystroke decides what you get:

| Keystroke              | Result                                   | Example                                                          |
| ---------------------- | ----------------------------------------- | ------------------------------------------------------------------ |
| plain key               | the lowercase or unshifted entry         | <kbd>AltGr</kbd>+<kbd>A</kbd> then <kbd>e</kbd> → é                |
| <kbd>Shift</kbd>+key    | the uppercase or shifted entry           | <kbd>AltGr</kbd>+<kbd>A</kbd> then <kbd>Shift</kbd>+<kbd>E</kbd> → É |
| **APL key**+key         | the entry built on that key's APL glyph  | <kbd>AltGr</kbd>+<kbd>Y</kbd> then <kbd>Caps</kbd>+<kbd>E</kbd> → ∉ |
| <kbd>Space</kbd>        | the bare accent character                | <kbd>AltGr</kbd>+<kbd>A</kbd> then <kbd>Space</kbd> → ´            |
| <kbd>Esc</kbd>          | cancels, types nothing                   |                                                                    |
| anything unmapped       | types normally, and cancels              |                                                                    |

The sequence stays open until one of those happens. Here are some more examples:

```
AltGr+Y  then  =          ≈          AltGr+U  then  3          ₃
AltGr+U  then  a          ⓐ          AltGr+U  then  Shift+A    Ⓐ
AltGr+U  then  Caps+E     ⍷          AltGr+R  then  Caps+J     ⍤
AltGr+S  then  7          ┌          AltGr+W  then  Caps+O     ⌼
```

The APL-key case is where this differs from abrudz's Windows layout. There, the glyph you compose on is typed with <kbd>AltGr</kbd>; here it is typed with your APL key, because that is where the glyphs live in this layout. Six characters need **APL+Shift** because their base glyph does: ǽ ǿ ǻ Ǽ Ǿ Ǻ, on <kbd>AltGr</kbd>+<kbd>A</kbd> then <kbd>Caps</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd>/<kbd>V</kbd>/<kbd>B</kbd>/<kbd>F</kbd>/<kbd>G</kbd>/<kbd>H</kbd>.

The full tables live in `altgruk-compose.kbd`, one commented layer per dead key. Enabling the module (already done in the shipped file) takes two lines in `en-GB_APL.kbd`:

```
(deflayermap base
  ...
  $dia   @dia-mode                 ;; inside the base layer
```

```
(include altgruk-compose.kbd)      ;; near the bottom, above (defsrc)
```

To disable it, delete both lines or do not include the file next to the layout.

## Customising

**Add another APL shifting key.** Map any key to `@apl-mode` in the base layer of `en-GB_APL.kbd`:

```
(deflayermap base
  $apl   @apl-mode
  rctrl  @apl-mode
  rmet   @apl-mode      ;; added
  ...
```

**Change the main APL key.** Edit `(defvar apl caps)` near the top. If you also use `altgruk-compose.kbd`, update `$apl-held` and `$apl-sft` at the top of that file to name your key, since they cannot read `$apl`.

**Change the key that starts a sequence.** Edit `(defvar dia ralt)` near the top of `en-GB_APL.kbd`. `rmet` and the <kbd>Menu</kbd> key both work well.

## Troubleshooting

**AltGr behaves oddly, or applications see stray Ctrl presses.** In the `defcfg` block at the bottom of `en-GB_APL.kbd`, change `windows-altgr add-lctl-release` to `cancel-lctl-press`. If it stays awkward, use a different key for `$dia`.

**A glyph does not appear in one particular application.** Some applications do not accept synthesised Unicode input. Kanata's clipboard actions are an alternative; see [the configuration guide](https://jtroo.github.io/config.html).

**Editing the config.** Run with `--check` to validate before restarting. Kanata can also live-reload: map a key to `lrld` while you iterate.
