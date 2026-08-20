# Simply KISS

[![skills.sh](https://skills.sh/b/ioma8/simply-kiss)](https://skills.sh/ioma8/simply-kiss)

A concise skill that helps LLM coding agents write simple, obvious, maintainable code with cohesive modules, explicit behavior, local changes, and refactor-safe tests.

It works with any coding agent that supports [Agent Skills](https://www.skills.sh/docs), including:

- Claude Code
- Cursor
- Codex
- GitHub Copilot
- Windsurf

## Install

Install interactively with the Skills CLI:

```bash
npx skills add ioma8/simply-kiss
```

Or install globally for a specific agent without prompts (Codex example):

```bash
npx skills add ioma8/simply-kiss -g -a codex -y
```

In Codex, you can also ask:

```text
Use $skill-installer to install https://github.com/ioma8/simply-kiss.
```

## Use

Invocation syntax varies by agent. These examples use `/simply-kiss`.

### Review uncommitted changes

```text
/simply-kiss Analyze all uncommitted changes. List every finding. Do not patch anything yet.
```

### Review the whole repository

```text
/simply-kiss Analyze the whole repository. List every finding. Do not patch anything yet.
```

### Simplify a module

```text
/simply-kiss Review this module for unnecessary complexity. List the proposed cleanups before changing anything.
```

### Implement a feature

```text
/simply-kiss Implement this feature with the simplest maintainable design.
```
