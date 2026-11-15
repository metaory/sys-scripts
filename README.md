# sys-scripts

<div align="center">
  <img src=".github/banner.png" />
  <br>
  <hr>
  One-off system scripts for random use cases
  <br>
  Not a package, not a toolkit, just the bin
</div>

## Terminal

### `256`

Prints all 256 terminal colors as a grid that fits the terminal width.

```sh
256
```

### `counter`

Fullscreen centered blinking counter, ticks every second and shifts from green to red.

```sh
counter
```

### `key-monitor`

Prints X11 key presses with modifiers, e.g. `Ctrl+Shift+a`. Needs `xev`, `gawk`.

```sh
key-monitor
```

### `preview`

File previewer: tree for directories, `bat` for text, `pdftotext` for PDFs, `file` otherwise.

```sh
fzf --preview 'preview {}'
```

### `fskill`

Sourced helper to search skills, sorted by installs, pick one with `gum` and `add` or `use` it. Needs `skills`, `gum`.

```sh
source fskill
fskill react
```

## Kubernetes

### `k8s`

Sourced helpers to list pods and tail logs per env (`stage`, `prod`). Needs `kubectl`, `gum`, `tspin`, `ov`.

```sh
source k8s
k8pods stage
k8logs prod   # pick all or some running pods
```

## Proxy

Sourced, since they export env vars. They also set the global git proxy.

### `setproxy-http`

HTTP proxy on the default gateway, port `10809`.

```sh
source setproxy-http
```

### `setproxy-socks`

SOCKS5 proxy on the default gateway, port `10808`.

```sh
source setproxy-socks
```

### `setproxy-ssh`

HTTP proxy on `192.168.100.10:2080`, also sets the npm proxy.

```sh
source setproxy-ssh
```

### `setproxy-reset`

Unsets proxy env vars and removes the git and npm proxy config.

```sh
source setproxy-reset
```

## Recovery

### `recover-dd`

Block-level rescue of a failing disk onto two SSDs joined with device-mapper, using `ddrescue`. Supports retries, bad-sector marking, read-only mount and bad-file scan. Run as root, `-n` for a dry run.

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

```sh
recover-rsync /mnt/src /mnt/disk1 /mnt/disk2 recover.log
```

### `recover.0`

First draft of `recover-rsync`: splits files in half by size across two disks, with hardcoded paths.

```sh
recover.0
```
