# A history of hard-disk support on the Genie IIIs

This repo's `src/` is Thomas Holte's base CP/M 3.0 system (floppy + RAM disk
only) and `src-omti/` is one specific, later hard-disk lineage grafted onto
it. Between those two lie several other branches and experiments, scattered
across disk images (`.dmk`) and source trees in sibling repositories on
disk. This folder pulls the hard-disk-relevant ones together into one place,
each as a real, buildable snapshot rather than just a description.

## Timeline

| When | Who | What changed | Where |
|---|---|---|---|
| 1985–1986 | Thomas Holte | Original release, independently restored by Jens Guenther from <https://gitlab.com/jengun/holte-cpm> — the closest thing here to "the origin," predating any hard-disk work | [`00-jens-guenther-holte-cpm-fork/`](00-jens-guenther-holte-cpm-fork/) |
| 1985 | Thomas Holte | Base system as it survives in this repo; `initw.c` formatter supports six Winchester models, but no runtime driver ever gets wired to them | `src/`, [`01-format-only-winchester-support/`](01-format-only-winchester-support/) |
| 19-Oct-1987 | Peter Petersen | `HD2.MAC` v3.2 created — the first real runtime hard-disk driver | [`02-omti-mainline-1987-1993/`](02-omti-mainline-1987-1993/) (earliest state known only from the changelog header; no standalone 1987 copy survives) |
| 08-Jul-1990 | H. Bernhardt | Revises `HD2.MAC` ("weiter dran geschnitzt") | same |
| 1989 | Fritz Chwolka | Merges divergent `DISKIO.MAC`/`TABLES.MAC`/`DRIVER.MAC` versions; strips hard-disk wiring to make room for format-conversion pseudo-drives — see [`05-drive-p-alien-formats.md`](05-drive-p-alien-formats.md) | `src-omti/diskio1.mac`'s own header |
| Jan 1992 | Volker Dose | Branches: adapts `HD2.MAC`/`DISKIO1.MAC` for a 20.4 MB Seagate ST225, builds a complete disk **for Egbert Schröer** with the full format toolchain included | [`03-omti-schroeer-branch-1992/`](03-omti-schroeer-branch-1992/) |
| Nov 1992 | Volker Dose | Separately, adapts for a 10 MB drive ("pderr geändert für Genie IIIs") — the start of the lineage that continues into this repo | [`02-omti-mainline-1987-1993/`](02-omti-mainline-1987-1993/) |
| 30-Jul-1993 | Volker Dose | Mainline: adapts to a 20 MB Tandon TM262 | same |
| 21-Dec-1993 | Egbert Schröer | Mainline: adapts to a Seagate ST225 (MFM/RLL) — **this exact state is what `src-omti/` in this repo preserves today** | same; compare with `src-omti/hd2.mac` |
| undated | (unknown) | A wider experiment: Seagate ST251, 4 partitions, different naming convention (`hd1ini`, `hdsk0..hdsk3`) — doesn't match either surviving branch | [`04-omti-st251-4partition-experiment/`](04-omti-st251-4partition-experiment/) |
| 2024–2026 | Egbert Schröer (+ tooling) | Modern preservation: `cpmextract` (DMK reader, validated against `egcpm01.dmk`), `sdltrsOMTI` (OMTI 5527 controller emulation, reverse-engineered from `hd2.mac`/`ldrbiohd.mac`), and this `history/` consolidation | `cpmextract`, `sdltrsOMTI` repos |

## An independent fork, cross-checked

[`00-jens-guenther-holte-cpm-fork/`](00-jens-guenther-holte-cpm-fork/) isn't
part of the hard-disk lineage below — it's Jens Guenther's own restoration
of Thomas Holte's original CP/M release, worked through independently over
years of contact from the Club 80 computer-club days. Cross-checking it
file-by-file against `src/` turned up a fixed bug (real byte corruption in
`src/driver.mac`, one instance of which broke actual Z80 code), confirmed
that this repo's `src/booter.mac` carries a personal copy-protection-removal
customization absent from Holte's release, and turned up an undocumented
"3× NEC D5126, 1MB" configuration string with no matching driver anywhere
in this collection. Full write-up in that folder's own `README.md`.

## Two branches, not one line

The interesting wrinkle: **Volker Dose built two different disks for the
same drive** (both Seagate ST225 via OMTI) along two different paths. The
mainline went 10 MB → Tandon 20 MB → Seagate ST225, arriving in Dec 1993 at
what's now `src-omti/`. The other — built in January 1992 specifically
**"Version für Egbert Schröer"** — went straight from Bernhardt's 1990
revision to a 20.4 MB Seagate ST225, and is the only surviving copy that
still carries the actual formatting toolchain (`HDDTBL.ASM`, `HDNDF.Z80`)
alongside the driver. Both are preserved here rather than picking one as
"more canonical," since they demonstrate different things: the mainline
shows the incremental adaptation this repo continues, the branch shows the
complete original build-and-format workflow.

## What's preserved as-is

Every file under `00-`, `02-`, `03-`, and `04-` is copied byte-for-byte from
its source (see each folder's own `README.md` for exactly where). Unlike
`src-omti/`, these are **not** cleaned up — original CRLF line endings and
DIN 66003 "German ASCII" placeholder characters (`{`/`|`/`}`/`~` for
`ä`/`ö`/`ü`/`ß`) are left exactly as found. The point of this folder is
historical fidelity; `src/` and `src-omti/` are where active maintenance and
readability fixes belong. The one exception: cross-checking `00-` against
`src/driver.mac` found real encoding corruption (not a historical variant)
in the living copy, which was fixed there directly — see
`00-jens-guenther-holte-cpm-fork/README.md`.

## What's not here yet

The wider disk collection this was pulled from
(`GenieIIIs/DMK/`, `TRS80 Disks/diskimages/`, hundreds of `.dmk` images
including many `esnd-*.dmk` NewDOS-80 disks and `egcpm*.dmk`/`g3s_f*.dmk`
CP/M disks) hasn't been exhaustively mined — only the snapshots already
hand-extracted into `sdltrsOMTI/src ST 225/` and `GenieIIIs/Holte CPM src/`
were used here. In particular:

- No standalone 1987 (Petersen) or 1990 (Bernhardt) `HD2.MAC` has been
  located yet — only known via the cumulative changelog header every later
  copy carries forward.
- The pre-Chwolka BIOS that could apparently mount `IBMPC`/`KDS`/`RAIR`/
  `ALPHAP3` floppy formats through drive P: (see
  [`05-drive-p-alien-formats.md`](05-drive-p-alien-formats.md)) hasn't been
  found on any disk checked so far.
- The Seagate-ST251/4-partition experiment in `04-` is undated and unplaced
  in the timeline above — it doesn't match either surviving branch's
  `DISKIO1.MAC`, so its `HD2.MAC` counterpart (if it survives at all) is
  still unlocated.

`cpmextract` (currently tuned to the "Double Density Holte" floppy DPB) is
the tool that would extend this: teaching it the hard-disk DPBs and a
cataloguing pass over the full `.dmk` collection is the natural next step
for finding any of the above.
