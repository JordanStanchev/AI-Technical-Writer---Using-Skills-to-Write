# AI Technical Writer — Using Skills to Write

**JPDocu School of Technical Writing | www.jpdocu.com**
*Instructor: Jordan Stanchev*

---

## About this course

This course teaches you how to use reusable AI skills to dramatically speed up your documentation workflow. Instead of prompting AI from scratch every time, you build a set of skills — saved, version-controlled instruction sets — and run them across your documentation repository.

By the end of this course, you will have processed a set of developer-written drafts into reviewed, approved technical documentation using a three-step AI pipeline.

**Prerequisite:** [Getting Started with AI as a Technical Writer](https://www.udemy.com/course/ai-technical-writing-how-to-write-documentation-using-ai/?referralCode=6736B9D480D567B5B371)

---

## How to use this repository

### Step 1 — Fork this repository

1. Click **Fork** at the top right of this page
2. Click **Create fork**
3. You now have your own working copy at `github.com/your-username/AI-Technical-Writer---Using-Skills-to-Write`

You will do all your work in your fork. The original repository is the reference — your fork is your workspace.

### Step 2 — Explore the structure

| Folder | What it contains |
|---|---|
| `_skills/` | The three AI skills you will use and study |
| `developer-docs/` | 15 developer-written drafts — your raw material |
| `docs/` | Where your finished documentation will live |
| `style-guide/` | The SecurePath Style Guide — the rules the skills apply |

### Step 3 — You need

- A free account at [claude.ai](https://claude.ai) — or any equivalent AI tool
- A free GitHub account (you already have one if you're reading this)
- Nothing else

---

## The workflow you will learn

```
developer-docs/    →   _skills/style-enforcer     →   style-corrected source
style-corrected    →   _skills/write-the-docs      →   draft TW documents in docs/
draft documents    →   _skills/content-auditor     →   gap report + image placeholders
gap report         →   you (human review)          →   approved, final documentation
```

AI does the heavy lifting. You make every final decision.

---

## Repository structure

```
repo-root/
├── _skills/
│   ├── style-enforcer.md        ← applies the style guide to any document
│   ├── content-auditor.md       ← audits documents for gaps and missing content
│   └── write-the-docs.md        ← writes new TW documents from a source
├── developer-docs/              ← 15 engineer-written drafts for the passkeys feature
├── docs/
│   ├── _templates/              ← concept, task, and reference templates
│   └── passkeys/                ← your finished documentation goes here
└── style-guide/
    └── securepath-style-guide.md
```

---

*© JPDocu Services LTD | JPDocu School of Technical Writing | www.jpdocu.com*
*"Technical writing is easy, after all it's just plain docu."*
