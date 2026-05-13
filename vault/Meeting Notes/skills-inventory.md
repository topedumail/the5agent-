---
name: skills-inventory
description: מלאי כל ה-skills הזמינים תחת .claude/skills/ — תפקיד כל אחד ולמי הוא רלוונטי
type: reference
---

# Skills Inventory

## Overview

זוהי **רשימת כל היכולות** (skills) המותקנות תחת `.claude/skills/`. סוכן יכול להפעיל skill דרך כלי `Skill` כאשר משימה תואמת את התיאור שלו. ה-skills מתחלקים לשתי משפחות עיקריות:

1. **Superpowers** — סט פרקטיקות הנדסיות (תכנון, debugging, code review, TDD וכו'). מקור: commit `bbf4f3c`.
2. **Obsidian** — עבודה עם vault, markdown ו-bases.

ה-skill הקריטי לפרויקט זה: **`obsidian-vault-workflow`** — חובה בתחילת ובסוף כל משימה.

## רשימת ה-Skills

### Obsidian (3) — רלוונטי לכל הצוות

| Skill | תיאור | מתי מפעילים |
|---|---|---|
| `obsidian-vault-workflow` | **חובה.** פרוטוקול קריאה/כתיבה ל-vault — קונטקסט בתחילה, סיכום בסוף | **בתחילת ובסוף כל משימה** |
| `obsidian-markdown` | יצירה ועריכת Obsidian-flavored markdown (wikilinks, callouts, frontmatter, embeds) | כתיבת קבצי `.md` בתוך ה-vault |
| `obsidian-bases` | יצירה ועריכת קבצי `.base` (תצוגות table/card עם פילטרים ופורמולות) | יצירת תצוגות דמויות-DB מעל ה-vault |

### Superpowers — תהליכי פיתוח (15)

| Skill | תיאור | למי |
|---|---|---|
| `brainstorming` | חקירת כוונה ודרישות לפני עבודה יצירתית | ראובן (לפני כל בנייה) |
| `writing-plans` | כתיבת תוכניות מימוש למשימות מרובות-שלבים | ראובן |
| `executing-plans` | ביצוע תוכנית קיימת עם נקודות review | ראובן |
| `subagent-driven-development` | ביצוע משימות עצמאיות בתוך הסשן | ראובן |
| `dispatching-parallel-agents` | הרצת 2+ משימות עצמאיות במקביל | ראובן |
| `test-driven-development` | TDD — מבחנים לפני קוד | מפתחים |
| `systematic-debugging` | debugging שיטתי לבאג / טסט שנכשל | מפתחים |
| `verification-before-completion` | אימות לפני הכרזה על "סיום" | כולם |
| `requesting-code-review` | בקשת code review לפני merge | מפתחים |
| `receiving-code-review` | טיפול ב-feedback של code review | מפתחים |
| `using-git-worktrees` | בידוד workspace עם git worktrees | מפתחים |
| `finishing-a-development-branch` | סגירת branch — merge / PR / cleanup | מפתחים |
| `writing-skills` | יצירה ועריכה של skills חדשים | תחזוקת תשתית |
| `using-superpowers` | מטא-skill — איך למצוא ולהשתמש בכל ה-skills | ראובן (בתחילת כל סשן) |

### Skills גלובליים (Anthropic, Cowork) — לא ב-`.claude/skills/` אלא ברמת המערכת

| Skill | תיאור |
|---|---|
| `anthropic-skills:docx` | יצירה/עריכה של Word docs |
| `anthropic-skills:pdf` | עבודה עם PDFs |
| `anthropic-skills:pptx` | יצירה/עריכה של presentations |
| `anthropic-skills:xlsx` | spreadsheets |
| `anthropic-skills:firecrawl` | web scraping ו-crawling (רלוונטי לחן) |
| `anthropic-skills:skill-creator` | יצירת skills חדשים |
| `claude-api` | פיתוח עם Claude API / Anthropic SDK |
| `loop` / `schedule` | משימות מחזוריות / מתוזמנות |
| `update-config`, `keybindings-help`, `fewer-permission-prompts` | תחזוקת harness של Claude Code |

## Open Questions

- האם יותקנו skills מותאמים אישית לפרויקט? (למשל `draft-instagram-post` ליעל, `generate-cover-image` ליובל) — skill-creator כבר זמין, ראה [[skill-creator-install]]
- האם יוגדר skill מותאם לחן עבור research pipeline (Tavily → Firecrawl → סיכום)?
- 11 ה-skills של example-skills שלא ביקשנו במפורש (algorithmic-art, brand-guidelines, canvas-design, doc-coauthoring, frontend-design, internal-comms, mcp-builder, slack-gif-creator, theme-factory, web-artifacts-builder, webapp-testing) פעילים — האם להסיר את ה-plugin כשהצרכים מתבהרים?

## Session Log

### 2026-05-13 — מיפוי כל ה-skills הזמינים [shipped]
- **What was done:** ספירה וקטלוג של כל ה-skills תחת `.claude/skills/` (18) + skills גלובליים מהמערכת. סיווג לפי משפחה (Obsidian / Superpowers / Anthropic Globals) ולפי הסוכן שצריך אותם.
- **Decisions:** הסומן את `obsidian-vault-workflow` כ"קריטי" — הוא חייב לרוץ בתחילת/סוף כל משימה. תיעדנו אותו גם בזיכרון של Claude כדי שהפעלתו תהיה אוטומטית.
- **Notes / Caveats:** מצאנו תיקיית כפילות `obsidian-bases-20260513T112230Z-3-001` שצריך לבדוק אם זה backup. לא נמחק לבד — דורש אישור משתמש.
- **Related:** [[project-overview]], [[claude-team-config]], [[obsidian-vault-setup]]

### 2026-05-13 — פירוט פר-skill + מחיקת תיקיית כפילות [shipped]
- **What was done:** יצירת קובץ נושא נפרד לכל אחד מ-17 ה-skills תחת `vault/Meeting Notes/skill-*.md`. עדכון `_index.md` עם קטגוריזציה (Obsidian / תכנון / מקביליות / איכות / git / תשתית). אימות שתיקיית `obsidian-bases-20260513T112230Z-3-001/` מכילה SKILL.md זהה (`diff` עבר נקי) ומחיקתה — סה"כ ירדנו מ-18 ל-17 skills.
- **Decisions:** דגם של "skill-` prefix" בתוך Meeting Notes שומר על מבנה שטוח (כפי שה-skill דורש) תוך הקלת ניווט לקסיקלי. הכפילות נמחקה בהיתר מפורש של המשתמש ("תסיים מה שהתחלת אני מאפשר").
- **Notes / Caveats:** ספירת ה-skills בטבלה למעלה צריכה עדכון מ-18 ל-17 בעת קריאת הקובץ הבאה — נטפל אם תידרש עדכון מהותי לטבלה.
- **Related:** [[skill-obsidian-vault-workflow]], [[skill-obsidian-bases]], [[skill-using-superpowers]], [[obsidian-vault-setup]]

### 2026-05-13 — הוספת plugin example-skills (project scope) [shipped]
- **What was done:** הותקן `example-skills@anthropic-agent-skills` ב-project scope. הביא 12 skills נוספים, כולל skill-creator. עודכן `.claude/settings.json` (commit `19e6d92`). ראה תיעוד מלא ב-[[skill-creator-install]].
- **Decisions:** התקנה דרך plugin (לא העתקה ידנית) — שומרת על update semantics. ה-11 skills הנוספים יישארו ישנים אלא אם תיאוריהם תואמים למשימה.
- **Notes / Caveats:** מספר ה-skills תחת `.claude/skills/` עדיין 17 (Obsidian + Superpowers); ה-12 החדשים יושבים תחת `~/.claude/plugins/.../anthropic-agent-skills/skills/` ולא ב-repo.
- **Related:** [[skill-creator-install]], [[skill-writing-skills]]
