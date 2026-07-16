# 전역 에이전트 규칙 (Global Instruction Rules)

## 새로운 프로젝트 등록 자동화 (New Project Registration)

사용자가 "새로운 프로젝트로 등록해줘"라는 요청을 하면 다음 절차를 자동으로 수행합니다.

1. **프로젝트 폴더 구조 생성:**
   현재 작업 디렉터리 기준 아래의 권장 프로젝트 폴더 구조를 자동 생성합니다.
   * .agents/
   * .agents/rules/
   * .agents/skills/
   * .agents/workflows/
   * .agents/knowledge/ (이 아래에 기본 안내 파일인 README.md를 자동 생성. README.md에는 ADR 작성 템플릿과 관리 정책 명시)
   * 필요시 기본 설정 파일 예시(.agents/config.json) 생성

2. **Git 저장소 초기화 및 .gitignore 생성:**
   * .git 폴더가 존재하지 않거나 초기화되어 있지 않은 경우 git init 자동 실행.
   * .gitignore 파일 자동 생성 (예시 패턴: node_modules/, .venv/, dist/ 등)

3. **전역 프로젝트 히스토리 등록:**
   * C:/Users/hdseo/.gemini/projects.json 파일을 읽어 현재 경로를 프로젝트 목록에 등록.

4. **도구 사용방법 안내 및 가이드 문서 생성:**
   * 루트 폴더에 GUIDE.md를 생성하여 슬래시 명령어 사용법, 아티팩트 활용법 등 정리.

5. **피드백 제공 및 초기화 실행:**
   * 작업 완료 후 결과를 사용자에게 요약 보고.

## 세션 종료 및 아티팩트 아카이브 규정 (Session Cleanup & Artifact Archiving)

사용자가 "오늘은 여기까지.", "세션 종료해줘.", "세션 정리해줘." 등의 세션 종료 명령을 내리거나 새로운 프로젝트 등록 명령 시 세션 종료를 포함할 경우 다음을 수행해야 합니다.

1. **아티팩트 프로젝트 내부 저장**:
   현재 세션에서 작성된 워크스루(Walkthrough), 작업 목록(Task), 구현 계획서(Implementation Plan) 등 임시 아티팩트들의 내용을 모두 취합하여, 프로젝트 내부 폴더인 .agents/knowledge/ 경로 하위에 영구적인 마크다운 문서(예: Session_Summary_[날짜].md 또는 ADR_xxx.md)로 복사하여 저장합니다.
2. **Git 기록 및 관리**:
   저장된 문서가 소스코드와 함께 버전 관리되도록 git add 하고, 의미 있는 커밋 메시지(예: "docs: 세션 아티팩트 및 작업 진행 내역 백업")로 git commit을 자동 수행하거나 사용자에게 제안합니다.
3. **세션 종료 안내**:
   위 작업이 모두 완료되었음을 요약 보고하고 대화 세션을 깔끔하게 종료합니다.

## Knowledge Base Management (Obsidian Integration)
When generating or updating any markdown files in the `.agents/knowledge/` directory, always ensure Obsidian-compatible wiki-links and tags are included at the bottom of the file (or inline where appropriate) to maintain a connected knowledge graph.

- **Tags**: Add relevant tags such as `#ADR`, `#Session`, `#Knowledge`, `#Phase1`, etc., at the bottom of the file.
- **Wiki-Links**: Link to related documents using the `[[filename]]` format (without the `.md` extension). 
- **Hub Connections**: Ensure every new document links back to central hub documents like `[[STATUS]]` or `[[README]]`.
