---
name: chen-agent-definition
description: יצירת sub-agent חן (חוקרת הרשת) ב-.claude/agents/chen.md, תיקיית memory, וזרימת תוכן חדש מהרשת ב-CLAUDE.md
type: project
---

# חן — Agent Definition

## Overview

חן היא ה-sub-agent השלישי של הצוות (אחרי [[yael-agent-definition|יעל]] ו-[[yuval-agent-definition|יובל]]), שהוגדר ב-2026-05-13. תפקידה: למצוא מאמרים, מקורות ופיסות תוכן רלוונטיות ברשת לפי בקשה שמגיעה מראובן, ולהכינן כקלט ליעל (קובץ ב-`Content/`). היא הסוכנת היחידה בצוות עם **WebSearch + WebFetch**, ולפיכך החוליה היחידה שיכולה להזרים תוכן חיצוני עדכני לתוך הצנרת — לפניה, יעל הייתה תלויה במאמרים שראובן/המשתמש הכניסו ידנית ל-`Content/`.

**קונפיגורציה**: [.claude/agents/chen.md](../../.claude/agents/chen.md) עם `tools: WebSearch, WebFetch, Read, Write, Edit, Glob, Grep` (ללא Bash, ללא API חיצוני) ו-`model: opus` — שיקול דעת על איכות מקור הוא ה-bottleneck של חן, ולכן opus כמו ביעל.

**Workflow** (7 שלבים): קבלת בקשה → בדיקת זיכרון (`Grep` על `chen/Memory/searches.md`) → WebSearch של 2–3 שאילתות → WebFetch על 2–4 candidates → סינון לפי קריטריוני איכות (⭐⭐⭐ ומעלה) → שמירה ל-`Content/<YYYY-MM-DD>-<slug>.md` עם header של מקור (url, title, date, fetched_at) → תיעוד entry ב-`chen/Memory/searches.md`.

**זיכרון**: `chen/Memory/searches.md` — לוג כל החיפושים בפורמט קבוע (נושא, מילות מפתח, שאילתות, מקורות שנמצאו ב-⭐, נבחר ולמה, קובץ Content/). תחת tracking ב-git כדי שיישמר בין סשנים. חן בודקת את הקובץ לפני כל חיפוש; אם יש entry מ-30 הימים האחרונים על נושא דומה היא עוצרת ומחזירה את הקיים, חוץ מנושאים דינמיים (חדשות, מחירים, releases) שתמיד נחפשים מחדש.

**Routing ב-CLAUDE.md**: ראובן מפעיל את חן על trigger keywords (`חפש`, `מצא`, `מחקר`, `מאמר על`, `latest on`, `research`). אחרי שחן מחזירה — ראובן מחליט אוטומטית לפי הבקשה: "מצא לי" → עוצר; "מצא ושכתב" → מפעיל את יעל; אם יעל מסמנת `{{IMAGE_NEEDED}}` → מפעיל את יובל. הזרימה החדשה תועדה ב-CLAUDE.md תחת "זרימת עבודה — תוכן חדש מהרשת" וחוברת לזרימת התמונות הקיימת.

## Open Questions

- **end-to-end test** טרם בוצע — דורש בקשה אמיתית של נושא לחיפוש. ההמלצה: בסשן הבא, בקשה כמו "מצא לי מאמר על Anthropic prompt caching" כדי לראות שה-workflow רץ מקצה לקצה ושה-Memory מתעדכן.
- הסוכן `chen` לא ב-Agent registry של הסשן הנוכחי — צפוי לפי הניסיון עם [[yael-agent-definition|יעל]] ו-[[yuval-agent-definition|יובל]]. בשימוש ראשון אחרי restart יטען.
- האם להגדיר תקרה ל-WebFetches per request (לדוגמה 4)? כרגע ההנחיה היא "2–4 candidates" אבל בלי enforce. אם חן תתחיל לבצע 10+ fetches בלי תוצאה, יש לחזק את הסעיף.
- הסף "30 ימים" לבדיקת זיכרון הוא ניחוש — ייתכן שצריך להיות 14 ימים לנושאים חצי-דינמיים (טכנולוגיה) ו-90 ימים ל-evergreen. כרגע single threshold; ייבחן אחרי שייצברו כמה entries.
- האם יוצא slash command (כמו `/research <topic>`) שיפעיל את חן ישירות, או להישאר עם ניתוב טבעי דרך ראובן? בסיס דומה לשאלה שעלתה ב-[[yael-agent-definition#Open Questions|יעל]].

## Session Log

### 2026-05-13 — יצירת chen.md + memory + זרימת תוכן מהרשת [shipped]
- **What was done:**
  - **Agent חדש**: [`.claude/agents/chen.md`](../../.claude/agents/chen.md) — system prompt בעברית, 8 סעיפים: זהות, כללי-יסוד (אסור הזיות), בדיקת זיכרון, workflow 7-שלבי, קריטריוני מקור איכותי (✅/❌), גבולות, פורמט דיווח של 3 שורות, דוגמה. `tools: WebSearch, WebFetch, Read, Write, Edit, Glob, Grep`, `model: opus`.
  - **Memory bootstrap**: `chen/Memory/searches.md` — header + פורמט entry + סעיף `## Entries` ריק. `chen/Memory/.gitkeep` להבטחת תיקייה בגיט. אין `.gitignore` הוספה — `chen/` tracked כברירת מחדל.
  - **CLAUDE.md עודכן** (4 שינויים):
    - שורה 15: סטטוס חן → "מוגדרת" (היה "טרם הוגדרה").
    - סעיף ניתוב מלא לחן עם trigger keywords (עברית + English), קלט/פלט, כלים, זיכרון. החליף את ה-placeholder של "טרם הוגדרה" בסעיף "ניתוב — מתי מפעילים את מי".
    - סעיף חדש "זרימת עבודה — תוכן חדש מהרשת" אחרי "זרימת עבודה — מאמר עם תמונות". מתאר את החלטת ההמשך האוטומטית (חן → אם בקשה כללה "שכתב" → יעל → אם יעל סימנה placeholders → יובל).
    - מבנה התיקיות עודכן עם `chen/` תחת תיקיות עבודה בשורש.
  - **Vault**: קובץ זה (`chen-agent-definition.md`) + הוספת שורה ל-`_index.md` בסעיף "Topics — פעולות/התקנות" אחרי `yuval-agent-definition`.
- **Decisions:**
  - **Model: opus** (claude-opus-4-7) — כמו יעל. חן צריכה להעריך איכות מקור, לסנן clickbait/aggregators, ולסכם בצורה שמשרתת את יעל. שיקול דעת הוא ה-bottleneck. sonnet היה זול יותר אבל פחות מתאים.
  - **Memory ב-git** (לא ignored) — הזיכרון הוא נכס של הפרויקט ולא working file חולף. שווה גם לסשנים עתידיים וגם לשיתוף בין branches/contributors.
  - **`chen/Memory/` ולא `chen/memory/`** — Memory ב-Capital כדי שיובדל ויזואלית מ-`reference/` ו-`outputs/` של יובל ו-`reference/` של יעל. עקבי עם semantic ("זה זיכרון פרסיסטנטי, לא working data").
  - **Auto-chain ב-CLAUDE.md** — ראובן ממשיך אוטומטית מ-חן ליעל ויובל אם הבקשה כללה "שכתב"/"פרסם" (לפי בחירת המשתמש). חוסך turn לכל מעבר. למשתמש תמיד יש שליטה דרך הניסוח של הבקשה.
  - **כללי-יסוד מודגשים ב-system prompt**: "אסור הזיות" + "אסור URLs ממוצאים" + "אסור ציטוטים מהזיכרון של המודל" — מוטמעים בולט ב-top של ה-prompt כי זה ה-failure mode הסביר ביותר ל-research agent.
  - **`Content/` יישאר ignored ב-git** — חן כותבת לשם אבל הפלט שלה דומה למאמרי גלם של יעל (working file לסבב נוכחי, לא חלק מהריפו).
- **Notes / Caveats:**
  - **end-to-end test לא בוצע** — אין בקשה אמיתית בסשן הזה. ייבחן בסשן הבא.
  - הסוכן `chen` לא ייטען ב-Agent registry של הסשן הנוכחי — כצפוי לפי [[yael-agent-definition]] ו-[[yuval-agent-definition]] (צריך restart).
  - **WebFetch על דפים ישראליים** עלול להיתקל ב-paywall (Calcalist, TheMarker). מקרי קצה — חן תדווח שלא הצליחה לקרוא ותחזור ל-fallback.
  - **גודל קבצי `Content/`** — אם חן שואבת מאמר ארוך, הקובץ יכול להיות גדול. יעל קוראת את כולו אבל קונטקסט שלה מוגבל. ייתכן שצריך להוסיף ליעל הנחיה לקצר ראשונית אם המקור מעל N מילים. מסומן כשאלה פתוחה לסשן עתידי.
  - **תקני הציון ⭐** הם heuristic של חן ולא מקודדים פורמלית. ⭐⭐⭐ = "אמין, רלוונטי, מעודכן". ⭐⭐⭐⭐⭐ = "ראשוני מהמקור". יורד לפי גיל המקור ועליית רעש/clickbait.
- **Related:** [[yael-agent-definition]], [[yuval-agent-definition]], [[claude-team-config]], [[project-overview]]
