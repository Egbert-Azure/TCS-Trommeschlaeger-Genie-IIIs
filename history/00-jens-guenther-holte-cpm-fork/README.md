# Jens Guenther's `holte-cpm` — an independent restoration

Upstream: <https://gitlab.com/jengun/holte-cpm> (vendored into `upstream/`
as a full snapshot, `.git` history excluded; single commit at time of
pulling: "Change RAM disk size to 726 KB"). License: CC BY-NC-SA 4.0,
matching this repo's own non-commercial/historical purpose.

One cleanup was applied on top of the vendored snapshot: every text source
file there originally ended in a CP/M-style trailing Ctrl-Z (0x1A) EOF
marker — pure sector-padding noise with no informational content, but
visibly "a weird character at the end of the file" in an editor. That's
stripped throughout `upstream/`. Everything else — CRLF line endings, DIN
66003 encoding, actual content — is untouched, same as the rest of
`history/`.

Jens Guenther is the author of the [sdltrs](https://gitlab.com/jengun/sdltrs)
TRS-80 emulator that `sdltrsOMTI` (see `../../` root readme) is itself
forked from. Independently of that, he worked through Thomas Holte's
original CP/M release and restored it in this repository. This isn't part
of the hard-disk lineage documented in `01`–`05` — it's a separate,
independent line back to Holte's own release, predating any of the
hard-disk customization, and it's the closest thing in this whole
collection to "the origin." The connection back to this project is
personal: Volker Dose and Egbert Schröer were both members of Club 80, a
computer club (long since gone) that Thomas Holte's CP/M work circulated
through in the 1990s — the same community whose personal customizations
(`Version für Egbert Schröer`, etc.) fill out `02`–`04`.

## What cross-checking it against this repo turned up

Comparing file-by-file against `src/` (ignoring whitespace/line-ending
differences — Jens's copy, like the other `history/` snapshots, keeps its
original CRLF line endings):

**Near-identical** (single trailing-blank-line differences only):
`bnkbios.mac`, `chario.mac`, `font12.mac`, `modebaud.mac`, `scb.mac`,
`necd5124.h`, `sq306.h`, `st412.h`, `initwe.mac`, `initwg.mac`. Good
confirmation that both collections trace back to the same release for
these modules.

**A real bug found and fixed.** `src/driver.mac` had four spots with a
stray Unicode replacement character (`�`, U+FFFD) — leftover damage from
some past lossy encoding conversion. Three were in comments, but one had
eaten actual code: line 1655 read `L   A,(IX+4		;botto� lin� --� accu` —
missing the `D` of `LD`, the closing `)`, and letters out of "bottom line
--> accu". That line would not have assembled as written. Cross-referencing
Jens's clean copy (`upstream/driver.mac`) fixed all four spots; `src/driver.mac`
now reads correctly. This is the one place this cross-check changed a
"living" file rather than just adding history.

**`necd5126.h`**: `src/`'s copy has a leftover comment bug — "characteristics
of NEC D5124 Winchester drive" inside the file for the D5126 (fixed to
D5126 as part of this pass). Jens's copy already had it right.

**`initw.c`: a compiler migration, dated.** Jens's copy (`upstream/utils/initw.c`,
26-Nov-1985) uses BDS-C's own `movmem()`/`setmem()` runtime functions.
`src/initw.c` (15-Sep-1986 — ten months later) uses standard `memcpy()`/
`memset()` instead. Thomas Holte evidently rewrote this file for a different
C compiler sometime in 1986 — consistent with the top-level readme's mention
of Hi-Tech C support. `tm252.h` in `../01-format-only-winchester-support/`
was cross-confirmed byte-for-byte against `upstream/utils/tm252.h`.

**`boot.mac`: an undocumented multi-drive configuration.** `src/boot.mac`'s
boot banner just reads `'GENIE IIIs SYSTEM'`. Jens's reads
`'GENIE IIIs SYSTEM (1 MB + 3 * NEC D5126 21.6 MB Winchester/8K)'` — a 1 MB
memory expansion plus **three** NEC D5126 (21.6 MB) Winchester drives. This
doesn't line up with any lineage in `01`–`04`: `upstream/diskio.mac` and
`upstream/drvtbl.mac` still only have floppy-routine placeholders, not a
real multi-drive hard-disk driver, so this looks like a banner string
describing the target machine's paper configuration rather than a wired
driver. No source for the actual 3×NEC-D5126 driver has turned up anywhere
in this collection yet.

**`booter.mac` confirms what `src-omti/hd2.mac`'s changelog only implies.**
Jens's `upstream/booter.mac` has *no* `"geändert von Volker Dose für Egbert
Schröer"` block at all — confirming that customization (already fixed up
for formatting earlier in this repo's history) is personal/local, not part
of Holte's release.

**`ldrbios.mac` explains *why* that customization exists.** Jens's copy
retains an entire anti-piracy mechanism absent from `src/ldrbios.mac`: a
checksum over an embedded copyright message plus a serial-number check
(`SERCHK`/`ADDS`/a "violate" message shown on mismatch). `src/booter.mac`'s
NOP'd-out block, with its comment "Die Übertragung der Seriennummer vom
Bildschirm in den Speicher zur Copyright-Protection wird gelöscht" (the
transfer of the serial number for copyright protection is deleted), is
Volker Dose disabling exactly this mechanism for Egbert Schröer's personal
copy. Two independent repos, each preserving one half of the same removal —
and mining the disk collection directly turned up a second, more mundane
reason for the same change: see `../03-omti-schroeer-branch-1992/README.md`,
which found a sibling file (`HDBOOTER.MAC` on `egcpm11.dmk`) giving a Z180
opcode-crash bug as the explanation instead of copy-protection removal.
Probably both are true at once.

**`diskio.mac`/`drvtbl.mac`: this repo's `src/` already isn't pristine
Holte.** Jens's copy (dated 27-Jul-1985) has a full seven-drive floppy-only
layout: `FD0`/`FD1`/`FD2`/`FD3` plus `DS0`/`DS1`/`DS2` (a different, larger
format family — `DS$WRITE`/`DS$READ`, unrelated to `src-omti/`'s later
same-named hard-disk partitions), no hard-disk placeholders at all.
`src/diskio.mac` (dated 23-Jul-1985 — four days *earlier*) already collapses
that down to two floppy drives (`MF0`/`MF1`) plus three `HD0`/`HD1`/`HD2`
slots wired to the floppy routines as placeholders. Whichever direction the
edit actually went, it shows the base system in this repo's `src/` had
already been reshaped in the field before any real hard-disk driver
(`HD2.MAC`) existed.

**`driver.mac`: tuning and format-support differences.** Key-debounce/
autorepeat timing differs (`src/`: 500/5000/35/3 vs Jens's: 250/2500/25/2 —
this repo's copy is tuned slower), several CTRL-key mappings differ
(space, arrow keys, function keys), and Jens's Drive Control Table still
carries explicit comments naming two alien floppy formats: `"drive 2: Fritz
(8", High Density, 512 bytes)"` and `"drive 3: Montezuma Micro CP/M 80T
DATA DS/DD"`. These aren't the same symbols as the `IBMPC`/`KDS`/`RAIR`/
`ALPHAP3` fossil comment in `../05-drive-p-alien-formats.md`, but they're
concrete, independent confirmation that this BIOS family really did read
more than one alien CP/M floppy format at various points — just not
necessarily the same ones, or from the same moment in time.

**`move.mac`: extra bank-switching.** Jens's copy has additional code
handling a second system port (`$SYS2`) for a 256 KB banking scheme that
`src/move.mac` doesn't have. Not yet reconciled with the "1MB-Umbau"
(1 MB memory expansion) mentioned in `src-omti/diskio1.mac`'s own history —
possibly an earlier or alternate banking scheme.

## What's here that isn't anywhere else in this collection

`upstream/utils/` has nine utilities with no counterpart in this repo or
any other `history/` folder, each following the `X.c` (source) /
`Xe.mac`/`Xg.mac` (English/German-assembled) naming convention already
familiar from `initw`/`initwe`/`initwg`: `backup`, `chargen` (a character/
font generator — companion to `font12.mac`), `checktrk`, `config`, `copy`,
`fkey` (function-key definitions), `format`, `formtrk`, and `m6845` (direct
programming of the Motorola 6845 CRT controller — the same chip
`src/driver.mac`'s `$VDINIT` initializes). `upstream/copysys.asm` is a
full `COPYSYS` utility. `upstream/doc/cpm3.asc` and `cpm3.ws` look like a
user manual (ASCII/WordStar); not yet reviewed in depth.

`upstream/bnkbdos3.spr`, `resbdos3.spr`, `ccp.com`, and `cpmldr.rel` are
prebuilt binary artifacts (the actual assembled/linked BDOS and CCP) —
useful as a known-good reference build, not reviewed byte-level here.
