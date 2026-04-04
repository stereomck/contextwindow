# Context Window — Site README
**CW:[]** · contextwindow.to · Maintained by Claude

---

## Site structure

```
contextwindow/
├── index.html          # Homepage + post index — update when adding posts
├── about.html          # About page — rarely updated
├── feed.xml            # RSS feed — update when adding posts
├── CNAME               # Custom domain: contextwindow.to
│
├── css/
│   └── style.css       # All brand CSS — edit here for design changes
│
└── posts/
    └── what-is-an-ai-pm.html   # One .html file per post
```

**How GitHub Pages works:** push any file to this repo and GitHub serves it as a live website at contextwindow.to. No build step. No server. Just files.

---

## Brand reference

| Token        | Value      | Use                         |
|--------------|------------|-----------------------------|
| `--void`     | `#0D0D0E`  | Background                  |
| `--token`    | `#2FA865`  | Green accent, links, tags   |
| `--token-dim`| `#1A7A4A`  | Borders, secondary green    |
| `--paper`    | `#F5F3EE`  | Primary text                |
| `--noise`    | `#6B7280`  | Secondary text, meta        |
| Serif        | Fraunces   | Titles, display text        |
| Mono         | DM Mono    | Code, labels, nav, meta     |
| Sans         | DM Sans    | Body text                   |

---

## Adding a new post

### Step 1 — Generate the post file

Paste this prompt into Claude (or your ContentOS project):

```
CW:[] new post

Title: [title]
Date: [YYYY-MM-DD]
Tags: [tag1, tag2]
Excerpt: [one sentence description]
Content: [paste the video script or write "generate from Week X calendar"]

Generate the complete posts/[slug].html file for the Context Window site.
Use the same structure as posts/what-is-an-ai-pm.html. Include the full
post body as proper HTML. Return only the file contents.
```

### Step 2 — Update index.html

Paste this prompt into Claude:

```
CW:[] update index

Add this post to contextwindow/index.html post list (newest at top):
- File: posts/[slug].html
- Date: [Month DD, YYYY]  
- Tags: [tag1, tag2]
- Title: [title]
- Excerpt: [excerpt]

Return the updated post-list section of index.html only.
```

### Step 3 — Update feed.xml

Paste this prompt into Claude:

```
CW:[] update feed

Add this entry to contextwindow/feed.xml (newest at top inside <channel>):
- Title: [title]
- URL: https://contextwindow.to/posts/[slug].html
- Date: [Day, DD Mon YYYY 00:00:00 +0000]
- Description: [excerpt]

Return the updated feed.xml file.
```

### Step 4 — Push to GitHub

```bash
git add posts/[slug].html index.html feed.xml
git commit -m "post: [title]"
git push
```

Site is live at contextwindow.to within 30 seconds.

---

## Maintenance prompts

### Change a design element

```
CW:[] design update

In contextwindow/css/style.css, [describe the change].
Example: "change the post tag background to use a border instead of fill"
Example: "increase the hero title font size on mobile"
Example: "add a reading progress bar to post pages"

Return the updated CSS block only, with a comment marking what changed.
```

### Add a new page

```
CW:[] new page

Create [page-name].html for the Context Window site.
Purpose: [what this page is for]
Nav label: [text shown in header nav]
Content: [describe or provide content]

Match the exact structure of about.html — same header, footer, CSS link.
Add the nav link to: index.html, about.html, all posts/*.html, and the new page itself.
Return each file's updated <nav> block and the complete new page.
```

### Update all nav links

```
CW:[] nav update

Add/change/remove this nav item across all Context Window pages:
[describe the change]

Files to update: index.html, about.html, posts/what-is-an-ai-pm.html,
[list any other post files].
Return each file's updated <nav> block only.
```

### Generate RSS feed from scratch

```
CW:[] regenerate feed

Generate a complete feed.xml for Context Window from this post list:
[paste post titles, URLs, dates, and excerpts]

Use the same format as the existing feed.xml.
```

### Add a banner or announcement

```
CW:[] add banner

Add a sitewide announcement banner to Context Window.
Message: [text]
Link: [URL] (optional)
Style: token green, sits between header and main content.
Dismissable: [yes/no]

Return the HTML snippet to add below <header> and any CSS to add to style.css.
```

---

## Post file template

When asking Claude to generate a new post, it will use this structure. You don't need to do anything with this — it's here for reference.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[POST TITLE] — Context Window</title>
  <meta name="description" content="[EXCERPT]">
  <meta name="author" content="Michael Kent">
  <meta property="og:title" content="[POST TITLE]">
  <meta property="og:description" content="[EXCERPT]">
  <meta property="og:type" content="article">
  <meta property="og:url" content="https://contextwindow.to/posts/[SLUG].html">
  <meta property="article:published_time" content="[YYYY-MM-DD]">
  <link rel="canonical" href="https://contextwindow.to/posts/[SLUG].html">
  <link rel="alternate" type="application/rss+xml" title="Context Window" href="/feed.xml">
  <link rel="stylesheet" href="../css/style.css">
</head>
<body>
  [HEADER — identical to other pages]
  <main class="page-wrapper">
    <article>
      <header class="post-header">
        <div class="post-item-tags">
          <span class="post-tag">[TAG]</span>
        </div>
        <h1 class="post-title">[TITLE]</h1>
        <div class="post-meta">
          <span class="post-meta-item">Michael Kent</span>
          <span class="post-meta-sep"></span>
          <span class="post-meta-item">[DATE]</span>
          <span class="post-meta-sep"></span>
          <span class="post-meta-item">[N] min read</span>
        </div>
      </header>
      <div class="post-body">
        [CONTENT]
      </div>
      <footer class="post-footer">
        <a href="/" class="post-back">← All posts</a>
        <div class="post-channel-links">
          <a href="https://postsynaptic.dev">PostSynaptic →</a>
          <a href="https://substack.com/@contextwindow">Newsletter →</a>
        </div>
      </footer>
    </article>
  </main>
  [FOOTER — identical to other pages]
</body>
</html>
```

---

## Git workflow

```bash
# First time setup
git clone https://github.com/stereomck/contextwindow.git
cd contextwindow

# Every time you add/update content
git add .
git commit -m "post: [title]"   # for new posts
git commit -m "fix: [change]"   # for corrections
git commit -m "design: [change]"# for CSS/layout changes
git push

# Check build status
# Go to: github.com/stereomck/contextwindow/actions
```

---

## Shortcodes used in prompts

| Shortcode | Meaning                         |
|-----------|---------------------------------|
| `CW:[]`   | Context Window site task        |
| `PS://`   | PostSynaptic site task          |
| `D/AI`    | Diary of an AI PM task          |

Using these at the start of a prompt in your ContentOS Claude project triggers the right brand context automatically.

---

## Checklist — going live

- [ ] Create GitHub repo named `contextwindow` (public)
- [ ] Push all files from this folder
- [ ] Settings → Pages → Deploy from branch → main / root
- [ ] Buy domain: contextwindow.to (Namecheap or Cloudflare Registrar)
- [ ] Add CNAME record: `www` → `stereomck.github.io`
- [ ] Add A records: four GitHub IPs (185.199.108-111.153)
- [ ] Settings → Pages → Custom domain → contextwindow.to
- [ ] Wait up to 24 hours for DNS propagation
- [ ] Tick "Enforce HTTPS" once it becomes available
- [ ] Update all placeholder URLs (YouTube, LinkedIn, Substack) in HTML files
- [ ] Replace `PLACEHOLDER` in the YouTube link on the first post
- [ ] Update `<!-- POST_COUNT -->` in index.html to `1`
