# Drive P: and the lost alien-format drives

`src/diskio.mac` and `src-omti/diskio1.mac` both wire up a virtual drive,
`P:`, via `P$READ`/`P$WRITE`/`P$TASK` (see `src/diskio.mac:476` /
`src-omti/diskio1.mac:648`). The trick: CP/M's BIOS calls the same physical
read/write entry point regardless of which logical drive is selected, so
drive P: hijacks **drive B:'s** physical floppy driver at the last moment —

1. Save drive B:'s live Drive Control Table entry (`DCT`, at the fixed
   system address `SYSTAB+...`) into a scratch area (`TDCT`).
2. Copy drive P:'s own parameters (`PDCT`) into that same live DCT slot.
3. Jump into the ordinary floppy read/write routine — it reads B:'s slot as
   usual, but now finds P:'s geometry/skew there instead.
4. On return, restore B:'s original DCT entry from `TDCT`.

This is why drive P: can use a completely different disk parameter block
(`DPBF`, "VIRTUELLES DRIVE -P-") and sector-translation table (`XLTF`) from
drive B:'s own, while reusing all of B:'s physical driver code — the classic
CP/M pattern for reading a foreign floppy format on hardware built for one
native format. Today, in both `src/` and `src-omti/`, `XLTF` is a single
fixed straight-through table (no reordering) — P: currently reads one
specific format, not several.

## The fossil comment

`src-omti/diskio1.mac` (and its `src/diskio.mac` counterpart) carry a
commented-out line that the working code around it no longer matches:

```
;Disk drive dispatching tables for linked BIOS
;GLOBAL DS0,MF0,MF1,FD0,IBMPC,KDS,RAM,RAIR,ALPHAP3,DRIVEP

	GLOBAL	MF0,MF1,DS0,DS1,RAM,DRIVEP
```

The commented line lists five drive symbols that don't exist anywhere in
any surviving snapshot's linked BIOS: `FD0`, `IBMPC`, `KDS`, `RAIR`,
`ALPHAP3`. `IBMPC` almost certainly means an MS-DOS/IBM-PC-formatted floppy;
`ALPHAP3` most likely refers to the Alphatronic P3, another CP/M-based
machine of the era with its own disk format. `KDS` and `RAIR` are less
certain — plausibly Kaypro and a RAIR-branded CP/M system respectively, but
that's an inference from the naming pattern, not confirmed from any
recovered source. None of the format-specific code (their DPBs, translation
tables, or the drive-dispatch entries themselves) survives in any snapshot
this pass turned up.

The explanation is in `src-omti/diskio1.mac`'s own header, from Fritz
Chwolka (1989): he merged several divergent versions of `DISKIO.MAC`,
`TABLES.MAC`, and `DRIVER.MAC` and *removed* hard-disk wiring "um
Pseudo-Laufwerke zur Formatkonvertirung einzubinden" (to make room for
pseudo-drives for format conversion). That rework is almost certainly what
collapsed a BIOS that could mount several alien floppy formats at once down
to the single `DRIVEP` slot that survives — the `GLOBAL` line was edited to
match the code that resulted, but the old comment line above it was left in
place instead of deleted, so today it's the only trace that the wider set
ever existed. If a disk carrying the pre-Chwolka, multi-format version
turns up in the wider collection (`GenieIIIs/DMK/`, `TRS80 Disks/`), it
would be worth extracting into a `06-` folder here.
