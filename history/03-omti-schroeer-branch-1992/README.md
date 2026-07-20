# The "Version für Egbert Schröer" branch (Jan 1992)

A parallel branch, separate from the mainline in `../02-omti-mainline-1987-1993/`.
Volker Dose built this one specifically for Egbert Schröer, dated January
1992, and it took a different shortcut: straight from Bernhardt's 1990
revision to a 20.4 MB Seagate ST225 (4 heads × 615 cylinders × 17
sectors/track) via the OMTI controller — no intermediate 10 MB or Tandon
TM262 stage. `HD2.MAC`'s own changelog header reads:

```
Created : 19-oct-87   by Peter Petersen
Rev     : 22-oct-87..17-aug-89
weiter dran geschnitzt: ...08.07.90  H. Bernhardt
   Januar 92  V.Dose
        pderr geändert für Genie IIIs
        angepasst an Harddisk 20.4 MByte

Version für Egbert Schröer
Seagate ST225
```

Pulled from `sdltrsOMTI/src ST 225/egcpm001` (the fuller of two surviving
copies of this branch — `egcpm001`/`egcpm012` — since it also carries the
BIOS-linking files `DRIVER.MAC`/`FONT12.MAC`/`LDRBIOS.MAC`/`BOOTER.MAC`/the
hard-disk boot chain that `egcpm012` had already dropped).

What makes this branch particularly valuable is that it's the only
surviving copy that still has the actual **formatting toolchain** alongside
the runtime driver:

- **`HDDTBL.ASM`** — computes the disk parameter block (DPB) for
  `DISKIO1.MAC` using Peter Petersen's `XCPM3.LIB`. Its own header spells out
  the (very manual) workflow: assemble with RMAC, link, then hand-copy the
  resulting byte sequence out of the `.COM` file into `DISKIO1.MAC`'s `DPBHD1`
  table. This is the tool `diskio1.mac`'s own comments refer to
  ("Der DPB wurde mit XCPM3.LIB aus HDDTBL.ASM errechnet").
- **`HDNDF.Z80`** — Volker Dose's low-level formatter for the Seagate ST225
  itself (02-Nov-1992, hand-tuned by Egbert Schröer 19-Jan-1993): sends the
  OMTI `SET CHARACTERISTICS`/`WRITE SECTOR BUFFER`/`FORMAT` command sequence
  directly to ports 0x40–0x43 to low-level-format the drive with an `0xE5`
  fill pattern before CP/M's own directory init ever runs. This is the exact
  command sequence later reverse-engineered independently (from the compiled
  driver, not this source) into `sdltrsOMTI/src/trs_omti.c`'s emulation — see
  `sdltrsOMTI/docs/OMTI_CONTROLLER.md`.

Like the mainline snapshot, this is preserved untouched, DIN 66003 artifacts
and all.

## Mined directly from Egbert's own disk collection

The files above came from `sdltrsOMTI`'s prior extraction. Going one level
further back, mining the disk collection directly
(`GenieIIIs/DMK/Egbert/*.dmk`, via `cpmextract`, per `disks.md`'s own
catalog) turned up more of this branch:

- **`XCPM3.LIB`** — Peter Petersen's actual macro library (`dtbl`/`dph`/
  `skew`/`dpb` macro definitions), referenced everywhere but never
  recovered as a file until now. Byte-identical across every disk it
  appears on (`egcpm06`, `egcpm12`, `egcpm13`) — a stable, unchanging part
  of the toolchain. This is what `HDDTBL.ASM` needs (`maclib xcpm3`) to
  actually assemble.

- **`HD2.BAK` / `HD2-corrected-17sec.MAC`** (from `egcpm12.dmk`) — a
  genuine before/after pair from one editing session, and it resolves a
  real bug. `HD2.BAK` is word-for-word the mainline's Nov-1992 "10 MByte"
  `HD2.MAC` (see `../02-omti-mainline-1987-1993/`). `HD2-corrected-17sec.MAC`
  is the same file *after* Volker Dose edited it into this branch: header
  rewritten to "Version für Egbert Schröer / Seagate ST225", the OMTI
  drive-characteristics block updated (2 heads→4, last cylinder 99→67h,
  write-precomp/reduced-current cylinders updated), and — this is the
  bug — `fdhead: cp 17` instead of the old `cp 26` (sectors-per-head used
  to locate the physical head from a flat sector number). 17 matches this
  drive's real 17-sectors/track (`HDDTBL.ASM` and `FXPARK.PAS` both agree);
  26 doesn't correspond to anything about a Seagate ST225.

  **The existing `HD2.MAC` in this folder (from `egcpm001`/`sdltrsOMTI`)
  still has the old, wrong `cp 26`** — despite its header already claiming
  "Version für Egbert Schröer / Seagate ST225". Two other disks
  (`egcpm06.dmk`, `egcpm34.dmk`) carry that same stale value, so this
  wasn't a one-off typo, it round-tripped across several copies before
  `egcpm12.dmk` shows it fixed. Anyone building from the `HD2.MAC` in this
  folder as-is would end up with a driver whose head math doesn't match
  its own DPB; `HD2-corrected-17sec.MAC` is the version to actually use.

- **`FXPARK.PAS`** (from `egcpm11.dmk`) — a Turbo Pascal utility to park
  the drive's heads before power-off (standard practice for MFM/RLL
  drives of this era). Header: "Programm FXPARK.PAS, Parkt die Festplatte
  mit OMTI xxxx Controller, Autor: Volker Dose, Original Programm-Name:
  Park.pas, ein wenig herumgepfuscht hat Egbert Schröer" (a little
  tinkered-with by Egbert Schröer). Builds its own 6-byte OMTI command
  block by hand (`cfield` record: command/address/sector/track/bcount/
  termin) — the same CDB layout documented in `sdltrsOMTI/docs/
  OMTI_CONTROLLER.md`.

- **Two more dated refinements**, from `egcpm11.dmk`'s `HDBOOTER.MAC`/
  `LDRBIOHD.MAC` (which otherwise differ substantially from the copies
  above — not reconciled here, just noted): `LDRBIOHD.MAC` is dated
  "Februar 1992" for "20MB Festplatte mit OMTI-Controller"; `HDBOOTER.MAC`
  is dated "Feb. 1993" and gives a *different* reason for the same
  serial-number-removal this repo's own `src/booter.mac` documents (see
  `../00-jens-guenther-holte-cpm-fork/README.md`): "Die Speicherung der
  Seriennummer entfällt, weil sie Illegals enthält, die auf dem Z180 zum
  Absturz führen" — the serial-number code used undocumented ("illegal")
  Z80 opcodes that crash on the Z180 CPU. Both explanations are probably
  true at once: it was a real Z180-compatibility bug, and removing it also
  happened to disable the copy-protection check.

- A third, still-later date turned up in a corrupted `LDRBIOHD.BAK` on
  `egcpm34.dmk` (mostly unreadable — the file itself appears bit-rotted,
  not just re-encoded — so not copied in here): legible fragments read
  "...DPH für 21,4 MB Harddisk 2 Partition angepaßt 21.02.9[3] E.Schröer",
  suggesting Egbert Schröer made his own further adjustment in Feb 1993,
  between Volker Dose's 1992 build of this branch and the unrelated
  mainline's Dec-1993 arrival at the same drive.

Only the disks `disks.md` flagged as hard-disk/source-relevant
(`egcpm03/04/06/09/11/12/13/14/27/34`, plus a failed attempt at `egcpm38`,
whose Kaempf CP/M 3 format `cpmextract` can't parse) were mined so far —
not the full 38-disk collection. See `../README.md`'s "What's not here yet"
for what a fuller pass would still need to check.
