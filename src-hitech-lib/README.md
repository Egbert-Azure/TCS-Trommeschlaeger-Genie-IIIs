# Hi-Tech C library headers

Thomas Holte's C utilities (`src/initw.c` and others) were originally
written against BDS-C — Jens Guenther's independent restoration
(`history/00-jens-guenther-holte-cpm-fork/upstream/utils/initw.c`, dated
26-Nov-1985) still uses BDS-C's own `movmem()`/`setmem()` runtime calls.
This repo's own `src/initw.c` (dated 15-Sep-1986) already uses standard
`memcpy()`/`memset()` instead — evidence of a later migration to Hi-Tech C.
To build against Hi-Tech C on a modern machine, see
<https://github.com/Egbert-Azure/Install-HITECH-C-COMPILER>.

These four header files are Holte's own small, shared C libraries — no
executable code, just typedefs/constants (each file says so in its own
header comment). Each belongs to its own oddly-named library, per its
banner:

| File | Library | Library named in its own header | What it defines |
|---|---|---|---|
| `stdio.h` | `CLIBXXX` | `S T D I O * C L I B X X X` | `BOOL`/`RESULT`/`TRUE`/`FALSE`/`NULL`/`SUCCESS`/`ERROR`/`BDOSERR` |
| `errno.h` | `CLIBYYY` | `E R R N O * C L I B Y Y Y` | Mi-C-style `errno` values (`ENOENT`, `EIO`, `EBADF`, ...) |
| `bios.h` | `LIBCZZZ` | `B I O S * L I B C Z Z Z` | CP/M-80 BIOS jump-table entry-point numbers (`BOOT`=0 .. `USERF`=30), called via BDOS function 50 — this is the `BIOS3.H` referenced in the "Grafik des Genie IIIs" articles below |
| `getargs.h` | `TOOLS005` | `G E T A R G S * T O O L S 0 0 5` | Typedefs for a command-line argument parser (`ARG` struct: switch char, type, variable pointer, error message) |

`bios.h`/`LIBCZZZ` pairs with `BIOS3.OBJ` in `HOLTE-LIB-linker-map.md`
below — the actual C source (`BIOS3.C`) wasn't recovered, only its compiled
object's symbol list and this header.

## `HOLTE-LIB-linker-map.md`

The linker symbol map for `HOLTE.LIB`, Volker Dose's and Egbert Schröer's
Hi-Tech C graphics library for the Genie IIIs' HRG (high-resolution
graphics) hardware — recovered from `GenieIIIs/Holte CPM src/`. It's not
source code, just the `U`(ndefined)/`D`(efined) symbol table per object
module (`OPENPL.OBJ`, `CLOSEPL.OBJ`, `CIRCLE.OBJ`, `ARC.OBJ`, `LINE.OBJ`,
`POINT.OBJ`, `SYSTEM.OBJ`, `ERASE.OBJ`, `GRAFIK.OBJ`, `UNARC.OBJ`,
`UNPOINT.OBJ`, `UNLINE.OBJ`, `MOVE.OBJ`, `UNCIRCLE.OBJ`, `BIOS3.OBJ`,
`WINDOW.OBJ`) — i.e. exactly what needs to be linked, and what each module
depends on. `GenieIIIs/How to article/Grafik des Genie IIIs Teil 3.md`
gives the actual C function prototypes this library provides —
`erase()`/`openpl()`/`closepl()`/`point()`/`line()`/`arc()`/`circle()`/
`move()`/`cont()`/`unpoint()`/`unline()`/`uncircle()`/`unarc()`/`uncont()`
— plus `window()` (mentioned as present but its prototype wasn't listed in
that article), all built on top of `bios.h`'s BDOS-function-50 BIOS call
and `system()` — the same USERF-based low-level pattern `src/driver.mac`'s
graphics-mode `USERF` calls implement on the assembly side. `HOLTE.LIB`
itself (the actual compiled `.LIB`/`.OBJ` files, or `BIOS3.C`'s source) has
not been located in this collection yet — only this linker map, and the
prototypes quoted in the article series, survive here.

The article series that documents all of this — the same graphics
capability in three languages (Turbo Pascal 3.0, ANSI C via Hi-Tech C, and
raw Z80 assembler) — lives in
`GenieIIIs/How to article/Grafik des Genie IIIs{,  Teil 2,  Teil 3}.md`
(Volker Dose & Egbert Schröer, July 1993; not yet copied into this repo).
