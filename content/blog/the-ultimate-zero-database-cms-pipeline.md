---
title: "The Ultimate Zero-Database CMS Pipeline"
pubDate: "2026-05-26"
description: "A deep dive into bypassing heavy databases using Git, serverless endpoints, and raw Markdown."
---

# The Ultimate Zero-Database CMS Pipeline

When building a personal portfolio, adding a traditional database (like MongoDB or PostgreSQL) often introduces unnecessary overhead. You have to handle connections, pay for hosting instances, manage backups, and deal with complex visual text editors. 

This blog leverages a **Git-as-a-CMS** pattern to completely bypass those bottlenecks.

## ⚙️ How It Works Under the Hood

Instead of writing to a database cluster, our admin form talks directly to a serverless API endpoint. That endpoint uses the GitHub REST API to commit files right into the codebase repository.

Here is the exact structure used to securely update files with the GitHub API:

```javascript
// PUT request structure to save content straight to GitHub
const response = await fetch(`https://api.github.com/repos/${owner}/${repo}/contents/${path}`, {
  method: 'PUT',
  headers: {
    'Authorization': `Bearer ${process.env.GITHUB_TOKEN}`,
    'Accept': 'application/vnd.github+json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    message: `cms: publish "${title}"`,
    content: base64Payload, // Text must be base64 encoded
    sha: existingFileSha    // Required if updating an existing file!
  })
});