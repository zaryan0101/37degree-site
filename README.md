# 37degree — Studio Calendar

The 37degree site, told as a calendar: how the studio started, then every project delivered, on the day it happened.

Static site, no build step:

```
python3 -m http.server 4370
```

then open http://localhost:4370 (v3, the main site), http://localhost:4370/v2/ (white work cards) or http://localhost:4370/v1/ (the first take). Live at https://madebypuru.github.io/37degree-site/.

v2 is the cleaner take: no toolbar, a three-line intro, and two kinds of item. **Notes** are pastel sticky notes: how the studio got here. They open centred with the page blurred, in their own colour. **Work** is white cards: what's been shipped. It opens the case-study side panel. Note colours follow the kind of note (story yellow, people blue, testimonials pink, milestones green, logos purple, snippets orange). Any event can override its colour with `color: 'pink'` etc. in `js/data.js`.

The bottom bar holds the view switch (**Stories**, where the site opens: every card by date; **Projects**: the work itself, one piece at a time) and, in Stories, a minimap you can drag (ticks: notes in their colour, projects in ink, today in orange); in Projects it gives way to a counter with arrows that step one project at a time. Projects follows Puru's design: each project with real work (its clip, muted and playing while in view, or up to three of its real images; stand-ins in `assets/img/mock/` never count) stands centre stage under its client's logo and name, with its title, View project or Read case study, and up to two Facts from Pages CMS (or its scope). Every third has a sticky note beside it, a client's words then a bit of our story. The sidebar is the same in both views: our intro ("Our story, *by the day*." with Work with us right under it as a plain pill, as on Puru's, then the line about the studio and the Notes · Work legend), then Sudhanshu's words behind frosted glass, the studio's facts (`STUDIO_FACTS` in `js/data.js`) and two rows of client logos. It slides away as you scroll on and comes back when you scroll back or reach for the left edge. The legend is a guide: hover or tap one and the rest fades back, so you can follow just the notes or just the work. Phones have no sidebar: both views open on the same intro, with a strip of logos. The filter uses Puru's categories and opens on **Selected work** (pieces marked Highlight): All work, Case studies, Websites, Branding, Video & motion, Product & web apps, Campaigns & events, Pitch decks. It reads each piece's Services; the case studies' services live in `CASE_TAGS` in `js/app.js`. Selected work ends on a button to see all of it. **Exploration**, the third view, is a moodboard of design references: the 21 images from the Pinterest board [DevX_2](https://in.pinterest.com/zaryan01/devx_2/), saved in `assets/img/explore/` (sources in `SOURCE.txt` there; they belong to their creators, and opening one shows "via Pinterest"). The images stand alone on a dotted table, no frames or labels, and can be seen three ways, toggled beside Exploration (`?board=`): **Islands** (default: grouped by tone into islands with wide space between; drag, and scroll or pinch to zoom out to the whole map), **Spectrum** (one row ordered by colour, darks through hues to lights, drifting sideways and looping, the room faintly taking the colour of what's in the middle) and **Deal** (one image on an empty table; click, Space or → deals the next, the older ones shrinking out to the edges). Each image's colour is read in the browser; its size comes from `js/cms.js`, so run `scripts/build-content.mjs` after adding images. The filter sits in the top bar while Projects is showing, lined up with the left edge of the work so it reads as the heading for it, with its list dropping down; when the sidebar slides away it moves beside the 37degree name, and on phones it sits beside the light/dark switch. Opening a note, a piece of work or the viewer takes one step in the browser's history, so Back (or a phone's back swipe) closes it. The view is kept in the address (`?view=projects`, `?view=explore`), so a refresh or a shared link lands on it, and `#work/<id>` opens a piece of work directly. A shared link previews with `assets/img/og.png`; its address in `index.html` is the fork's (`zaryan0101.github.io/37degree-site`), so change it if the site moves. Write-ups from Pages CMS are cleaned to an allowlist in `scripts/build-content.mjs`: plain formatting, web and mail links, and images in `assets/`. "Work with us" sits in the sidebar (on phones, the intro). The bottom bar's own button only slides in once the sidebar has slid away, and leaves again when it comes back; phones, with no sidebar, always show it. The sun/moon button top-right switches light and dark; it follows the system setting until someone picks one.

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

- `index.html`, `css/site.css`, `js/app.js`: v3, the main site. Work shows as glass folders tinted in the note pastels
- `v2/`: the previous main site (white work cards), a frozen copy of its page, styles and script that reads the same content
- `content/calendar/`: **the calendar's cards**, edited in Pages CMS and built into `js/cms.js`
- `js/data.js`: the long-form case studies, people, testimonials and logos, plus fallback events, shared by every version
- `v1/`: the first take, kept for comparison
- `assets/img`, `assets/video` — pulled from the current 37degr.ee case studies

## Adding a client logo

Which clients show under Trusted by, and in which row, is set by `TRUSTED_ROWS` in `js/data.js` (taken from the Figma Website page); the other logos still badge their projects. Logos live in `LOGOS` in `js/data.js` and scroll in two rows in the sidebar (one strip on phones), every row at the same speed. Each carries `ar` (width ÷ height) and `d` (how much of that box is ink), which together size it so every logo reads the same size.

1. Save a trimmed, single-colour logo on a transparent background to `assets/img/logos/`.
2. Add `{ name, src, ar, h }`: `ar` is width ÷ height. `h` is its height in the row, set by eye so all read the same size: wordmarks 12 to 18, icon-only marks 18 to 20. Add `avatarOnly: true` for a badge too detailed for the row.

## Editing content

Cards are edited in Pages CMS (see above). Columns, all-day project bars, the minimap and the mobile agenda are all computed from them.
A new long-form case study goes in `CASES` in `js/data.js`, plus one Pages CMS card with Card **Project** and its **Case study** set. Its steps are **Moment** cards with the same Case study and a **Case section**: the `id` of the block the click scrolls to.

## Before going live: confirm these

**Dummy funding facts.** Most Facts on work cards (Raised, Backed by) are stand-ins for layout, kept in the git-ignored `js/dummy-facts.local.js`, which only localhost loads. They're never committed, so the live site shows the real facts or the services instead. To replace one, add the real fact to the card in the CMS and delete it from that file. Real so far: Deccan AI's round, Y Combinator for Almanac and lab0, Omnipair on Solana, and Sidecar's and KAMpanion's numbers from their case studies.

Open `/?draft` to see every event marked `tbc: true` outlined in orange.

- Every inception date (the first idea, Puru starting the studio, 37degr.ee going live, Haneef joining) is a placeholder.
- The case study dates are placeholders too. Only their durations (6 months / 10 days / 4 days) come from the live case studies.
- Testimonial dates are placeholders.
- Sidecar's result is written as "$1M+ pipeline". The live page says "7 Digit Pipeline", "$1M+" and "6 digit ($)" in different places, so pick one.
- The "Team trusted by the likes of" logos: two small logos on the live site couldn't be identified, so they were left out. The animal-head mark is assumed to be Goatfish.
