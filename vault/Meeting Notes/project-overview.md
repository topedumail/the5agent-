---
name: project-overview
description: מבט-על על פרויקט "צוות 5 הסוכנים" — מטרה, מבנה כללי, ומבנה הצוות
type: project
---

# Project Overview — צוות 5 הסוכנים

## Overview

זהו פרויקט של **מערכת רב-סוכנית ליצירת תוכן**. במקום סוכן אחד גנרי שמנסה לעשות הכל, יש כאן צוות מתמחה שבו לכל סוכן תפקיד ברור. הסוכן הראשי (ראובן) פועל כמנכ"ל שמקבל את הבקשות מהמשתמש, ומנתב אותן לחברי הצוות המתאימים.

**חברי הצוות:**
- **ראובן** (מנכ"ל) — מקבל בקשות, מבין מה צריך, ומנתב למי שצריך. לא מבצע עבודה בעצמו.
- **יעל** (כותבת) — ניסוח, כתיבה ועריכה של טקסטים.
- **יובל** (מעצב) — יצירה ועיצוב של חומרים ויזואליים (יכול להשתמש ב-DALL-E).
- **חן** (חוקרת) — איסוף מידע, מחקר ובדיקת עובדות (יכולה להשתמש ב-Tavily / Firecrawl / SerpAPI).

**מצב נוכחי:** **יעל** ([[yael-agent-definition]]) ו-**יובל** ([[yuval-agent-definition]]) הוגדרו (2026-05-13). יובל מצויד ב-skill מותאם `gpt-image-gen` לקריאת OpenAI Images API ([[skill-gpt-image-gen]]). הזרימה המשולבת יעל→יובל מתועדת ב-CLAUDE.md ("זרימת עבודה — מאמר עם תמונות"). חן עדיין לא הוגדרה; `.claude/commands/` עדיין ריק.

## Open Questions

- מתי ייכתב קובץ ה-agent של **חן** תחת `.claude/agents/`? (יעל ויובל יכולים לשמש תבנית.)
- אילו slash commands מתוכננים תחת `.claude/commands/` להפעלה מהירה של זרימות עבודה?
- האם תוגדר זרימת עבודה משולבת (חן→יעל→יובל) כפקודה אחת? כרגע יעל→יובל מתועד בקובץ CLAUDE.md.
- מתי יתמלא `yael/style-guide.md` ויאוכלסו דוגמאות ב-`yael/reference/` ו-`yuval/reference/`? עד אז שני הסוכנים פועלים לפי best-practices.

## Session Log

### 2026-05-13 — תיעוד ראשוני של מבנה הפרויקט [shipped]
- **What was done:** סריקה מלאה של הפרויקט ויצירת קבצי תיעוד ב-vault עבור כל אזור: שורש, `.claude/`, `.obsidian/` ו-`vault/`, ומלאי כל ה-skills.
- **Decisions:** ארגון לפי **אזורים** ולא קובץ-לקובץ — קל יותר לנווט ולתחזק. כל קובץ נושא מקשר באמצעות `[[wikilinks]]` לקבצים הקשורים.
- **Notes / Caveats:** הצוות עצמו (יעל, יובל, חן) טרם הוגדר בפועל — `agents/` ו-`commands/` עדיין מכילים רק `.gitkeep`.
- **Related:** [[root-config-files]], [[claude-team-config]], [[obsidian-vault-setup]], [[skills-inventory]]

### 2026-05-13 — יעל הוגדרה כ-sub-agent הראשון [shipped]
- **What was done:** עדכון ה-Overview לשקף שיעל הוגדרה ב-`.claude/agents/yael.md` (פרטים מלאים ב-[[yael-agent-definition]]). הסרת השאלה הגנרית "מתי ייכתבו הסוכנים" והחלפתה בשאלה ממוקדת על יובל וחן. הוספת שאלה חדשה על מילוי `yael/style-guide.md`.
- **Decisions:** השארת המבנה הקיים של הסעיף — רק עדכון משפט הסטטוס במקום שכתוב מלא.
- **Notes / Caveats:** בסשן הזה ראובן (אני) הוסיף גם תשתית סביבה: תיקיות `Content/`, `Output/`, `yael/` + עדכוני `CLAUDE.md` ו-`.gitignore`.
- **Related:** [[yael-agent-definition]], [[claude-team-config]], [[root-config-files]]

### 2026-05-13 — יובל הוגדר + skill מותאם ראשון + שילוב עם יעל [shipped]
- **What was done:** הוספת הסוכן השני (יובל) + skill מותאם אישית ראשון (`gpt-image-gen`) + שילוב משולש: יעל מסמנת `{{IMAGE_NEEDED: ...}}` placeholders, יובל יוצר תמונות, ראובן משלב ב-Output. פרטים מלאים: [[yuval-agent-definition]], [[skill-gpt-image-gen]].
- **Decisions:** Sonnet ליובל (heavy lifting ב-OpenAI). `gpt-image-2` כמודל קבוע (לא להחליף בשום מצב — הוראת המשתמש). שילוב התמונות: העתקה ל-`Output/images/` ושימוש בנתיב יחסי, כדי שה-Output יישאר נייד.
- **Notes / Caveats:** end-to-end test לא בוצע — דורש `OPENAI_API_KEY` תקין וגישה ל-`gpt-image-2` בחשבון. הסוכן `yuval` יידרש restart כדי להירשם ב-Agent registry.
- **Related:** [[yuval-agent-definition]], [[skill-gpt-image-gen]], [[yael-agent-definition]]
