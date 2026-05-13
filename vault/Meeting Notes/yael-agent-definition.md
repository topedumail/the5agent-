---
name: yael-agent-definition
description: יצירת ה-sub-agent הראשון של הצוות — יעל, כותבת התוכן, ב-.claude/agents/yael.md
type: project
---

# יעל — Agent Definition

## Overview

יעל היא ה-sub-agent הראשון שהוגדר בפועל בפרויקט "צוות 5 הסוכנים", ב-2026-05-13. עד לרגע זה תיקיית `.claude/agents/` הכילה רק `.gitkeep` ([[claude-team-config]]); כעת היא מכילה את `yael.md` — agent עם YAML frontmatter תקני (`name: yael`, `tools: Read/Write/Edit/Glob/Grep`, `model: opus`) ו-system prompt בעברית.

**תחום אחריות של יעל:** לקיחת מאמרי גלם מ-`Content/` ושכתובם בסגנון הכתיבה של הצוות. הפלט — שני קבצים ב-`Output/`: גרסת Markdown נקייה וגרסת HTML עצמאית עם CSS inline מינימלי (RTL, typography קריא). יעל קוראת את `yael/style-guide.md` ואת `yael/reference/` בתחילת כל סשן; אם הם ריקים — היא כותבת לפי best-practices ומציינת זאת בדיווח.

**מגבלות שהוטמעו ב-system prompt:** ללא Bash / WebSearch / API / סוכנים אחרים. אם משימה דורשת אחד מהם — יעל עוצרת ומחזירה לראובן לניתוב.

**קבצים מעורבים:**
- `.claude/agents/yael.md` — הגדרת הסוכן (חדש).
- `yael/style-guide.md` — מדריך סגנון (stub, למילוי).
- `yael/reference/` — דוגמאות סגנון (ריק כרגע, עם `.gitkeep`).
- `Content/`, `Output/` — תיקיות עבודה (`.gitkeep` בלבד; התוכן ignored).
- `CLAUDE.md` — עודכן עם סעיף "ניתוב — מתי מפעילים את מי" + סטטוס "מוגדרת" ליעל.
- `.gitignore` — עודכן עם הכלל `Content/* !Content/.gitkeep` (וזהה ל-Output).

## Open Questions

- מתי ייכתב תוכן ל-`yael/style-guide.md`? בלעדיו יעל פועלת לפי best-practices ולא לפי הקול של הצוות. **קיים כעת corpus התחלתי** של החלטות סגנון ב-[[yael-rewrites]] שאפשר לזקק ממנו.
- אילו דוגמאות סגנון יוצבו ב-`yael/reference/`? כמות מינימלית מומלצת היא 3 דוגמאות מגוונות (טון רשמי, סיפורי, טכני). הפלט של מאמר CRM יכול לשמש כדוגמה ראשונה.
- האם להוסיף slash command (כמו `/draft <file>`) שמפעיל את יעל ישירות, או להישאר עם ניתוב טבעי דרך ראובן?
- הסוכן `yael` לא מופיע ב-Agent registry של הסשן הנוכחי (חייב restart לרישום). האם זה צפוי או באג בקונפיג?

## Session Log

### 2026-05-13 — יצירת yael.md ותשתית סביבה [shipped]
- **What was done:**
  - יצירת `.claude/agents/yael.md` — agent עם frontmatter תקני (`name: yael`, `tools: Read,Write,Edit,Glob,Grep`, `model: opus`) ו-system prompt מובנה ב-8 חלקים: זהות, קריאת סגנון, flow 5-שלבי, template HTML, כללי תוכן קשיחים, גבולות, פורמט דיווח, דוגמה.
  - יצירת `yael/style-guide.md` כ-stub עם כותרות סעיפים ריקות (Tone, Voice, Vocabulary, Sentence length, Paragraph, Do/Don't).
  - יצירת תיקיות עבודה עם `.gitkeep`: `yael/reference/`, `Content/`, `Output/`.
  - עדכון `CLAUDE.md`: סעיף "ניתוב" חדש עם trigger keywords (עברית + English), קלט/פלט, כלים. סטטוס יעל שונה ל-"מוגדרת".
  - עדכון `.gitignore`: `Content/* !Content/.gitkeep` ואותו דבר ל-Output — שומר את מבנה הספרייה ב-git, מסתיר את התוכן.
  - אומת ש-`git check-ignore Content/foo.txt` עובד ושה-`.gitkeep` נשאר tracked.
- **Decisions:**
  - **Model: opus** (claude-opus-4-7) — נבחר ע"י המשתמש, איכות מקסימלית לתוכן.
  - **HTML styling: inline CSS מינימלי** ולא template file חיצוני — קובץ עצמאי שניתן להעביר, פחות תלויות.
  - **Content/ + Output/ ignored** ב-git (אך `.gitkeep` משאיר את הספרייה) — תכנים נכנסים/יוצאים הם working files של כל מפעיל, לא חלק מהריפו.
  - **system prompt בעברית** — מיישר את יעל עם הפרויקט שכולו עברית (CLAUDE.md, vault).
  - **גבולות מפורשים בפרומפט** — כללי "אסור להוסיף קישורים/CTAs" וגם "אסור להמציא תוכן" — מקודדים את כללי המשתמש כדי להבטיח עמידה.
- **Notes / Caveats:**
  - `yael/style-guide.md` כרגע stub — עד שיתמלא, יעל כותבת לפי best-practices ומציינת זאת.
  - `yael/reference/` ריק כרגע — אין דוגמאות סגנון מוחשיות.
  - לא הורץ end-to-end test (אין עדיין מאמר דוגמה ב-Content/).
  - יובל וחן עוד לא הוגדרו — `agents/` מכילה רק את `yael.md` ואת `.gitkeep`.
- **Related:** [[claude-team-config]], [[project-overview]], [[root-config-files]], [[skills-inventory]]

### 2026-05-13 — End-to-end test ראשון + חריגת סמכות [shipped]
- **What was done:**
  - **קלט:** `Content/crm מאמר.txt` (~1,050 מילים, עברית, מאמר על CRM).
  - **דיספאץ׳:** הסוכן `yael` לא נמצא ב-Agent registry של הסשן (`Agent type 'yael' not found`) — חייב restart לרישום. ראובן השתמש ב-workaround: הפעלת `general-purpose` עם ה-system prompt המלא של יעל כפרומפט. תוצאתית זהה.
  - **פלט:** `Output/crm-article.md` ו-`Output/crm-article.html` נכתבו תקין. HTML עצמאי, `lang="he" dir="rtl"`, CSS inline (כולל תוספת של יעל ל-`ul { padding-right: 1.5rem; }` — שיפור ויזואלי קטן ל-RTL שלא היה ב-template).
  - **התוצר עצמו טוב:** CTA הסופי הוסר, "התלמידים שלי" שונה לניסוח כללי, Make וסיפור הפיצה נשמרו לפי ההנחיה, אורך ירד מ-~1,050 ל-~880 מילים.
- **Decisions:**
  - **שמירת הפלט של יעל ב-vault** ([[yael-rewrites]]) למרות שזו חריגה מסמכותה — הקובץ איכותי ושימושי כ-corpus התחלתי לזיקוק style-guide. אבל **חיזוק הפרומפט של יעל** כדי שלא תכתוב ל-vault בעתיד.
  - **לא משחזרים את הניסיון להפעיל `yael` ישירות** עד שייערך restart לסשן.
- **Notes / Caveats:**
  - **חריגת סמכות של יעל:** הסוכן יצר באופן עצמאי את `vault/Meeting Notes/yael-rewrites.md` וגם הוסיף שורה ל-`vault/Meeting Notes/_index.md`. ה-system prompt שלה לא הורה לעשות זאת — תיעוד ב-vault הוא תחום של ראובן, לא של יעל. ייעודית: יעל נחשפה לתוכן של ה-skill obsidian-vault-workflow (שטעון בקונטקסט של הסשן) ו"לקחה יוזמה". יש להוסיף הוראה מפורשת ב-`yael.md` שאסור לה לכתוב ל-`vault/`.
  - **בדיקת רינדור HTML בדפדפן** לא בוצעה — אומת רק שהקובץ כתוב תקין textually.
- **Related:** [[yael-rewrites]], [[claude-team-config]], [[project-overview]]

### 2026-05-13 — הרחבת yael.md ל-image placeholders + שילוב עם יובל [shipped]
- **What was done:**
  - שלב 4 ("שכתוב") ב-yael.md הורחב עם פרוטוקול לסימון `{{IMAGE_NEEDED: "<english description>"}}` במקומות שתמונה תוסיף ערך. דוגמה מלאה ופירוט מה לכלול בתיאור (subject, style, mood, palette).
  - סעיף "דיווח חזרה" הורחב — יעל תחזיר רשימה מנוסחת של ה-placeholders שהשארה + המיקום שלהם.
  - **גבולות לא שונו** — יעל עדיין לא יוצרת תמונות. רק מסמנת איפה.
  - הזרימה המשולשת מתועדת ב-CLAUDE.md ("זרימת עבודה — מאמר עם תמונות"): יעל → ראובן קורא ליובל לכל placeholder → cp ל-`Output/images/` → Edit החלפה.
- **Decisions:**
  - **תיאור placeholder באנגלית** — יעל כותבת בעברית אבל ה-prompt לתמונה חייב להיות באנגלית (OpenAI Images עובד טוב יותר). זה מבלבל בקריאה אבל הכרחי לתוצאה.
  - **Sweet spot של 2 placeholders** — לא חוקתי. יעל יכולה להוסיף יותר אם בא לה, אבל היא תדווח לראובן ש"יש הרבה תמונות" כדי לוודא שהוא מסכים.
  - **`{{IMAGE_NEEDED: "..."}}`** — סוגריים מסולסלים כפולים (Mustache-like). אין סבירות שזה יופיע בטקסט אמיתי, וקל ל-Grep/Edit.
- **Notes / Caveats:**
  - השאלה הפתוחה "מתי ייכתבו יובל וחן" נסגרת חלקית — יובל נכתב היום ([[yuval-agent-definition]]). נשארת חן.
- **Related:** [[yuval-agent-definition]], [[skill-gpt-image-gen]], [[claude-team-config]]
