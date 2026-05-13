---
name: skill-gpt-image-gen
description: skill מותאם — מעטפת ל-OpenAI Images API (gpt-image-2). ב-.claude/skills/gpt-image-gen/. בשימוש בעיקר ע"י יובל.
type: project
---

# Skill: `gpt-image-gen`

## Overview

ה-skill הראשון שנכתב **מותאם אישית לפרויקט** (להבדיל מה-Superpowers / Obsidian / example-skills שהותקנו דרך plugins). הוא נמצא ב-[`.claude/skills/gpt-image-gen/SKILL.md`](../../.claude/skills/gpt-image-gen/SKILL.md) ומשמש כמעטפת מתועדת לקריאת OpenAI Images API.

**מי משתמש בזה**: יובל ([[yuval-agent-definition]]). כל סוכן עם `Bash` יכול לקרוא, אבל הסקיל לא עוסק בניסוח ה-prompt — זה תפקיד של הסוכן הקורא.

**מודל**: `gpt-image-2` של OpenAI (יצא 21 באפריל 2026). **שם המודל קבוע**. הסקיל מכיל אזהרה בולטת בראשו וטבלת שגיאות שמסבירה למה — גם אם המודל מחזיר 400 על "model not found" — *לא* להחליף מודל, אלא לפנות לבעיות access/billing.

**שני נתיבי decoding**:
- **A** — `jq` + `base64 --decode`
- **B** — fallback ב-python (כי `jq` לא תמיד מותקן ב-Git Bash על Windows)

## Open Questions

- האם להוסיף לסקיל גם דרך C — `Node.js` decoding — למקרה ש-`python` לא זמין? כרגע מניחים ש-Python כן מותקן.
- האם להוסיף סקריפט עזר (`scripts/generate.sh`) במקום לתעד ב-prose, כפי שעושות חלק מה-skills של Superpowers? כרגע prose-only.
- האם הסקיל צריך לתעד גם את ה-`POST /v1/images/edits` ו-`POST /v1/images/variations` של OpenAI, או להתמקד רק ב-generations?

## Session Log

### 2026-05-13 — יצירת הסקיל [shipped]
- **What was done:**
  - יצירת [.claude/skills/gpt-image-gen/SKILL.md](../../.claude/skills/gpt-image-gen/SKILL.md) עם frontmatter תקני (name + description).
  - גוף ה-skill: תכלית, אזהרת מודל, דרישות קדם, שני נתיבי שימוש (jq / python), בדיקת תקינות, טבלת פרמטרים, וטבלת שגיאות נפוצות.
  - **לא** נכתב script עזר; ה-Bash inline במקום זה. הסיבה: ה-skill דק וברור גם בלי script.
- **Decisions:**
  - **אזהרת המודל בראש המסמך** — לא בסוף ולא במרכז. גם בכותרות הראשיות (`⚠️ שם המודל`), גם בטבלת השגיאות, גם בהערה לכל קוד דוגמה. ההנחה: ה-LLM שיקרא את הסקיל בעתיד עלול "לתקן" את שם המודל לפי הידע הפנימי שלו אם האזהרה תהיה מינורית.
  - **Python fallback ולא רק jq** — Windows + Git Bash זה הסביבה של המשתמש הספציפי, ושם jq לא תמיד קיים. Python בדרך כלל כן (מותקן עם הפיתוח).
  - **לא הוסף סקריפט exec** — ניסיון לשמור על Skill פשוט: דקומנטציה במקום קוד שצריך לתחזק.
- **Notes / Caveats:**
  - הסקיל לא נטסט מול API אמיתי. ראה [[yuval-agent-definition]] — תלוי ב-`OPENAI_API_KEY` תקין ב-`.env`.
  - יש סיכון של "model not found" אם החשבון של המשתמש לא מאופשר ל-`gpt-image-2`. הסקיל מנחה במפורש לפנות ל-OpenAI ולא להחליף מודל.
- **Related:** [[yuval-agent-definition]], [[skills-inventory]], [[skill-writing-skills]]
