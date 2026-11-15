# sys-scripts

<div align="center">
  <img src=".github/banner.png" />
  <br>
  <hr>
  One-off system scripts for random use cases
  <br>
  Not a package, not a toolkit, just the bin
</div>

## Setup

Three kinds of scripts:

- **Standalone**, run from `PATH`: `256`, `counter`, `key-monitor`, `preview`, `recover-dd`, `recover-rsync`, `recover.0`
- **Shell functions**, source once in `~/.bashrc` or `~/.zshrc`: `fskill`, `k8s`
- **Env setters**, source on demand in the current shell: `setproxy-*`

```sh
export PATH="$HOME/dev/sys-scripts:$PATH"
source ~/dev/sys-scripts/fskill
source ~/dev/sys-scripts/k8s
```

## Terminal

### `256`

Prints all 256 terminal colors as a grid that fits the terminal width.

Needs: `tput`

```sh
256
```

### `counter`

Fullscreen centered blinking counter, ticks every second and shifts from green to red.

Needs: `tput`

```sh
counter
```

### `key-monitor`

Prints X11 key presses with modifiers, e.g. `Ctrl+Shift+a`.

Needs: `xev`, `gawk`

```sh
key-monitor
```

### `preview`

File previewer: tree for directories, syntax highlight for text, text dump for PDFs, file type otherwise.

Needs: `eza`, `bat`, `pdftotext`, `file`

```sh
fzf --preview 'preview {}'
```

### `fskill`

Shell function, source in rc. Searches skills sorted by installs, pick one with `gum` and `add` or `use` it.

Needs: `skills`, `gum`

```sh
fskill react
```

## Kubernetes

### `k8s`

Shell functions, source in rc. List pods and tail logs per env (`stage`, `prod`). Picking some pods loops back to the picker after closing the viewer; `ov.yaml` makes `q`, `Esc` and `Ctrl+C` all quit it.

Needs: `kubectl`, `gum`, `tspin`, `ov`, `pkill`

```sh
k8pods stage
k8logs prod   # pick all or some running pods, tab to mark, enter for current
```

## Proxy

Source on demand, since they export env vars into the current shell. They also set the global git proxy. Written for zsh.

### `setproxy-http`

HTTP proxy on the default gateway, port `10809`.

Needs: `zsh`, `ip`, `git`

```sh
source setproxy-http
```

### `setproxy-socks`

SOCKS5 proxy on the default gateway, port `10808`.

Needs: `zsh`, `ip`, `git`

```sh
source setproxy-socks
```

### `setproxy-ssh`

HTTP proxy on `192.168.100.10:2080`, also sets the npm proxy.

Needs: `zsh`, `git`, `npm`

```sh
source setproxy-ssh
```

### `setproxy-reset`

Unsets proxy env vars and removes the git and npm proxy config.

Needs: `zsh`, `git`, `npm`

```sh
source setproxy-reset
```

## Recovery

### `recover-dd`

Block-level rescue of a failing disk onto two SSDs joined with device-mapper, using `ddrescue`. Supports retries, bad-sector marking, read-only mount and bad-file scan. Run as root, `-n` for a dry run.

Needs: root, `ddrescue`, `ddrescuelog`, `dmsetup`, `lsblk`, `blockdev`, `partx`, `findmnt`, `udevadm`

```sh
recover-dd first /dev/sdX /dev/sdY /dev/sdZ rescue.map
recover-dd retry /dev/sdX /dev/sdY /dev/sdZ rescue.map 3
recover-dd mark  /dev/sdY /dev/sdZ rescue.map
recover-dd open  /dev/sdY /dev/sdZ rescue.map
recover-dd mount /dev/mapper/hdd-recover1 /mnt/clone
recover-dd scan  /mnt/clone rescue.map
recover-dd close
```

### `recover-rsync`

File-level copy of a read-only ext4 mount, split across two ext4 destinations by free space. Writes manifests, overflow list and a bad-file log. Run as root, `-n` for a dry run.

Needs: root, `rsync`, `findmnt`, `mountpoint`

```sh
recover-rsync /mnt/src /mnt/disk1 /mnt/disk2 recover.log
```

### `recover.0`

First draft of `recover-rsync`: splits files in half by size across two disks, with hardcoded paths.

Needs: `rsync`

```sh
recover.0
```

---

## License

[MIT](LICENSE)
