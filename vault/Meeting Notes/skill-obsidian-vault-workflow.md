---
name: skill-obsidian-vault-workflow
description: Skill קריטי — פרוטוקול חובה לקריאה וכתיבה ל-vault בתחילת וסוף כל משימה
type: reference
---

# Skill: obsidian-vault-workflow

## Overview

**מיקום:** `.claude/skills/obsidian-vault-workflow/`
**משפחה:** Obsidian — **קריטי**
**משויך ל:** **כל הצוות, בכל סשן, בכל פקודה**
**מתי להפעיל:** בתחילת כל משימה (קריאת הקובץ הרלוונטי + 2-3 Meeting Notes אחרונים) ובסיומה (עדכון Overview, הוספת `### YYYY-MM-DD — title [status]` ל-Session Log, וקישור `[[wikilinks]]`).
**למה זה חשוב:** ה-vault הוא הזיכרון ארוך-הטווח של הפרויקט. בלי ההפעלה הזו, סוכנים עתידיים יחזרו על עבודה או יסתרו החלטות קודמות. ראובן שמר על כך זיכרון feedback מפורש.

**מבנה קובץ נושא:**
```
# <Topic Title>
## Overview
## Open Questions
## Session Log
### YYYY-MM-DD — title [status]
```

**תיקיות יעד:**
- `vault/Meeting Notes/` — קוד, ארכיטקטורה, החלטות
- `vault/Content Briefs/` — תוכן
- `vault/Publishing Log/` — פרסומים
- `vault/Brand Guidelines/` — מותג / טון

## Open Questions
- none

## Session Log

### 2026-05-13 — תיעוד ראשוני [shipped]
- **What was done:** תיעוד ה-skill כקריטי. מקובע גם בזיכרון Claude (`feedback_obsidian_vault_workflow.md`) להפעלה אוטומטית בתחילת כל פקודה.
- **Decisions:** סווג כ"חובה" — לא רק "מומלץ". כולל פירוט מבנה קובץ הנושא ותיקיות היעד.
- **Related:** [[skills-inventory]], [[skill-obsidian-markdown]], [[obsidian-vault-setup]]
