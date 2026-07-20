# OMTI mainline (Petersen 1987 → Schröer 1993)

A complete, buildable source snapshot of the lineage that this repo's
`src-omti/` continues today: Peter Petersen's original `HD2.MAC` (19-Oct-1987)
→ H. Bernhardt's revision (08-Jul-1990) → Volker Dose adapting it for the
Genie IIIs and a 10 MB drive (Nov 1992) → Volker Dose re-adapting it for a
20 MB Tandon TM262 (30-Jul-1993) → Egbert Schröer re-adapting it for a
Seagate ST225 MFM/RLL drive via the OMTI controller (21-Dec-1993).

This snapshot (pulled from `sdltrsOMTI/src ST 225/egcpm002`) is byte-for-byte
what's now maintained as this repo's `src-omti/` — compare `HD2.MAC` here
with `src-omti/hd2.mac` and the changelog headers match exactly. See
`src-omti/List of sys components.md` for a per-file breakdown of what each
module does; this folder exists to preserve the *exact disk snapshot* as
found, untouched — including the DIN 66003 "German ASCII" placeholder
characters (`{`/`|`/`}`/`~` standing in for `ä`/`ö`/`ü`/`ß`) and original
CRLF line endings, which the living `src-omti/` copy has since had cleaned
up for readability. This one hasn't, on purpose: it's the historical record.
