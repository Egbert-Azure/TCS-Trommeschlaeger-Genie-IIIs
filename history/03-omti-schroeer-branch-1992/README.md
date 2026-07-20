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
