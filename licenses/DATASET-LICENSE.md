# KUEST — Dataset Licensing

KUEST is distributed as a single data file, `KUEST.jsonl`. Its items are derived
from open-source game-localization data released under **two different, mutually
incompatible copyleft licenses** (GPL-2.0 and CC BY-SA 3.0). `KUEST.jsonl` is
therefore **not under a single license**: it is a *collection* in which **each
record is independently licensed** according to its `license` field.

> Even though the data itself is one file, a compliant release must also ship this
> notice and copies of the two license texts — see "Redistribution & compliance".

## Component licenses

| Upstream source | Records | License (SPDX) | Upstream repository |
|---|---:|---|---|
| Cataclysm-DDA | 855 | `CC-BY-SA-3.0` | https://github.com/CleverRaven/Cataclysm-DDA |
| Battle for Wesnoth | 941 | `GPL-2.0-or-later` | https://github.com/wesnoth/wesnoth |
| **Total** | **1796** | *(mixed collection)* | |

Each record in `KUEST.jsonl` carries:

- `source` — the upstream project the record's text is derived from.
- `license` — the SPDX identifier governing that record's `src_text` and `ref_text`.

Filter by the `license` field to obtain a single-license subset.

## Attribution

**Cataclysm-DDA** — Source and reference-translation text for 855 records is derived
from *Cataclysm: Dark Days Ahead*, © the Cataclysm-DDA contributors, licensed under
the Creative Commons Attribution-ShareAlike 3.0 Unported License (CC BY-SA 3.0).
License: https://creativecommons.org/licenses/by-sa/3.0/

**Battle for Wesnoth** — Source and reference-translation text for 941 records is
derived from *The Battle for Wesnoth*, © the Battle for Wesnoth Project, licensed
under the GNU General Public License, version 2 or later (GPL-2.0-or-later).
License: https://www.gnu.org/licenses/old-licenses/gpl-2.0.html

## Compilation

The selection, structuring, and original fields authored by the KUEST authors
(e.g., the `doc_id` scheme and `prompt` field) are offered under **`CC-BY-4.0`**.
This applies **only** to the KUEST authors' original material and does **not**
relicense the `src_text`/`ref_text`, which remain under their component licenses
above. Where upstream text was modified (e.g., the gender-neutral reference
corrections), the modified text is a *derivative* of the copyleft source and stays
under that source's license (`CC-BY-SA-3.0`).

## Redistribution & compliance

If you redistribute `KUEST.jsonl` or a derivative:

- **Keep each record under its component license** (the `license` field). Do not
  relicense the whole file under a single or permissive license.
- **Ship the license texts.** Include this notice and verbatim copies of the
  CC BY-SA 3.0 and GPL-2.0 license texts alongside the data (e.g., in `licenses/`).
  Both licenses require the license to accompany the work; GPL requires including a
  copy of its text.
- **Preserve attribution** to both upstream projects.
- **ShareAlike / copyleft.** Distribute any adaptation of a record under the same
  (or a compatible) license as that record.
- **GPL source form.** For the Battle for Wesnoth records, the text itself is the
  corresponding source form, provided under GPL-2.0-or-later.

**More conservative option.** The single mixed file is a *labeled collection* and is
defensible, but if you want the airtight "separable aggregation" posture, split the
data by license into two files — e.g., `KUEST-cdda.jsonl` (`CC-BY-SA-3.0`) and
`KUEST-wesnoth.jsonl` (`GPL-2.0-or-later`) — instead of one mixed file. This removes
any ambiguity about whether the file is a single combined work.

## Note

This is a *mere-aggregation* collection: records under different licenses are
bundled together, but each remains independently licensed via its `license` field.
This document is **not legal advice** — verify license compatibility and your
redistribution obligations with your institution before public release.
