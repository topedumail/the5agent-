---
name: yuval-agent-definition
description: יצירת sub-agent יובל (מעצב התמונות) ב-.claude/agents/yuval.md, כולל skill gpt-image-gen ושילוב עם יעל
type: project
---

# יובל — Agent Definition

## Overview

יובל הוא ה-sub-agent השני של הצוות (אחרי יעל), שהוגדר ב-2026-05-13. תפקידו: יצירת תמונות לפי בקשה, עם עקביות ויזואלית מול דוגמאות השראה ב-`yuval/reference/`. הסוכן מוגדר ב-[.claude/agents/yuval.md](.claude/agents/yuval.md) עם `tools: Read, Write, Bash, Glob` ו-`model: sonnet` (heavy lifting מבוצע ע"י OpenAI Images API, לא ע"י Claude).

**Flow של יובל** (7 שלבים): סריקת `yuval/reference/` → ניתוח בקשה → בחירת רכיבי השראה → ניסוח prompt באנגלית → קריאה ל-`gpt-image-gen` skill דרך Bash → שמירה ב-`yuval/outputs/<YYYY-MM-DD>-<slug>.png` + sibling `.txt` עם ה-prompt → אימות שהקובץ קיים ו-size > 0.

**שילוב עם יעל**: יעל מסמנת מקומות שצריכים תמונה בעזרת placeholder בפורמט `{{IMAGE_NEEDED: "<english description>"}}`. ראובן ([CLAUDE.md](../../CLAUDE.md)) מפעיל את יובל פעם לכל placeholder, מעתיק את התמונות ל-`Output/images/`, ומחליף את ה-placeholders ב-`<img>` (HTML) / `![]()` (Markdown) ב-Output הסופי של יעל.

**מודל יצירת תמונה**: `gpt-image-2` של OpenAI (יצא 21 באפריל 2026). שם המודל קבוע — אסור להחליף ל-`dall-e-3`, `gpt-image-1` או אלטרנטיבה. ראה [[skill-gpt-image-gen]] לפרטים.

## Open Questions

- מתי יוצבו תמונות השראה ב-`yuval/reference/`? בלעדיהן יובל מנסח prompts גנריים יותר.
- האם להגדיר default size שונה מ-1024x1024 (למשל 1792x1024 ל-hero images)? כרגע ריבוע ברירת מחדל.
- האם להוסיף mechanism ליובל לבחור איכות `high` כברירת מחדל עבור hero images, ו-`medium` ל-inline illustrations?
- end-to-end test לא רץ עדיין — דורש `OPENAI_API_KEY` תקין ב-`.env` + restart לסשן (כי הסוכן `yuval` לא ייטען ב-Agent registry אחרת).

## Session Log

### 2026-05-13 — יצירת yuval.md + skill gpt-image-gen + שילוב עם יעל [shipped]
- **What was done:**
  - **Skill חדש**: [`.claude/skills/gpt-image-gen/SKILL.md`](../../.claude/skills/gpt-image-gen/SKILL.md) — מעטפת ל-OpenAI Images API. שני נתיבי decoding (jq + python fallback), טבלת שגיאות, ואזהרה בולטת על שם המודל `gpt-image-2` (אסור להחליף).
  - **Agent חדש**: [`.claude/agents/yuval.md`](../../.claude/agents/yuval.md) — system prompt בעברית עם 7 שלבי workflow, גבולות ברורים (ראו [[yael-agent-definition]] לדפוס), ופורמט דיווח של 4 שורות.
  - **תיקיות עבודה**: `yuval/reference/` (תמונות השראה, ב-git) ו-`yuval/outputs/` (תוצרים, ignored).
  - **`.gitignore` עודכן** עם הכלל `yuval/outputs/* !yuval/outputs/.gitkeep`. אומת ש-`yuval/outputs/test.png` ignored ושה-`.gitkeep` נשאר tracked.
  - **`.env.example` עודכן** — ההערה ליד `OPENAI_API_KEY` שונתה מ-"(DALL-E) if used" ל-"required for Yuval via gpt-image-2".
  - **CLAUDE.md עודכן**: יובל ב-"מוגדר" במקום "טרם הוגדר"; סעיף ניתוב מלא ליובל; סעיף חדש "זרימת עבודה — מאמר עם תמונות" עם 8 שלבים שראובן מבצע (יעל → yuval x N → cp ל-Output/images → Edit החלפת placeholders); מבנה תיקיות עודכן.
  - **yael.md עודכן**: שלב 4 ("שכתוב") עודכן עם הוראות לסימון `{{IMAGE_NEEDED: "..."}}` placeholders. סעיף דיווח עודכן עם דרישה לרשימת placeholders שהושארו. גבולות נשארו זהים (יעל עדיין לא יוצרת תמונות).
- **Decisions:**
  - **Model ליובל: sonnet** ולא opus. ה-heavy lifting הוא ב-OpenAI Images API; ה-LLM של יובל רק מנסח prompt וקורא Bash. sonnet זול יותר מ-opus וזה מתאים.
  - **שם מודל `gpt-image-2` קבוע** — המשתמש קבע מפורשות שזהו מודל אמיתי (יצא 21 באפריל 2026) ושאסור להחליף, גם אם ה-LLM "חושב" שיש מודל "נכון יותר". יישמר בזיכרון פנימי (`feedback_gpt_image_2_model_name.md`) כדי שכל סשן עתידי לא יחזור על הטעות.
  - **שילוב תמונות ב-Output/**: העתקה ל-`Output/images/` + נתיב יחסי `images/file.png`. ה-Output נשאר נייד; אפשר לפתוח את ה-HTML מכל מקום בלי לשבור קישורים.
  - **`{{IMAGE_NEEDED: ...}}` כפורמט placeholder** — זוגות סוגריים מסולסלים כפולים (Mustache-like) הם פחות סבירים להופיע בטקסט אמיתי, וקל ל-Grep/Edit עליהם.
  - **תיאור באנגלית בתוך ה-placeholder** — OpenAI Images עובד טוב יותר באנגלית. יעל מנסחת באנגלית למרות שהמאמר עצמו בעברית.
- **Notes / Caveats:**
  - הסוכן `yuval` לא ייטען ב-Agent registry של הסשן הנוכחי (כמו שקרה עם yael בסשן הקודם). בשימוש ראשון אחרי restart — `claude /agents` או `claude /quit; claude` כדי לטעון.
  - End-to-end test לא בוצע — דורש `OPENAI_API_KEY` תקין. ההמלצה: רוץ קריאה ידנית קטנה לפי הסקיל כדי לאמת ש-key עובד וש-`gpt-image-2` זמין לחשבון לפני שיובל יורץ אוטומטית.
  - אין כרגע תמונות ב-`yuval/reference/` — יובל ינסח prompts לפי הבקשה הגולמית בלי השראה.
  - יעל קיבלה sweet-spot של "2 placeholders למאמר ממוצע" — לא כללי קשיח, אבל הנחיה.
- **Related:** [[yael-agent-definition]], [[skill-gpt-image-gen]], [[claude-team-config]], [[project-overview]]
