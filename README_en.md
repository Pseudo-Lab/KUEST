# **KUEST**: **K**orean game **U**sage **E**valuation **S**uite for llm **T**ranslation

📄 **Paper:** [KUEST: Korean Game Usage Evaluation Suite for LLM Translation](https://www2.statmt.org/wmt26/pdf/2026.wmt-1.86.pdf) (WMT 2026)

KUEST is a WMT26 test suite built to evaluate the machine translation weaknesses that surface when translating English into Korean in the game domain.

**Translation direction:** English → Korean (`en` → `ko_KR`)

## Research team

- **Affiliation:** Pseudo Lab
- **Team members:** **Minjoo Choi**, Junhyeop Kim, Hyemi Kim, Yiji Yoon, Jaejun Jung, Yeonseo Cho, Gunwoo Han, Jinwoo Jang
- **Contact:** mjchoi0831@gmail.com

## Overview

- **Language direction:** English → Korean (`src_lang` = `en`, `tgt_lang` = `ko_KR`)
- **Domain:** Video games — Cataclysm: Dark Days Ahead (2025 `ko_25.po`), Battle for Wesnoth (PO modules)
- **Submission file:** [`data/KUEST.jsonl`](data/KUEST.jsonl) (JSON Lines, UTF-8)
- **Segments:** 1,796
- **Source tokens:** ~19,996 (whitespace-delimited), within the per-participant cap of 1M tokens
- **License:** Each record follows the license specified in its `license` field (`CC-BY-SA-3.0` for Cataclysm-DDA, `GPL-2.0` for Battle for Wesnoth). Submitted source sentences may be made public for research purposes. See [Sources & licensing](#sources--licensing) for details.

## Repository layout

| Path | Contents |
|---|---|
| [`data/KUEST.jsonl`](data/KUEST.jsonl) | Test suite data (1,796 rows, JSON Lines) |
| [`licenses/DATASET-LICENSE.md`](licenses/DATASET-LICENSE.md) | Dataset licensing notice (per-source licenses, attribution, compilation and redistribution terms) |
| [`licenses/CC-BY-SA-3.0.txt`](licenses/CC-BY-SA-3.0.txt) | Full text of CC BY-SA 3.0 (applies to Cataclysm-DDA records) |
| [`licenses/GPL-2.0.txt`](licenses/GPL-2.0.txt) | Full text of the GNU GPL v2 (applies to Battle for Wesnoth records) |
| [`README.md`](README.md) / `README_en.md` | Korean / English README |

## Evaluation scope

KUEST focuses on phenomena where a translation can read fluently yet still be wrong in Korean. It consists of the following four categories.

| Category | Items | Focus |
|---|---:|---|
| **TA** — Terminology & Accuracy | 331 | Adherence to canonical glossary terms; context-dependent polysemy disambiguation |
| **PP** — Persona & Pragmatics | 388 | Consistency of speech style and honorifics appropriate to the speaker persona; **dual-prompt** setup (baseline / persona-enriched regime R) |
| **TR** — Translation Robustness | 507 | Placeholder reordering, tag preservation, length-limit compliance |
| **CT** — Cultural Transcreation | 570 | Honorifics, onomatopoeia, idioms, name localization, hate speech, etc. |
| **Total** | **1,796** | |

Items appear in the data file in the order TA → PP → TR → CT.

### Subcategory breakdown

| Category | Subcategory | Items |
|---|---|---:|
| TA | `glossary_compliance` | 264 |
| TA | `polysemy` | 67 |
| PP | `persona_pragmatics` (baseline) | 194 |
| PP | `persona_pragmatics-regime_r` (regime R) | 194 |
| TR | `placeholder_reordering_consistency` | 172 |
| TR | `placeholder_reordering_diverse` | 190 |
| TR | `tag_preservation` | 95 |
| TR | `length_constraint_refine` | 50 |
| CT | `cultural_reference` | 33 |
| CT | `gender_stereotyping` | 20 |
| CT | `hate_speech` | 56 |
| CT | `idiom_proverb` | 16 |
| CT | `interjection` | 39 |
| CT | `naming_transcreation` | 45 |
| CT | `speech_style_adaptation` | 273 |
| CT | `wordplay` | 88 |

### PP dual-prompt (regime R)

The PP category includes the same 194 speaker utterances (97 speaker pairs × `-a`/`-b`) under **two prompt conditions** (regimes). This makes it possible to measure how persona information affects the translation.

- **baseline** — uses the fixed prompt `Translate from en to ko_KR.` (subcategory `PP-persona_pragmatics`, 194 rows).
- **regime R** — uses an English prompt carrying the speaker's persona and relationship information, such as speaker name, role tier, in-game unit type (where available), speech act, and social-standing relationship. It does not prescribe specific Korean endings (subcategory `PP-persona_pragmatics-regime_r-001-a … -097-b`, 194 rows).

`src_text` and `ref_text` are identical across the two conditions; only `prompt` differs.

## Record format

Each line consists of seven WMT-format fields and two provenance fields (`source`, `license`).

```json
{"dataset_id": "KUEST", "doc_id": "en-ko_KR_#_KUEST_#_CT-idiom_proverb-001", "src_lang": "en", "tgt_lang": "ko_KR", "prompt": "Translate from en to ko_KR.", "src_text": "Your skill in speaking to other people. Covers ability in boasting, flattery, threats, persuasion, lies, and other facets of interpersonal communication. Works best in conjunction with a high level of intelligence.", "ref_text": "사람들과 대화하는 기술입니다. 자랑, 아첨, 위협, 설득, 거짓말 등 사람들과의 소통 전반에 영향을 미칩니다. 지능이 높은 캐릭터와 결합하면 금상첨화일 것입니다.", "source": "CDDA", "license": "CC-BY-SA-3.0"}
```

- `doc_id`: follows the format `en-ko_KR_#_KUEST_#_<category>-<subcategory>-<NNN>`. Numbering starts at `001` for each subcategory and is written with three digits. PP splits each speaker pair into two rows, `-a`/`-b`; regime R rows insert `-regime_r-` between the original subcategory name and the number (`PP-persona_pragmatics-regime_r-…`) and keep the original pair numbering (`001`–`097`).
- `prompt`: fixed to `Translate from en to ko_KR.` for every row; only the 194 regime R rows (`PP-persona_pragmatics-regime_r-*`) are an **exception** and use prompts carrying persona information (see 'PP dual-prompt' above).
- `ref_text`: the Korean reference translation. There are no null values, and it is identical across both PP conditions.
- `source`: the upstream project the record's text comes from (`CDDA` or `Wesnoth`).
- `license`: the SPDX identifier that applies to the record's `src_text` and `ref_text` (`CC-BY-SA-3.0` or `GPL-2.0`).

## Sources & licensing

`data/KUEST.jsonl` is not distributed under a single license. It is a collection in which each record individually follows the license specified in its `license` field.

| Source (`source`) | License (`license`) | TA | PP | TR | CT | Total |
|---|---|---:|---:|---:|---:|---:|
| Cataclysm: Dark Days Ahead (`CDDA`) | `CC-BY-SA-3.0` | 243 | 0 | 412 | 200 | 855 |
| Battle for Wesnoth (`Wesnoth`) | `GPL-2.0` | 88 | 388 | 95 | 370 | 941 |
| **Total** | | **331** | **388** | **507** | **570** | **1,796** |

- The `license` field of the Wesnoth records reads `GPL-2.0`, but the applicable license stated in [`licenses/DATASET-LICENSE.md`](licenses/DATASET-LICENSE.md) is GPL-2.0-or-later (GPL version 2 or any later version).
- The data selection and structuring, and the fields written by the KUEST authors (e.g., the `doc_id` scheme and the `prompt` field), are covered by `CC-BY-4.0`. This does not affect the license of `src_text`/`ref_text`, and modified upstream text also remains under its source's license.

If you need single-license data, filter on the `license` field. Attribution, compilation terms, and redistribution requirements are described in [`licenses/DATASET-LICENSE.md`](licenses/DATASET-LICENSE.md), and the full license texts are in [`licenses/CC-BY-SA-3.0.txt`](licenses/CC-BY-SA-3.0.txt) and [`licenses/GPL-2.0.txt`](licenses/GPL-2.0.txt). When redistributing the data, include this notice and both license texts with it.

## Text normalization

- Paragraph breaks and line breaks are encoded as `\n\n`, following the WMT format guidelines.
- Tabs and other control characters were replaced with a single space.
- Placeholders and tags (`%s`, `{forest_type}`, `<span ...>`, `$player_…`) were preserved as-is, and source/reference placeholder parity was verified for all TR items.

## Citation

If you use KUEST, please cite the following paper.

```bibtex
@inproceedings{choi-etal-2026-kuest,
  title     = {{KUEST}: Korean Game Usage Evaluation Suite for {LLM} Translation},
  author    = {Choi, Minjoo and Jung, Jaejun and Han, Gunwoo and Kim, Junhyeop and
               Yoon, Yiji and Kim, Hyemi and Cho, Yeonseo and Jang, Jinwoo},
  booktitle = {Proceedings of the Eleventh Conference on Machine Translation (WMT)},
  pages     = {1491--1512},
  year      = {2026},
  publisher = {Association for Computational Linguistics},
  url       = {https://www2.statmt.org/wmt26/pdf/2026.wmt-1.86.pdf}
}
```