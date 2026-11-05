# 37degree — Studio Calendar

The 37degree site, told as a calendar: how the studio started, then every project delivered, on the day it happened.

Static site, no build step:

```
python3 -m http.server 4370
```

then open http://localhost:4370 (v3, the main site), http://localhost:4370/v2/ (white work cards) or http://localhost:4370/v1/ (the first take). Live at https://madebypuru.github.io/37degree-site/.

v2 is the cleaner take: no toolbar, a three-line intro, and two kinds of item. **Notes** are pastel sticky notes: how the studio got here. They open centred with the page blurred, in their own colour. **Work** is white cards: what's been shipped. It opens the case-study side panel. Note colours follow the kind of note (story yellow, people blue, testimonials pink, milestones green, logos purple, snippets orange). Any event can override its colour with `color: 'pink'` etc. in `js/data.js`.

The bottom bar holds the view switch (**Stories**, where the site opens: every card by date; **Projects**: one column per company, newest first, ending with the open slot) and a minimap you can drag (ticks: notes in their colour, projects in ink, today in orange). Each company column in Projects keeps only the essentials linked to its work by Case study or Part of: its folders, the cards with pictures or a film, and one testimonial, sized the way a busy day is in Stories: the same 1-2-1-3 starting heights, folders and quotes at the column's full width, and the cards around them smaller, two side by side at half width. A company whose cards run out of rows grows by one Stories column. Milestones, text-only chapters and notes stay in Stories and inside the folders. A quote linked to the work sits under the column's first card. A case study's folder is followed by its own two cards, built from `js/data.js`: one real screen from the chapter named in its `screen` key (Sidecar's film stands in for it) and its headline result, the first of its `results` that isn't just its length. In Projects a filter offers All work, Case studies, Launch videos, Branding, Websites & product, Pitch decks, Events and Campaigns. It reads each card's Services, or its project's when the card has none, and a case study answers to Case studies plus any Services it's given. With no filter on, two pauses sit between companies, each a full column with a small label and one piece of the studio's own proof: **So far** (the "50+ projects completed" milestone) after the fourth company, and **Trusted by** (the logos card) just before the case studies. Three filter designs are in for review: `?filter=1` one pill by "Work with us" that drops a list (default), `?filter=2` that pill opening sideways into every filter, `?filter=3` a pill growing out of the Projects button with its list opening upward. "Work with us" sits on its own in the bottom-right corner. The sun/moon button top-right switches light and dark; it follows the system setting until someone picks one.

Events with `photos` show a stack of prints on the card. When opened, the note shows a deck you click (or press Enter) to flip through. **The photos in `assets/img/placeholder/` are Pinterest mood placeholders** owned by their creators (sources in `SOURCE.txt`). Replace them with real studio photos before the site goes live. Both versions read the same `js/data.js`.

## Managing content: Pages CMS

Everything on the calendar is a card in `content/calendar/`, one small file each, edited in **Pages CMS** (a free editor for GitHub repos):

1. Go to https://app.pagescms.org and sign in with GitHub.
2. Open `madebypuru/37degree-site`, then **Calendar**.
3. Edit a card or add one, and save. The **Build content** Action rebuilds the site in about a minute.

To bring someone in who doesn't use GitHub, invite them by email from the repo's **Collaborators** settings in Pages CMS.

| Field | What it does |
|---|---|
| Name | The card title. Wrap one word in `*asterisks*` for the Gentium italic accent. For a Testimonial, the person's name. |
| Card | Project, Moment, Milestone, Note, Person, Testimonial, Quote, Logos, Snippets. |
| Kind | **Work** (glass folders, open a case sheet) or **Note** (pastel sticky notes, open centred). |
| Date / End date | When it happened. An end date shows as the duration on the folder. |
| Client | Shown on work, with the client's mark on the folder when their logo is in `js/data.js`. For a Person, `Name · Role`; for a Testimonial, their role. |
| Summary | The line shown when it opens. For a Testimonial, the quote; for a Person, their bio. |
| Metric, Services, Color, Live link | The big number on a milestone, tags, a note colour override, and a link. |
| Photos | Upload or pick images (they go to `assets/img/uploads`). The first is the cover; all of them peek out of the folder and fill the photo deck. |
| Part of | The project a card belongs to. It shows under that company in the Projects view. A Moment also borrows the client name. |
| Case study / Case section | Links to the three long-form case studies that live in `js/data.js`. |
| Highlight | Marks a highlight. Not used by the current views. |
| Date is a placeholder | Placeholder dates show outlined at `/?draft`. |
| Order within the day | When two cards share a date, lower comes first. |
| Publish | Untick to hide a card without deleting it. |
| Write-up | For a Project or Moment, the rich-text write-up shown in its sheet. |

How it works: every save is a commit to `content/calendar/`. `.github/workflows/build-content.yml` runs `scripts/build-content.mjs`, which turns the cards into `js/cms.js`, then redeploys Pages. There are no tokens or secrets. The editor's fields are defined in `.pages.yml`. To rebuild by hand, run `node scripts/build-content.mjs`. If `js/cms.js` is empty, the site falls back to the events in `js/data.js`.

The calendar used to come from a Notion database. That sync is gone, so edits in Notion no longer reach the site.

## Files

- `index.html`, `css/site.css`, `js/app.js`: v3, the main site. Work shows as glass folders tinted in the note pastels; add `?cards` to see the white work cards instead
- `v2/`: the previous main site (white work cards), a frozen copy of its page, styles and script that reads the same content
- `content/calendar/`: **the calendar's cards**, edited in Pages CMS and built into `js/cms.js`
- `js/data.js`: the long-form case studies, people, testimonials and logos, plus fallback events, shared by every version
- `v1/`: the first take, kept for comparison
- `assets/img`, `assets/video` — pulled from the current 37degr.ee case studies

## Editing content

Cards are edited in Pages CMS (see above). Columns, all-day project bars, the minimap and the mobile agenda are all computed from them.
A new long-form case study goes in `CASES` in `js/data.js`, plus one Pages CMS card with Card **Project** and its **Case study** set. Its steps are **Moment** cards with the same Case study and a **Case section**: the `id` of the block the click scrolls to.

## Before going live: confirm these

Open `/?draft` to see every event marked `tbc: true` outlined in orange.

- Every inception date (the first idea, Puru starting the studio, 37degr.ee going live, Haneef joining) is a placeholder.
- The case study dates are placeholders too. Only their durations (6 months / 10 days / 4 days) come from the live case studies.
- Testimonial dates are placeholders.
- Sidecar's result is written as "$1M+ pipeline". The live page says "7 Digit Pipeline", "$1M+" and "6 digit ($)" in different places, so pick one.
- The "Team trusted by the likes of" logos: two small logos on the live site couldn't be identified, so they were left out. The animal-head mark is assumed to be Goatfish.
