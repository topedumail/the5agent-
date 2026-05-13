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
| `.claude/agents/` | הגדרות הסוכנים בצוות (יעל, יובל, חן) — כל קובץ הוא agent עם system prompt משלו | יעל / יובל / חן | מאוכלסת ב-2/3 — `yael.md` ([[yael-agent-definition]]) + `yuval.md` ([[yuval-agent-definition]]); חן טרם |
| `.claude/commands/` | פקודות slash מותאמות (`/<name>`) להפעלה מהירה של זרימות עבודה | ראובן (להפעלה) | **ריק** (רק `.gitkeep`) |
| `.claude/skills/` | יכולות מותאמות שהסוכנים יכולים להפעיל באמצעות `Skill` tool | כל הצוות | מאוכלסת — ראה [[skills-inventory]] |

**איך זה עובד יחד:**
- ראובן (המוגדר ב-`CLAUDE.md`) מקבל את הבקשה.
- ראובן מחליט איזה **agent** לקרוא לו (מ-`agents/`) — לפי טבע המשימה: כתיבה→יעל, ויזואל→יובל, מחקר→חן.
- כל סוכן יכול להפעיל **skills** רלוונטיות שלו לעזרה בביצוע.
- **commands** הם קיצורי דרך — `/research-topic` למשל עשוי להריץ את כל הזרימה (חן→יעל) במכה אחת.

**הערות חשובות:**
- **יעל הוגדרה** (2026-05-13) — `.claude/agents/yael.md`, ראה [[yael-agent-definition]].
- **יובל הוגדר** (2026-05-13) — `.claude/agents/yuval.md` + skill `gpt-image-gen`, ראה [[yuval-agent-definition]] ו-[[skill-gpt-image-gen]]. חן עדיין לא.
- ה-skills שכבר מותקנים מקורם בעיקר מ-Superpowers plugin (commit `bbf4f3c`), אוסף `obsidian-*`, ו-skill **מותאם אישית** ראשון לפרויקט: `gpt-image-gen`.
- ה-skill הקריטי לפרויקט: [[obsidian-vault-setup#obsidian-vault-workflow]] — חובה בכל משימה.

## Open Questions

- מתי ייכתב קובץ ה-agent של **חן** תחת `.claude/agents/`? (מבנה ה-agent של יעל ויובל יכול לשמש תבנית.)
- אילו slash commands מתוכננים? למשל `/research`, `/draft`, `/design`, `/full-content-pipeline`?
- האם יוגדר agent נפרד לראובן עצמו או שהוא נשאר רק ב-`CLAUDE.md`?

## Session Log

### 2026-05-13 — מיפוי ראשוני של מבנה .claude/ [shipped]
- **What was done:** סריקת `.claude/agents/`, `.claude/commands/`, `.claude/skills/`, ויצירת טבלה שמסבירה את תפקיד כל תיקייה ומצבה הנוכחי.
- **Decisions:** ה-skills מנותקים לקובץ נושא נפרד ([[skills-inventory]]) כי הרשימה ארוכה ומשתנה.
- **Notes / Caveats:** `agents/` ו-`commands/` ריקות בפועל — תיעדנו רק את ה**כוונה** שלהן, לא תוכן ממשי.
- **Related:** [[project-overview]], [[root-config-files]], [[skills-inventory]], [[obsidian-vault-setup]]

### 2026-05-13 — agents/ כבר לא ריק [shipped]
- **What was done:** עדכון הטבלה והערות שיקפו את העובדה ש-`yael.md` הוגדר ו-`.claude/agents/` כבר מאוכלסת חלקית. הסרת השאלה "מה יהיה המבנה המדויק" מ-Open Questions, כי המבנה כעת קיים בפועל ([[yael-agent-definition]]).
- **Decisions:** השארת `commands/` בסטטוס "ריק" — slash commands עדיין לא נכתבו. שמירת השאלה על יובל/חן ב-Open Questions, כי הם ייכתבו לפי תבנית yael.
- **Notes / Caveats:** התבנית של יעל (frontmatter YAML + system prompt מובנה) צפויה להחזור בקבצי יובל וחן עם התאמות לתחום שלהם.
- **Related:** [[yael-agent-definition]], [[project-overview]]

### 2026-05-13 — יובל הוגדר + skill מותאם ראשון [shipped]
- **What was done:** הוגדר הסוכן השני, יובל ([[yuval-agent-definition]]), בשילוב עם skill מותאם אישית ראשון לפרויקט — `gpt-image-gen` ([[skill-gpt-image-gen]]). הטבלה עודכנה (`agents/` עכשיו 2/3); ה-skill נכנס לקטגוריה חדשה ב-_index ("Skills מותאמים אישית לפרויקט").
- **Decisions:** שילוב הצוותי בין יעל ויובל מוגדר ב-CLAUDE.md כסעיף "זרימת עבודה — מאמר עם תמונות" עם 8 שלבים שראובן מבצע. יעל מסמנת `{{IMAGE_NEEDED: ...}}` placeholders; ראובן קורא ליובל ומחליף ב-Output הסופי. **התבנית של ה-system prompt חוזרת בדפוס דומה** (זהות → workflow → גבולות → דיווח), כצפוי.
- **Notes / Caveats:** הסוכן `yuval` לא ייטען ב-Agent registry של הסשן הנוכחי — דורש restart. בנוסף, end-to-end test תלוי ב-`OPENAI_API_KEY` תקין ובחשבון שמורשה ל-`gpt-image-2`.
- **Related:** [[yuval-agent-definition]], [[skill-gpt-image-gen]], [[yael-agent-definition]], [[project-overview]]
