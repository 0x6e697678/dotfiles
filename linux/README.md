# CachyOS setup

## Dual-booting with Windows

For more details, see `_docs/dual-boot/CachyOS_Windows.md`.

## Bootstrap

Run `bootstrap.sh` for app installation and config setup.

```bash
chmod +x linux/bootstrap.sh
linux/bootstrap.sh
```

## Set theme

Run `set-theme.sh` to recreate the symbolic links for theme configs.

```bash
chmod +x linux/set-theme.sh
linux/set-theme.sh boreal
```

## Font and Cursor

> [!INFO]
>
> Use `nwg-look` for easier GTK settings tweaking.

### Font

#### Installation

Place fonts in `~/.local/share/fonts` then run:

```bash
fc-cache -fv
```

> [!INFO]
>
> Dank Mono P is my patched version of Dank Mono, with added Vietnamese glyphs and Nerd Font icons, and fixes for ligature rendering issues.

#### Other fonts I use

- Inter: https://fonts.google.com/specimen/Inter
- Maple Mono: https://github.com/subframe7536/maple-font.
- More nerd fonts: https://github.com/ryanoasis/nerd-fonts.

### Cursor

#### Installation

Place cursor themes in `/usr/share/icons` then config your DE/WM.

#### Awesome cursor themes

- Bibata: https://github.com/ful1e5/Bibata_Cursor.
- Google Dot: https://github.com/ful1e5/Google_Cursor.
- More cursor themes: https://www.gnome-look.org/browse?cat=107&ord=latest.
