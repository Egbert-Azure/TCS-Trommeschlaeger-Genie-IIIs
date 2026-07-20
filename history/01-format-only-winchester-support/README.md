# Format-only Winchester support (1985)

Thomas Holte's original base system (`src/`) never had a working runtime
hard-disk *driver* — `HD0`/`HD1`/`HD2` in `src/diskio.mac` are wired to the
floppy routines (`FD$WRITE`/`FD$READ`), not real hard-disk code. What it did
have was `src/initw.c`, a low-level disk-*formatter* utility with one
machine-dependent header per supported Winchester drive model, selected at
compile time. Five of those headers are already in `src/`:

| Header | Drive | Capacity |
|---|---|---|
| `src/necd5124.h` | NEC D5124 | 10.9 MB — **the original stock Genie IIIs hard disk** |
| `src/necd5126.h` | NEC D5126 | 21.6 MB |
| `src/sq306.h` | SyQuest 306 | 5.4 MB |
| `src/st412.h` | Seagate ST412 | 10.8 MB |
| `basf6188.h` (not preserved as a file; quoted in `GenieIIIs/Holte CPM src/Hard drive definitions C P M S Y S 5 a.md`) | BASF 6188 | 12.6 MB |

`tm252.h` in this folder is the sixth: the Tandon TM252 (10.4 MB) config. It
was reconstructed from a markdown transcript in `GenieIIIs/Holte CPM src/`
(no standalone `.h` file survived in this repo's own collection) and later
confirmed byte-for-byte against the authentic file preserved in Jens
Guenther's independent restoration — see
`../00-jens-guenther-holte-cpm-fork/upstream/utils/tm252.h`. It's included
here for completeness of the format-only lineage; it was never wired into a
build the way the others were.

None of these six ever got a matching runtime BIOS driver in the base
system. That had to wait for Peter Petersen's `HD2.MAC` — see
`../02-omti-mainline-1987-1993/`.
