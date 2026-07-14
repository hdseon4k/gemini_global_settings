# Gemini Global Settings Repository

## Overview
본 저장소는 Gemini IDE 에이전트의 전역 설정(Global Configuration), 커스텀 규칙(Rules), 그리고 확장 기능(Skills & Plugins)을 관리하기 위한 형상 관리(Configuration Management) 저장소입니다. 다중 워크스테이션 환경에서 에이전트의 컨텍스트와 자동화 정책을 동일하게 동기화하고 유지하는 것을 목적으로 합니다.

## Repository Structure
- **AGENTS.md**: 에이전트의 전역 행동 수칙 및 생명주기 관리 프로토콜(예: 프로젝트 초기화, 세션 아티팩트 아카이빙 트리거)을 정의하는 핵심 설정 파일입니다.
- **GEMINI프로젝트_관리방법.md**: 작업자(Human)를 위한 표준 운영 절차(SOP) 및 프롬프트 명령어 레퍼런스 가이드입니다.
- **plugins/**: 에이전트의 기능을 확장하기 위한 커스텀 플러그인 모듈이 포함됩니다.
- **skills/**: 특정 워크플로우에 종속된 운영 가이드라인 및 커스텀 스킬 구현체가 위치합니다.

## Environment Synchronization Guide
새로운 PC 환경에 본 전역 설정을 배포(Clone)하려면 터미널(CMD/PowerShell)에서 아래의 절차를 수행하십시오.

### 1. 대상 디렉터리 이동
`ash
cd %USERPROFILE%\.gemini
`

### 2. 저장소 복제 (Clone)
기존에 생성된 config 디렉터리가 존재할 경우 백업 후 삭제하고, 본 저장소를 config라는 디렉터리명으로 복제합니다.
`ash
git clone https://github.com/hdseon4k/gemini_global_settings.git config
`

### 3. 환경 적용
복제가 완료되면 Gemini 에이전트가 AGENTS.md 및 플러그인을 자동으로 로드합니다. (경우에 따라 IDE 재시작이 필요할 수 있습니다.)

## Configuration Policy
1. **Rule Centralization (규칙 일원화)**: 에이전트의 자동화 트리거 및 행동 강령은 반드시 AGENTS.md 파일 내에 중앙화되어야 합니다.
2. **Separation of Concerns (관심사의 분리)**: 사용자 안내 목적의 가이드라인 문서에는 에이전트가 직접 파싱하고 의존하는 제약 조건(Rule)을 혼용하지 않습니다.
3. **Local Context Isolation (로컬 컨텍스트 격리)**: 대화 세션에서 발생하는 임시 아티팩트(Walkthrough, Task, Plan)는 본 전역 저장소가 아닌, 개별 소스코드 프로젝트 단위의 .agents/knowledge/ 경로에 독립적으로 보관되어야 합니다.
