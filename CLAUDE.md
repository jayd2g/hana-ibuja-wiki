# 아이부자 컨설팅 Wiki — LLM 스키마

이 문서는 LLM(Claude 등)이 이 위키를 유지·관리할 때 따라야 할 규칙과 워크플로우를 정의합니다.

---

## 위키 목적

하나은행 아이부자 UX 컨설팅 프로젝트의 리서치, 분석, 전략을 체계적으로 축적하는 지식 베이스.
- 원본 PDF는 `sc/`에 보관 (LLM이 절대 수정하지 않음)
- 분석 작업 문서는 `Deskresearch/`에 저장 (카테고리별 정리)
- 정리된 지식 베이스는 `wiki/`에 저장 (LLM이 작성·업데이트)

---

## 폴더 구조

```
hana-ibuja-wiki/
├── CLAUDE.md              ← 이 파일 (LLM 지침)
├── sc/                    ← 원본 PDF 보관소 — 절대 수정하지 않음
├── Deskresearch/          ← 분석 작업 문서 (카테고리별)
│   ├── 00_가설/
│   ├── 01_시장/
│   ├── 02_경쟁사/
│   ├── 03_사용자/
│   ├── 04_인사이트/
│   └── 05_Tobe전략/
└── wiki/
    ├── index.md           ← 전체 목차 (매번 업데이트)
    ├── log.md             ← 활동 로그 (append-only)
    ├── HOME.md            ← 빠른 탐색 허브
    ├── entities/          ← 인물, 프로젝트, 조직
    ├── concepts/          ← 아이디어, 프레임워크, 개념
    └── sources/           ← sc/ PDF 요약 마크다운 페이지
```

> **폴더 역할 구분**
> - `sc/` : 원본 PDF 원본 파일. 불변.
> - `wiki/sources/` : sc/의 PDF를 읽고 요약·정리한 마크다운. 하나의 PDF = 하나의 소스 페이지.
> - `Deskresearch/` : 소스들을 분석·종합해서 작성한 작업 문서. 카테고리별로 분류.

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

- 원본(`sc/`) PDF 파일은 절대 수정하지 않는다
- 모든 위키 페이지는 YAML frontmatter를 포함한다
- 다른 페이지 참조는 항상 `[[폴더/파일명]]` 형식으로
- log.md는 append-only — 기존 항목은 수정하지 않는다
- index.md는 매번 ingest 후 반드시 업데이트한다
- 한국어로 작성한다 (소스가 영어여도 위키는 한국어)
- 하나의 PDF → 하나의 sources/ 페이지만 생성한다 (중복 처리 금지)
- 새 sources/ 페이지 작성 전 index.md를 확인해 동일 자료가 이미 있는지 검토한다
