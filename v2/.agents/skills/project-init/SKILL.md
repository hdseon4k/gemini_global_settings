---
name: project-init
description: Use this skill when the user asks to register or initialize a new project (e.g., "새로운 프로젝트로 등록해줘").
---

# New Project Initialization Procedure

1. **Check Requirements**:
   - 명령에 구체적 프로젝트 목표/스택이 없으면 `목표/스택 입력 요망:`으로 단답형 요구 후 대기.
2. **Directory Scaffolding**:
   - Create directories: `.agents/rules/`, `.agents/skills/`, `.agents/knowledge/`.
3. **Obsidian Hub Creation**:
   - Create `.agents/knowledge/GOAL.md` (프로젝트 개요, 목표, 기술스택 포함, `[[README]]` 링크 포함).
4. **Git Setup**:
   - `.git` 없으면 `git init` 수행.
   - `.gitignore` 생성 (`.env`, `node_modules/`, `.venv/`, `dist/`).
5. **Project Guide**:
   - 루트에 `GUIDE.md` 생성.
6. **Report**:
   - `✅ Init 완료` 및 생성된 핵심 파일 링크 3줄 이내 보고.
