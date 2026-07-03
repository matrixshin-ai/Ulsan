---
title: Wiki Health Report — 2026-06-17
category: synthesis
tags: [wiki관리, lint, 품질감사]
aliases: [lint-report-2026-06-17]
relationships: []
sources: []
summary: wiki-lint 13개 항목 전체 감사 결과. Critical 5건(수정 완료), Warn 7건(4건 수정 완료 + 3건 권고사항), Info 4건 로드맵 제안.
provenance:
  extracted: 0.0
  inferred: 1.0
  ambiguous: 0.0
base_confidence: 1.0
lifecycle: stable
lifecycle_changed: "2026-06-17"
tier: peripheral
created: 2026-06-17T00:00:00Z
updated: 2026-06-17T00:00:00Z
---

# Wiki Health Report — 2026-06-17

**감사 범위**: 27개 위키 페이지 (entities×5, synthesis×3, references×12, concepts×7)  
**감사 일시**: 2026-06-17  
**결과 요약**: CRITICAL 5건(전건 수정), WARN 3건(오픈), INFO 4건(로드맵)

---

## CRITICAL (수정 완료)

### C1. 깨진 위키링크 — `entities/김현.md`

| 위치 | 원본 (수정 전, 존재하지 않는 경로) | 수정 |
|---|---|---|
| frontmatter relationships | `concepts/수요응답형교통-DRT` | `[[concepts/울산-DRT]]` |
| body 관련 항목 | `concepts/수요응답형교통-DRT` | `[[concepts/울산-DRT]]` |
| body 관련 항목 | `교통/ulsan_drt_learning_material` (소스 파일 직접 링크) | `[[concepts/울산-대중교통]]`으로 교체 |

실제 페이지는 `concepts/울산-DRT.md`. `수요응답형교통-DRT` 경로는 존재하지 않음.

### C2. 잘못된 관계 타입 — `synthesis/민선9기-간담회-종합.md`

```yaml
# 수정 전
- target: "[[entities/김상욱]]"
  type: created_by    # ← 허용 타입 아님

# 수정 후
- target: "[[entities/김상욱]]"
  type: related_to
```

허용 타입: `extends | implements | contradicts | derived_from | uses | replaces | related_to`

### C3. 잘못된 관계 타입 — `references/김상욱-경제공약.md`

```yaml
# 수정 전
- target: "[[entities/김상욱]]"
  type: detail_of    # ← 허용 타입 아님

# 수정 후
- target: "[[entities/김상욱]]"
  type: derived_from
```

---

## WARN — 소프트 오펀 (수정 완료)

### W1. `references/울산-SAFE-X.md` — incoming 링크 없음

`index.md`에만 등재. 콘텐츠 페이지로부터 인바운드 링크 없음.  
**조치**: `references/초광역-AI-AX.md` 관련 항목에 `[[references/울산-SAFE-X]]` 추가.

### W2. `references/반구천-암각화-브랜드.md` — incoming 링크 없음

`index.md`에만 등재. `민선8기-핵심사업.md` 본문에 "반구대 암각화 연계사업(19번)" 언급이 있으나 위키링크 없음.  
**조치**: `references/민선8기-핵심사업.md` 관련 항목에 `[[references/반구천-암각화-브랜드]]` 추가.

### W3. 위키링크 경로 형식 — `concepts/울산-산업전환.md`

```markdown
# 수정 전 (짧은 이름 형식)
([[초광역-AI-AX]])

# 수정 후 (경로 형식 — 권장)
([[references/초광역-AI-AX]])
```

Obsidian 파일명 기반 resolve는 가능하나 경로 형식이 모호성 차단에 유리함. 수정 완료.

---

## WARN — 태그 단편화 (수정 완료)

### W4. `하청노동` vs `하청` 태그 불일치

| 파일 | 수정 전 | 수정 후 |
|---|---|---|
| `entities/천현우.md` | `하청노동` | `하청` |

`concepts/울산-하청노동-구조.md`는 `하청` 태그 사용. 통일.

---

## WARN — 프로버넌스 드리프트 (오픈)

추출 비율이 높지만 신뢰도가 낮게 책정된 페이지 5건. 단일 출처 기반이나 얕은 커버리지 이유로 낮게 설정된 것으로 보임. 재검토 권고.

| 파일 | extracted | base_confidence | 권고 |
|---|---|---|---|
| `references/역대-울산-경제부시장.md` | 1.00 | 0.55 | → 0.70 |
| `entities/김우성.md` | 0.95 | 0.55 | → 0.70 |
| `entities/김태선.md` | 0.95 | 0.55 | → 0.70 |
| `references/울산-출연기관-거버넌스.md` | 0.90 | 0.55 | → 0.65 |
| `references/울산-시금고.md` | 0.90 | 0.55 | → 0.65 |

---

## INFO — 체크 결과 (통과)

| 체크 | 결과 |
|---|---|
| C3. 필수 프론트매터 | PASS — 27/27 페이지 전 필드 완비 |
| C4. 오래된 콘텐츠 | PASS — 전 페이지 2026-06-17 생성, 스탈 없음 |
| C5. 모순 탐지 | PASS — `contradicts` 관계 없음; 숫자 데이터 수동 검토 권고 |
| C6. 인덱스 일관성 | PASS — 27/27 페이지 index.md 등재 |
| C12. 스키마 유효성 | PASS — lifecycle(all:draft), tier(core/supporting/peripheral), confidence(0~1) 전부 유효 |

---

## INFO — 로드맵 제안

### 합성 갭 (Check 11)

다음 3개 주제 클러스터는 별도 synthesis 페이지 없음:

1. **노동/하청**: entities/천현우 + concepts/울산-하청노동-구조 + references/울산-복지-이슈
   → 제안: `synthesis/울산-노동이슈-2026.md`

2. **교통 시스템**: entities/김현 + concepts/울산-대중교통 + concepts/울산-DRT
   → 제안: `synthesis/울산-교통개혁-로드맵.md`

3. **에너지·안전 인프라**: concepts/울산-에너지전환 + references/울산-SAFE-X
   → 제안: `synthesis/울산-그린인프라.md`

### 티어 승격 후보 (Check 10)

- `references/울산-복지-이슈.md` (현재: peripheral)  
  → 민선9기 간담회 최우선 의제이며 2개 페이지에서 인바운드 링크. `supporting` 승격 검토.

---

## 수정 요약

```
links_fixed=3  orphans_rescued=2  tag_fixes=1  relationship_type_fixes=2
wikilink_format_fixes=1  total_edits=9
open_warns=5(provenance_drift)  open_info=4(roadmap)
```

QMD skipped: QMD_WIKI_COLLECTION unset
