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

### Step 2 — Research (in-depth)

Launch multiple agents in parallel to research the topic **in depth**:
- Agent 1: Search for **scientific papers, benchmarks, and technical reports** (arXiv, Google Scholar, official technical reports from OpenAI/Anthropic/Google/Meta, research blogs). Look for hard data: benchmark scores, quantitative comparisons, peer-reviewed findings.
- Agent 2: Search for **expert opinions and analysis** from recognised voices in the field (Andrej Karpathy, Simon Willison, Ethan Mollick, Yann LeCun, industry analysts at Gartner/McKinsey/BCG, etc.). Look for blog posts, newsletters, conference talks, and interviews that provide unique insights.
- Agent 3: Search for **real-world case studies, enterprise deployments, and practical experience reports**. Look for companies that have published results, ROI data, implementation challenges, or lessons learned.

All research should be recent (2024-2026). Prioritise **primary sources** over generic blog posts. Cite sources in the article (author name, publication, date) to build credibility.

### Step 3 — Write the article

Based on the research, write a ~1000-word article in BOTH French and English.

**Writing style:**
- **Professional and technically informed** — write as an expert who can explain complex topics clearly, not as a beginner summarising Wikipedia
- Technical terms are welcome when relevant, but always explained in context
- Show depth of knowledge: cite specific benchmarks, numbers, expert quotes, and real examples
- Offer **original analysis and perspective** — don't just list features, explain WHY something matters and WHAT it means for businesses
- Challenge common assumptions when appropriate ("you might think X, but actually...")
- NOT a sales pitch — informative and genuinely useful content
- Do NOT mention EMPIRA services or add CTAs. These are purely informative articles
- The tone should position the author as a knowledgeable specialist who has done the research, not someone parroting marketing materials

**Structure:**
- Engaging, specific title (SEO-optimised, avoid generic clickbait)
- Strong opening that shows expertise (a surprising stat, a contrarian take, a concrete problem)
- 3-5 sections with clear subheadings
- Data, citations, and expert references throughout
- Practical takeaways grounded in evidence
- Conclusion with a clear, opinionated perspective

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
