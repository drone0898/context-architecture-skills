# Context Architecture Skills

Karpathy식 작업 원칙과 “컨텍스트 아키텍처” 철학을 결합해, 프로젝트용 AI 컨텍스트 파일을 **크게 한 장으로 몰아넣지 않고 계층적으로 설계·생성**하도록 만든 스킬입니다.

## Quick Start

```bash
# install plugin
/plugin marketplace add drone0898/context-architecture-skills

/plugin install context-architecture-skills@context-architecture-skills

# Usage
/scaffolding-context-files 한국어; CLAUDE.md 생성
```

## 핵심 철학

이 스킬은 다음 두 축을 합칩니다.

### 1) Context architecture > prompt engineering
- 루트 파일 하나에 모든 규칙을 넣지 않습니다.
- 규칙은 **가장 좁은 경계**에 둡니다.
- 루트 파일은 전역 불변식만 담고, 하위 파일은 서브트리 로컬 규칙만 담습니다.
- “모든 팀 / 모든 스택 / 모든 디렉터리” 규칙을 한 파일에 넣는 방식은 피합니다.

### 2) Karpathy-inspired working style
- **Think Before Coding**: 먼저 구조를 읽고, 가정과 경계를 명시합니다.
- **Simplicity First**: 필요한 파일만 만듭니다. 쓸데없이 세 벌을 뿌리지 않습니다.
- **Surgical Changes**: 기존 컨텍스트가 있으면 좋은 부분은 살리고, 필요한 곳만 나눕니다.
- **Goal-Driven Execution**: “예쁜 문서”가 아니라 “도구가 잘 읽는 계층형 컨텍스트”를 목표로 검증합니다.

## 이 스킬이 하는 일

리포지토리를 읽고 다음 파일들을 **필요한 범위만큼만** 설계·생성합니다.

- `CLAUDE.md`
- `AGENTS.md`
- `GEMINI.md`
- 필요 시 `apps/web/CLAUDE.md`, `services/payments/AGENTS.md`, `infra/GEMINI.md` 같은 **nested context files**

또한 다음을 함께 수행합니다.

- 기존 거대한 컨텍스트 파일을 **루트 + 하위 파일 구조로 분해**
- 루트/하위 파일의 역할 분리
- 도구별 로딩 방식 차이(Claude / Codex / Gemini / Copilot)를 반영
- 각 파일이 왜 존재하는지와 어떤 범위를 담당하는지 설명

## 포함된 구성

- `.claude-plugin/`
  - Claude Code 플러그인 패키징용 메타데이터
- `skills/scaffolding-context-files/SKILL.md`
  - 메인 스킬
- `skills/scaffolding-context-files/reference/`
  - 철학 및 도구별 로딩 규칙 참고 자료
- `skills/scaffolding-context-files/templates/`
  - 루트 / 하위 컨텍스트 파일 템플릿
- `skills/scaffolding-context-files/examples/`
  - 모노레포 예시

## 설치 방법

### Claude Code 프로젝트 스킬로 사용
`skills/scaffolding-context-files` 디렉터리를 프로젝트의 `.claude/skills/` 아래로 복사하세요.

예시:
```bash
mkdir -p .claude/skills
cp -R skills/scaffolding-context-files .claude/skills/
```

### GitHub Copilot 프로젝트 스킬로 사용
같은 디렉터리를 `.github/skills/` 또는 `.claude/skills/` 아래로 복사하세요.

예시:
```bash
mkdir -p .github/skills
cp -R skills/scaffolding-context-files .github/skills/
```

### Claude Code 플러그인으로 배포
이 저장소 구조를 그대로 유지한 채 원격 저장소에 올리면 `.claude-plugin/plugin.json` 과 `.claude-plugin/marketplace.json` 을 사용해 플러그인으로도 패키징할 수 있습니다.

## 사용 예시

Claude Code / Copilot 등에서:

```text
/scaffolding-context-files monorepo; create CLAUDE.md, AGENTS.md, GEMINI.md for root and only the subtrees that actually need local rules
```

또는

```text
/scaffolding-context-files split our bloated root CLAUDE.md into root + apps/web + apps/api + infra
```

## 이 스킬의 기본 정책

- **기본은 최소 생성**입니다.
- 팀이 실제로 쓰는 도구가 분명하면 그 도구용 파일만 생성합니다.
- 사용자가 명시적으로 `CLAUDE.md`, `AGENTS.md`, `GEMINI.md` 전부를 원할 때만 모두 만듭니다.
- 하위 디렉터리에 로컬 규칙이 없으면 nested file을 만들지 않습니다.

## 가장 중요한 규칙

> **모든 규칙은, 그 규칙이 실제로 필요한 가장 좁은 경계에 둔다.**

루트에 둘지 하위에 둘지 애매하면, 기본값은 **하위로 내리는 것**입니다.
