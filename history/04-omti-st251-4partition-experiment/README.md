# The Seagate ST251 / 4-partition experiment (undated)

Two standalone fragments, found only in `GenieIIIs/Holte CPM src/` — not
part of either the mainline or the Schröer-branch disk snapshots, so
presumably a separate, later exploration that never made it onto a
surviving system disk in this collection.

**`hddtbl.mac`** — a more ambitious hard-disk table than either branch's
`HDDTBL.ASM`: geometry for a Seagate ST251 (6 heads × 820 cylinders,
"für Harddisks mit 6 Köpfen und 820 Zylindern"), and **four** partitions
(`hdsk0`..`hdsk3` / `hddpb0`..`hddpb3`) instead of the mainline's two
(`DS0`/`DS1`) or the Schröer branch's one (`hdsk0`). It also references a
routine name, `hd1ini`, and a naming convention (`hdsk0`..`hdsk3`) that
doesn't match `DISKIO1.MAC` in either surviving branch — evidence this was
built against a different, wider `HD2.MAC` variant that isn't among the
recovered snapshots.

**`hdchar.mac`** — a standalone OMTI `SET CHARACTERISTICS` data block (the
same 8-byte command payload `HDNDF.Z80` sends, see
`../03-omti-schroeer-branch-1992/`) for a drive with 612 cylinders/2 heads —
closer to the Tandon TM262 geometry than either ST225 config, and likely a
leftover example/test fragment from that stage rather than a complete
driver.

Kept here as evidence that hard-disk support kept being experimented with
beyond what any single surviving disk snapshot captures whole.
