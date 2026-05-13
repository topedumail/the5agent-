---
name: obsidian-vault-setup
description: תיעוד תיקיית .obsidian/ ותיקיית vault/ — תצורת Obsidian והזיכרון ארוך הטווח של הפרויקט
type: project
---

# Obsidian Vault Setup

## Overview

Obsidian הוא **הזיכרון ארוך הטווח של הפרויקט**. כל החלטה, סיכום סשן, תיעוד, וקונטקסט נשמרים כאן בקבצי `.md` עם wikilinks (`[[...]]`) לקישור בין נושאים. זה מאפשר לסוכנים בעתיד לטעון קונטקסט במהירות.

**שני הרכיבים:**

### `.obsidian/` — תצורת Obsidian (לא תוכן)

| קובץ | תפקיד |
|---|---|
| `app.json` | קונפיגורציה כללית של Obsidian (כרגע `{}` — ברירת מחדל) |
| `appearance.json` | תצורת מראה (`{}` — ברירת מחדל) |
| `core-plugins.json` | רשימת תוספי הליבה הפעילים: file-explorer, graph, backlink, properties, daily-notes, templates, bookmarks, outline, **bases**, sync, ועוד |
| `workspace.json` | מצב הסביבה האחרון של המשתמש (פאנלים פתוחים, קבצים אחרונים) |

**שימו לב:** התוסף `bases` מופעל — מאפשר תצוגות דמויות-DB על קבצי המארק-דאון. ראה [[skills-inventory#obsidian-bases]] לפרטים.

### `vault/` — תוכן ה-vault

| תיקייה | תפקיד | מתי לכתוב לכאן |
|---|---|---|
| `vault/Meeting Notes/` | החלטות, ארכיטקטורה, סיכומי סשן של פיצ'רים/באגים/refactors | **משימות קוד וארכיטקטורה** (כולל קבצים אלה) |
| `vault/Content Briefs/` | בריפים של תוכן, מפרטי קמפיינים | משימות עריכת תוכן (יעל) |
| `vault/Publishing Log/` | תיעוד פרסומים, תוצאות, post-mortems | אחרי פרסום בפועל |
| `vault/Brand Guidelines/` | טון, ויזואל, UI, קווי מותג | החלטות מותג / עיצוב (יובל) |

**כללי כתיבה (מתוך skill obsidian-vault-workflow):**
- **קובץ אחד לכל נושא**, לא קובץ אחד לכל יום.
- שם הקובץ: `<topic-hyphenated>.md`, **בלי תאריך**.
- מבנה: Overview → Open Questions → Session Log (כל סשן `### YYYY-MM-DD — title [status]`).
- כל ערך כולל `**Related:** [[wikilink]]`.
- כל תיקייה כוללת `_index.md` עם רשימת כל הנושאים.

**מצב נוכחי של ה-vault:**
- `Meeting Notes/` — מאוכלסת כעת בקבצי תיעוד התשתית (5 נושאים + `_index.md`).
- שאר התיקיות (`Content Briefs`, `Publishing Log`, `Brand Guidelines`) ייווצרו לפי הצורך — `Write` יוצר תיקיות הורה אוטומטית.

## Open Questions

- מתי ייווצר התוכן הראשון תחת `Content Briefs/` ו-`Publishing Log/`?
- האם יוגדר template אחיד ל-`Content Briefs` (פלטפורמת יעד, audience, KPIs)?
- האם להפעיל `daily-notes` ככלי עבודה של ראובן, או להישאר עם topic files בלבד?

## Session Log

### 2026-05-13 — תיעוד תשתית Obsidian והקמת מבנה Meeting Notes [shipped]
- **What was done:** קריאת תצורת `.obsidian/` (app, appearance, core-plugins), מיפוי 4 תתי-תיקיות `vault/`, ויצירת `Meeting Notes/_index.md` + 5 קבצי נושא ראשונים.
- **Decisions:** ארגון לפי **אזורי פרויקט** (root config / claude team / obsidian / skills) ולא קובץ-לקובץ — מתאים יותר ל"קובץ אחד לכל נושא" שמכתיב ה-skill.
- **Notes / Caveats:** `app.json` ו-`appearance.json` ריקים (`{}`) — Obsidian נטען עם ברירות מחדל. `bases` מופעל אז אפשר בעתיד ליצור תצוגות table על קבצי הנושא.
- **Related:** [[project-overview]], [[claude-team-config]], [[skills-inventory]]
