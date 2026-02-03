# Gemini Delegation Rule

**Gemini CLI is your highly capable specialist with deep reasoning, massive context, and multimodal capabilities.**

## Context Management (CRITICAL)

**コンテキスト消費を意識してGeminiを使う。** Gemini出力は大きくなりがちなので、サブエージェント経由を推奨。

| 状況 | 推奨方法 |
|------|----------|
| 短い質問・短い回答 | 直接呼び出しOK |
| 設計相談 | サブエージェント経由（出力大） |
| デバッグ分析 | サブエージェント経由（出力大） |
| コードベース分析 | サブエージェント経由（出力大） |
| ライブラリ調査 | サブエージェント経由（出力大） |
| マルチモーダル処理 | サブエージェント経由 |

```
┌──────────────────────────────────────────────────────────┐
│  Main Claude Code                                        │
│  → 短い質問なら直接呼び出しOK                             │
│  → 大きな出力が予想されるならサブエージェント経由          │
│                                                          │
│  ┌────────────────────────────────────────────────────┐ │
│  │  Subagent (general-purpose)                         │ │
│  │  → Calls Gemini CLI                                 │ │
│  │  → Saves full output to .claude/docs/research/      │ │
│  │  → Returns key findings only                        │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
```

## About Gemini

Gemini CLI excels at:
- **Deep reasoning** — Design decisions, debugging, trade-off analysis
- **1M token context window** — Analyze entire codebases at once
- **Google Search grounding** — Access latest information
- **Multimodal processing** — Video, audio, PDF analysis

Think of Gemini as your trusted expert who can handle complex reasoning, research, and analysis.

**When you need deep thinking or research → Delegate to subagent → Subagent consults Gemini.**

## When to Consult Gemini

ALWAYS consult Gemini BEFORE:

1. **Design decisions** - How to structure code, which pattern to use
2. **Debugging** - If cause isn't obvious or first fix failed
3. **Implementation planning** - Multi-step tasks, multiple approaches
4. **Trade-off evaluation** - Choosing between options
5. **Pre-implementation research** - Best practices, library comparison
6. **Large codebase analysis** - Repository-wide understanding
7. **Documentation search** - Latest official docs, breaking changes
8. **Multimodal tasks** - Video, audio, PDF content extraction

### Trigger Phrases (User Input)

Consult Gemini when user says:

| Japanese | English |
|----------|---------|
| 「どう設計すべき？」「どう実装する？」 | "How should I design/implement?" |
| 「なぜ動かない？」「原因は？」「エラーが出る」 | "Why doesn't this work?" "Error" |
| 「どちらがいい？」「比較して」「トレードオフは？」 | "Which is better?" "Compare" |
| 「〜を作りたい」「〜を実装して」 | "Build X" "Implement X" |
| 「考えて」「分析して」「深く考えて」 | "Think" "Analyze" "Think deeper" |
| 「調べて」「リサーチして」「調査して」 | "Research" "Investigate" "Look up" |
| 「このPDF/動画/音声を見て」 | "Analyze this PDF/video/audio" |
| 「コードベース全体を理解して」 | "Understand the entire codebase" |
| 「最新のドキュメントを確認して」 | "Check the latest documentation" |
| 「〜について情報を集めて」 | "Gather information about X" |

## When NOT to Consult

Skip Gemini for:

- Simple, straightforward tasks
- Simple file operations (do directly)
- Running tests/linting (do directly)
- Standard operations (git commit, running tests)
- Tasks with clear, single solutions

## How to Consult (via Subagent)

**IMPORTANT: Use subagent to preserve main context.**

### Recommended: Subagent Pattern

Use Task tool with `subagent_type: "general-purpose"`:

```
Task tool parameters:
- subagent_type: "general-purpose"
- run_in_background: true (for parallel work)
- prompt: |
    Research: {topic}

    1. Call Gemini CLI:
       gemini -p "{research question}" 2>/dev/null

    2. Save full output to: .claude/docs/research/{topic}.md

    3. Return CONCISE summary (5-7 bullet points):
       - Key findings
       - Recommended approach
       - Important caveats
```

### Subagent Patterns by Task Type

**Design Decision Pattern:**
```
prompt: |
  Design decision: {topic}

  gemini -p "Design: {topic}. Analyze trade-offs, recommend
  approach, identify risks." 2>/dev/null

  Return CONCISE summary:
  - Recommended approach
  - Key rationale (2-3 points)
  - Important risks/concerns
```

**Debugging Pattern:**
```
prompt: |
  Debug: {issue}

  Context:
  {error logs, code snippet, reproduction steps}

  gemini -p "Debug: Why is this happening? Root cause analysis
  and solution." 2>/dev/null

  Return:
  - Root cause
  - Recommended fix
  - Prevention strategy
```

**Research Pattern:**
```
prompt: |
  Research best practices for {topic}.

  gemini -p "Research: {topic}. Include recommended approaches,
  common pitfalls, and library recommendations." 2>/dev/null

  Save to .claude/docs/research/{topic}.md
  Return 5-7 key bullet points.
```

**Codebase Analysis Pattern:**
```
prompt: |
  Analyze codebase for {purpose}.

  gemini -p "Analyze architecture, key modules, data flow,
  and entry points." --include-directories . 2>/dev/null

  Save to .claude/docs/research/codebase-analysis.md
  Return architecture summary and key insights.
```

**Multimodal Pattern:**
```
prompt: |
  Extract information from {file}.

  gemini -p "{extraction prompt}" < {file_path} 2>/dev/null

  Save to .claude/docs/research/{output}.md
  Return key extracted information.
```

### Step 2: Continue Your Work

While subagent is processing, you can:
- Work on other files
- Run tests
- Spawn another subagent for parallel work

### Step 3: Receive Summary

Subagent returns concise summary. Full output available in `.claude/docs/research/` if needed.

## Gemini CLI Commands Reference

For use within subagents:

```bash
# Research
gemini -p "{question}" 2>/dev/null

# Codebase analysis
gemini -p "{question}" --include-directories . 2>/dev/null

# Multimodal
gemini -p "{prompt}" < /path/to/file.pdf 2>/dev/null

# JSON output
gemini -p "{question}" --output-format json 2>/dev/null
```

**Language protocol:**
1. Ask Gemini in **English**
2. Subagent receives response in **English**
3. Subagent summarizes and saves full output
4. Main receives summary, reports to user in **Japanese**

## Why Subagent Pattern?

- **Context preservation**: Main orchestrator stays lightweight
- **Full capture**: Subagent can save entire Gemini output to file
- **Concise handoff**: Main only receives key findings
- **Parallel work**: Background subagents enable concurrent research

**Use Gemini (via subagent) for deep reasoning and research, Claude for orchestration and implementation.**
