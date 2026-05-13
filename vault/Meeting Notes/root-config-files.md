---
name: root-config-files
description: תיעוד קבצי הקונפיגורציה בשורש הפרויקט — CLAUDE.md, .env, .env.example, .gitignore
type: project
---

# Root Configuration Files

## Overview

תיעוד של כל הקבצים שיושבים בשורש הפרויקט (`C:\Users\USER\Desktop\the5agent\`). כולם קבצי קונפיגורציה — אין כאן עדיין קוד אפליקציה. הם מגדירים את **מי הסוכן הראשי**, **אילו מפתחות API** המערכת צריכה, ו**מה לא להעלות ל-git**.

**הקבצים:**

| קובץ | משויך ל | תפקיד |
|---|---|---|
| `CLAUDE.md` | ראובן (מנכ"ל) | מגדיר את זהות הסוכן הראשי, הצוות שלו, ומבנה התיקיות. נטען אוטומטית בכל סשן. |
| `.env.example` | כל הצוות | תבנית למפתחות API. מועתק ל-`.env` ומתמלא בערכים אמיתיים. נשמר ב-git. |
| `.env` | כל הצוות | המפתחות בפועל (ANTHROPIC_API_KEY, OPENAI_API_KEY, TAVILY/FIRECRAWL/SERPAPI). **לא** נשמר ב-git. |
| `.gitignore` | תשתית | מסנן מה לא להעלות: `.env`, `node_modules/`, `__pycache__/`, `.venv/`, פלטי build, וקבצי OS/editor. |

**מיפוי API → סוכן** (לפי `.env.example`):
- `ANTHROPIC_API_KEY` — משמש את כל הצוות (ראובן, יעל, יובל, חן)
- `OPENAI_API_KEY` — אופציונלי, ליובל אם משתמש ב-DALL-E
- `TAVILY_API_KEY` / `FIRECRAWL_API_KEY` / `SERPAPI_API_KEY` — לחן (מחקר ואיסוף מידע)
- `NODE_ENV` — מצב סביבה כללי (development | production)

## Open Questions

- האם תיווצר בעתיד גם תיקיית `src/` או `app/` עם קוד אפליקציה בפועל? כיום אין קוד ריצה — רק תשתית קונפיגורציה.
- האם מפתחות ה-API ישותפו בין כל הסוכנים (`.env` יחיד) או שכל סוכן יקבל קובץ נפרד?

## Session Log

### 2026-05-13 — תיעוד ראשוני של קבצי השורש [shipped]
- **What was done:** קריאת `.env.example`, `.gitignore`, ו-`CLAUDE.md`, ויצירת טבלת מיפוי קובץ→סוכן→תפקיד.
- **Decisions:** הקבצים שייכים לקטגוריית **תשתית** ולא לסוכן בודד — `CLAUDE.md` משויך לראובן כי הוא מגדיר את הזהות שלו, השאר משויכים ל"כל הצוות".
- **Notes / Caveats:** `.env` עצמו לא נקרא (קובץ סודי). מבנה המפתחות מבוסס על `.env.example` בלבד.
- **Related:** [[project-overview]], [[claude-team-config]]
