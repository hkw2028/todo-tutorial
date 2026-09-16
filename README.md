# Todo Tutorial

[Claude Code Playbook](https://docs.claude-hunt.com) 강의의 실습용 저장소입니다. Next.js 와 shadcn/ui 로 시작하는 작은 Todo 앱을 단계별로 발전시키며 Claude Code 사용법을 익힙니다.

## 관련 링크

- 강의 본문: https://docs.claude-hunt.com
- 수강생 결과물 공유: https://claude-hunt.com

## 주요 기능

- 할 일 추가 시 우선순위(높음/보통/낮음), 마감일, 카테고리(업무/개인/쇼핑)를 함께 지정
- 완료 토글, 삭제, 제목 인라인 편집 (더블클릭으로 편집, 빈 값으로 저장하면 삭제 처리)
- 제목 검색, 상태별 필터(전체/진행중/완료), 카테고리별 필터
- 생성일순 / 이름순 / 마감일순 정렬 (마감일 없는 항목은 항상 뒤로)
- 목록은 `localStorage`에 저장되어 새로고침 후에도 유지 (저장 데이터가 손상된 경우 덮어쓰지 않고 보존)
- 다크 모드는 시스템 설정을 따르며, `d` 키로 전환 (입력 중에는 동작하지 않음)

## 기술 스택

- Next.js 16 (App Router, Turbopack)
- React 19
- Tailwind CSS v4
- shadcn/ui (`radix-mira` 스타일, `taupe` 베이스, Phosphor 아이콘)
- Magic UI 레지스트리(`@magicui`) — 타이틀의 `AuroraText`
- next-themes (다크 모드)
- Vitest + Testing Library (jsdom 환경)
- TypeScript / ESLint / Prettier
- 패키지 매니저: bun

## 시작하기

```bash
bun install
bun dev
```

개발 서버는 기본적으로 [http://localhost:3000](http://localhost:3000) 에서 열립니다.

자주 쓰는 스크립트:

```bash
bun dev            # 개발 서버 실행 (Turbopack)
bun run build      # 프로덕션 빌드
bun run start      # 빌드 결과 실행
bun run lint       # ESLint
bun run typecheck  # tsc --noEmit
bun run format     # Prettier 포맷팅
bun run test       # Vitest 1회 실행
bun run test:watch # Vitest watch 모드
```

## 프로젝트 구조

```
app/              # App Router 진입점 (layout.tsx, page.tsx, globals.css)
components/       # Todo 기능 컴포넌트 + *.test.tsx
components/ui/    # shadcn/ui 컴포넌트 (button, input, card, checkbox, aurora-text)
hooks/use-todos   # 할 일 상태 + localStorage 연동
lib/types.ts      # Todo·우선순위·카테고리·정렬/필터 타입과 메타데이터
lib/todo-utils.ts # 정렬(sortTodos)·검색(matchesSearch) 순수 함수
```

상태는 `useTodos` 훅에 모아두고, 목록 표시용 상태(검색어·필터·정렬)는 `TodoList`의 로컬 상태로만 관리합니다.

## 테스트

`components`, `hooks`, `lib` 에 컴포넌트·훅·유틸 테스트가 함께 들어 있습니다.

```bash
bun run test               # 전체 테스트
bun run test todo-item     # 특정 파일만
```

- 환경: jsdom (`vitest.config.ts`), 테스트마다 DOM 자동 정리 (`vitest.setup.ts`)
- Node의 실험적 Web Storage 전역이 jsdom의 `localStorage`를 가리는 문제를 피하려고 `--no-experimental-webstorage` 로 실행합니다.

## 챕터별 시작 브랜치

각 레슨은 시작 시점의 코드 상태를 브랜치로 제공합니다. 레슨 본문에서 안내하는 브랜치로 전환한 뒤 따라가시면 됩니다. (이 저장소는 현재 `main` 기준이며, 챕터 브랜치가 없다면 강의에서 안내하는 원본 저장소를 확인하세요.)

```shell
git checkout ch02-03
```

## 컴포넌트 추가

shadcn/ui 컴포넌트는 다음과 같이 추가합니다.

```bash
bunx --bun shadcn@latest add button
```

`components/ui` 디렉토리에 컴포넌트가 추가됩니다. Magic UI 컴포넌트는 레지스트리 접두사를 붙여 추가합니다.

```bash
bunx --bun shadcn@latest add @magicui/aurora-text
```

## 컴포넌트 사용

```tsx
import { Button } from "@/components/ui/button";
```

## Claude Code 설정

이 저장소에는 실습용 Claude Code 설정이 포함되어 있습니다.

- `CLAUDE.md` — 아키텍처·워크플로우·답변 규칙
- `.claude/skills/` — `shadcn`, `commit`, `ui-bug-report` 스킬
- `.claude/agents/test-planner.md` — 테스트 공백을 찾아 계획을 세우는 서브에이전트
- `.claude/hooks/lint.sh` — 파일을 수정할 때마다 ESLint `--fix` 를 실행하는 PostToolUse 훅
- `.claude/launch.json` — `bun run dev` 실행 설정
- `.github/workflows/` — 이슈·PR에서 `@claude` 멘션에 응답하고, PR이 열리면 자동으로 코드 리뷰하는 워크플로우

## Contributors

- 토이크레인 - Frontend Developer
