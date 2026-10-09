# 울산 Vault 운영 규칙

## Vault 구조

- `index.md` — vault 루트의 전체 인덱스
- `entities/` — 인물·기관 등
- `concepts/` — 개념·구조
- `references/` — 자료·기록
- `synthesis/` — 여러 소스를 종합한 분석
- `daily/` — 일일 브리핑 (수정 금지)
- 그 외 루트의 원본 자료 폴더(`0 일반/`, `교통/`, `산업1/` 등) — ingest 대상 소스

## 명령어

### "ingest"
1. 처리 대상 파일을 판정한다. 기준은 `.manifest.json`의 `manifest_entries`에 기록된 `content_hash`(`sha256:` + 파일 원본 바이트의 SHA-256)다:
   - **미처리**: `manifest_entries`에 해당 파일의 `content_hash`가 없다 → 읽고 ingest한다.
   - **변경**: 저장된 `content_hash`와 현재 파일 해시가 다르다 → 다시 읽고 ingest한다.
   - **스킵**: 해시가 같다 → 변경이 없으므로 읽지 않는다.
   - mtime은 판정 기준으로 쓰지 않는다.
2. 대상 제외: `entities/`, `concepts/`, `references/`, `synthesis/`는 산출물이다. `daily/`는 읽기 전용 폴더이며 ingest하지 않는다. 이 폴더들은 판정과 읽기 대상에서 모두 빠진다.
3. 핵심 엔티티(인물·기관·사업 등)와 개념을 추출한다.
3. 한국어 요약 페이지를 내용 성격에 맞는 폴더에 작성한다:
   - 인물·기관·사업 주체 → `entities/`
   - 개념·구조·정책 메커니즘 → `concepts/`
   - 단일 자료·기록의 정리본 → `references/`
   - 여러 소스를 엮은 종합 분석 → `synthesis/`
4. `index.md`를 갱신한다.

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
- 모든 wiki 페이지(`entities/`, `concepts/`, `references/`, `synthesis/`)는 한국어로 작성한다.
- 모든 wiki 페이지는 해당 폴더의 기존 frontmatter 스키마를 그대로 따른다 (임의로 새 스키마를 도입하지 않는다).
