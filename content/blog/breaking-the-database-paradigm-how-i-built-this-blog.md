---
title: "Breaking the Database Paradigm: How I Built This Blog"
pubDate: "2026-05-26"
description: "Auto-published from Admin Editor."
---

---
title: "Breaking the Database Paradigm: How I Built This Blog"
pubDate: 2026-05-26
description: "Ditching traditional databases for an automated, Git-backed Markdown pipeline."
author: "Chemitha"
---

# Breaking the Database Paradigm

Welcome to my first blog post! This entire platform runs completely **database-less**. Instead of relying on a clunky SQL or NoSQL database, every single post you read here is parsed directly from a plain-text Markdown file. 

## 🛠️ The Architecture

When I publish a post from my secret web editor, it triggers a lightweight pipeline:

1. **Frontend Request:** Encodes the text payload into Base64.
2. **GitHub API Call:** Pushes the file directly into my `Noir-Chaperone` repository.
3. **Automated CI/CD:** The host detects the repository commit and rebuilds the static pages in seconds.

### The API Payload Structure

Here is a quick look at how the backend formats the commit payload securely using a Serverless function:

```javascript
const fileContent = `---
title: "${title}"
pubDate: "${new Date().toISOString().split('T')[0]}"
---

${content}`;

// Base64 encoding required by the GitHub Contents REST API
const base64Content = btoa(unescape(encodeURIComponent(fileContent)));