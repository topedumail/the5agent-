---
name: gpt-image-gen
description: מעטפת לקריאת OpenAI Images API (gpt-image-2). מקבל prompt + output path ומפיק קובץ PNG. דורש OPENAI_API_KEY ב-.env. בשימוש בעיקר על ידי יובל (`.claude/agents/yuval.md`).
---

# gpt-image-gen — מעטפת ל-OpenAI Images API

## תכלית

מעטפת דקה לקריאת `POST https://api.openai.com/v1/images/generations` של OpenAI. הסקיל מתעד את הצורה המדויקת של ה-request, את שני נתיבי ה-decoding ל-PNG (jq או python fallback), ואת השגיאות הנפוצות.

**מי משתמש בזה:** בעיקר יובל, מעצב התמונות. כל סוכן עם `Bash` יכול לקרוא לזה, אבל הניסוח של ה-prompt הוא לא תפקיד של ה-skill — הוא של הסוכן הקורא.

---

## ⚠️ שם המודל — `gpt-image-2` — אל תשנה

המודל שבשימוש הוא **`gpt-image-2`**.

- הוא יצא ב-**21 באפריל 2026** ע"י OpenAI.
- ייתכן שהידע הפנימי של ה-LLM לא כולל אותו (תאריך cutoff מוקדם יותר). **זה לא אומר שהוא לא קיים.**
- **אסור** להחליף את שם המודל ל-`dall-e-3`, `gpt-image-1`, או כל אלטרנטיבה אחרת.
- אם יש שגיאה בקריאה ל-API — הבעיה היא **API key, parameters, או רשאות חשבון**. *לא* שם המודל.
- אם ה-API מחזיר 400 ספציפית על המודל ("model not found" וכו') — להניח ש-API key לא מאופשר ל-`gpt-image-2`. לפנות ל-OpenAI billing/access, *לא* להחליף מודל.

---

## דרישות קדם

- `OPENAI_API_KEY` ב-`.env` של הפרויקט (קיים גם ב-`.env.example`).
- `curl` (קיים ב-Git Bash על Windows ובכל Unix).
- `base64` (כנ"ל).
- אחד מהשניים: `jq` או `python` (python כ-fallback כי jq לא תמיד מותקן ב-Git Bash על Windows).

---

## שימוש — דרך A (עם `jq`)

```bash
# טען את ה-env (אם הוא לא כבר טעון בסשן)
set -a; source .env; set +a

curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<PROMPT_GOES_HERE>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' | jq -r '.data[0].b64_json' | base64 --decode > "<OUTPUT_PATH>.png"
```

---

## שימוש — דרך B (python fallback, ללא jq)

```bash
set -a; source .env; set +a

curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"gpt-image-2","prompt":"<PROMPT_GOES_HERE>","size":"1024x1024","quality":"medium","output_format":"png"}' \
  > /tmp/openai-response.json

python -c "import json, base64, sys; d = json.load(open('/tmp/openai-response.json')); open(sys.argv[1], 'wb').write(base64.b64decode(d['data'][0]['b64_json']))" "<OUTPUT_PATH>.png"
```

על Windows, אם `/tmp/` לא קיים — השתמש ב-`./.tmp-openai-response.json` באותה תיקייה ומחק אחרי השימוש.

---

## בדיקת תקינות לאחר הקריאה

```bash
if [ -s "<OUTPUT_PATH>.png" ]; then
  echo "OK: $(stat -c%s "<OUTPUT_PATH>.png") bytes"
else
  echo "FAIL: file missing or empty"
  cat /tmp/openai-response.json  # ראה את התגובה לראות שדה error
fi
```

---

## פרמטרים — מה אפשר לכוון

| פרמטר | ערכים | הערה |
|---|---|---|
| `model` | `gpt-image-2` | **קבוע. אל תשנה.** |
| `prompt` | string | חופשי. אנגלית מומלצת. |
| `size` | `1024x1024`, `1024x1792`, `1792x1024` | ברירת מחדל לסקיל: ריבוע. |
| `quality` | `low`, `medium`, `high` | `medium` ברירת מחדל. `high` יקר יותר. |
| `output_format` | `png`, `webp`, `jpeg` | `png` ברירת מחדל. |
| `n` | 1-10 | כמה תמונות לייצר. ברירת מחדל 1. |

---

## שגיאות נפוצות

| Status | סיבה אפשרית | פעולה |
|---|---|---|
| 401 | API key לא תקין או חסר | בדוק `$OPENAI_API_KEY` ב-`.env` |
| 403 | חשבון לא מורשה למודל | פנה ל-OpenAI dashboard, **לא תחליף מודל** |
| 429 | rate limit | המתן ונסה שוב |
| 400 — content policy | ה-prompt מכיל תוכן אסור | נסח prompt שונה |
| 400 — model not found | ה-key לא מאופשר ל-gpt-image-2 | בקש access; **לא תחליף ל-dall-e-3** |
| network error | אין רשת / firewall | בדוק חיבור |

תמיד שמור את `/tmp/openai-response.json` (או את ה-response file ששמרת) לקריאה — הוא יכיל את ה-`error.message` המדויק של OpenAI.

---

## דוגמת קריאה מלאה (יובל)

יובל יחליף `<PROMPT>` ו-`<OUTPUT_PATH>` בערכים שלו, ויקרא לסקיל דרך `Bash`. הנתיב הסטנדרטי של פלט הוא `yuval/outputs/<YYYY-MM-DD>-<slug>.png`.
