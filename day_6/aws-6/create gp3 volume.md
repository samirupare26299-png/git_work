# How to Upload Notes on GitHub

A quick guide to get your notes onto GitHub in minutes.

---

## Prerequisites

- A [GitHub account](https://github.com)
- Git installed on your machine (`git --version` to check)

---

## Method 1: Using the GitHub Website (Easiest)

1. **Create a new repository**
   - Go to [github.com](https://github.com) → click **"New"** (green button)
   - Give it a name (e.g., `my-notes`), set visibility, click **"Create repository"**

2. **Upload your file**
   - Inside your repo, click **"Add file" → "Upload files"**
   - Drag and drop your notes file (`.md`, `.txt`, `.pdf`, etc.)
   - Scroll down, add a commit message (e.g., `Add notes`), click **"Commit changes"**

---

## Method 2: Using Git (Command Line)

```bash
# Step 1: Initialize a local repo (skip if already done)
git init my-notes
cd my-notes

# Step 2: Copy your notes file into the folder, then add it
git add notes.md

# Step 3: Commit
git commit -m "Add my notes"

# Step 4: Link to GitHub repo (replace URL with yours)
git remote add origin https://github.com/YOUR_USERNAME/my-notes.git

# Step 5: Push
git push -u origin main
```

---

## Method 3: Create a Note Directly on GitHub

1. Inside a repo, click **"Add file" → "Create new file"**
2. Name it `notes.md`
3. Type your content in the editor
4. Click **"Commit new file"**

---

## Tips

| Tip | Details |
|-----|---------|
| **Use Markdown** | Name files `.md` so GitHub renders them with formatting |
| **README.md** | If you name your file `README.md`, it auto-displays on the repo homepage |
| **Private repo** | Set visibility to *Private* if your notes are personal |
| **Folders** | You can organize notes in folders — GitHub shows them as a file tree |

---

## Quick Markdown Cheat Sheet

```markdown
# Heading 1
## Heading 2

**bold**   _italic_

- Bullet item
1. Numbered item

`inline code`

> Blockquote
```

---

*That's it! Your notes are now on GitHub.*
