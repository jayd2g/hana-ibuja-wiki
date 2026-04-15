# My Wiki — LLM 스키마

이 문서는 LLM(Claude 등)이 이 위키를 유지·관리할 때 따라야 할 규칙과 워크플로우를 정의합니다.

---

## 위키 목적

개인 성장, 아이디어/기획, 리서치를 체계적으로 축적하는 지식 베이스.
- 원본 자료는 `raw/`에 저장 (LLM이 수정하지 않음)
- 위키 파일은 `wiki/`에 저장 (LLM이 작성·업데이트)

---

## 폴더 구조

```
my-wiki/
├── CLAUDE.md           ← 이 파일 (LLM 지침)
├── raw/                ← 원본 자료 (아티클, 노트, PDF 텍스트 등) — 불변
└── wiki/
    ├── index.md        ← 전체 목차 (매번 업데이트)
    ├── log.md          ← 활동 로그 (append-only)
    ├── entities/       ← 인물, 프로젝트, 조직, 제품
    ├── concepts/       ← 아이디어, 프레임워크, 개념
    └── sources/        ← 원본 자료 요약 페이지
```

---

## 페이지 형식

### sources/ 페이지 (원본 요약)
```markdown
---
type: source
title: [제목]
date_ingested: [YYYY-MM-DD]
tags: [태그1, 태그2]
---

## 핵심 요약
[2-4문장 핵심 요약]

## 주요 인사이트
- [인사이트 1]
- [인사이트 2]

## 관련 위키 페이지
- [[concepts/개념명]]
- [[entities/이름]]

## 메모
[추가 맥락이나 의문점]
```

### concepts/ 페이지 (개념/아이디어)
```markdown
---
type: concept
title: [개념명]
source_count: [참조된 소스 수]
tags: [태그]
---

## 정의
[이 개념이 무엇인지]

## 핵심 내용
[개념에 대한 축적된 이해]

## 연관 개념
- [[concepts/다른개념]]

## 출처
- [[sources/소스제목]]
```

### entities/ 페이지 (인물/프로젝트/조직)
```markdown
---
type: entity
title: [이름]
category: [person | project | org | product]
tags: [태그]
---

## 개요
[한 줄 설명]

## 상세
[알고 있는 것들]

## 관련 개념
- [[concepts/개념명]]

## 출처
- [[sources/소스제목]]
```

---

## 워크플로우

### 1. Ingest (자료 추가)
새 자료를 처리할 때 순서:
1. 자료를 읽고 핵심 인사이트를 사용자와 논의
2. `wiki/sources/[제목].md` 요약 페이지 작성
3. 관련 `concepts/` 페이지 업데이트 (없으면 새로 생성)
4. 관련 `entities/` 페이지 업데이트 (없으면 새로 생성)
5. `wiki/index.md` 업데이트
6. `wiki/log.md`에 항목 추가: `## [YYYY-MM-DD] ingest | [제목]`

### 2. Query (질문에 답하기)
1. `wiki/index.md`를 먼저 읽어 관련 페이지 파악
2. 관련 페이지들을 읽고 종합해서 답변
3. 가치 있는 답변은 새 위키 페이지로 저장

### 3. Lint (위키 건강 점검)
주기적으로 다음을 확인:
- 모순된 내용이 있는 페이지
- 링크가 없는 고립된 페이지 (orphan)
- 언급은 됐지만 페이지가 없는 개념
- 오래돼서 업데이트가 필요한 내용

---

## 규칙

- 원본(`raw/`) 파일은 절대 수정하지 않는다
- 모든 위키 페이지는 YAML frontmatter를 포함한다
- 다른 페이지 참조는 항상 `[[폴더/파일명]]` 형식으로
- log.md는 append-only — 기존 항목은 수정하지 않는다
- index.md는 매번 ingest 후 반드시 업데이트한다
- 한국어로 작성한다 (소스가 영어여도 위키는 한국어)
