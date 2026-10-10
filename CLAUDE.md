# 울산 Vault 운영 규칙

## Vault 구조

- `index.md` — vault 루트의 전체 인덱스
- `entities/` — 인물·기관 등
- `concepts/` — 개념·구조
- `references/` — 자료·기록
- `synthesis/` — 여러 소스를 종합한 분석
- `daily/` — 일일 브리핑 (ingest 소스. 단, 수정 금지)
- `hot.md` — 최근 활동·추적 중인 현안 캐시 (산출물)
- `daily/`와 그 외 루트의 원본 자료 폴더(`0 일반/`, `교통/`, `산업1/` 등) — ingest 대상 소스
- 외부 소스 `C:\Users\신민식\ax-vault-full` — korea-industry-ax가 매일 자동 커밋하는 산업 AX 기사 전문 vault(`AX뉴스/YYYY/YYYY-MM-DD/*.md`). ingest 대상이지만 읽기 전용이다

## 명령어

### "ingest"
1. 처리 대상 파일을 판정한다. 기준은 `.manifest.json`의 `manifest_entries`에 기록된 `content_hash`(`sha256:` + 파일 원본 바이트의 SHA-256)다:
   - **미처리**: `manifest_entries`에 해당 파일의 `content_hash`가 없다 → 읽고 ingest한다.
   - **변경**: 저장된 `content_hash`와 현재 파일 해시가 다르다 → 다시 읽고 ingest한다.
   - **스킵**: 해시가 같다 → 변경이 없으므로 읽지 않는다.
   - mtime은 판정 기준으로 쓰지 않는다.
   - 외부 소스 `ax-vault-full`도 같은 해시 기준으로 판정한다. 대상은 `AX뉴스/` 아래 `.md` 파일뿐이고 `.git/`, `.obsidian/`, `README.md`, `.gitattributes`는 뺀다. manifest 키는 `ax-vault-full/` 뒤에 vault 내부 경로를 붙인다(예: `ax-vault-full/AX뉴스/2026/2026-10-06/파일명.md`).
2. 대상 제외: `entities/`, `concepts/`, `references/`, `synthesis/`와 루트의 `index.md`, `log.md`, `hot.md`, `CLAUDE.md`, `.manifest.json`은 산출물·설정 파일이라 판정과 읽기 대상에서 빠진다. `daily/`는 ingest 대상이지만 파일을 수정하지는 않는다.
3. 핵심 엔티티(인물·기관·사업 등)와 개념을 추출한다. 사적인 대화와 인사(人事) 관련 내용도 제외하지 않고 반영한다.
4. 한국어 요약 페이지를 내용 성격에 맞는 폴더에 작성한다:
   - 인물·기관·사업 주체 → `entities/`
   - 개념·구조·정책 메커니즘 → `concepts/`
   - 단일 자료·기록의 정리본 → `references/`
   - 여러 소스를 엮은 종합 분석 → `synthesis/`
5. 소스 처리 규칙:
   - **불확실한 수치**: 자료 간 수치가 다르거나 범위·기준이 불분명하면 한쪽으로 확정하지 말고 두 값과 출처를 함께 적은 뒤 `^[ambiguous]`를 단다.
   - **중복 파일**: 같은 내용의 파일(해시가 같거나, 원본 변환본과 재정리본처럼 내용이 사실상 같은 경우)은 한 번만 반영한다. 나머지 파일의 manifest `note`에 어느 파일과 중복인지 적는다.
   - **빈 파일**: 내용이 없는 파일(0바이트, 빈 캔버스 등)은 반영하지 않고 manifest `note`에 사유를 적는다.
   - **외부 소스 ax-vault-full 읽기 범위**: 대상 기사 전부의 제목·메타(등급·섹션·산업)를 검토하고, 본문은 울산 관련 기사(제목이나 본문에 '울산'이 있는 기사)와 S등급 기사 중 주제별 대표 기사만 읽는다. 같은 보도를 여러 매체가 받아 쓴 기사는 하나만 읽고 나머지는 중복으로 기록한다. 결과는 `synthesis/전국-산업AX-동향-2026`(해가 바뀌면 해당 연도 페이지)에 종합하고, 울산 관련 사실은 해당 울산 페이지에도 반영한다. manifest `note`에 본문 열람·중복·제목·메타만 검토 중 어느 쪽인지 적는다.
6. `index.md`를 갱신한다.
7. 실행이 끝날 때마다 기록한다:
   - `.manifest.json`: 미처리·변경으로 판정된 파일(형식 미지원 등으로 읽지 못한 파일 포함)마다 `manifest_entries`에 `ingested_at`, `size_bytes`, `modified_at`, 현재 `content_hash`, `pages_created`, `pages_updated`를 쓰고, 읽지 못했거나 반영할 내용이 없으면 사유를 `note`에 단다. 해시가 같아 스킵한 파일의 항목은 건드리지 않는다. `last_ingest`, `sources_processed`, `pages_created`도 같이 갱신한다. glob 같은 묶음 키는 쓰지 않고 파일 하나에 항목 하나를 둔다.
   - `log.md`: `INGEST` 줄(소스, pages_created/updated 수, 스킵 사유)과 `STATUS` 줄을 기존 형식대로 덧붙인다.
   - `hot.md`: frontmatter의 `updated`를 갱신하고, `Recent Activity`에 이번 ingest 요약을 맨 위에 추가하며, `Active Threads`를 새 소스 기준으로 고친다(해소된 현안은 해소로 표시하거나 정리).

### "lint"
`entities/`, `concepts/`, `references/`, `synthesis/`를 점검하고 다음을 보고한다 (수정하지 않고 보고만 한다):
- 깨진 위키링크
- 고아 페이지 (어디서도 링크되지 않는 페이지)
- 내용 간 모순
- 내용 공백 (다뤄야 하는데 빠진 주제)

### "query [질문]"
1. 먼저 `index.md`를 읽는다.
2. `entities/`, `concepts/`, `references/`, `synthesis/` 내용에만 근거해 한국어로 답변한다. 거기에 없는 내용은 추측하지 않는다.

## 제약

- `daily/` 안의 파일은 절대 수정하지 않는다.
- `ax-vault-full` 안에는 파일을 수정·추가·삭제하지 않는다(GitHub Actions가 자동 커밋하는 저장소다).
- 모든 wiki 페이지(`entities/`, `concepts/`, `references/`, `synthesis/`)는 한국어로 작성한다.
- 모든 wiki 페이지는 해당 폴더의 기존 frontmatter 스키마를 그대로 따른다 (임의로 새 스키마를 도입하지 않는다).
