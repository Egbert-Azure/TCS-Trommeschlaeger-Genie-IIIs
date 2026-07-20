# TCS Computer GmbH: company history

Primary source: Frank Seger, *"Marketingstrategien im PC-Markt – Eine
Fallstudie"* (Diplomarbeit, Rheinische Friedrich-Wilhelms-Universität Bonn,
advisor Prof. Dr. Manfred Perlitz, submitted 29-Oct-1986), specifically its
case-study section "Geschichtliches" / "Die Unternehmensentwicklung":
<https://ia803207.us.archive.org/30/items/Trommelschlaeger_TCS/Geschichtliches.pdf>.
Seger had six years at the company himself and had the full company archive
(five filing-cabinets' worth) available while writing it — this is a
firsthand, contemporary account, not a retrospective.

## Before computers

Fred Trommeschläger started in 1969 as a sole trader dealing in electronic
components (wound down in 1980 once computer trading proved more
profitable). From 1975 he also held the exclusive German distributorship
for Mooney sport aircraft — he was a pilot himself. Computer trading began
almost incidentally in 1979: TRS-80 microcomputers came along for the ride
when he was ferrying aircraft from the USA.

## GENIE, and the TCS name

1980: "Trommeschläger Computer GmbH" founded (renamed "TCS Computer GmbH"
in mid-1983 — this repo's own "TCS" comes from that). At the 1980 Hannover
Messe, a tip from a Tandy dealer led Trommeschläger's staff straight to a
TRS-80-compatible machine from EACA (Hong Kong): the GENIE I. Trommeschläger
negotiated the exclusive German distribution rights in Holland — flying
himself there in his own plane reportedly impressed EACA considerably.

| Model | Introduced |
|---|---|
| GENIE I | June 1980 |
| GENIE II | April 1981 |
| GENIE III | July 1982 |
| COLOUR-GENIE | August 1982 |

GENIE I/II were largely TRS-80 clones. GENIE III and COLOUR-GENIE had
construction defects costly enough to fix that, in June 1983, TCS began
developing its own computer in response: **the GENIE IIIs**.

## The GENIE IIIs

Built in Germany — Siemens produced the electronics, TCS did the complete
assembly — with an improved 8-bit processor (twice the GENIE III's speed),
full compatibility with GENIE I/II/III, and new high-resolution graphics
(HRG). By December 1983 it sold for 6900 DM (5000 DM to dealers) — the top
of TCS's four-model GENIE line (GENIE I/II 1195 DM, GENIE III 5900 DM,
COLOUR-GENIE 645 DM). A 16-bit "GENIE 16" was announced but not yet
available.

**It sold only about 200 units, ever**, despite heavy advertising — this
according to Seger's own account of TCS's later attempt to make it a
volume product. That's the entire population this repo, `history/`, and
the wider disk collection are about: a machine that a handful of Club 80
members personally owned and kept alive.

## Crisis and the end

On 5-Oct-1983, TCS's main supplier, EACA (Hong Kong), collapsed — its
head, Eric Chung, reportedly left the country with over $10 million while
EACA was bankrupt. The promised 16-bit follow-on product never arrived.
1983 was already TCS's first loss-making year (revenue 12.65M DM); Seger's
case study is built around the moment TCS's leadership learned of EACA's
collapse and had to decide how to survive it.

TCS picked up a Ferranti PC (as "GENIE 16 A"/"16 B", available from
May 1984 — later than promised, and not fully IBM-PC-compatible) and, a
year later, a Taiwan-built "GENIE 16 C." Neither reversed the trend.
Mid-1984 TCS lost its Star-printer supply, and 1984 was a loss again
despite a new hall built in Windhagen, Rheinland-Pfalz.

**In August 1985, TCS Computer GmbH went bankrupt.** Fred Trommeschläger
founded Phoenix Computer GmbH & Co KG, which continued selling IBM-PC/XT/
AT-compatible microcomputers from Taiwan under the retained "GENIE" brand
name — CP/M and the GENIE IIIs were finished at TCS from that point on.
Less than two years separate the GENIE IIIs' launch from the company's
collapse: precisely the "last big CP/M computer, technically ahead of its
time but too late for a market the IBM PC had already taken" framing this
repo's own readme uses.

## What this doesn't cover

Seger's thesis is a marketing case study, not an engineering one — it has
nothing on the GENIE IIIs' hard-disk controller or hardware internals
specifically (confirmed by a full-text search of the PDF: no hits for
"Xebec," "SASI," "Winchester," or "OMTI" outside a generic glossary entry
defining what a hard disk is). The GENIE IIIs' original SASI hard-disk
controller was a **Xebec** controller (confirmed directly by Egbert
Schröer, who owned one) — this matches `src/initw.c`'s `#ifdef SASI` code
path ("port addresses of Xebec SASI Controller," `TEST`/`REST`/`FORMAT`/
`INIDRV` commands), which is Holte's *original* formatter code for that
controller, as distinct from the `#else` Western Digital WD1000/1010 path
in the same file. See `01-format-only-winchester-support/README.md` for
how this fits with the Tandon drive TCS shipped.
