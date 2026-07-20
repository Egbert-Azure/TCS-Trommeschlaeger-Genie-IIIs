# Drive P: the alien-format drive

The mechanism works, plainly, in every
recovered snapshot. What varies from one source snapshot to the next is
simply *which* format it's configured to read in diskio.mac: different versions carry
different `XLTF`/`DPBF` wiring, some read one alien format, some read none
beyond the default. The `IBMPC`/`KDS`/`RAIR`/`ALPHAP3` line further down is
one specific configuration this collection hasn't recovered a matching
source for yet — an open cataloguing gap, not evidence that the feature
itself disappeared.

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
native format, and it works correctly wherever it's configured. In both
`src/` and `src-omti/`, `XLTF` happens to be a single fixed straight-through
table (no reordering) — P: in *these particular* snapshots is set up for
one specific format, not several, but that's a property of these snapshots'
configuration, not a limitation of the mechanism itself.

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
reconfigured a BIOS that could mount several alien floppy formats at once
down to the single `DRIVEP` slot in this particular lineage — the `GLOBAL`
line was edited to match the code that resulted, but the old comment line
above it was left in place instead of deleted, so today it's a pointer
toward a differently-configured version rather than proof that one no
longer exists anywhere. If a disk carrying that earlier, multi-format
configuration turns up in the wider collection (`GenieIIIs/DMK/`, `TRS80
Disks/`), it would be worth extracting into a `06-` folder here.

## Two confirmed, concrete data points (neither is a full match)

Mining `GenieIIIs/DMK/Egbert/` directly (per its own `disks.md` catalog)
turned up two real things related to the "alien format" idea, though
neither hands over the missing `IBMPC`/`KDS`/`RAIR`/`ALPHAP3` drivers
themselves:

- **`egcpm38.dmk`**: `disks.md` describes it as "Assembler (in USER 1);
  format is Kaempf CP/M 3. To read with Holte CP/M use `format.com` tool
  and virtual drive P:" — a genuine, named alien CP/M format (a variant
  from "Kaempf") that this community really did use drive P: to read.
  `cpmextract` can't parse it (it assumes the Holte "Double Density" DPB,
  which this disk doesn't use), so its directory/DPB weren't recovered
  here — but it's concrete confirmation that drive P:'s aliasing trick was
  operationally used for at least one specific, named format beyond
  whatever `XLTF`/`DPBF` are currently set to.

- **`PC2CPM.PAS`** (found on `egcpm09.dmk`, part of the wider mining pass
  — see `../03-omti-schroeer-branch-1992/README.md`): "Konvertiert
  Textfiles mit IBM-8bit Zeichensatz in 7bit ASCII wie bei CP/M. Aus Club
  80 Info 34" (converts text files with the IBM 8-bit character set into
  7-bit ASCII as used by CP/M; from Club 80 Info newsletter issue 34). This
  is real, dated evidence that the Club 80 community did IBM-PC
  interoperability work — but it's a text-encoding converter (a `umcode`
  translation table applied byte-by-byte to a file already read some other
  way), not a disk-format driver. It doesn't explain how an `IBMPC` floppy
  would have been *read* by drive P:, only what happened to the text
  afterward. Worth keeping in mind if the actual `IBMPC` drive code ever
  turns up: this utility may have been what it fed into.
