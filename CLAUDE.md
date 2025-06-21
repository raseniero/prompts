# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is an AI assistant prompts and rules repository that provides configurations, commands, and workflows for various AI coding assistants (Claude, Cursor, Windsurf). It serves as a centralized collection of development patterns and task management workflows.

## Key Development Tool: Task Master

This project uses Task Master for task-driven development. Install globally with:
```bash
npm install -g task-master-ai
```

### Common Task Master Commands

**Project initialization:**
- `task-master init` - Initialize a new project
- `task-master parse-prd --input=<prd-file.txt>` - Parse a PRD file to generate tasks

**Task management:**
- `task-master list` - View all tasks
- `task-master next` - Find the next task to work on
- `task-master show <id>` - Show details of a specific task
- `task-master set-status --id=<id> --status=done` - Update task status

**Task analysis and expansion:**
- `task-master analyze-complexity --research` - Analyze task complexity
- `task-master expand --id=<id> --force --research` - Expand complex tasks into subtasks
- `task-master add-task --prompt="..." --research` - Add a new task

**Dependencies and validation:**
- `task-master add-dependency --id=<id> --depends-on=<id>` - Add task dependencies
- `task-master validate-dependencies` - Check dependency integrity
- `task-master fix-dependencies` - Auto-fix dependency issues

## Repository Structure

```
/prompts/
├── ai_docs/           # General AI development documentation
├── claude/            # Claude-specific commands and prompts
│   └── commands/      # Reusable command templates
├── cursor/            # Cursor IDE rules and configurations
│   └── rules/         # Development workflows
├── windsurf/          # Windsurf assistant rules
│   └── rules/         # Development strategies
└── .taskmaster/       # Task Master configuration
```

## Claude-Specific Commands

Located in `/claude/commands/`:
- `rc-create-prd.md` - Guide for creating Product Requirements Documents
- `rc-generate-tasks.md` - Task generation from PRDs
- `rc-process-task-list-coding-changes.md` - Processing task lists for code changes
- `explain-implementation-details.md` - Explaining implementation approaches

## Development Workflow

1. **Product Requirements Document (PRD) Creation**
   - Use the `rc-create-prd.md` template to gather requirements
   - Ask clarifying questions before generating PRD
   - Save PRDs as `prd-[feature-name].md` in `/tasks/`

2. **Task Generation and Management**
   - Parse PRDs into tasks using Task Master
   - Use task-driven development with clear subtask breakdown
   - Follow the iterative development process for each task

3. **Implementation Process**
   - Understand task requirements
   - Explore and plan implementation
   - Log plans and progress
   - Implement with test-driven development
   - Review and update development patterns
   - Mark tasks complete when verified

## Configuration Files

- `.windsurfrules` - Comprehensive Windsurf AI assistant rules
- `.roomodes` - Custom AI modes configuration
- `.taskmaster/config.json` - Task Master configuration (managed via `task-master models --setup`)
- `.env` - API keys for CLI usage

## Development Patterns

This repository emphasizes:
- Task-driven development with AI assistance
- Test-driven development (TDD) principles
- Structured branching and version control
- Continuous improvement of AI rules and patterns
- Clear separation of concerns between different AI tools

## Notes

- This is a prompts/rules repository, not a traditional code project
- No build, test, or lint commands are needed
- Focus is on managing AI assistant behaviors and development workflows
- Use Task Master for all task management operations