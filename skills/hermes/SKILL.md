---
name: hermes
description: "Hermes Agent CLI reference for Claude Code: oneshot image generation, baoyu-infographic, and blog automation."
version: 1.0.0
author: kimdh
license: MIT
platforms: [macos]
metadata:
  tags: [hermes, image-gen, infographic, blog-automation, ai-agent]
  homepage: https://github.com/kimdh/hermes-claude-skill
---

# Hermes Agent — Claude Code 활용 가이드

**Version**: v1.0.0 (2026-05-11 최초 작성. `hermes --help` + `hermes tools list` + `hermes status` 직접 실행 기반)

---

## 사전 조건

```bash
# 설치 위치 확인
which hermes                          # /Users/<user>/.local/bin/hermes
hermes version                        # 버전 확인

# 인증 상태 + 현재 모델/프로바이더 확인
hermes status                         # Provider: OpenAI Codex, Model: gpt-5.5 (검증완료/2026-05-11)

# 툴셋 활성화 상태 확인
hermes tools list                     # image_gen, vision, file, web 등 활성화 여부
```

> `hermes model` 및 `hermes config` 하위 명령은 대화형 TTY 필요. 파이프·서브프로세스에서 실행 불가.

---

## 핵심 CLI 패턴

### 자동화용 비대화형 실행 (검증완료/2026-05-11, Hermes 직접 자문)

```bash
# 자동화 권장 형태 — hermes chat -Q (quote 깨짐 방지를 위해 프롬프트 파일 사용)
hermes chat -Q \
  --source tool \
  -s baoyu-infographic \
  -t image_gen,file,skills \
  -q "$(cat "$PROMPT_FILE")"

# stdout/stderr 분리 로그
hermes chat -Q \
  --source tool \
  -s baoyu-infographic \
  -t image_gen,file,skills \
  -q "$(cat "$PROMPT_FILE")" \
  > "$OUTPUT_DIR/logs/hermes.stdout.txt" \
  2> "$OUTPUT_DIR/logs/hermes.stderr.txt"

# 인라인 프롬프트 (짧은 테스트용)
hermes chat -Q --source tool -s baoyu-infographic -t image_gen,file -q "PROMPT"
```

> **[FACT]** `-Q` = 비대화형(non-interactive) 모드. `--source tool` = 자동화 소스 마커.
> `-s` 는 `--skills` 의 단축 플래그. (출처: Hermes 직접 자문 2026-05-11)

### 단순 oneshot (탑레벨 `-z` 플래그)

```bash
# hermes --help에서 확인된 탑레벨 플래그 (-z)
hermes -z "PROMPT" -t image_gen,file --skills baoyu-infographic --accept-hooks --yolo

# 모델 오버라이드
hermes -z "PROMPT" -m gpt-5.5 --provider codex
```

> **[ESTIMATE]** `-z` 플래그는 탑레벨 help에 표시되나, 자동화 안정성은 `hermes chat -Q` 형태가 더 권장됨 (Hermes 자문 기반). quote 처리가 복잡한 멀티라인 프롬프트는 반드시 프롬프트 파일 방식 사용.

> **[UNVERIFIED]** 이미지 생성 모델(gpt-image-2) 개별 지정 플래그는 검증 중. 현재 환경은 `Provider: OpenAI Codex / Model: gpt-5.5`이며 image_gen 툴셋은 Codex 연결을 통해 gpt-image-2를 호출하는 것으로 추정됨.

---

## 빌트인 툴셋 (검증완료/2026-05-11, `hermes tools list` 기준)

| 툴셋 | 아이콘 | 상태 | 설명 |
|------|--------|------|------|
| `image_gen` | 🎨 | ✓ enabled | 이미지 생성 (baoyu-infographic 등과 연동) |
| `vision` | 👁️ | ✓ enabled | 이미지 분석 / Vision |
| `file` | 📁 | ✓ enabled | 파일 읽기/쓰기 (manifest 저장 핵심) |
| `web` | 🔍 | ✓ enabled | 웹 검색 & 스크래핑 |
| `browser` | 🌐 | ✓ enabled | 브라우저 자동화 |
| `terminal` | 💻 | ✓ enabled | 터미널 & 프로세스 |
| `code_execution` | ⚡ | ✓ enabled | 코드 실행 |
| `tts` | 🔊 | ✓ enabled | 텍스트 음성 변환 |
| `computer_use` | 🖱️ | ✓ enabled | macOS Computer Use |
| `video` | 🎬 | ✗ disabled | 비디오 분석 |
| `moa` | 🧠 | ✗ disabled | Mixture of Agents |

`-t` 플래그로 여러 툴셋 활성화: `-t image_gen,file,web`

---

## 이미지 관련 추천 스킬 카탈로그 (검증완료/2026-05-11, `hermes skills list` 기준)

| 스킬 | 카테고리 | 설명 |
|------|----------|------|
| `baoyu-infographic` | creative | 21 layouts × 21 styles 인포그래픽 생성기 |
| `kie-image-generator` | openclaw-imports | 13+ 모델 선택, 가격 추적, 고품질 이미지 |
| `image-prompt-guide` | openclaw-imports | 이미지 프롬프트 구조 가이드 |
| `architecture-diagram` | creative | 아키텍처 다이어그램 자동 생성 |
| `excalidraw` | creative | Excalidraw 스타일 다이어그램 |
| `claude-design` | creative | Claude 스타일 디자인 |

---

## baoyu-infographic 레이아웃 × 스타일 (출처: `~/.hermes/skills/creative/baoyu-infographic/SKILL.md` [FACT])

### 레이아웃 21종
| 레이아웃 | 용도 |
|----------|------|
| `linear-progression` | 타임라인, 프로세스, 튜토리얼 |
| `binary-comparison` | A vs B, 비교/대조 |
| `comparison-matrix` | 다중 요소 비교 |
| `hierarchical-layers` | 피라미드, 우선순위 |
| `tree-branching` | 카테고리, 분류 |
| `hub-spoke` | 중심 개념 + 관련 항목 |
| `structural-breakdown` | 분해도, 단면도 |
| `bento-grid` | 다중 주제 개요 (기본값) |
| `iceberg` | 표면 vs 숨겨진 측면 |
| `bridge` | 문제-해결 구조 |
| `funnel` | 전환, 필터링 |
| `isometric-map` | 공간 관계 |
| `dashboard` | 메트릭, KPI |
| `periodic-table` | 카테고리 컬렉션 |
| `comic-strip` | 내러티브, 시퀀스 |
| `story-mountain` | 플롯 구조 |
| `jigsaw` | 상호 연결 구성 요소 |
| `venn-diagram` | 겹치는 개념 |
| `winding-roadmap` | 여정, 마일스톤 |
| `circular-flow` | 사이클, 반복 프로세스 |
| `dense-modules` | 고밀도 데이터 가이드 |

### 스타일 21종
| 스타일 | 설명 |
|--------|------|
| `craft-handmade` | 손으로 그린 종이 공예 (기본값) |
| `technical-schematic` | 청사진, 엔지니어링 도면 |
| `hand-drawn-edu` | 마카롱 파스텔, 손그림, 교육용 |
| `corporate-memphis` | 평면 벡터, 선명한 색상 |
| `bold-graphic` | 코믹 스타일, 하프톤 |
| `ikea-manual` | 미니멀 선 아트 |
| `dashboard` | 회색조 인터페이스 목업 |
| `cyberpunk-neon` | 네온 글로우, 미래적 |
| `kawaii` | 일본 귀여운 스타일 |
| `chalkboard` | 칠판 위 분필 |
| `storybook-watercolor` | 부드러운 수채화 |
| `aged-academia` | 빈티지 과학, 세피아 |
| `origami` | 접힌 종이, 기하학 |
| `pixel-art` | 레트로 8비트 |
| `subway-map` | 지하철 노선도 |
| `knolling` | 정리된 플랫레이 |
| `lego-brick` | 레고 블록 |
| `pop-laboratory` | 청사진 격자, 좌표 마커 |
| `morandi-journal` | 손그림 낙서, 따뜻한 색조 |
| `retro-pop-grid` | 1970년대 레트로 팝 |
| `claymation` | 3D 클레이 피규어 |

---

## 한국어 블로그 권장 조합 ([출처: Hermes 직접 자문 2026-05-11])

| 섹션 유형 | 레이아웃 | 스타일 | 비고 |
|-----------|----------|--------|------|
| 기술 설명/아키텍처 | `bento-grid` | `technical-schematic` | 기술 독자 기본값 |
| 초보자 개념 설명 | `hub-spoke` | `hand-drawn-edu` | beginner audience |
| 단계별 튜토리얼 | `linear-progression` | `ikea-manual` | Step N 구조 |
| 초보 튜토리얼 | `linear-progression` | `hand-drawn-edu` | beginner audience |
| 비교/A vs B | `binary-comparison` | `corporate-memphis` | 비교 섹션 |
| 체크리스트/Q&A | `dashboard` | `corporate-memphis` | 요약 섹션 |
| 인트로/결론/썸네일 | `bento-grid` | `bold-graphic` | 시각적 임팩트 |
| 내부 구조/원리 | `structural-breakdown` | `technical-schematic` | 아키텍처 설명 |

---

## JSON I/O 계약 (블로그 이미지 자동화용)

### 입력 스키마 (`cardnews-input.json`)
```json
{
  "article_id": "20260511-example",
  "output_dir": "/path/to/runs/<run-id>/images",
  "default_model": "codex/gpt-image-2",
  "aspect_ratio": "16:9",
  "items": [
    {
      "paragraph_id": "section-01",
      "heading": "섹션 제목",
      "text": "섹션 핵심 내용 2~3문장",
      "layout": "bento-grid",
      "style": "technical-schematic"
    }
  ]
}
```

### 출력 매니페스트 (`cardnews-manifest.json`)
```json
{
  "article_id": "20260511-example",
  "generated_at": "2026-05-11T12:00:00Z",
  "model": "codex/gpt-image-2",
  "items": [
    {
      "paragraph_id": "section-01",
      "heading": "섹션 제목",
      "image_path": "/path/to/images/section-01.png",
      "image_url": null,
      "prompt_path": "/path/to/prompts/section-01.md",
      "layout": "bento-grid",
      "style": "technical-schematic",
      "aspect_ratio": "16:9",
      "alt_ko": "섹션 제목 관련 인포그래픽",
      "cost_usd": null
    }
  ]
}
```

> `image_path` 또는 `image_url` 중 하나가 반드시 존재해야 함. URL이면 다운로드 후 `image_path`에 로컬 경로 기록.

---

## 주의 사항

- **cmux pane 자동화 비추천**: `cmux send`로 Hermes 페인에 메시지 전송 시 멀티라인 텍스트가 Enter로 분리됨. 자동화는 반드시 `hermes -z` oneshot 모드 사용.
- **URL vs 로컬 path**: `image_generate` 툴이 URL 또는 로컬 path를 반환할 수 있음. wrapper 스크립트에서 URL 감지 시 `curl -L -o` 다운로드 처리 필수.
- **Codex 인증**: `hermes status`에서 `Provider: OpenAI Codex` 표시 시 Codex 연결 활성. 인증 만료 시 `hermes login codex` 재실행.
- **한국어 텍스트 렌더링**: gpt-image-2의 한글 인포그래픽 내 텍스트 렌더링 안정성은 실측 필요 [UNVERIFIED]. 깨짐 시 baoyu 스킬 프롬프트에 "minimal text, use English labels only" 추가 또는 `alt_ko`/`figcaption`으로 보완.
- **비용 가드**: 생성 비용은 사용하는 이미지 모델에 따라 다름 [UNVERIFIED, 실측 필요]. `HERMES_BUDGET_USD` 환경변수로 상한 설정 권장.

---

## 트러블슈팅

| 증상 | 확인 방법 |
|------|----------|
| `hermes: command not found` | `~/.local/bin/hermes` 경로 확인, PATH 설정 |
| image_gen 결과 없음 | `hermes tools list`에서 image_gen enabled 확인 |
| `hermes model` 실행 불가 | 대화형 TTY 필요, `hermes -z` oneshot으로 대체 |
| baoyu-infographic 스킬 없음 | `hermes skills list \| grep baoyu` 확인 |
| manifest 미생성 | `--yolo --accept-hooks` 플래그 추가 후 재시도 |
| URL 이미지 반환됨 | wrapper에서 `curl -L -o images/<id>.png <url>` 다운로드 |
