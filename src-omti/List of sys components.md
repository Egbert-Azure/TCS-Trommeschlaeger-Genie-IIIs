# OMTI 5527 hard-disk BIOS additions

These six modules add Seagate ST225 / OMTI 5527 SASI-MFM hard-disk support
(Peter Petersen, H. Bernhardt, Volker Dose, Egbert Schröer) on top of the
base system in `src/` (see `src/List of sys components.md`): a runtime
driver pair (`hd2.mac`/`diskio1.mac`+`drvtbl1.mac`, linked into `BNKBIOS3`
alongside the base modules) plus a separate, minimal boot-time chain
(`hdbooter.mac`+`ldrbiohd.mac`, linked into `CPMLDR`) that lets the system
boot directly from the hard disk via a dedicated boot EPROM (see
`rom/g3s_hd-omti_bootrom_2764.bin`, written by Arnolf Sopp) with no floppy
inserted. `hdboot.mac` is the runtime warm-boot (CCP-reload) counterpart,
used once CP/M 3 is already running.

These are kept in a separate `src-omti/` folder rather than mixed into
`src/` so the original Holte sources stay untouched and these additions
stay clearly attributed to their own authors/dates.

## src-omti/hd2.mac

``` as
H D  2 . M A C   Version : 3.2
Created : 19-oct-87
by      : Peter Petersen
Rev     : 22-oct-87..17-aug-89
weiter dran geschnitzt:
   ...08.07.90  H. Bernhardt
   November 92  V.Dose
        pderr geaendert fuer Genie IIIs
        angepasst an Harddisk 10 MByte
   30.7.93      V.DOSE
        angepasst an 20 MByte Platte
        TANDON TM 262
   21.12.93     E.SCHROEER
        angepasst an 20 MByte Platte
        SEAGATE ST 225 Typ MFM/RLL
```

Runtime OMTI 5527 SASI command-descriptor-block driver: read/write/seek and
the SET DRIVE CHARACTERISTICS/SET SECTOR BUFFER/FORMAT commands, ports
0x40-0x43. Linked into `BNKBIOS3` alongside `driver.mac`.

## src-omti/diskio1.mac

``` as
D I S K I O  *  C P M S Y S 4 f  *  T h o m a s   H o l t e * 8 5 0 9 2 5
DISK HANDLER
Thomas Holte  Version 1.0
```

OMTI-aware variant of `diskio.mac`: adds the extended disk-parameter
headers/blocks (`DPBHD1`/`DPBHD2`) and drive-dispatch entries (`DS0`/`DS1`)
for the hard disk's two logical partitions (C: and D:) on top of the base
floppy/RAM-disk drive table.

## src-omti/drvtbl1.mac

``` as
D R V T B L  *  C P M S Y S 4 e  *  T h o m a s   H o l t e * 8 5 0 9 2 5
DRIVE TABLE
Thomas Holte  Version 1.0
```

OMTI-aware variant of `drvtbl.mac`: adds `DS0`/`DS1` (the hard disk's C:/D:
partitions) to the `@DTBL` drive-dispatch table.

## src-omti/hdbooter.mac

``` as
H D B O O T E R * C P M S Y S 2  *  T h o m a s   H o l t e  * 8 5 1 1 2 0
V O L K E R  D O S E  *  9 3 1 2 1 0
BOOTSTRAP LOADER FOR CP/M 3 ON THE GENIE IIIs MICROCOMPUTER SYSTEM
Thomas Holte  Version 1.1
geaendert von Volker Dose, um von einer Harddisk zu booten. Nov. 1992
jetzt mit Initialisierung des Z180-Prozessors, Wait-Zyklen und
Refreshzeiten, die optimiert und an den Z180 ausgegeben werden muessen.
Jan. 1993
```

Cold-boot loader for the direct-hard-disk-boot path: the boot EPROM loads
this at a fixed address, it relocates itself and reads `CPMLDR` off the
hard disk's system tracks. Linked into `CPMLDR` alongside `ldrbiohd.mac`.

## src-omti/ldrbiohd.mac

``` as
L D R B I O S  *  C P M S Y S 3  *  T h o m a s   H o l t e * 8 5 0 9 0 8
MINIMUM BIOS FOR CPMLDR
Thomas Holte  Version 1.0
Jetzt angepasst an Tandon TM 262 HD mit 20 MB, 3.Aug 1993
Nun an Seagate ST 225 mit 2 Partitionen. Die Charakteristik von Partition 1
wird in LDRBIOHD zum Bootvorgang von der Festplatte benoetigt.
```

Minimal, hand-rolled BIOS (`SELDSK`/`SETTRK`/`SETSEC`/`READ`) that
`CPMLDR.COM` calls into to load `CPM3.SYS` off the hard disk during boot,
before full BDOS exists yet. Its own boot-time disk-parameter block
(`DPB0`) must stay field-for-field consistent with `diskio1.mac`'s runtime
`DPBHD1` for C: to be readable at both boot time and afterward.

## src-omti/hdboot.mac

``` as
H D B O O T  * C P M S Y S 4 b *  T h o m a s   H o l t e  *  8 5 1 0 2 3
BOOT LOADER MODULE FOR CP/M 3.0
Thomas Holte  Version 1.0
Version um von Festplatten Laufwerk C: zu booten.  V.Dose  15.9.93
```

Runtime warm-boot module: reloads `CCP.COM` from C: after CP/M 3 is
already running (distinct from `hdbooter.mac`, the cold-boot loader).
