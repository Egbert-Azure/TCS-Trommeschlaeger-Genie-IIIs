# Hi-Tech C library headers

**Correction:** an earlier version of this page claimed Thomas Holte himself
migrated his utility toolkit from Mi-C to Hi-Tech C in 1986. That's wrong,
per Egbert Schröer directly. **Holte only ever used Mi-C** — a commercial
CP/M C compiler that was still being sold for serious money into the early
'90s. Once Fritz Chwolka got Holte to release the source in 1990 (see
`history/00-jens-guenther-holte-cpm-fork/README.md`), Egbert built it
himself with **Hi-Tech C** instead, specifically because Mi-C wasn't free
and Hi-Tech C was (and, in his view, simply better). The Hi-Tech C work in
this collection — including `src/initw.c`'s `memcpy()`/`memset()` calls,
where `history/00-.../upstream/utils/initw.c` (Jens's copy) still has
Mi-C's own `movmem()`/`setmem()` — is Egbert's own adaptation of Holte's
released source, not something Holte did.

That leaves an open dating question rather than a settled one: `src/initw.c`'s
header still reads "15-Sep-1986," predating the 1990 release entirely. That
date almost certainly reflects Holte's own last edit to the file (still
under Mi-C at the time), carried forward unchanged when Egbert later
adapted the code for Hi-Tech C — this codebase's simple, single-date header
style (unlike `hd2.mac`'s full running changelog) doesn't get updated by
every subsequent editor. `errno.h` in this folder still names Mi-C
explicitly in its own header comment ("common definitions for **Mi-C** file
system error numbers"), which is what confirmed the compiler's actual name
here in the first place.

`HOLTE.LIB` (below) is a separate, later Hi-Tech C artifact from the start:
Volker Dose's and Egbert Schröer's own graphics library, from July 1993 —
not a port of anything Holte wrote.

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
