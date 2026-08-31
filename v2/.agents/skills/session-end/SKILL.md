---
name: session-end
description: Use this skill when the user indicates session termination (e.g., "오늘은 여기까지", "세션 종료", "세션 정리").
---

# Session Termination Procedure

1. **Extract & Summarize**:
   - 세션 중 논의된 아키텍처 결정(ADR), 변경 파일, 남은 작업을 요약.
   - `.agents/knowledge/Session_Summary_[YYYYMMDD].md` 생성 (하단 `#Session` 태그 및 `[[GOAL]]` 링크 필수).
2. **Git Commit**:
   - 변경 사항 확인 후 안전하게 스테이징 및 커밋:
     `git add .agents/knowledge/` (및 세션 중 수정된 코드)
     `git commit -m "docs: 세션 백업 [YYYYMMDD]"`
3. **Report**:
   - `✅ Session 종료 및 커밋 완료` 1줄 출력 후 대화 마감.
