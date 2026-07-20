# Format-only Winchester support (1985)

The base Genie IIIs, as TCS sold it, was floppy-only — the hard disk was
an add-on option, not standard equipment (and so were its Xebec SASI
controller and a real-time clock, each purchased/fitted separately). Egbert
Schröer's own machine had the hard disk from early in his ownership, though
not the RTC at first. In the base system as it survives in `src/`,
`HD0`/`HD1`/`HD2` in `src/diskio.mac` are wired to the floppy routines
(`FD$WRITE`/`FD$READ`), not real hard-disk code — no working runtime
hard-disk *driver* survives there. Whether an add-on Tandon drive was
actually driven some other way at this stage (boot-EPROM/firmware level,
outside Holte's CP/M BDOS layer) or whether a real driver existed and simply
hasn't been recovered yet is an open question — see the correction below.
What `src/` does have is `src/initw.c`, a low-level disk-*formatter* utility
with one machine-dependent header per supported Winchester drive model,
selected at compile time. Five of those headers are already in `src/`:

| Header | Drive | Capacity |
|---|---|---|
| `src/necd5124.h` | NEC D5124 | 10.9 MB |
| `src/necd5126.h` | NEC D5126 | 21.6 MB |
| `src/sq306.h` | SyQuest 306 | 5.4 MB |
| `src/st412.h` | Seagate ST412 | 10.8 MB |
| `basf6188.h` (not preserved as a file; quoted in `GenieIIIs/Holte CPM src/Hard drive definitions C P M S Y S 5 a.md`) | BASF 6188 | 12.6 MB |
| `tm252.h` (this folder) | Tandon TM252 | 10.4 MB — **the drive Egbert Schröer's own Genie IIIs was fitted with** |

**Correction:** an earlier version of this page called the NEC D5124 "the
original stock Genie IIIs hard disk," and after that, claimed the hard disk
was standard equipment "from the start." Both were wrong. The hard disk was
a Tandon 10 MB drive (a large, heavy full-height unit) — but it, its Xebec
SASI controller, and a real-time clock were all optional add-ons TCS sold
separately, not part of the base machine. `tm252.h`'s 10.4 MB Tandon TM252
config is the one that matches the drive actually fitted to Egbert
Schröer's own unit (present from early in his ownership; the RTC came
later); the other five headers are simply the other Winchester models
`initw.c` could format, not evidence of what any particular machine had.

`tm252.h` was reconstructed from a markdown transcript in
`GenieIIIs/Holte CPM src/` (no standalone `.h` file survived in this repo's
own collection) and later confirmed byte-for-byte against the authentic
file preserved in Jens Guenther's independent restoration — see
`../00-jens-guenther-holte-cpm-fork/upstream/utils/tm252.h`.

None of these six has a matching runtime BIOS driver anywhere in this
collection. The earliest *recovered* one is Peter Petersen's `HD2.MAC`
(19-Oct-1987) — see `../02-omti-mainline-1987-1993/` — but given hard disks
were already an available add-on well before then, there was almost
certainly something driving them before 1987 that just hasn't turned up in
this collection yet.

**The original controller for this add-on was a Xebec SASI controller**,
confirmed directly by Egbert Schröer (who owned one) — not the OMTI 5527
that `src-omti/` and most of this history document. `src/initw.c` already
has Holte's original code for it, gated behind `#ifdef SASI`: "port
addresses of Xebec SASI Controller," with `TEST`/`REST`/`FORMAT`/`INIDRV`
commands — right next to an `#else` branch for a Western Digital
WD1000/1010-style controller. Whatever drove an add-on Tandon drive before
1987 almost certainly used this Xebec path, not the WD one. See
`../06-company-history.md` for how the Genie IIIs and its hard-disk option
fit into TCS's own short, commercially unsuccessful history, and
`../07-this-machine.md` for the personal story of Egbert Schröer's own
machine.
