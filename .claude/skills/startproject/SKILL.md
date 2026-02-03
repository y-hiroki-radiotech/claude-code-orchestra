---
name: startproject
description: |
  Start a new project/feature implementation with multi-agent collaboration.
  Includes multi-session review workflow for quality assurance.
metadata:
  short-description: Project kickoff with multi-agent collaboration
---

# Start Project

**マルチエージェント協調でプロジェクトを開始する。**

## Overview

このスキルはGeminiとClaudeを協調させ、プロジェクト開始から実装までをカバーする。

## Workflow

```
Phase 1: Research (Gemini via Subagent)
    ↓
Phase 2: Requirements & Planning (Claude)
    ↓
Phase 3: Design Review (Gemini via Subagent)
    ↓
Phase 4: Task Creation (Claude)
    ↓
Phase 5: CLAUDE.md Update (Claude)
    ↓
[Implementation...]
```

---

## Phase 1: Gemini Research (Background)

**Task tool でサブエージェントを起動し、Gemini でリポジトリ分析。**

```
Task tool parameters:
- subagent_type: "general-purpose"
- run_in_background: true
- prompt: |
    Research for: {feature}

    1. Call Gemini CLI:
       gemini -p "Analyze this repository for: {feature}

       Provide:
       1. Repository structure and architecture
       2. Relevant existing code and patterns
       3. Library recommendations
       4. Technical considerations
       " --include-directories . 2>/dev/null

    2. Save full output to: .claude/docs/research/{feature}.md

    3. Return CONCISE summary (5-7 bullet points)
```

---

## Phase 2: Requirements Gathering (Claude)

**ユーザーに質問して要件を明確化。**

Ask in Japanese:

1. **目的**: 何を達成したいですか？
2. **スコープ**: 含めるもの・除外するものは？
3. **技術的要件**: 特定のライブラリ、制約は？
4. **成功基準**: 完了の判断基準は？

**Draft implementation plan based on Gemini research + user answers.**

---

## Phase 3: Gemini Design Review (Background)

**Task tool でサブエージェントを起動し、Gemini で計画レビュー。**

```
Task tool parameters:
- subagent_type: "general-purpose"
- run_in_background: true
- prompt: |
    Review plan for: {feature}

    Draft plan: {plan from Phase 2}

    1. Call Gemini CLI:
       gemini -p "Review this implementation plan:
       {plan}

       Analyze:
       1. Approach assessment
       2. Risk analysis
       3. Implementation order
       4. Improvements
       " 2>/dev/null

    2. Return CONCISE summary:
       - Top 3-5 recommendations
       - Key risks
       - Suggested order
```

---

## Phase 4: Task Creation (Claude)

**サブエージェントの要約を統合し、タスクリストを作成。**

Use TodoWrite to create tasks:

```python
{
    "content": "Implement {specific feature}",
    "activeForm": "Implementing {specific feature}",
    "status": "pending"
}
```

---

## Phase 5: CLAUDE.md Update (IMPORTANT)

**プロジェクト固有の情報を CLAUDE.md に追記する。**

Add to CLAUDE.md:

```markdown
---

## Current Project: {feature}

### Context
- Goal: {1-2 sentences}
- Key files: {list}
- Dependencies: {list}

### Decisions
- {Decision 1}: {rationale}
- {Decision 2}: {rationale}

### Notes
- {Important constraints or considerations}
```

**This ensures context persists across sessions.**

---

## User Confirmation

Present final plan to user (in Japanese):

```markdown
## プロジェクト計画: {feature}

### 調査結果 (Gemini)
{Key findings - 3-5 bullet points}

### 設計方針 (Gemini レビュー済み)
{Approach with refinements}

### タスクリスト ({N}個)
{Task list}

### リスクと注意点
{From Gemini analysis}

### 次のステップ
この計画で進めてよろしいですか？

---
この計画で進めてよろしいですか？
```

---

## Output Files

| File | Purpose |
|------|---------|
| `.claude/docs/research/{feature}.md` | Gemini research output |
| `CLAUDE.md` | Updated with project context |
| Task list (internal) | Progress tracking |

---

## Tips

- **All Gemini work through subagents** to preserve main context
- **Update CLAUDE.md** to persist context across sessions
- **Ctrl+T**: Toggle task list visibility
