# Meeting Notes — Index

תיעוד מבנה הפרויקט "צוות 5 הסוכנים" — כל קובץ נושא מתאר אזור אחד של הפרויקט: לאיזה קובץ הוא מתייחס, למי הוא משויך, ולמה הוא משמש.

## Topics — סקירה כללית

- [[project-overview]] — מבט-על על הפרויקט, מטרתו, ומבנה הצוות (ראובן + יעל, יובל, חן)
- [[root-config-files]] — קבצי השורש: `CLAUDE.md`, `.env`, `.env.example`, `.gitignore`
- [[claude-team-config]] — תיקיית `.claude/` ותתי-התיקיות agents, commands, skills
- [[obsidian-vault-setup]] — תיקיית `.obsidian/` ותיקיית `vault/` — הזיכרון ארוך הטווח של הפרויקט
- [[skills-inventory]] — מלאי כל היכולות (skills) — סקירה כללית עם טבלה

## Topics — פעולות/התקנות

- [[skill-creator-install]] — התקנת example-skills plugin ב-project scope (כולל skill-creator)

## Topics — פירוט פר-skill (17)

### Obsidian (3)
- [[skill-obsidian-vault-workflow]] — **קריטי**: פרוטוקול קריאה/כתיבה ל-vault, חובה בכל משימה
- [[skill-obsidian-markdown]] — תחביר Obsidian (wikilinks, callouts, frontmatter, embeds)
- [[skill-obsidian-bases]] — קבצי `.base` עם תצוגות, פילטרים, פורמולות

### תכנון וביצוע (4)
- [[skill-brainstorming]] — חקירת כוונה לפני עבודה יצירתית
- [[skill-writing-plans]] — כתיבת תוכניות מימוש
- [[skill-executing-plans]] — ביצוע תוכנית עם נקודות review
- [[skill-subagent-driven-development]] — פיצול תוכנית ל-subagents בסשן

### עבודה במקביל (1)
- [[skill-dispatching-parallel-agents]] — הפעלת 2+ משימות עצמאיות במקביל

### איכות וקוד (5)
- [[skill-test-driven-development]] — מבחנים לפני קוד
- [[skill-systematic-debugging]] — גישה שיטתית ל-debugging
- [[skill-verification-before-completion]] — ראיות לפני טענות על "סיום"
- [[skill-requesting-code-review]] — בקשת review בסיום
- [[skill-receiving-code-review]] — טיפול ב-feedback של review

### זרימת עבודה ב-git (2)
- [[skill-using-git-worktrees]] — בידוד workspace ב-worktrees
- [[skill-finishing-a-development-branch]] — סגירת branch (merge / PR)

### תחזוקת תשתית (2)
- [[skill-using-superpowers]] — meta: איך למצוא ולהשתמש ב-skills
- [[skill-writing-skills]] — יצירה ועריכה של skills חדשים
