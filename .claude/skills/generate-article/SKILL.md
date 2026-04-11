---
name: generate-article
description: Generate a bilingual draft article for the EMPIRA blog on AI/automation topics
user_invocable: true
---

# Generate Article — EMPIRA Blog

## Overview

This skill generates a bilingual (FR/EN) draft article for the EMPIRA website blog. It picks the next pending topic from `articles/topics.json`, researches the subject, writes the article, and saves it as a ready-to-review HTML page in `articles/drafts/`.

## Process

### Step 1 — Pick the next topic

Read `articles/topics.json` and select the first topic with `"status": "pending"`. Update its status to `"in-progress"`.

### Step 2 — Research

Launch multiple agents in parallel to research the topic:
- Agent 1: Search for recent news, developments, and statistics on the topic
- Agent 2: Search for practical tools, products, or services relevant to the topic
- Agent 3: Search for best practices, case studies, or expert opinions

All research should be recent (2024-2026) and focused on practical, actionable information.

### Step 3 — Write the article

Based on the research, write a ~1000-word article in BOTH French and English.

**Writing style:**
- Simple, clear, accessible language — written for non-technical readers
- No unnecessary jargon. If a technical term is needed, explain it briefly
- Conversational but professional tone
- Short paragraphs, use of subheadings, bullet points where appropriate
- Concrete examples and real-world applications
- NOT a sales pitch — informative and genuinely useful content
- Do NOT mention EMPIRA services or add CTAs. These are purely informative articles

**Structure:**
- Engaging title (SEO-optimised)
- Brief intro (2-3 sentences, hook the reader)
- 3-5 sections with clear subheadings
- Key takeaways or practical tips
- Brief conclusion

**SEO:**
- Title tag, meta description, and Open Graph tags in both languages
- Natural keyword integration (no stuffing)
- Proper heading hierarchy (h1, h2, h3)
- Alt text on all images

### Step 4 — Find an image

Search for a relevant free stock image on Unsplash or Pexels. Use the image URL directly from their CDN (no download needed). If no good stock image fits, use a Lucide icon illustration instead.

### Step 5 — Generate the HTML page

Create the article as a standalone HTML page that matches the EMPIRA website design:
- Same Tailwind CDN, color palette (gold #a3891f, dark #161616), fonts (Montserrat + Inter)
- Same navbar and footer as the main site
- Language switcher (FR/EN) using the same `data-i18n` system from `js/main.js`
- The article translations should be added to a `<script>` block within the page
- Responsive, readable layout with good typography (max-width ~720px for content)
- Include reading time estimate
- Include publication date

Save to: `articles/drafts/YYYY-MM-DD-slug.html`

### Step 6 — Update topics.json

Set the topic status to `"draft"` and add the filename:
```json
{
  "status": "draft",
  "draft_file": "articles/drafts/YYYY-MM-DD-slug.html",
  "draft_date": "YYYY-MM-DD"
}
```

### Step 7 — Commit

Commit the draft and updated topics.json with message:
```
draft: [Article title FR]
```

## Publishing workflow (manual)

When the user approves a draft:
1. Move the file from `articles/drafts/` to `articles/published/`
2. Update `topics.json` status to `"published"`
3. Regenerate the resources listing page
4. Deploy via `/deploy-ftp`
