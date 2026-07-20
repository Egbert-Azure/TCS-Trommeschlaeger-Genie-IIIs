# Format-only Winchester support (1985)

The Genie IIIs shipped from TCS with a Tandon 10 MB hard disk built in —
this machine was never floppy-only. But in the base system as it survives
in `src/`, `HD0`/`HD1`/`HD2` in `src/diskio.mac` are wired to the floppy
routines (`FD$WRITE`/`FD$READ`), not real hard-disk code — no working
runtime hard-disk *driver* survives there. Whether the stock Tandon drive
was actually driven some other way at this stage (boot-EPROM/firmware level,
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
| `tm252.h` (this folder) | Tandon TM252 | 10.4 MB — **the drive TCS actually shipped the Genie IIIs with** |

**Correction:** an earlier version of this page called the NEC D5124 "the
original stock Genie IIIs hard disk." That was wrong. The Genie IIIs shipped
from TCS with a Tandon 10 MB hard disk from the start (a large, heavy
full-height drive) — this was never a floppy-only machine at the hardware
level. `tm252.h`'s 10.4 MB Tandon TM252 config is the one that matches what
actually came in the box; the other five headers are simply the other
Winchester models `initw.c` could format, not evidence of what shipped.

`tm252.h` was reconstructed from a markdown transcript in
`GenieIIIs/Holte CPM src/` (no standalone `.h` file survived in this repo's
own collection) and later confirmed byte-for-byte against the authentic
file preserved in Jens Guenther's independent restoration — see
`../00-jens-guenther-holte-cpm-fork/upstream/utils/tm252.h`.

None of these six has a matching runtime BIOS driver anywhere in this
collection. The earliest *recovered* one is Peter Petersen's `HD2.MAC`
(19-Oct-1987) — see `../02-omti-mainline-1987-1993/` — but given the
Tandon shipped from day one, there was almost certainly something driving
it before 1987 that just hasn't turned up in this collection yet.

**The original controller was a Xebec SASI controller**, confirmed
directly by Egbert Schröer (who owned one) — not the OMTI 5527 that
`src-omti/` and most of this history document. `src/initw.c` already has
Holte's original code for it, gated behind `#ifdef SASI`: "port addresses
of Xebec SASI Controller," with `TEST`/`REST`/`FORMAT`/`INIDRV` commands —
right next to an `#else` branch for a Western Digital WD1000/1010-style
controller. Whatever drove the stock Tandon drive before 1987 almost
certainly used this Xebec path, not the WD one. See
`../06-company-history.md` for how the Tandon-equipped GENIE IIIs fits into
TCS's own short, commercially unsuccessful history.
