
# GitHub API Interaction Guidelines

This document codifies all rules for interacting with the GitHub REST API v3 in the context of programmatically managing LaTeX-based documents, including `.tex` section files and structural manifests like `toc.json`.

---

## 🔐 Authentication and Permissions

- All requests must be authenticated via token or secure method approved by the GitHub REST API.
 - Ensure repository permissions allow read/write access to contents.

---

## 🔔 File Creation and Updates

### Create or Update Endpoint

Use:

```text
**PUT /repos/{owner}/{repo}/contents/{path}** ```

### Required Parameters: 
- `message`: A commit message (string).
 - `content` : Base64-encoded UTF-8 content (string).
 - `sha` *(optional)*: Required only when updating an existing file.

### File Existence Workflow:

1. Check file status:

```text
GET /repos/{owner}/{repo}/contents/{path}
```
2. If file exists, extract `sha`.
3. If file does not exist, omit `sha` in the PUT request.
