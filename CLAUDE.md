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
1. vault 내 아직 처리되지 않은 파일을 모두 읽는다 (`entities/`, `concepts/`, `references/`, `synthesis/`, `daily/`는 소스가 아니라 산출물/제외 대상이므로 읽기 대상에서 제외).
2. 핵심 엔티티(인물·기관·사업 등)와 개념을 추출한다.
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
