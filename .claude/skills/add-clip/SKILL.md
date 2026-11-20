---
name: add-clip
description: Add one of Emma's published stories to the Clips section of her site from a link, using the headline exactly as published plus the outlet and date, then push it live. Use when Emma runs /add-clip with a story link, or asks to add a clip or story to her site.
argument-hint: <link to the story>
---

# Add a clip

Emma gives you a link to one of her published stories. Add it to the Clips section of `index.html`, push it live, and tell her what you added.

Follow the rules in `CLAUDE.md` throughout, especially: use her words exactly, never add facts she didn't give, commit straight to main.

## 1. Get the link

The link is in the arguments: $ARGUMENTS

If there is no link, ask Emma for one and stop.

## 2. Open the link and read three things

Open the page (with WebFetch, or the web-fetch agent if you have no WebFetch tool; `curl` is fine as a fallback). Find:

- **Headline:** the headline as published on the story page itself, the large heading at the top of the article. Copy it character for character: same words, capitalization, punctuation, and quote marks.
  - Never rewrite, shorten, summarize, fix or "improve" it.
  - Don't use the browser tab title if it differs from the headline on the page. Tab titles often add the outlet name (" - The Washington Sun") or are cut short.
  - If the page shows more than one possible headline (for example a different headline in the share preview than on the article), or a subheadline, use only the main headline on the article and mention the difference to Emma.
- **Outlet:** the publication's name as it appears on the site (for example "The Washington Sun").
- **Date:** the publication date shown on the story. Use the date the story was published, not "updated" dates. Write it as Month Day, Year, for example "November 20, 2026".

**If you can't open the link, or can't find all three clearly on the page** (paywall, login wall, error, blocked, no date shown, anything unclear): stop and ask Emma for the headline, outlet and date. Tell her which part you couldn't get and why. Never guess, and never fill in a headline from memory, the URL slug, or search results.

## 3. Check for duplicates

If this link (or the same headline) is already in the Clips section, tell Emma and don't add it again.

## 4. Add it to index.html

Pull first: `git pull --ff-only origin main` on the `main` branch.

The Clips section is `<section id="clips">`.

**If it still has the placeholder** (`<!-- CLIPS GO HERE ... -->` and `<p class="placeholder">...`), replace both with a list holding this first clip:

```html
      <ul class="clip-list">
        <li class="clip">
          <p class="clip-meta">OUTLET · DATE</p>
          <h3 class="clip-headline"><a href="URL" target="_blank" rel="noopener">HEADLINE</a></h3>
        </li>
      </ul>
```

**If the list already exists**, insert a new `<li class="clip">` block (same markup as above, including `target="_blank" rel="noopener"` so the story opens in a new tab) in date order, newest first. Usually that means the top of the list. If this story is older than some clips already listed, put it in its correct date position instead, and tell Emma where it went.

Details:
- `URL` is the link Emma gave, as is (remove only tracking parameters like `utm_...`).
- In `HEADLINE`, `OUTLET` and `DATE`, escape `&` as `&amp;`, `<` as `&lt;`, `>` as `&gt;`. Keep curly quotes and apostrophes exactly as published.
- Type the outlet and date normally. The page's styling shows them in small capitals; don't type them in capitals.
- Don't change the styles. They already exist in `index.html` (`.clip-list`, `.clip`, `.clip-meta`, `.clip-headline`). Keep the "Site style" in `CLAUDE.md`.
- Don't touch any other part of the page.

## 5. Push it live

```bash
git add index.html
git commit -m "Add clip: HEADLINE"
git push origin main
```

If the push fails, tell Emma plainly that the clip is saved but not live yet, and why.

## 6. Tell Emma what you added

In plain words, tell her:
- the headline, outlet and date exactly as you added them,
- where in the list it went,
- that the live site can take a minute or two to update.

If she gave you the details herself because the link didn't open, say so.
