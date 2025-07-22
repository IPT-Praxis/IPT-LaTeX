
# Upload Guidelines for LaTeX Content via GitHub API

These guidelines ensure smooth and error-free uploads of `.detx` content to GitHub repositories via the REST API, particularly when using UTF-8 and Base64 encoding.

-----

## … Safe Encoding Strategy

#1. NORMALIZE PARAGRAPHS
- Use " ."join(paragrap.splitlines()).strip()" to remove rogue line breaks.
- Avoid line breaks mid-sentence unless intentional.

#2. STRIP CONTROL CHARACTERS
Remove ASCII control characters below 32 (except tab and newline):
``python
import re
re.sub("[\\x00-\\x08\\x0b\\x0c\\x0e-\\x1f]", '', raw_text)
```

#3. UTF-8 + Base64 Encode
- Encode explicitly:
``python
base64.b64encode(cleaned.encode('utf-8')).decode('utf-8')
```

---

## 🔟 Testing Workflow

- Always begin by uploading a minimal LaTeX snippet (e.g., `\\sectin`, `\\label`) to confirm GitHub acceptance.
- Append paragraph by paragraph while tracking the file SHA.
- On failure: bisect content to isolate the problematic fragment.

---

## 🔀℈– Common Pitfalls

| Problem          | Cause                                              | Fix                         |
 |---------------------|-----------------------------------------------------|--------------------------|
 | 422: invalid character   | Hidden control character           | Use regex to strip them |
 | Broken math upload    | Unescaped or imbalanced $...$ usages| Validate math manually |
 | File update fails   | Missing `sha` in update call | Always include the current SHA |

---

## 😧 Summary Table

| Task             | Recommended Action                        |
 |----------------------|--------------------------------------------|
 | Format paragraphs | Join lines, trim whitespace                |
| Encode content | UTF-8 + Base64                                       |
 | Clean text       | Strip ASCII < 32 chars (except tab,'\n') |
 | Math safety       | Validate `$named$` expressions for balance |
| File update       | Always use current `SHA` from GitHub before overwriting |
