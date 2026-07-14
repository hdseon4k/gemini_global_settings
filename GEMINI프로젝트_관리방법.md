# Antigravity 프로젝트별 설정 및 전용 스킬 관리 가이드

윈도우즈용 **Google Antigravity (AGY)** 및 **Antigravity IDE** 환경에서 전역(Global) 설정과 프로젝트(Workspace) 설정을 분리하여 관리할 때의 계층 구조와 발생할 수 있는 주요 쟁점 및 해결 방안에 대한 가이드라인입니다.

---

## 1. 설정 및 커스텀 요소의 계층 구조 (Precedence)

Antigravity는 프로젝트별 독립성을 보장하기 위해 **"프로젝트 수준 설정이 전역 설정을 덮어쓰거나 우선한다"**는 기본 원칙을 따릅니다.

| 구분 | 전역(Global) 경로 | 프로젝트(Workspace) 경로 | 우선순위 및 결합 방식 |
| :--- | :--- | :--- | :--- |
| **기본 설정 (Settings)** | `C:/Users/hdseo/.gemini/antigravity-cli/settings.json`<br>(또는 [settings.json](file:///C:/Users/hdseo/.gemini/antigravity-cli/settings.json)) | `<workspace-root>/.agents/config.json` | **덮어쓰기 (Override)**: 프로젝트 수준 설정이 전역 설정을 덮어씁니다. |
| **규칙 (Rules)** | `C:/Users/hdseo/.gemini/GEMINI.md`<br>(또는 [GEMINI.md](file:///C:/Users/hdseo/.gemini/GEMINI.md)) | `<workspace-root>/.agents/rules/` | **병합 및 특수화 (Co-existence & Specialization)**: 기본적으로 전역 규칙과 프로젝트 규칙이 함께 로드되지만, 구체적인 프로젝트 규칙이 더 높은 우선순위로 작동합니다. |
| **스킬 (Skills)** | `C:/Users/hdseo/.gemini/config/skills/` | `<workspace-root>/.agents/skills/` | **가리기 (Shadowing)**: 이름이 동일한 스킬이 양쪽에 존재할 경우, 프로젝트 전용 스킬이 전역 스킬을 완전히 무시하고 우선 사용됩니다. |

---

## 2. 구조 설계 시 발생 가능한 문제점 및 주의사항 (Conflict Points)

### ① 폴더 명명 규칙의 레거시 호환성 문제
* **이슈:** 과거 Antigravity 버전에서는 프로젝트 루트 폴더에 `.agent/`를 사용했으나, 최신 규격은 **`.agents/`**(복수형)를 권장합니다.
* **해결:** 프로젝트별 전용 스킬과 청사진을 관리할 때는 반드시 복수형인 `.agents/` 경로에 맞추어 구성해야 IDE와 CLI가 설정을 올바르게 감지합니다.
  * 스킬 경로: `<workspace-root>/.agents/skills/`
  * 규칙 경로: `<workspace-root>/.agents/rules/`

### ② 전역 규칙(Rules)과 프로젝트 규칙의 충돌
* **이슈:** 전역 규칙([GEMINI.md](file:///C:/Users/hdseo/.gemini/GEMINI.md))에 특정 코딩 스타일이나 스택을 "강제"해 두면, 다른 기술 스택을 사용하는 프로젝트 빌드 시 충돌이 일어날 수 있습니다.
* **해결:** 전역 규칙에는 "일반적인 커뮤니케이션 스타일", "기본적인 보안 요구사항" 등 모든 프로젝트에 공통 적용할 규칙만 최소한으로 작성하고, 기술 스택(예: React, Go, Spring 등)이나 프로젝트 아키텍처 가이드라인은 각 프로젝트의 `.agents/rules/` 디렉터리에 별도 마크다운 파일로 세분화하여 배치하십시오.

### ③ 스킬 이름 충돌 (Shadowing)
* **이슈:** 전역 스킬 폴더와 프로젝트 스킬 폴더에 동일한 `name` 속성(YAML frontmatter 기준)을 가진 스킬이 정의되어 있다면, 전역 스킬은 해당 프로젝트 안에서 사용할 수 없게 가려집니다.
* **해결:** 공용으로 쓰는 전역 스킬과 프로젝트 전용 스킬의 이름이 중복되지 않도록 접두사(Prefix) 등을 사용해 구분하는 것이 안전합니다. (예: 전역 스킬 `db-helper` vs 프로젝트 전용 스킬 `projA-db-helper`)

### ④ 에이전트 권한 설정 계층 (Agent Permissions)
* **이슈:** 파일 읽기/쓰기, 명령어 실행 등 보안과 직결된 권한은 계층 구조상 **Deny(거부) > Ask(확인) > Allow(허용)** 규칙을 따릅니다.
* **해결:** 프로젝트 설정에서 특정 디렉터리에 허용(`Allow`) 권한을 부여했더라도, 전역 설정이나 보안 정책에서 거부(`Deny`)되어 있으면 최종적으로 거부되므로 보안 권한 설정을 다룰 때는 전역 설정을 엄격하게 유지하고 프로젝트별로 필요한 영역만 좁게 샌드박스를 푸는 설계가 필요합니다.

---

## 3. 권장하는 프로젝트 관리 폴더 구조

```text
C:/Users/hdseo/projects/my-project-A/
├── .agents/
│   ├── config.json          # 프로젝트 전용 에이전트 설정
│   ├── rules/
│   │   └── coding_style.md  # 프로젝트 A의 코딩 컨벤션
│   ├── skills/
│   │   └── my_custom_skill/ # 프로젝트 A 전용 커스텀 스킬
│   │       ├── SKILL.md
│   │       └── scripts/
│   └── workflows/           # 프로젝트 A 전용 워크플로우
└── src/
```

위 구조처럼 `.agents/` 디렉터리를 프로젝트 루트마다 구성하면, Antigravity IDE를 통해 해당 폴더를 열었을 때 자동으로 전용 설정과 스킬이 로드되므로 계층 구조상의 혼선 없이 효율적으로 운영할 수 있습니다.
# GEMINI 에이전트 프로젝트 관리 명령어 사용 예제

## 1. 프로젝트 초기화 및 등록
새로운 폴더에서 작업을 시작하거나 현재 폴더를 정식 프로젝트로 등록할 때 사용합니다.

**명령어 입력:**
> "새로운 프로젝트로 등록해줘."

**수행되는 작업:**
- .agents/ 폴더 구조 자동 생성
- Git 저장소 초기화 및 .gitignore 생성
- 전역 프로젝트 히스토리에 현재 경로 등록
- 도구 사용 가이드 문서(GUIDE.md) 자동 생성

## 2. 작업 세션 종료 및 내용 영구 보존
하루의 작업을 마치거나 특정 이슈의 구현이 끝나서, 임시로 작성된 계획서, 워크스루, 작업 목록 등을 프로젝트에 영구 보존하고 싶을 때 사용합니다.

**명령어 입력:**
> "오늘은 여기까지."
> "세션 종료해줘."
> "세션 정리해줘."

**수행되는 작업:**
- 현재 대화 세션에 흩어진 모든 중요 아티팩트를 추출.
- 프로젝트의 .agents/knowledge/ 하위에 영구 보존용 마크다운 파일(예: Session_Summary.md)로 복사.
- 소스 코드와 함께 관리되도록 Git에 자동 dd 및 commit 수행.

### 🚨 [중요] 세션 종료 및 백업 시 주의사항 (Best Practice)
세션 종료 명령("오늘은 여기까지", "세션 정리해줘")은 **명령을 입력한 바로 그 대화창(현재 활성화된 탭)**의 아티팩트만 추출하여 프로젝트로 백업합니다.
다른 탭이나 과거에 나눴던 대화 기록까지 알아서 취합하지 않으므로, **하나의 기능 단위나 버그 수정이 끝날 때마다 해당 대화 탭에서 즉시 종료 명령을 내려 개별적으로 백업**하는 것이 가장 완벽하고 안전한 지식 관리 방법입니다.
