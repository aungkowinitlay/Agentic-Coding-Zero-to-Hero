# Agentic Coding Zero to Hero 


```text
- Mobile stack ကို PWA-first အဖြစ်ဆုံးဖြတ်ထားခြင်း
- Hooks ကို dedicated safety/automation chapter အဖြစ်ထည့်ခြင်း
- Plan Mode ကို named feature အဖြစ်သင်ခြင်း
- Checkpoints / Rewind ကို Git Recovery နဲ့ပေါင်းခြင်း
- Plugins & Marketplace ထည့်ခြင်း
- Claude Agent SDK နဲ့ mini harness prototype ထည့်ခြင်း
- MCP Apps ထည့်ခြင်း
- Accessibility ကို frontend foundation ကတည်းက ထည့်ခြင်း
- Anti-spam / rate limiting ထည့်ခြင်း
- Legacy codebase onboarding ထည့်ခြင်း
- Deployment cost visibility ထည့်ခြင်း
```

Claude Code ကို agentic coding environment အဖြစ်သုံးတဲ့အခါ file တွေဖတ်ခြင်း၊ command run ခြင်း၊ code ပြင်ခြင်း၊ plan/implement loop လုပ်ခြင်းတို့ပါဝင်ပြီး official best-practices docs မှာ hooks, skills, custom subagents, plugins, MCP servers, checkpoints, parallel sessions စတဲ့ workflow တွေကိုလည်း ဖော်ပြထားပါတယ်။ ဒါကြောင့် ဒီ course က “prompt ရေးနည်း” တစ်ခုတည်းမဟုတ်ဘဲ **agentic software development system တည်ဆောက်နည်း** ကို သင်မယ့် course ဖြစ်ပါတယ်။ ([Claude][1])

---

# Course Goal

ဒီ course ရဲ့ရည်ရွယ်ချက်က **ကွန်ပျူတာသုံးတတ်ပေမယ့် coding မသုံးဖူးသေးသူ** ကို—

```text
AI ကို code မေးတတ်သူ
→ AI coding agent နဲ့ project ဆောက်တတ်သူ
→ Frontend / Backend / PWA / Admin project တည်ဆောက်တတ်သူ
→ SPEC, Test, Verify, Audit, Deploy workflow သုံးတတ်သူ
→ Multi-agent workflow + MCP + CI/CD + production readiness နားလည်သူ
```

အထိ တက်လာအောင် သင်ပေးတာပါ။

Capstone Project က—

```text
Event Management Platform

ပါဝင်မယ့်အရာ:
- Public event website
- Backend API
- Admin dashboard
- PWA check-in app
- Offline check-in queue
- SPEC-driven workflow
- Test / Verify / Audit
- MCP tools
- MCP Apps optional dashboard
- GitHub Actions CI/CD
- DigitalOcean App Platform deployment
```

---

# Course Format

Chapter တစ်ခုချင်းစီကို အသေးစိတ်ပြန်ရေးတဲ့အခါ ဒီ template ကို consistent သုံးပါ။

```text
1. Learning Objectives
2. Beginner Explanation
3. Key Concepts
4. Step-by-step Lab
5. Copy-paste Prompts
6. Expected Output
7. Common Errors
8. Debug / Recovery
9. Verification Checklist
10. Mini Assignment
```

---

# Course-Wide Artifacts

Course တစ်ခုလုံးပြီးသွားရင် learner ဆီမှာ ဒီ artifacts တွေရှိရမယ်။

```text
CLAUDE.md
AGENTS.md
SPEC.md
COURSE_DECISIONS.md
FEATURE_BRIEF.md
REPO_MAP.md
TRACEABILITY.md
TASK_SCHEMA.json
VERIFICATION_REPORT.md
AUDIT_REPORT.md
DEPLOYMENT.md
RUNBOOK.md

.claude/
  skills/
  agents/
  settings.json

event-management/
  apps/web
  apps/admin
  apps/checkin-pwa
  services/api
  packages/shared

.github/workflows/
  test.yml
  build.yml
  deploy-staging.yml
  deploy-production.yml
```

---

# Course Decision Record

Course မစရေးခင် ဒီ decisions တွေကို fixed လုပ်ထားမယ်။

## Decision 1 — Mobile Stack

Default mobile check-in app ကို **PWA-first** လုပ်မယ်။

အကြောင်းရင်းက learner တွေက React + Vite ကို သင်ပြီးသားဖြစ်မယ်။ PWA ကိုရွေးရင် mobile check-in app ကို React stack ပေါ်မှာပဲ ဆောက်နိုင်ပြီး service worker, cache, offline storage နဲ့ venue internet မကောင်းတဲ့အချိန် check-in queue ကိုသင်နိုင်မယ်။ MDN docs အရ service worker က web app, browser, network ကြားက proxy လိုအလုပ်လုပ်ပြီး offline experience, network request interception, asset caching စတာတွေကို enable လုပ်ပေးနိုင်ပါတယ်။ ([MDN Web Docs][2])

```text
Default:
- React + Vite PWA

Optional Advanced Track:
- Expo / React Native
```

## Decision 2 — Notification Scope

MVP မှာ email/SMS/Viber notification ကို auto မပို့သေးဘူး။

```text
MVP:
- Registration success page မှာ ticket code ပြ
- User က ticket code ကို copy/save လုပ်နိုင်
- Admin dashboard မှာ registration ပေါ်

Later Scope:
- Email confirmation
- SMS confirmation
- Viber / Telegram notification
- Reminder notification
```

## Decision 3 — Safety Enforcement

Safety ကို rule အနေနဲ့ရေးရုံမဟုတ်ဘဲ **Hooks** နဲ့ deterministic enforce လုပ်မယ်။ Claude Code hooks docs အရ hooks တွေက Claude Code lifecycle points မှာ automatic run လုပ်တဲ့ shell commands, HTTP endpoints, LLM prompts ဖြစ်ပြီး `PreToolUse`, `PostToolUse`, session events စတာတွေမှာ custom decision/action ထည့်နိုင်ပါတယ်။ SDK hooks docs မှာလည်း dangerous operations block, tool call audit, sensitive actions approval စတာတွေကို hooks နဲ့လုပ်နိုင်တယ်လို့ဖော်ပြထားပါတယ်။ ([Claude][3])

## Decision 4 — Recovery Model

```text
Fast session undo:
- Claude checkpoints / rewind

Durable project history:
- Git commits
```

Claude Code VS Code docs မှာ checkpoints က Claude file edits ကို track လုပ်ပြီး previous state ပြန်သွားနိုင်စေကြောင်း၊ rewind option တွေမှာ conversation fork, code rewind, fork + rewind ပါကြောင်းဖော်ပြထားပါတယ်။ ([Claude][4])

## Decision 5 — Plugin Layer

Skills, agents, hooks, MCP servers တွေကို solo project ထဲမှာထားတာကောင်းပေမယ့် team/course/community အတွက် **plugin** အဖြစ် package လုပ်နိုင်ရမယ်။ Claude Code plugin docs အရ plugins တွေက skills, agents, hooks, MCP servers, LSP servers စတာတွေကို shareable package အဖြစ်ပေါင်းနိုင်ပြီး marketplace က discovery, version tracking, updates ကိုကူညီပါတယ်။ ([Claude][5])

## Decision 6 — MCP Direction

MCP ကို API direct usage → MCP tools/resources/prompts → local MCP server → secure MCP usage → MCP Apps အထိသွားမယ်။ MCP official docs က MCP ကို AI applications တွေက external systems, tools, data sources, workflows တွေနဲ့ချိတ်နိုင်တဲ့ open-source standard အဖြစ်ဖော်ပြထားပြီး MCP Apps docs က interactive dashboards, forms, visualizations ကို chat ထဲ render လုပ်နိုင်တဲ့ extension အဖြစ်ဖော်ပြထားပါတယ်။ ([Model Context Protocol][6])

## Decision 7 — Deployment Provider

Deployment section မှာ default provider ကို **DigitalOcean App Platform** ထားမယ်။

DigitalOcean docs အရ App Platform က Git repositories/container images ကနေ applications deploy လုပ်တဲ့ fully managed PaaS ဖြစ်ပြီး YAML app spec နဲ့ service/static site/database config တွေသတ်မှတ်နိုင်ပါတယ်။ Environment variables အတွက် encrypted values သုံးနိုင်ပေမယ့် runtime access ရှိသူတွေက secret ကိုမြင်နိုင်နိုင်တာကြောင့် team permissions နဲ့ PR review ကိုလိုအပ်တယ်လို့ docs မှာသတိပေးထားပါတယ်။ ([DigitalOcean Docs][7])

---

# Master Course Outline

---

# Part 0 — Orientation: Course Map & Mindset

## Chapter 0.1 — Agentic Coding ဆိုတာဘာလဲ

**Outcome:** Learner က ChatGPT code sample, autocomplete, editor agent, CLI agent, coding agent, multi-agent orchestration ကွာခြားချက်ကိုနားလည်မယ်။

ပါဝင်မယ့်အရာ—

```text
- AI chat vs coding agent
- Human as product owner
- Agent as junior developer
- Agentic loop: explore → plan → code → verify
- “Done” ဆိုတာ run/test/verify ပြီးမှ Done ဖြစ်ကြောင်း
```

## Chapter 0.2 — Course Capstone Overview

**Outcome:** Event Management Platform ကို capstone အဖြစ် ဘာကြောင့်ရွေးလဲ နားလည်မယ်။

ပါဝင်မယ့်အရာ—

```text
- Project goal
- User roles
- Public website
- Backend API
- Admin dashboard
- PWA check-in app
- Offline check-in
- MCP + deployment
```

## Chapter 0.3 — Course Decisions & Learning Contract

**Outcome:** Course ထဲမှာ ဘာတွေ fixed decision ဖြစ်လဲ သိမယ်။

ပါဝင်မယ့်အရာ—

```text
- PWA-first mobile decision
- Notification MVP scope
- Safety enforcement with hooks
- Git + checkpoints recovery model
- DigitalOcean deployment decision
- Learner responsibility
```

Artifact—

```text
COURSE_DECISIONS.md
```

---

# Part 1 — Beginner Bridge: Coding မသိသူအတွက် အခြေခံတံတား

## Chapter 1 — Computer, Files, Folders, Paths

**Outcome:** Project folder, file path, extension, root folder, hidden file ဆိုတာတွေ နားလည်မယ်။

Lab Output—

```text
agentic-coding-practice/
  notes/
  projects/
  screenshots/
  README.md
```

## Chapter 2 — Terminal Survival Guide

**Outcome:** Terminal ကိုမကြောက်တော့ဘဲ folder ရွှေ့၊ file ကြည့်၊ command run နိုင်မယ်။

Commands—

```text
pwd
ls / dir
cd
mkdir
touch
cat
clear
code .
```

## Chapter 3 — Internet & Web App Basics

**Outcome:** Browser, frontend, backend, database, API တို့ ဘယ်လိုဆက်စပ်လဲ နားလည်မယ်။

ပါဝင်မယ့်အရာ—

```text
Browser
URL
localhost
Frontend
Backend
Database
API
Request / Response
```

## Chapter 4 — HTML, CSS, JavaScript Survival

**Outcome:** Code အကုန်မရေးတတ်သေးပေမယ့် HTML/CSS/JS ရဲ့ တာဝန်ကိုဖတ်နိုင်မယ်။

ပါဝင်မယ့်အရာ—

```text
HTML = structure
CSS = style
JavaScript = behavior
Variable
Function
Array
Object
Event
```

## Chapter 5 — JSON, npm, package.json

**Outcome:** Agentic project တွေမှာ အမြဲတွေ့မယ့် `package.json`, dependency, scripts, JSON format ကိုနားလည်မယ်။

ပါဝင်မယ့်အရာ—

```text
JSON syntax
npm install
npm run dev
npm run build
dependencies
devDependencies
```

## Chapter 6 — Git, GitHub, and .gitignore

**Outcome:** Git ကို “project save game system” အနေနဲ့သုံးနိုင်မယ်။

ပါဝင်မယ့်အရာ—

```text
git init
git status
git add
git commit
git diff
GitHub repo
push / pull concept
.gitignore
node_modules ignore
dist ignore
.env ignore
.env.example commit
```

---

# Part 2 — Coding Agents Level 0 to Level 5

## Chapter 7 — Level 0: ChatGPT Code Example

**Outcome:** Chat AI က code sample ပေးတာနဲ့ agentic coding ကွာခြားချက်ကိုနားလည်မယ်။

## Chapter 8 — Level 1: AI Autocomplete

**Outcome:** Copilot-style autocomplete ကို ဘယ်အချိန်အသုံးဝင်လဲ၊ ဘယ်အချိန်အန္တရာယ်ရှိလဲ သိမယ်။

## Chapter 9 — Level 2: Editor Agent

**Outcome:** Editor ထဲက AI agent နဲ့ file-level changes လုပ်တတ်မယ်။

## Chapter 10 — Level 3: CLI Agent

**Outcome:** Terminal-based agent က file ဖတ်၊ edit, command run, test run လုပ်နိုင်တာနားလည်မယ်။

## Chapter 11 — Level 4: Coding Agent

**Outcome:** Feature တစ်ခုကို plan → implement → test → fix → summarize loop နဲ့ ခိုင်းတတ်မယ်။

## Chapter 12 — Level 5: Multi-Agent Orchestration

**Outcome:** Planner, frontend, backend, verifier, auditor, deployer စတဲ့ role ခွဲ workflow ကိုနားလည်မယ်။

---

# Part 3 — Tooling Setup & Safe Environment

## Chapter 13 — Development Tools Setup

**Outcome:** Local machine မှာ development environment တည်ဆောက်နိုင်မယ်။

Tools—

```text
VS Code
Git
Node.js / npm
Browser DevTools
Claude Code
Codex / other coding agents overview
```

## Chapter 14 — Project Workspace Setup

**Outcome:** Agentic coding အတွက် safe workspace တစ်ခုဆောက်နိုင်မယ်။

Lab Output—

```text
agentic-zero-to-hero/
  playground/
  myaree-store/
  event-management/
  notes/
```

## Chapter 15 — Safe Agent Permissions

**Outcome:** Agent ကို command run ခိုင်းတဲ့အခါ ဘယ် command တွေ approval လိုလဲ သိမယ်။

ပါဝင်မယ့်အရာ—

```text
Allow / Ask / Deny mindset
Dangerous commands
File delete safety
Package install approval
.env protection
Cloud action approval
```

## Chapter 16 — Secrets & API Key Safety

**Outcome:** API key, token, password မပေါက်အောင် သိမ်း/သုံး/commit မလုပ်နည်း သိမယ်။

ပါဝင်မယ့်အရာ—

```text
.env
.env.example
.gitignore
GitHub secrets
DigitalOcean token safety
Do not paste secrets into chat
Secret rotation
Accidentally committed secret recovery
```

## Chapter 17 — Claude Code Hooks: Deterministic Safety & Automation

**Outcome:** Learner က Agent ကို “ယုံကြည်” ရုံမဟုတ်ဘဲ Hooks နဲ့ policy enforce လုပ်နိုင်မယ်။

ပါဝင်မယ့်အရာ—

```text
Hook ဆိုတာဘာလဲ
Lifecycle events
PreToolUse
PostToolUse
PermissionRequest
ExitPlanMode hook
.env edit block လုပ်နည်း
dangerous bash command block လုပ်နည်း
write/edit ပြီးတိုင်း format/lint run ခိုင်းနည်း
hook log သိမ်းနည်း
model-based decision vs deterministic hook rule
```

Lab—

```text
- .env file edit ကို block လုပ်တဲ့ hook
- rm -rf / sudo / curl | bash command တွေကို ask/deny လုပ်တဲ့ hook
- file edit ပြီးတိုင်း npm run lint run ခိုင်းတဲ့ hook
```

Artifacts—

```text
.claude/settings.json
scripts/claude-hooks/guard-secrets.js
scripts/claude-hooks/post-edit-check.sh
```

---

# Part 4 — Prompting & Agent Control

## Chapter 18 — Prompt Anatomy

**Outcome:** Agent ကို vague မဟုတ်ဘဲ precise task prompt ပေးနိုင်မယ်။

Prompt structure—

```text
Context
Goal
Scope
Constraints
Acceptance Criteria
Verification
```

## Chapter 19 — Plan Before Code: Plan Mode & Generic Planning Workflow

**Outcome:** File မပြင်ခင် Agent ကို plan တင်ခိုင်းနိုင်မယ်။

Claude Code cost docs မှာ complex tasks အတွက် Plan Mode ကိုသုံးပြီး Claude က codebase explore လုပ်ကာ approach ကို approval အတွက်တင်နိုင်တာကြောင့် re-work cost လျော့စေတယ်လို့ဖော်ပြထားပါတယ်။ အဲဒီ docs မှာ `/rewind` သို့မဟုတ် Escape pattern နဲ့ checkpoint ပြန်ယူနိုင်တာကိုလည်း cost-control workflow အဖြစ်ဖော်ပြထားပါတယ်။ ([Claude][8])

ပါဝင်မယ့်အရာ—

```text
Plan Mode ဆိုတာဘာလဲ
Prompt-only planning vs actual plan mode
Plan review checklist
ExitPlanMode approval
SPEC နဲ့ plan စစ်နည်း
Acceptance criteria နဲ့ plan စစ်နည်း
```

## Chapter 20 — Task Decomposition

**Outcome:** Feature ကြီးကို task သေးသေးလေးတွေခွဲနိုင်မယ်။

## Chapter 21 — Good vs Bad Prompts

**Outcome:** “website လုပ်ပေး” ကနေ “acceptance criteria ပါတဲ့ feature prompt” အဆင့်ထိ ရေးနိုင်မယ်။

## Chapter 22 — Agent Review Prompts

**Outcome:** Agent ရေးထားတဲ့ code ကို ပြန်စစ်ခိုင်းနိုင်မယ်။

## Chapter 23 — Bug Report Prompts

**Outcome:** Bug ကို reproduce steps, expected, actual နဲ့ ရှင်းပြနိုင်မယ်။

---

# Part 5 — Project Memory & Context Engineering

## Chapter 24 — Rules / Instructions

**Outcome:** Project rules ကို reusable instruction အဖြစ်ရေးတတ်မယ်။

## Chapter 25 — CLAUDE.md

**Outcome:** Claude Code project memory file တစ်ခုရေးတတ်မယ်။

ပါဝင်မယ့်အရာ—

```text
Project purpose
Tech stack
Commands
Code style
Safety rules
Definition of Done
```

## Chapter 26 — AGENTS.md

**Outcome:** Codex / generic agents အတွက် project instruction file တစ်ခုရေးတတ်မယ်။

## Chapter 27 — Context Packing

**Outcome:** Agent ကို project တစ်ခုလုံးမဖတ်ခိုင်းဘဲ relevant files / relevant SPEC section ပဲပေးနိုင်မယ်။

Artifacts—

```text
FEATURE_BRIEF.md
REPO_MAP.md
TASK_CONTEXT.md
```

## Chapter 28 — Context Window & Token Control

**Outcome:** Long session, huge logs, generated files ကြောင့် context မဖြုန်းနည်းသိမယ်။

ပါဝင်မယ့်အရာ—

```text
node_modules မဖတ်ခိုင်းနည်း
dist/build output မဖတ်ခိုင်းနည်း
log summary တောင်းနည်း
session clear / resume
small task scope
subagent cost awareness
```

## Chapter 29 — Skills

**Outcome:** Reusable workflow တွေကို skill အဖြစ်သိမ်းနိုင်မယ်.

Skills—

```text
frontend-starter
backend-starter
pwa-starter
admin-starter
test-verify
security-check
deployer-do
```

## Chapter 30 — Commands

**Outcome:** `/make-spec`, `/verify-feature`, `/audit-release` လို command workflow တွေ design လုပ်တတ်မယ်။

## Chapter 31 — Subagents

**Outcome:** Specialist agents တွေကို role, tool access, scope နဲ့ ခွဲနိုင်မယ်။

## Chapter 32 — Plugins & Marketplaces

**Outcome:** Skills, subagents, hooks, MCP configs တွေကို team-shareable plugin package အဖြစ်စဉ်းစားတတ်မယ်။

ပါဝင်မယ့်အရာ—

```text
Plugin ဆိုတာဘာလဲ
Skill vs plugin
Subagent vs plugin
Hook vs plugin
Marketplace ဆိုတာဘာလဲ
Versioning
Team distribution
Trust and review checklist
```

Artifacts—

```text
plugin.json
marketplace.json draft
```

---

# Part 6 — Frontend Foundation

## Chapter 33 — React + Vite Starter

**Outcome:** React + Vite project create, run, build လုပ်နိုင်မယ်။

## Chapter 34 — React Components

**Outcome:** Component, props, reusable UI ကိုနားလည်မယ်။

## Chapter 35 — State & Events

**Outcome:** Button click, form input, cart count စတာတွေ state နဲ့ချိတ်နိုင်မယ်။

## Chapter 36 — Lists, Conditions, Empty States

**Outcome:** Product list, event list, loading, empty, error UI တွေထည့်နိုင်မယ်။

## Chapter 37 — Frontend Accessibility Basics

**Outcome:** Accessibility ကို audit အဆင့်ထိမစောင့်ဘဲ frontend စတည်ဆောက်ချိန်ကတည်းကထည့်တတ်မယ်။

ပါဝင်မယ့်အရာ—

```text
semantic HTML
label + input association
button vs div click
keyboard navigation
focus state
error message accessibility
color contrast basics
mobile touch target
Myanmar text readability
```

## Chapter 38 — Forms & Validation

**Outcome:** Registration form, phone validation, error messages တည်ဆောက်နိုင်မယ်။

## Chapter 39 — Tailwind & Responsive UI

**Outcome:** Mobile-first UI ဆောက်တတ်မယ်။

## Chapter 40 — Myanmar Localization

**Outcome:** မြန်မာ UI text, မြန်မာဂဏန်း, date/time, phone normalization, font fallback စတာတွေထည့်နိုင်မယ်။

## Chapter 41 — Frontend Testing Basics

**Outcome:** Component/form tests တွေ Agent နဲ့ရေးခိုင်းနိုင်မယ်။

---

# Part 7 — Warm-up Project: Myaree Store

## Chapter 42 — Myaree Store SPEC

**Outcome:** Mini ecommerce project တစ်ခုအတွက် simple SPEC ရေးမယ်။

## Chapter 43 — Product List

**Outcome:** JSON data ကနေ product card list ပြမယ်။

## Chapter 44 — Cart

**Outcome:** Add/remove/update quantity, total calculation, localStorage ထည့်မယ်။

## Chapter 45 — Checkout

**Outcome:** Buyer info form, validation, order summary ဆောက်မယ်။

## Chapter 46 — Viber Order Flow

**Outcome:** Myanmar context အတွက် screenshot + Viber instruction flow ဆောက်မယ်။

## Chapter 47 — Warm-up Project Review

**Outcome:** Build, test, review, Git commit, README update လုပ်မယ်။

---

# Part 8 — Single-Agent Development Workflow

## Chapter 48 — Feature Branch Workflow

**Outcome:** Agent ကို branch တစ်ခုထဲမှာ safely အလုပ်လုပ်စေနိုင်မယ်။

## Chapter 49 — Implementation Loop

**Outcome:** Plan → Code → Run → Fix → Test → Summary loop သုံးတတ်မယ်။

## Chapter 50 — Debugging with Agents

**Outcome:** Error log, stack trace, reproduce steps ကို Agent နဲ့ analyze လုပ်တတ်မယ်။

## Chapter 51 — Recovery: Checkpoints, Rewind, and Git

**Outcome:** Agent ပြင်ပြီး project ပျက်သွားရင် fast undo နဲ့ durable rollback နှစ်မျိုးလုံးသုံးနိုင်မယ်။

ပါဝင်မယ့်အရာ—

```text
Agent made a mess ဆိုတာဘာလဲ
Checkpoint / Rewind ဆိုတာဘာလဲ
Git commit နဲ့ checkpoint ကွာခြားချက်
When to use rewind
When to use git restore
When to use git reset
When to create a branch
Recovery checklist
```

Rule—

```text
Feature မစခင် Git commit
Session အတွင်း mistake အသေးစားဆို Rewind
Feature-level rollback ဆို Git
```

## Chapter 52 — Code Review with Agents

**Outcome:** Agent output ကို security, readability, maintainability, bug risk အတိုင်း review ခိုင်းနိုင်မယ်။

## Chapter 53 — Definition of Done

**Outcome:** “ပြီးပြီ” ဆိုတာ build/test/verify/documentation ပြီးမှဖြစ်ကြောင်း workflow ချနိုင်မယ်။

---

# Part 9 — Harness System

## Chapter 54 — Harness System Overview

**Outcome:** Model တစ်ခုကို real coding assistant ဖြစ်အောင် ပတ်ပတ်လည်မှာ ဘာတွေလိုလဲ နားလည်မယ်။

Components—

```text
Planner
Tools
Memory
Context Manager
Agent
Verification
Persistence
```

## Chapter 55 — Planner

**Outcome:** User idea ကို SDLC task plan အဖြစ်ခွဲတတ်မယ်။

## Chapter 56 — Tools

**Outcome:** Agent tool access ကို capability policy နဲ့ထိန်းနိုင်မယ်။

## Chapter 57 — Memory

**Outcome:** Project memory, workflow memory, session memory ခွဲသိမယ်။

## Chapter 58 — Context Manager

**Outcome:** Relevant context ပဲရွေးပြီး Agent ကိုပေးတတ်မယ်။

## Chapter 59 — Agent & Verification

**Outcome:** Agent output ကို independent verification နဲ့စစ်မယ်။

## Chapter 60 — Persistence

**Outcome:** Git commit, SPEC update, changelog, test report, audit report တွေသိမ်းမယ်။

## Chapter 61 — Harness Task Schema

**Outcome:** Agent handoff အတွက် structured task object တစ်ခုရေးနိုင်မယ်။

Artifacts—

```text
TASK_SCHEMA.json
AGENT_RUN_LOG.md
VERIFICATION_REPORT.md
```

## Chapter 62 — Build a Mini Agent Harness with Claude Agent SDK

**Outcome:** Harness System ကို diagram အဖြစ်သာမဟုတ်ဘဲ mini prototype အဖြစ်နားလည်မယ်။

Claude Agent SDK docs အရ SDK က Claude Code ကို library အဖြစ်သုံးနိုင်စေပြီး files ဖတ်ခြင်း၊ commands run ခြင်း၊ web search, code edit စတဲ့ agentic capabilities ကို Python/TypeScript ကနေ programmatic သုံးနိုင်စေပါတယ်။ TypeScript reference မှာ `@anthropic-ai/claude-agent-sdk` package နဲ့ native Claude Code binary ပါဝင်တယ်လို့ဖော်ပြထားပါတယ်။ ([Claude Platform Docs][9])

ပါဝင်မယ့်အရာ—

```text
SDK ဆိုတာဘာလဲ
Query loop
Tool permission policy
Hook-based guardrail
Task schema
Verification report
Cost/usage logging
Human approval gate
```

Lab—

```text
TASK_SCHEMA.json ဖတ်
Agent ကို single task run ခိုင်း
Output ကို VERIFICATION_REPORT.md ထဲရေး
Dangerous tool call ကို block လုပ်
```

---

# Part 10 — SPEC-Driven SDLC

## Chapter 63 — SDLC for Agentic Coding

**Outcome:** Idea → SPEC → task → code → test → deploy lifecycle ကိုနားလည်မယ်။

## Chapter 64 — SPEC ဆိုတာဘာလဲ

**Outcome:** SPEC ကို single source of truth အဖြစ်သုံးတတ်မယ်။

## Chapter 65 — SPEC.md Template

**Outcome:** Product goal, roles, journeys, features, API, database, acceptance criteria ပါတဲ့ SPEC ရေးနိုင်မယ်။

## Chapter 66 — SPEC ဖန်တီးနည်း

**Outcome:** Vague idea ကို precise requirements အဖြစ်ပြောင်းနိုင်မယ်။

## Chapter 67 — Acceptance Criteria

**Outcome:** စစ်လို့ရတဲ့ requirement ရေးတတ်မယ်။

## Chapter 68 — Traceability Matrix

**Outcome:** Requirement → Task → Test → Evidence ချိတ်နိုင်မယ်။

Artifact—

```text
TRACEABILITY.md
```

## Chapter 69 — SPEC Maintainer Subagent

**Outcome:** SPEC update ကို product code နဲ့မရောအောင် subagent ခွဲနိုင်မယ်။

## Chapter 70 — Coding Subagents

**Outcome:** frontend, backend, PWA, admin, test, verifier, auditor subagents ခွဲနိုင်မယ်။

## Chapter 71 — Agent Handoff Workflow

**Outcome:** Planner → Spec Maintainer → Coding Agent → Verifier → Auditor workflow တည်ဆောက်မယ်။

---

# Part 11 — Full-Stack Foundation

## Chapter 72 — API, HTTP, JSON Basics

**Outcome:** GET/POST/PUT/DELETE, status code, request/response နားလည်မယ်။

## Chapter 73 — Backend Starter

**Outcome:** Simple backend API ဆောက်နိုင်မယ်။

## Chapter 74 — Database Basics

**Outcome:** table, row, column, primary key, foreign key, migration, seed data နားလည်မယ်။

## Chapter 75 — PostgreSQL Basics

**Outcome:** Event Management project အတွက် relational database model ဆောက်မယ်။

## Chapter 76 — Validation & Shared Types

**Outcome:** Frontend/backend validation ကို shared package နဲ့ချိတ်မယ်။

## Chapter 77 — Authentication

**Outcome:** Login/session/token concept နားလည်မယ်။

## Chapter 78 — Authorization & RBAC

**Outcome:** Public visitor, organizer, staff, admin role ခွဲနိုင်မယ်။

## Chapter 79 — Error Handling Strategy

**Outcome:** Consistent API error format တစ်ခုသတ်မှတ်မယ်။

## Chapter 80 — Logging Strategy

**Outcome:** Debug log, audit log, PII-safe log ခွဲသိမယ်။

## Chapter 81 — File Uploads, Exports, Background Jobs

**Outcome:** Real-world backend features တွေအတွက် design options နားလည်မယ်။

---

# Part 12 — Full-Stack Starter Skills

## Chapter 82 — Frontend Starter Skill

**Outcome:** React frontend task တွေအတွက် reusable skill တစ်ခုရေးမယ်။

## Chapter 83 — Backend Starter Skill

**Outcome:** API/database/backend task တွေအတွက် reusable skill တစ်ခုရေးမယ်။

## Chapter 84 — Mobile Stack Decision: PWA-first vs Expo Native

**Outcome:** Event Management check-in app အတွက် PWA-first ဘာကြောင့်ရွေးမလဲ၊ ဘယ်အချိန် Expo Native သုံးသင့်လဲ ဆုံးဖြတ်နိုင်မယ်။

Default decision—

```text
MVP Mobile Check-in:
- React + Vite PWA

Advanced Native Track:
- Expo React Native
```

ပါဝင်မယ့်အရာ—

```text
PWA ဆိုတာဘာလဲ
Service worker ဆိုတာဘာလဲ
Offline storage ဆိုတာဘာလဲ
Installable web app concept
Expo / React Native ရဲ့ အားသာချက်
PWA vs Expo comparison table
Course capstone အတွက် final decision
```

## Chapter 85 — PWA Starter Skill

**Outcome:** PWA screen, offline queue, API sync, installable app UX အတွက် skill တစ်ခုရေးမယ်။

## Chapter 86 — Admin Starter Skill

**Outcome:** Dashboard/table/form/admin CRUD task တွေအတွက် skill တစ်ခုရေးမယ်။

## Chapter 87 — Shared Package Skill

**Outcome:** Types, validators, utils ကို project တစ်ခုလုံးမှာ reuse လုပ်တတ်မယ်။

---

# Part 13 — Capstone Project: Event Management Platform

## Chapter 88 — Event Management Project Setup

**Outcome:** Monorepo structure တစ်ခုတည်ဆောက်မယ်။

Structure—

```text
event-management/
  apps/web
  apps/admin
  apps/checkin-pwa
  services/api
  packages/shared
  SPEC.md
  CLAUDE.md
  AGENTS.md
```

## Chapter 89 — Event Management SPEC

**Outcome:** MVP scope နဲ့ Later scope ခွဲပြီး SPEC ရေးမယ်။

MVP—

```text
Public event list
Event detail
Registration
Ticket code confirmation
Staff check-in PWA
Admin event management
Admin attendee management
```

Later—

```text
Email/SMS/Viber notification
Payment
QR scan advanced
Multi-tenant organizations
Analytics
Native mobile app
```

## Chapter 90 — Database Design

**Outcome:** Event, attendee, ticket, user, role tables တွေ design လုပ်မယ်။

## Chapter 91 — Backend API: Events

**Outcome:** Event list/detail API ဆောက်မယ်။

## Chapter 92 — Backend API: Registration

**Outcome:** Attendee registration, duplicate prevention, ticket code generation ဆောက်မယ်။

## Chapter 93 — Registration Abuse Protection

**Outcome:** Public registration API ကို spam/bot/abuse ကနေကာကွယ်နည်းသင်မယ်။

ပါဝင်မယ့်အရာ—

```text
duplicate registration prevention
rate limiting
IP throttling
phone/email throttling
CAPTCHA as later/optional
suspicious activity logging
user-friendly error message
```

## Chapter 94 — Backend API: Check-in

**Outcome:** Ticket code check, attendee status, duplicate check-in prevention ဆောက်မယ်။

## Chapter 95 — Public Frontend: Event List

**Outcome:** Public user ကြည့်မယ့် event list page ဆောက်မယ်။

## Chapter 96 — Public Frontend: Event Detail

**Outcome:** Date, location, capacity, registration CTA ပါတဲ့ detail page ဆောက်မယ်။

## Chapter 97 — Public Frontend: Registration Form

**Outcome:** Validated registration form ဆောက်မယ်။

## Chapter 98 — Public Frontend: Ticket Confirmation

**Outcome:** Ticket code / confirmation page ဆောက်မယ်။

## Chapter 99 — Admin System: Dashboard

**Outcome:** Organizer/Admin dashboard shell ဆောက်မယ်။

## Chapter 100 — Admin System: Event CRUD

**Outcome:** Event create/edit/delete/list flow ဆောက်မယ်။

## Chapter 101 — Admin System: Attendee Management

**Outcome:** Attendee list, search, filter, export, status view ဆောက်မယ်။

ထည့်ရန်—

```text
pagination
server-side search
filters
sorting
export limits
loading skeleton
empty state
```

## Chapter 102 — PWA Check-in App Setup

**Outcome:** Staff check-in PWA initial setup ဆောက်မယ်။

## Chapter 103 — PWA: Staff Login & Event Select

**Outcome:** Staff login placeholder + event selector flow ဆောက်မယ်။

## Chapter 104 — PWA: Attendee Search

**Outcome:** Ticket code/name/phone နဲ့ attendee search ဆောက်မယ်။

## Chapter 105 — PWA: Check-in Flow

**Outcome:** Check-in success/warning/error states ဆောက်မယ်။

## Chapter 106 — PWA Offline Queue & Sync

**Outcome:** Venue internet မကောင်းရင် offline queue နဲ့ later sync concept ထည့်မယ်။

## Chapter 107 — Frontend Data Fetching Pattern

**Outcome:** Integration chapters မတိုင်ခင် frontend data fetching pattern ကို standardize လုပ်မယ်။

ပါဝင်မယ့်အရာ—

```text
fetch wrapper
API base URL
loading state
error state
retry
empty response
typed/shared response shape
```

## Chapter 108 — Integration: Web + API

**Outcome:** Frontend registration form ကို real backend API နဲ့ချိတ်မယ်။

## Chapter 109 — Integration: Admin + API

**Outcome:** Admin CRUD ကို backend API နဲ့ချိတ်မယ်။

## Chapter 110 — Integration: PWA + API

**Outcome:** PWA check-in app ကို backend API နဲ့ချိတ်မယ်။

## Chapter 111 — Optional Native Track: Expo Build Strategy

**Outcome:** PWA ပြီးတဲ့ learner များအတွက် Expo / React Native build path ကိုနားလည်မယ်။

---

# Part 14 — Test, Verify, Audit

## Chapter 112 — Test Pyramid

**Outcome:** Unit, integration, E2E, contract, smoke, regression tests ကွာခြားချက်နားလည်မယ်။

## Chapter 113 — Unit Tests

**Outcome:** Utility, validator, component-level test ရေးမယ်။

## Chapter 114 — API Integration Tests

**Outcome:** Registration/check-in backend flow tests ရေးမယ်။

## Chapter 115 — Frontend E2E Tests

**Outcome:** Event registration user journey ကို browser-level test လုပ်မယ်။

## Chapter 116 — Contract Tests

**Outcome:** Frontend/backend API contract မကွဲအောင်စစ်မယ်။

## Chapter 117 — Performance & Load Test Basics

**Outcome:** Advanced/optional အနေနဲ့ performance smoke test, API rate-limit test, admin attendee large-list test တွေသိမယ်။

## Chapter 118 — Test Skill

**Outcome:** Test workflow ကို `.claude/skills/test-verify` အဖြစ်သိမ်းမယ်။

## Chapter 119 — Test Engineer Subagent

**Outcome:** Test writing/running/failure analysis ကို subagent ခွဲမယ်။

## Chapter 120 — Verifier Subagent

**Outcome:** SPEC acceptance criteria အတိုင်း pass/fail report ထုတ်မယ်။

## Chapter 121 — Auditor Subagent

**Outcome:** Security, privacy, maintainability, deployment risk audit report ထုတ်မယ်။

## Chapter 122 — UX & Accessibility Audit

**Outcome:** Mobile usability, keyboard navigation, color contrast, form copy, Myanmar localization စစ်မယ်။

## Chapter 123 — Data Privacy & PII Audit

**Outcome:** phone/email/name/ticket data ကို safe handling လုပ်မယ်။

---

# Part 15 — CI/CD

## Chapter 124 — CI/CD ဆိုတာဘာလဲ

**Outcome:** Local test/build ကနေ automated pipeline အထိ နားလည်မယ်။

GitHub Actions official docs က GitHub Actions ကို build, test, deployment pipeline automate လုပ်နိုင်တဲ့ CI/CD platform အဖြစ်ဖော်ပြထားပြီး pull request တိုင်း build/test run ခြင်း သို့မဟုတ် merged pull request ကို production deploy ခြင်းတွေ workflow နဲ့လုပ်နိုင်တယ်လို့ ဖော်ပြထားပါတယ်။ ([GitHub Docs][10])

## Chapter 125 — GitHub Actions: Test Workflow

**Outcome:** Pull request တိုင်း test/build run မယ့် workflow ဆောက်မယ်။

## Chapter 126 — GitHub Actions: Build Workflow

**Outcome:** Web/admin/api/PWA build checks ခွဲမယ်။

## Chapter 127 — GitHub Actions: Security Checks

**Outcome:** Secrets scan, dependency check, lint/typecheck workflow ထည့်မယ်။

## Chapter 128 — Staging Deployment Pipeline

**Outcome:** main branch merge ပြီး staging deploy workflow ချမယ်။

## Chapter 129 — Production Approval Gate

**Outcome:** Production deploy မလုပ်ခင် manual approval gate ထည့်မယ်။

---

# Part 16 — MCP: Model Context Protocol

## Chapter 130 — API Direct Usage

**Outcome:** Agent/MCP မသုံးဘဲ API ကို curl/script နဲ့တိုက်ရိုက်သုံးတတ်မယ်။

## Chapter 131 — MCP ဆိုတာဘာလဲ

**Outcome:** MCP ကို AI application နဲ့ external tools/data/workflows ချိတ်တဲ့ standard အဖြစ်နားလည်မယ်။

## Chapter 132 — MCP Tools, Resources, Prompts

**Outcome:** MCP server တစ်ခုက tool, resource, prompt ဘယ်လို expose လုပ်လဲ နားလည်မယ်။

## Chapter 133 — MCP Server Design

**Outcome:** Event Management project အတွက် MCP tool design ရေးမယ်။

Tools—

```text
list_events
get_event
list_attendees
check_ticket_status
mark_attendee_checked_in
deployment_status
```

## Chapter 134 — Build a Local MCP Server

**Outcome:** Local MCP server prototype တစ်ခုဖန်တီးမယ်။

## Chapter 135 — Use MCP from Coding Agent

**Outcome:** Agent က MCP tools တွေကို safe workflow နဲ့သုံးနိုင်မယ်။

## Chapter 136 — MCP Security

**Outcome:** Read-only vs write-capable tools, approval gates, audit logs, scopes, destructive action policy ထည့်မယ်။

ပါဝင်မယ့်အရာ—

```text
local MCP vs remote MCP
read-only MCP
write-capable MCP
OAuth / authorization
scope design
tool confirmation
destructive action policy
audit log
rate limit
tool versioning
```

## Chapter 137 — MCP Apps: Interactive UI for Agent Tools

**Outcome:** MCP tool output ကို plain text မဟုတ်ဘဲ dashboard/form/visualization အဖြစ် render လုပ်နိုင်တဲ့ concept ကိုနားလည်မယ်။

MCP Apps docs အရ traditional MCP tools တွေက text/images/resources/structured data ပြန်ပေးနိုင်ပေမယ့် MCP Apps က tool description ထဲမှာ interactive UI resource reference ပါပြီး host က sandboxed iframe အဖြစ် conversation ထဲ render လုပ်နိုင်တဲ့ pattern ဖြစ်ပါတယ်။ Build guide မှာ MCP Apps ကို MCP tools/resources နားလည်ပြီး Node.js 18+ နဲ့စနိုင်ကြောင်းလည်းဖော်ပြထားပါတယ်။ ([Model Context Protocol][11])

Use case—

```text
Event check-in dashboard
Attendee status filter
Checked-in / not checked-in chart
Manual check-in confirmation form
```

---

# Part 17 — Deployment: DigitalOcean App Platform

## Chapter 138 — Deployment Concepts

**Outcome:** local, staging, production, environment variables, domain, SSL, logs concept နားလည်မယ်။

## Chapter 139 — DigitalOcean App Platform Overview

**Outcome:** App Platform ပေါ်မှာ frontend/backend deploy architecture နားလည်မယ်။

## Chapter 140 — DigitalOcean App Spec

**Outcome:** YAML app spec တစ်ခုရေးနိုင်မယ်။

DigitalOcean app spec glossary မှာ app spec ကို app domain, region, environment variables စတဲ့ configuration တွေသတ်မှတ်တဲ့ YAML manifest အဖြစ်ဖော်ပြထားပြီး control panel, API, CLI ကနေ app create/update လုပ်နိုင်ကြောင်းဖော်ပြထားပါတယ်။ ([DigitalOcean Docs][12])

## Chapter 141 — Environment Variables & Secrets

**Outcome:** Production secrets ကို safe storage, documentation, rotation plan နဲ့စီမံမယ်။

## Chapter 142 — Deployment Cost Checklist

**Outcome:** Learner က cloud deploy မလုပ်ခင် cost visibility ရှိမယ်။

ပါဝင်မယ့်အရာ—

```text
static site cost
API service cost
managed database cost
outbound bandwidth
logs/monitoring add-ons
staging vs production duplicate cost
budget limit
shutdown/cleanup checklist
pricing page ကို deploy မတိုင်ခင်ပြန်စစ်ခြင်း
```

## Chapter 143 — Database Deployment

**Outcome:** Managed PostgreSQL, migrations, seed, backup, restore strategy ချမယ်။

## Chapter 144 — Deploy Backend API

**Outcome:** API service ကို staging ပေါ်တင်မယ်။

## Chapter 145 — Deploy Public Web

**Outcome:** Public frontend ကို static site အဖြစ်တင်မယ်။

## Chapter 146 — Deploy Admin System

**Outcome:** Admin dashboard ကို protected deployment အဖြစ်တင်မယ်။

## Chapter 147 — DigitalOcean MCP

**Outcome:** DO MCP / API / doctl သုံးပြီး app status/deployment ကို agent နဲ့စစ်နိုင်မယ်။

## Chapter 148 — Deployer Skill

**Outcome:** DigitalOcean deployment workflow ကို reusable skill အဖြစ်သိမ်းမယ်။

## Chapter 149 — Deployer Subagent

**Outcome:** Staging deploy, production approval, rollback plan ကို subagent ခွဲမယ်။

## Chapter 150 — Smoke Tests After Deployment

**Outcome:** Deploy ပြီး health check, registration flow, check-in flow စစ်မယ်။

## Chapter 151 — Logs, Monitoring, Rollback

**Outcome:** Production issue ဖြစ်ရင် log ကြည့်၊ rollback, incident note ရေးနိုင်မယ်။

---

# Part 18 — Production Readiness

## Chapter 152 — Security Hardening

**Outcome:** Auth, RBAC, input validation, dependency risk, secret exposure စစ်မယ်။

## Chapter 153 — Prompt Injection Defense

**Outcome:** Agent / MCP / user-generated content ထဲက malicious instruction တွေကို ခွဲခြားနိုင်မယ်။

## Chapter 154 — Supply Chain Safety

**Outcome:** npm package install မလုပ်ခင် risk review workflow ချမယ်။

## Chapter 155 — Cost & Token Management

**Outcome:** Agent run cost, context size, subagent usage, model selection ကိုစီမံမယ်။

ပါဝင်မယ့်အရာ—

```text
which model for which task
subagent cost
large log cost
generated files မဖတ်ခိုင်းနည်း
token budget
cloud hosting budget
cost per feature
```

## Chapter 156 — Agent Quality Metrics

**Outcome:** Agent output quality ကို metric နဲ့တိုင်းမယ်။

Metrics—

```text
Build pass rate
Test pass rate
Bug recurrence
Files changed per task
Spec compliance
Security findings
Rollback frequency
Cost per feature
```

## Chapter 157 — Release Notes & Changelog

**Outcome:** Release တစ်ခုချင်းစီအတွက် changelog, known issues, rollback note ထုတ်မယ်။

## Chapter 158 — Documentation

**Outcome:** README, SETUP, SPEC, DEPLOYMENT, RUNBOOK docs တွေပြည့်စုံအောင်ရေးမယ်။

---

# Part 19 — Final Capstone Assessment

## Chapter 159 — Final Feature Freeze

**Outcome:** Capstone project ကို release candidate အဖြစ် freeze လုပ်မယ်။

## Chapter 160 — Final Verification

**Outcome:** SPEC acceptance criteria အားလုံး pass/fail စစ်မယ်။

## Chapter 161 — Final Audit

**Outcome:** Auditor subagent နဲ့ security/privacy/quality/deployment audit လုပ်မယ်။

## Chapter 162 — Final Deployment

**Outcome:** Staging → production deployment process အပြီးလုပ်မယ်။

## Chapter 163 — Portfolio Presentation

**Outcome:** Learner က project ကို README, screenshots, architecture diagram, demo steps နဲ့တင်ပြနိုင်မယ်။

## Chapter 164 — Capstone Rubric

**Outcome:** Hero level ရောက်/မရောက်ကို rubric နဲ့တိုင်းမယ်။

Rubric—

```text
Level A — Beginner
- Terminal သုံးနိုင်
- Vite app run နိုင်
- Agent ကို small edit ခိုင်းနိုင်

Level B — Builder
- Frontend page ဆောက်နိုင်
- Git checkpoint သုံးနိုင်
- Bug report prompt ရေးနိုင်

Level C — Full-stack Builder
- Frontend + backend + database ချိတ်နိုင်
- Test/build run နိုင်
- SPEC-based feature ဆောက်နိုင်

Level D — Production-ready Builder
- CI/CD ရှိ
- Staging deploy ရှိ
- Secrets/logs/rollback စီမံနိုင်

Level E — Agentic Engineer
- Skills, commands, subagents သုံးနိုင်
- Verifier/Auditor/Deployer workflow ရှိ
- Hooks နဲ့ safety enforce လုပ်နိုင်
- MCP ကို safe policy နဲ့သုံးနိုင်
- Full capstone project deploy ပြီး
```

---

# Appendices

## Appendix A — Prompt Library

```text
Explain codebase prompt
Plan feature prompt
Implement feature prompt
Debug prompt
Review prompt
Test prompt
Verify prompt
Audit prompt
Deploy prompt
Rollback prompt
Legacy onboarding prompt
```

## Appendix B — Templates

```text
CLAUDE.md
AGENTS.md
COURSE_DECISIONS.md
SPEC.md
FEATURE_BRIEF.md
REPO_MAP.md
TRACEABILITY.md
TASK_SCHEMA.json
VERIFICATION_REPORT.md
AUDIT_REPORT.md
DEPLOYMENT.md
RUNBOOK.md
```

## Appendix C — Skill Templates

```text
frontend-starter
backend-starter
pwa-starter
admin-starter
test-verify
security-audit
deployer-do
mcp-tool-designer
mcp-app-designer
```

## Appendix D — Subagent Templates

```text
spec-maintainer
frontend-developer
backend-developer
pwa-developer
admin-developer
test-engineer
verifier
auditor
deployer
```

## Appendix E — Common Errors & Recovery

```text
npm install error
port already in use
build failed
blank page
API CORS error
database connection error
migration failed
agent edited wrong files
secret accidentally committed
deployment failed
PWA service worker cache issue
offline sync conflict
```

## Appendix F — Existing / Legacy Codebase Onboarding

**Outcome:** Learner က codebase ရင့်ကြီးတစ်ခုကို agent နဲ့ safe onboarding လုပ်နိုင်မယ်။

Lab—

```text
1. Do not edit mode
2. file tree summary
3. entry points identify
4. architecture map
5. dependency map
6. test/build commands find
7. risk areas identify
8. REPO_MAP.md create
9. FIRST_TASK_PLAN.md create
```

Prompt—

```text
ဒီ codebase ကိုမပြင်ပါနဲ့။
ပထမဆုံး onboarding လုပ်ပါ။
architecture, entry points, commands, risks, first safe task ကိုရှာပြီး REPO_MAP.md draft ပေးပါ။
```

---

# Recommended Learning Paths

## Track 1 — Absolute Beginner Path

```text
Part 0
Part 1
Part 2
Part 3
Part 4
Part 6
Part 7
Part 8
```

ရလဒ် — learner က AI agent နဲ့ frontend mini project တစ်ခုဆောက်နိုင်မယ်။

## Track 2 — Builder Path

```text
Part 5
Part 9
Part 10
Part 11
Part 12
Part 13
Part 14
```

ရလဒ် — learner က SPEC-driven full-stack project တည်ဆောက်နိုင်မယ်။

## Track 3 — Production Path

```text
Part 15
Part 16
Part 17
Part 18
```

ရလဒ် — learner က CI/CD, MCP, DigitalOcean deployment, security audit, rollback အထိလုပ်နိုင်မယ်။

## Track 4 — Final Hero Path

```text
Part 19
Appendices
Final Capstone
```

ရလဒ် — learner က Agentic Coding portfolio project တစ်ခု production-ready အဆင့်ထိတင်ပြနိုင်မယ်။

---

# Suggested Book Split

ဒီ course ကို book series ပုံစံရေးမယ်ဆိုရင်—

```text
Book 1 — Beginner Foundation
Parts 0–4

Book 2 — Single-Agent Builder
Parts 5–8

Book 3 — Agentic System Design
Parts 9–12

Book 4 — Full-Stack Capstone
Parts 13–14

Book 5 — MCP, CI/CD, Deployment, Production
Parts 15–19
```

---

