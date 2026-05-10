# hermes-claude-skill

Claude Code skill plugin for [Hermes Agent](https://hermes.manaflow.ai/) CLI — image generation, baoyu-infographic (21 layouts × 21 styles), and blog automation.

## Installation

```bash
# Claude Code 플러그인으로 설치
claude plugin add https://github.com/kimdh/hermes-claude-skill
```

설치 후 Claude Code에서 Hermes 관련 작업 시 자동으로 스킬이 로드됩니다.

## Requirements

- macOS 14+
- Hermes Agent CLI (`~/.local/bin/hermes`)
- Hermes 인증 완료 (`hermes status` 로 확인)
- Codex provider 연결 (`hermes status` 에서 Provider: OpenAI Codex 확인)

## What's Included

### `skills/hermes/SKILL.md`

Claude Code가 Hermes를 자동 호출하기 위한 검증된 레퍼런스:

- **핵심 CLI 패턴**: oneshot `-z` 모드, 툴셋 지정 `-t`, 스킬 지정 `--skills`, 헤드리스 `--accept-hooks --yolo`
- **빌트인 툴셋 표** (`hermes tools list` 기반, 2026-05-11 검증)
- **baoyu-infographic 21×21**: 레이아웃 × 스타일 전체 참조
- **한국어 블로그 권장 조합**: 섹션 유형별 최적 layout + style
- **JSON I/O 계약**: 블로그 이미지 자동화용 input/manifest 스키마
- **트러블슈팅**: 주요 오류 및 해결 방법

## Usage in harness-blog-project-v2

이 스킬은 `harness-blog-project-v2`의 ILLUSTRATE 단계에서 사용됩니다:

```
PUBLISH → ★ ILLUSTRATE (hermes-cardnews.sh) → CRITIC_PASS
```

```bash
# --with-images 플래그로 ILLUSTRATE 단계 활성화
# (CLAUDE.md Step 7.5 참조)
```

## Versioning

Hermes 업데이트 시 `SKILL.md`의 검증 마커(`[검증완료/YYYY-MM-DD]`)를 갱신하고 CHANGELOG.md에 기록합니다.

## License

MIT
