---
name: claude-team-config
description: תיעוד תיקיית .claude/ — agents, commands, skills — תשתית הצוות של Claude Code
type: project
---

# Claude Team Configuration (`.claude/`)

## Overview

תיקיית `.claude/` היא **מרכז התצורה** של הצוות. שלוש תתי-תיקיות, כל אחת עם תפקיד מובחן:

| תיקייה | תפקיד | משויך ל | מצב נוכחי |
|---|---|---|---|
| `.claude/agents/` | הגדרות הסוכנים בצוות (יעל, יובל, חן) — כל קובץ הוא agent עם system prompt משלו | יעל / יובל / חן | **ריק** (רק `.gitkeep`) |
| `.claude/commands/` | פקודות slash מותאמות (`/<name>`) להפעלה מהירה של זרימות עבודה | ראובן (להפעלה) | **ריק** (רק `.gitkeep`) |
| `.claude/skills/` | יכולות מותאמות שהסוכנים יכולים להפעיל באמצעות `Skill` tool | כל הצוות | מאוכלסת — ראה [[skills-inventory]] |

**איך זה עובד יחד:**
- ראובן (המוגדר ב-`CLAUDE.md`) מקבל את הבקשה.
- ראובן מחליט איזה **agent** לקרוא לו (מ-`agents/`) — לפי טבע המשימה: כתיבה→יעל, ויזואל→יובל, מחקר→חן.
- כל סוכן יכול להפעיל **skills** רלוונטיות שלו לעזרה בביצוע.
- **commands** הם קיצורי דרך — `/research-topic` למשל עשוי להריץ את כל הזרימה (חן→יעל) במכה אחת.

**הערות חשובות:**
- כיום סוכני יעל/יובל/חן **טרם הוגדרו** — `agents/` מכילה רק `.gitkeep`.
- ה-skills שכבר מותקנים מקורם בעיקר מ-Superpowers plugin (commit `bbf4f3c`) ובאוסף `obsidian-*`.
- ה-skill הקריטי לפרויקט: [[obsidian-vault-setup#obsidian-vault-workflow]] — חובה בכל משימה.

## Open Questions

- מה יהיה המבנה המדויק של קובץ ה-agent של יעל/יובל/חן? (system prompt, tools, model)
- אילו slash commands מתוכננים? למשל `/research`, `/draft`, `/design`, `/full-content-pipeline`?
- האם יוגדר agent נפרד לראובן עצמו או שהוא נשאר רק ב-`CLAUDE.md`?

## Session Log

### 2026-05-13 — מיפוי ראשוני של מבנה .claude/ [shipped]
- **What was done:** סריקת `.claude/agents/`, `.claude/commands/`, `.claude/skills/`, ויצירת טבלה שמסבירה את תפקיד כל תיקייה ומצבה הנוכחי.
- **Decisions:** ה-skills מנותקים לקובץ נושא נפרד ([[skills-inventory]]) כי הרשימה ארוכה ומשתנה.
- **Notes / Caveats:** `agents/` ו-`commands/` ריקות בפועל — תיעדנו רק את ה**כוונה** שלהן, לא תוכן ממשי.
- **Related:** [[project-overview]], [[root-config-files]], [[skills-inventory]], [[obsidian-vault-setup]]
