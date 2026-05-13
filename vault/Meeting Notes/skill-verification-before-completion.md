---
name: skill-verification-before-completion
description: Skill — וידוא לפני הצהרה על "סיום" (ראיות לפני טענות)
type: reference
---

# Skill: verification-before-completion

## Overview

**מיקום:** `.claude/skills/verification-before-completion/`
**משפחה:** Superpowers
**משויך ל:** כל הצוות (לפני כל הצהרה על סיום משימה)
**מתי להפעיל:** ממש לפני שטוענים שהעבודה הושלמה, תוקנה, או עוברת מבחנים — לפני commit או יצירת PR. ה-skill מחייב הרצת פקודות וידוא והצגת הפלט לפני כל טענה על הצלחה. **ראיות לפני טענות, תמיד.**

## Open Questions
- none

## Session Log

### 2026-05-13 — תיעוד ראשוני [shipped]
- **What was done:** תיעוד ה-skill.
- **Decisions:** מסומן כקריטי לתרבות הצוות — מונע מהסוכנים להצהיר על "הצלחה" בלי הוכחה.
- **Related:** [[skills-inventory]], [[skill-systematic-debugging]], [[skill-requesting-code-review]]
