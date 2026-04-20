# AI Prompts & Guidelines

This directory contains structured prompts and guidelines for AI assistants (GitHub Copilot, custom agents, etc.) working on projects in this workspace.

---

## 📁 Available Prompt Sets

### Join Project - Vanilla JavaScript MPA

**Location:** [join/](join/)

Comprehensive guidelines for the Join task management application:

- Vanilla JavaScript (ES6+)
- Multi-Page Application (MPA)
- Firebase (Firestore, Auth)
- BEM methodology
- Mobile-first responsive design

**Quick Start:** See [join/README.md](join/README.md)

---

## 🎯 Using These Prompts

### With GitHub Copilot

1. **Open the relevant prompt file** before writing code
2. **Reference in comments**: `// See: .github/prompts/join/01-coding-standards.md`
3. **Ask specific questions**: Include the prompt file name in your question

### With Custom AI Agents

Configure agents to reference these prompts for automated code reviews, documentation generation, etc.

**Example:** [.github/agents/code-review/agent.md](../agents/code-review/agent.md)

---

## 📝 Prompt File Naming Convention

```
[number]-[topic-name].md

Examples:
01-coding-standards.md
02-architecture.md
03-page-structure.md
```

Each prompt set includes:

- **Standards & Conventions** - Coding rules, naming, file structure
- **Architecture** - Project structure, patterns, modules
- **Features** - Specific feature implementation guidelines
- **Quality Checklist** - Definition of Done, testing requirements

---

## 🔄 Updating Prompts

When project requirements change:

1. Update the relevant prompt file(s)
2. Update the prompt set's README.md
3. Test with AI assistant to verify understanding
4. Commit with clear message: `docs: Update [topic] prompt for [reason]`

---

## 🤝 Contributing New Prompt Sets

When adding prompts for a new project:

1. **Create a new folder** in this directory (e.g., `project-name/`)
2. **Add a README.md** explaining the prompt structure
3. **Structure prompts by topic** (coding standards, architecture, features, etc.)
4. **Use consistent numbering** (01-, 02-, etc.)
5. **Include code examples** (✅ Correct and ❌ Wrong patterns)
6. **Add checklists** for verification

---

**Last Updated:** February 2026
