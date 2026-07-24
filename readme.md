# TCS Trommeschläger Genie IIIs: Holte CP/M 3.0

This repository preserves and documents Thomas Holte's CP/M 3.0 system for
the **TCS Trommeschläger Genie IIIs** — not to be confused
with the plain **Genie III**, an earlier, unrelated machine manufactured by
EACA (Hong Kong) that TCS merely distributed in Germany. Genie III (and
EACA's COLOUR-GENIE) shipped with construction defects costly enough to fix
that TCS designed its own improved successor in response, starting June
1983: the **Genie IIIs** (note the trailing "s"), built in Germany with
Siemens producing the electronics and TCS doing final assembly. It's this
TCS-built Genie IIIs, Z80-based and TRS-80-compatible, that this repo is
about. The base machine was floppy-only — a Tandon 10 MB hard disk (via a
Xebec SASI controller) was an optional add-on, not standard equipment, and
so was a real-time clock (see `history/01-format-only-winchester-support/`
for the correction, and `history/07-this-machine.md` for the specific
machine this whole collection traces back to). It sold only about 200
units, ever, and TCS itself went bankrupt in August 1985 — under two years
after launch — as the market moved to the IBM PC. One of the last big CP/M
machines, technically ahead of its time and too late anyway; see
[`history/06-company-history.md`](history/06-company-history.md) for the
full, sourced company history. That history is the whole scope of this
repo: this is the Genie IIIs running Holte CP/M, nothing more. It's a
source-code archive, not an emulator — see "Running it" below for that.

The hardware always had a hard disk; Holte's *software* support for it is
a separate, longer story. A community of contributors (Peter Petersen, H.
Bernhardt, Fritz Chwolka, Volker Dose, Egbert Schröer) built and rebuilt
the CP/M hard-disk driver across 1987–1993 through Club 80, a computer club
(long since gone) that this work circulated through — almost certainly
long after TCS itself had stopped existing. `history/` documents that
whole software evolution — and, going further back, an independent
restoration of Holte's original release by Jens Guenther — as real,
buildable snapshots rather than just descriptions.

## Running it

This repo is source code and disk images, not a runnable program by
itself. To actually boot the Genie IIIs, you need an emulator:

- **[sdltrs](https://gitlab.com/jengun/sdltrs)**, by Jens Guenther — the
  SDL2-based TRS-80 Model I/III/4/4P emulator this all runs on, and the
  reason any of this works cross-platform at all: it builds and runs on
  Windows, Linux, and macOS from one codebase. It emulates the Genie IIIs'
  Western Digital WD1000/1010 hard-disk controller.
- **[sdltrs-MultiHDC](https://github.com/Egbert-Azure/sdltrs-MultiHDC)** — a fork of
  sdltrs adding emulation of the *other two* hard-disk controllers this system
  supports: the OMTI 5527 SASI/MFM controller and the Xebec S1410 SASI
  controller (see `src-omti/` and `history/`). This matters if you want to run
  any of the Seagate ST225/OMTI- or Xebec-based configurations documented in
  this repo — plain sdltrs can't drive those, only the WD1000/1010 path.
- **[SDLTRS-Wrapper](https://github.com/Egbert-Azure/SDLTRS-Wrapper)**
  ("TRS-80 Launcher") — a native macOS front-end wrapping sdltrs/sdltrs-MultiHDC
  in a proper Cocoa UI (machine presets, floppy and hard-disk slots, ROM/
  config controls), for when the emulator's own menus aren't what you want.
  It's a launcher, not a separate emulator — Windows and Linux users can
  just run sdltrs/sdltrs-MultiHDC directly.

## What's in this repo

- **`src/`** — Thomas Holte's original CP/M 3.0 BIOS, BDOS-linkage, and
  utility sources (floppy + RAM disk only). Start with
  [`src/List of sys components.md`](src/List%20of%20sys%20components.md)
  for what each module does and how `BOOTGEN.SUB`/`CPM3.SUB` build a
  system.
- **`src-omti/`** — the OMTI 5527/Seagate ST225 hard-disk BIOS additions,
  kept separate so the original Holte sources in `src/` stay untouched. See
  [`src-omti/List of sys components.md`](src-omti/List%20of%20sys%20components.md).
  `rom/g3s_hd-omti_bootrom_2764.bin` (written by Arnolf Sopp) is the boot
  EPROM that lets the system boot directly from the hard disk with no
  floppy inserted.
- **`src-hitech-lib/`** — Hi-Tech C library headers and the `HOLTE.LIB`
  graphics-library linker map, for writing your own C programs against
  this system's HRG (high-resolution graphics) hardware. See its own
  [`README.md`](src-hitech-lib/README.md); to actually get a working
  Hi-Tech C toolchain, see
  <https://github.com/Egbert-Azure/Install-HITECH-C-COMPILER>.
- **`history/`** — every hard-disk BIOS lineage and independent restoration
  found across the wider disk collection, as real, buildable snapshots:
  Jens Guenther's independent restoration of Holte's original release,
  Holte's format-only Winchester support, Peter Petersen's 1987 driver and
  its divergent 1992/1993 branches, an undated Seagate ST251 4-partition
  experiment, and drive P:'s working alien-floppy-format mechanism (its
  exact configuration varies by snapshot). Start at
  [`history/README.md`](history/README.md).
- **`rom/`** — boot EPROM images.

There used to be a note here about `UTILS/`/`CPMSYS/`/`LIBPLOT/`/`LIBCZZZ/`/
`CLIBYYY/` subfolders under `src/`. Those never existed as folders in this
repo — `src/` is flat. `LIBCZZZ`, `CLIBYYY`, and `CLIBXXX` were Holte's own
names for three small shared C libraries (now in `src-hitech-lib/`), not
directories; `LIBPLOT` was likely an early name for the `HOLTE.LIB`
graphics library documented there.

## License

This repo's own material (documentation, tooling, organization) is
[MIT-licensed](LICENSE). The vendored Holte CP/M sources themselves carry
their own original terms — see `history/00-jens-guenther-holte-cpm-fork/README.md`
for the license Thomas Holte released them under.

Contributions and corrections are welcome — especially if you can identify
any of the still-open questions in `history/README.md`'s "What's not here
yet" section, or have more disks from this era.
