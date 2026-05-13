---
name: skill-creator-install
description: התקנת skill-creator ב-project scope דרך plugin example-skills ממ-marketplace anthropics/skills
type: project
---

# skill-creator Plugin Install

## Overview

ב-2026-05-13 הותקן ה-plugin `example-skills@anthropic-agent-skills` ב-**project scope** כדי לאפשר שימוש ב-skill-creator (וב-skills נוספים) ליצירת skills מותאמים אישית לפרויקט. ה-marketplace `claude-plugins-official` לא הכיל את ה-plugin המבוקש, אז נוסף marketplace חדש `anthropic-agent-skills` (מקור: `anthropics/skills` ב-GitHub) ש**כן** הכיל את skill-creator — אך כחלק מחבילה רחבה יותר.

**הקבצים המעורבים:**
- `.claude/settings.json` — מכיל את הרישום של ה-marketplace וההפעלה של ה-plugin (project scope)
- מטמון: `~/.claude/plugins/marketplaces/anthropic-agent-skills/` — המקור המקומי, לא חלק מהריפו

**מה נכלל ב-example-skills (12 skills):** algorithmic-art, brand-guidelines, canvas-design, doc-coauthoring, frontend-design, internal-comms, mcp-builder, **skill-creator**, slack-gif-creator, theme-factory, web-artifacts-builder, webapp-testing.

## Open Questions

- האם להשאיר את כל 12 ה-skills פעילים, או למצוא דרך לבודד רק את skill-creator? (Claude Code לא תומך ב-disable של skill בודד בתוך plugin.)
- אילו skills מותאמים אישית נכתוב ראשונים? המועמדים הסבירים: ליעל — `draft-instagram-post`, ליובל — `generate-cover-image`, לחן — `research-pipeline`.

## Session Log

### 2026-05-13 — התקנה דרך marketplace חלופי [shipped]
- **What was done:**
  - שלב 1 (`skill-creator@claude-plugins-official`) נכשל: marketplace רשום אבל plugin לא נמצא.
  - שלב 3: הוסף marketplace `anthropic-agent-skills` ממקור `anthropics/skills`. בדיקה במניפסט הראתה ש-skill-creator הוא חלק מ-plugin בשם `example-skills`, לא plugin עצמאי.
  - הוחלט להתקין את `example-skills` המלא ב-project scope (אחרי אישור משתמש "תעשה מה שנראה לך").
  - אומת `claude plugin list`: scope=project, status=enabled. אומת קובץ `.claude/settings.json`.
  - 2 commits נדחפו ל-`origin/main`: `81653ba` (obsidian skills + vault) ו-`19e6d92` (settings.json).
- **Decisions:** העדפת התקנה דרך plugin (ולא העתקה ידנית) — שומר על update semantics ועל מקור אמת אחד. ה-11 skills הנוספים לא יופעלו אלא אם תיאוריהם יתאמו למשימה.
- **Notes / Caveats:**
  - ה-CLI של claude לא נמצא ב-PATH; קרא אותו ב-נתיב מלא: `C:\Users\USER\AppData\Roaming\Claude\claude-code\2.1.138\claude.exe`.
  - `npm.ps1` חסום ע"י execution policy ב-PowerShell — צריך `npm.cmd` ישירות אם נדרש npm.
  - אזהרות `LF→CRLF` ב-git נורמליות ב-Windows ולא משפיעות על התוכן.
- **Related:** [[skills-inventory]], [[skill-writing-skills]], [[claude-team-config]], [[root-config-files]]
