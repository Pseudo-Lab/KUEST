# **KUEST**: **K**orean game **U**sage **E**valuation **S**uite for llm **T**ranslation

📄 **논문:** [KUEST: Korean Game Usage Evaluation Suite for LLM Translation](https://www2.statmt.org/wmt26/pdf/2026.wmt-1.86.pdf) (WMT 2026)

KUEST는 게임 도메인에서 영어를 한국어로 번역할 때 드러나는 기계번역의 취약점을 평가하기 위해 구축한 WMT26 테스트 스위트입니다.

**번역 방향:** 영어 → 한국어 (`en` → `ko_KR`)

## 연구팀

- **소속:** Pseudo Lab
- **팀원:** **최민주**, 김준협, 김혜미, 윤이지, 정재준, 조연서, 한건우, 장진우
- **연락처:** mjchoi0831@gmail.com

## 개요

- **언어 방향:** 영어 → 한국어 (`src_lang` = `en`, `tgt_lang` = `ko_KR`)
- **도메인:** 비디오 게임 — Cataclysm: Dark Days Ahead(2025 `ko_25.po`), Battle for Wesnoth(PO 모듈)
- **제출 파일:** [`data/KUEST.jsonl`](data/KUEST.jsonl) (JSON Lines, UTF-8)
- **세그먼트 수:** 1,796개
- **원문 토큰 수:** 약 19,996개(공백 기준)로, 참가자당 상한인 100만 토큰 이내
- **라이선스:** 레코드마다 `license` 필드에 명시된 라이선스를 따릅니다(Cataclysm-DDA는 `CC-BY-SA-3.0`, Battle for Wesnoth는 `GPL-2.0`). 제출한 원문 문장은 연구 목적으로 공개될 수 있습니다. 자세한 내용은 [출처 및 라이선스](#출처-및-라이선스)를 참고하세요.

## 저장소 구성

| 경로 | 내용 |
|---|---|
| [`data/KUEST.jsonl`](data/KUEST.jsonl) | 테스트 스위트 데이터 (1,796행, JSON Lines) |
| [`licenses/DATASET-LICENSE.md`](licenses/DATASET-LICENSE.md) | 데이터셋 라이선스 고지 (출처별 라이선스, 저작자 표시, 편집물·재배포 조건) |
| [`licenses/CC-BY-SA-3.0.txt`](licenses/CC-BY-SA-3.0.txt) | CC BY-SA 3.0 전문 (Cataclysm-DDA 레코드에 적용) |
| [`licenses/GPL-2.0.txt`](licenses/GPL-2.0.txt) | GNU GPL v2 전문 (Battle for Wesnoth 레코드에 적용) |
| `README.md` / [`README_en.md`](README_en.md) | 한국어 / 영어 README |

## 평가 범위

KUEST는 번역 결과가 유창해 보이더라도 한국어로는 틀릴 수 있는 현상을 중점적으로 다루며, 다음 네 가지 카테고리로 구성됩니다.

| 카테고리 | 항목 수 | 초점 |
|---|---:|---|
| **TA** — Terminology & Accuracy | 331 | 용어집의 표준 번역어 준수, 문맥에 따른 다의어 판별 |
| **PP** — Persona & Pragmatics | 388 | 화자 페르소나에 맞는 말투·높임법의 일관성, **이중 프롬프트** 구성(baseline / 페르소나 정보를 더한 regime R) |
| **TR** — Translation Robustness | 507 | 플레이스홀더 순서 변경, 태그 보존, 길이 제한 준수 |
| **CT** — Cultural Transcreation | 570 | 높임법, 의성어, 관용구, 명칭 현지화, 혐오 표현 등 |
| **합계** | **1,796** | |

데이터 파일에는 TA → PP → TR → CT 순서로 항목이 수록되어 있습니다.

### 세부 카테고리 구성

| 카테고리 | 세부 카테고리 | 항목 수 |
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

### PP 이중 프롬프트 (regime R)

PP 카테고리는 같은 화자 발화 194개(화자 쌍 97개 × `-a`/`-b`)를 **두 가지 프롬프트 조건**(regime)으로 각각 수록합니다. 이를 통해 페르소나 정보가 번역에 미치는 영향을 측정할 수 있습니다.

- **baseline** — 고정 프롬프트 `Translate from en to ko_KR.`를 사용합니다(세부 카테고리 `PP-persona_pragmatics`, 194행).
- **regime R** — 화자 이름, 역할 등급(role tier), 게임 내 유닛 타입(있는 경우), 화행, 사회적 지위 관계 등 화자의 페르소나와 관계 정보를 담은 영어 프롬프트를 사용합니다. 구체적인 한국어 어미는 지시하지 않습니다(세부 카테고리 `PP-persona_pragmatics-regime_r-001-a … -097-b`, 194행).

두 조건에서 `src_text`와 `ref_text`는 동일하고, `prompt`만 다릅니다.

## 레코드 형식

각 행은 WMT 형식의 필드 7개와 출처 정보 필드 2개(`source`, `license`)로 구성됩니다.

```json
{"dataset_id": "KUEST", "doc_id": "en-ko_KR_#_KUEST_#_CT-idiom_proverb-001", "src_lang": "en", "tgt_lang": "ko_KR", "prompt": "Translate from en to ko_KR.", "src_text": "Your skill in speaking to other people. Covers ability in boasting, flattery, threats, persuasion, lies, and other facets of interpersonal communication. Works best in conjunction with a high level of intelligence.", "ref_text": "사람들과 대화하는 기술입니다. 자랑, 아첨, 위협, 설득, 거짓말 등 사람들과의 소통 전반에 영향을 미칩니다. 지능이 높은 캐릭터와 결합하면 금상첨화일 것입니다.", "source": "CDDA", "license": "CC-BY-SA-3.0"}
```

- `doc_id`: `en-ko_KR_#_KUEST_#_<카테고리>-<세부 카테고리>-<NNN>` 형식입니다. 번호는 세부 카테고리마다 `001`부터 시작하며 세 자리로 표기합니다. PP는 화자 쌍 하나를 `-a`/`-b` 두 행으로 나누며, regime R 행은 원래 세부 카테고리 이름과 번호 사이에 `-regime_r-`를 넣고(`PP-persona_pragmatics-regime_r-…`) 원래의 쌍 번호(`001`–`097`)를 그대로 사용합니다.
- `prompt`: 모든 행에 `Translate from en to ko_KR.`가 고정되어 있으며, regime R 194행(`PP-persona_pragmatics-regime_r-*`)만 **예외적으로** 페르소나 정보를 담은 프롬프트를 사용합니다(위의 'PP 이중 프롬프트' 참고).
- `ref_text`: 한국어 참조 번역입니다. null 값은 없으며, 두 PP 조건에서 동일합니다.
- `source`: 레코드 텍스트의 원출처 프로젝트(`CDDA` 또는 `Wesnoth`)입니다.
- `license`: 해당 레코드의 `src_text`와 `ref_text`에 적용되는 SPDX 식별자(`CC-BY-SA-3.0` 또는 `GPL-2.0`)입니다.

## 출처 및 라이선스

`data/KUEST.jsonl`은 하나의 라이선스로 배포되지 않습니다. 각 레코드가 `license` 필드에 명시된 라이선스를 개별적으로 따르는 컬렉션입니다.

| 출처 (`source`) | 라이선스 (`license`) | TA | PP | TR | CT | 합계 |
|---|---|---:|---:|---:|---:|---:|
| Cataclysm: Dark Days Ahead (`CDDA`) | `CC-BY-SA-3.0` | 243 | 0 | 412 | 200 | 855 |
| Battle for Wesnoth (`Wesnoth`) | `GPL-2.0` | 88 | 388 | 95 | 370 | 941 |
| **합계** | | **331** | **388** | **507** | **570** | **1,796** |

- Wesnoth 레코드의 `license` 필드는 `GPL-2.0`으로 표기되어 있으나, [`licenses/DATASET-LICENSE.md`](licenses/DATASET-LICENSE.md)에 명시된 적용 라이선스는 GPL-2.0-or-later(GPL 2판 또는 그 이후 판)입니다.
- 데이터 선별·구조화와 KUEST 저자가 직접 작성한 필드(`doc_id` 체계, `prompt` 필드 등)에는 `CC-BY-4.0`이 적용됩니다. 이는 `src_text`·`ref_text`의 라이선스에 영향을 주지 않으며, 원문을 수정한 텍스트 역시 원출처의 라이선스를 따릅니다.

단일 라이선스 데이터만 필요하다면 `license` 필드로 필터링해 사용하면 됩니다. 저작자 표시, 편집물 조건, 재배포 요건은 [`licenses/DATASET-LICENSE.md`](licenses/DATASET-LICENSE.md)에서, 라이선스 전문은 [`licenses/CC-BY-SA-3.0.txt`](licenses/CC-BY-SA-3.0.txt)와 [`licenses/GPL-2.0.txt`](licenses/GPL-2.0.txt)에서 확인할 수 있습니다. 데이터를 재배포할 때는 이 고지와 두 라이선스 전문을 함께 배포해야 합니다.

## 텍스트 정규화

- 문단 구분과 줄바꿈은 WMT 형식 안내에 따라 `\n\n`으로 표기했습니다.
- 탭과 기타 제어 문자는 공백 한 칸으로 치환했습니다.
- 플레이스홀더와 태그(`%s`, `{forest_type}`, `<span ...>`, `$player_…`)는 그대로 보존했으며, TR 전 항목에 대해 원문과 참조 번역의 플레이스홀더가 일치하는지 검증했습니다.

## 인용

KUEST를 사용하실 경우 아래 논문을 인용해 주세요.

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
