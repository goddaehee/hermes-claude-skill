# Changelog

## [1.0.0] - 2026-05-11

### Added (직접 실행 검증 기반)
- 초기 릴리스: Hermes Agent CLI 활용 가이드 for Claude Code
- 핵심 oneshot 플래그 문서화 (`-z`, `-m`, `--provider`, `-t`, `--skills`, `--accept-hooks`, `--yolo`)
- 빌트인 툴셋 표 (`hermes tools list` 출력 기반, 2026-05-11 검증)
- baoyu-infographic 21 layouts × 21 styles 참조 표
- 한국어 블로그 권장 레이아웃 × 스타일 조합 (Hermes 자문 기반)
- JSON I/O 계약 스키마 (cardnews-input.json + cardnews-manifest.json)
- 트러블슈팅 가이드

### Verified By
- `hermes --help` 직접 실행 (2026-05-11)
- `hermes tools list` 직접 실행 (2026-05-11)
- `hermes status` 직접 실행: Provider=OpenAI Codex, Model=gpt-5.5 (2026-05-11)
- `hermes skills list` 직접 실행: baoyu-infographic enabled (2026-05-11)
- cmux `surface:14` Hermes 페인 직접 자문 (2026-05-11)
