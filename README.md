```
██████╗ ██╗   ██╗███████╗████████╗██╗   ██╗██████╗ 
██╔══██╗██║   ██║██╔════╝╚══██╔══╝██║   ██║██╔══██╗
██████╔╝██║   ██║███████╗   ██║   ██║   ██║██████╔╝
██╔══██╗██║   ██║╚════██║   ██║   ██║   ██║██╔═══╝ 
██║  ██║╚██████╔╝███████║   ██║   ╚██████╔╝██║     
╚═╝  ╚═╝ ╚═════╝ ╚══════╝   ╚═╝    ╚═════╝ ╚═╝     
                                                   
```

[![arb](https://img.shields.io/badge/arb-package-05d9e8?style=flat-square)](https://github.com/MenkeTechnologies/arb)
![type](https://img.shields.io/badge/self--sourcing-dashboard-9b5de5?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-ff2a6d?style=flat-square)

### `[EVERY INSTALLED RUST TOOLCHAIN, LIVE FROM RUSTUP]`

> *"The toolchains rustup knows about, listed and re-read on a timer — never a stale `rustup toolchain list` again."*

A `rustup` dashboard that lists the Rust toolchains installed on the machine.
It is **self-sourcing** — it runs `rustup toolchain list` on its own timer, so
nothing needs to be piped in; open it and it starts polling. It is an `arb`
package — a dashboard for [`arb`](https://github.com/MenkeTechnologies/arb),
the pipeline-to-TUI language on the fusevm bytecode VM.

### [`arb`](https://github.com/MenkeTechnologies/arb) &middot; [`arb-registry`](https://github.com/MenkeTechnologies/arb-registry)

---

## Table of Contents

- [\[0x00\] Overview](#0x00-overview)
- [\[0x01\] Install](#0x01-install)
- [\[0x02\] Usage](#0x02-usage)
- [\[0x03\] The Spec](#0x03-the-spec)
- [\[0x04\] How It Works](#0x04-how-it-works)
- [\[0xFF\] License](#0xff-license)

---

## [0x00] OVERVIEW

`rustup toolchain list` prints a flat block of text you have to re-run to see
what changed. This dashboard turns that into a live view: every installed
toolchain in a list, refreshing on a fixed interval. It answers "which Rust
toolchains does this box have" without a manual command and without piping
anything in.

---

## [0x01] INSTALL

```sh
arb install rustup      # from the arb-registry git index
arb search rustup
```

---

## [0x02] USAGE

```sh
arb rustup                         # run it (self-sourcing; polls on its own timer)
arb rustup --serve                 # browser dashboard
arb rustup --html > rustup.html    # static HTML snapshot
```

---

## [0x03] THE SPEC

```arb
# rustup — Installed Rust toolchains, self-sourced from `rustup toolchain list`.
! rustup toolchain list every 10s
list .rustup
source .rustup { in }
grid .rustup -row 0 -col 0
```

- `! rustup toolchain list every 10s` — the self-source. arb runs
  `rustup toolchain list` every 10 seconds and feeds its stdout into the stream
  the widget reads from; no external pipe is required.
- `list .rustup` — a list widget named `.rustup`, one installed toolchain per
  row.
- `source .rustup { in }` — the widget's query pipeline is `in`, passing the
  sourced lines straight through so each toolchain line becomes a row.
- `grid .rustup -row 0 -col 0` — grid placement: the list sits at row 0,
  column 0.

---

## [0x04] HOW IT WORKS

The `!` self-source runs `rustup toolchain list` on the 10-second timer and
feeds its stdout into arb's stream. The widget's `source` block is a query
pipeline over that stream: `.rustup` passes the lines through with `in`, so each
toolchain becomes a row in the list. arb lowers the compute to `fusevm`
bytecode executed on the fusevm VM with a Cranelift JIT, then renders the widget
into the grid.

---

## [0xFF] LICENSE

MIT.
