# CLAUDE.md: Yeva Soroka portfolio website (version 3)

Для Евы: это файл с главными правилами и информацией о сайте. Claude читает его в начале каждой новой сессии в этой папке, поэтому правила не нужно объяснять заново.

## Who and what
- Owner: **Yeva Soroka**, web designer (Instagram @soroka.webdesign, WhatsApp +41 76 309 95 53, Switzerland). She writes in **Russian** (sometimes English): answer in the language she uses.
- Bilingual **EN / RU** one-page portfolio. It will be linked from her Instagram and offers paid services (landing pages, multi-page websites, reels editing, ad creatives, presentations).
- She has a limited token budget: prefer targeted edits, few screenshots, and say when something is expensive.

## Folders and versions (never delete an old version, never edit v1 or v2)
- `/Users/yevka/web design/portfolio-site` = v1, the original style. It is a git repo (remote `https://github.com/1evasoroka1/Portfolio.git`, still empty). **Do not change it.**
- `/Users/yevka/web design/portfolio-site-v2` = v2 (cards look). She did not prefer it. Do not change it.
- `/Users/yevka/web design/portfolio-site-v3` = **this folder, the current version.** Work only here.
- Design source: her Figma file `8JVByjZDs8d9ow11xOEGIY` (Frame 9 = hero, Slides 2 to 4 = projects). She decided to work in local HTML, not in Figma.

## Tech
- Single file `index.html` (HTML + CSS + JS) plus `img/` and `fonts/`. No build step.
- Translations live in the `T` object in the script, `data-i18n="key"` on elements, `setLang()`. English defaults are also written in the HTML. Russian: `?lang=ru`. A missing key falls back to English.
- Run: `cd "/Users/yevka/web design/portfolio-site-v3" && python3 -m http.server 8124`, open http://localhost:8124 (she uses Safari; testing is done with headless Chrome).
- **Fonts are self-hosted** in `fonts/` (Unbounded for headings, Manrope for text; Latin and Cyrillic). Never add a Google Fonts link again (the visitor's IP would go to Google). Upload `fonts/` together with `img/`.

## Layout rules (apply to every page)
- Design frame **1920×1080**. Every section is exactly **one screen tall**: nothing cut off, nothing scrolling inside a section.
- The whole page scales with the window: `--u = min(1vw, 1.7778svh)`. All sizes are written as `calc(N * var(--u))`, with small px floors that shrink on desktop (`--fk`).
- Margins: **50px** on the sides and top (`--m`), **30px** between blocks (`--g`). Header 50px from the top; titles start 30px under it (`--pad-top` 118px); content ends 84px above the bottom (`--pad-bottom`).
- Section titles: Unbounded ExtraBold, uppercase, **120px**, exactly **centred** on every page, between two thin lines (grid `1fr auto 1fr`).
- Font sizes (from her Figma): body text 24px, small text 20px, tags 18px, project name 44px.
- Style: the v1 look (centred titles with lines, tilted pill badges, burgundy cards, beige and light sections, grain texture) on the Figma grid. Colours: burgundy `#5E0B16`, beige `#EBDCC6`.
- Logo "Yeva Soroka / web design" stays in English in both languages.
- **No sideways overflow on phones**: grids use `minmax(0, 1fr)` and step/service titles shrink with `4vw`, because a long Russian word (Предложение, Многостраничный) used to make the page wider than a 334px screen and the phone panned it sideways. Test widths 320, 334, 360, 375, 390 (an iframe of that width works) after changing phone text.
- **Phone page frame 375×812**: every section is min-height 812px. She tried a "safe area" (335×660, then 335×710 with 20px margins) and went back to the normal margins (16px sides, 68px top, 24px bottom), so there is no separate safe-area rule. "PROJECTS" title is hidden on phones for projects 01 and 03 (order: badge, name, type, mockups, text, tags, button). Re-measure after any change.
- **Phones (≤760px): every section fits one 375×812 screen** (her choice, Nov 2026). Compact phone block at the end of the section CSS: How I work = 2 columns of small cards with no spotlight dimming; Services = compact rows (star, name, price on one line, text under it); smaller mockups, terms and about photo. Re-measure at 375×812 in EN and RU after any copy change.
- Phone hero (<860px): the photo is pulled up so the hair overlaps "PORTFOLIO", text and product pills (wrapped in rows) follow tightly, so the whole first screen fits on a phone.

## Style (the look she loved in v1, kept in v3)
She said about v1: "I loved it." The look comes from v1 and only the grid, margins and font sizes come from her Figma rules. When adding anything new, copy these patterns instead of inventing new ones.

**Colours (CSS variables):** burgundy `#5E0B16` (main), mid `#7A1220`, deep `#3A050C`, ink `#2A0409` (page background and footer), beige `#EBDCC6` (text and buttons on dark), light beige `#F4EADB` (light sections), cream `#FFF8EE` (service cards), accent = beige.

**Two kinds of sections, alternating:**
- **Wine (dark):** radial gradient from mid burgundy at the top centre to burgundy to deep burgundy at the bottom, beige text.
- **Light:** flat light beige, burgundy text.
- Both get the **crumpled-paper grain**: an SVG noise (feTurbulence) over the whole section, `overlay` blend at 25% on dark, `multiply` at 14% on light.

**Type:**
- Headings and big numbers: **Unbounded ExtraBold (800)**, UPPERCASE, line-height .95 (1.05 in Russian so Й and Ё do not clash), letter-spacing −0.02em.
- Text: **Manrope** (400 to 800). Labels like "Client / The idea / What I did" are Unbounded 800 in normal case.
- Subtitles use Unbounded 600 (`serif-i` class). Logo: "Yeva Soroka" Unbounded 20px, tight letter-spacing, with "WEB DESIGN" under it in small spaced capitals (14px, letter-spacing .25em).

**Shapes:**
- Everything small is a **pill** (radius 99px): buttons, tags, language switch, prices, counters.
- Cards use `--radius`: 24px at 1920 (it scales down).
- **Buttons:** 1.5px beige border. The solid one is beige with burgundy text and turns transparent only while the cursor is on the button itself. The ghost one has a beige outline and gets a faint beige tint on hover. Hover lifts the button 2px.
- **Tags:** thin beige outline (about 55% opacity), 18px text.
- **Number badges** ("01", "02", "03"): Unbounded, beige 2px outline pill, tilted −8°.

**Cards and blocks:**
- **How I work:** solid burgundy cards, big beige Unbounded number, step title in the same font and size as the service names (Unbounded 800, 32px at 1920, normal case), 20px text. Spotlight on hover (the card under the mouse is bright and slightly bigger, the others dim). On phones it follows the scroll instead.
- **Services:** cream cards with a soft burgundy shadow and a ✦ star icon; the middle "Multi-page website" card is the burgundy "featured" card. Big Unbounded price at the bottom.
- **Terms:** dark cards with a 1px beige outline (35% opacity) and a faint dark fill; one big Unbounded heading and text.
- **About:** the photo with a soft shadow and a burgundy pill tag "hi, I'm Yeva" tilted −4°, overlapping the photo corner. Chips are pills: the first one filled burgundy, the others outlined.
- **Contact:** photo in a circle with a 4px beige border and a deep shadow. QR codes sit on beige rounded squares, exactly as wide as their buttons.

**Project mockups (the signature of v1):**
- Two **browser windows** (beige title bar with three burgundy dots, rounded, big shadow) tilted a few degrees in opposite directions, overlapping, plus a **phone** (thick near-black frame `#1a0306`, rounded) on top. Back window ≈3° (Olha) or −3° (LightUp), front window ≈−2° or 2°.
- Hover: the screen under the cursor comes to the front and grows (`scale` 1.05, the tilt does not change, so the hover area never flickers).
- **Reels phones:** photos with a 6px near-black frame and rounded corners, alternately tilted −2° and 2° (every even one also lower by 16px), a caption pill on the bottom edge (beige, burgundy Unbounded text, tilted −3°). Hover straightens and enlarges a phone.

**Hero photo:** her cut-out portrait is centred and overlaps "PORTFOLIO"; the bottom of the photo fades out with a gradient mask.

**Motion:** content fades and rises 28px when it scrolls into view (`.reveal`, 0.8s). Smooth scrolling. The nav is transparent at the top and turns into a dark blurred bar after 40px of scrolling. Respect `prefers-reduced-motion` (animations off).

**Changed from v1 on purpose (do not bring back):** the timeline line with dots in How I work; the narrow 1240px centred column and the huge 72 to 140px gaps (now the 50px/30px grid); small cards in 2 rows of 3 instead of 6 tall cards; the "PROJECTS" counter "01 / 03" (now just the number badge); text-transform on project names stays uppercase.

## Page structure (approved)
1. **Hero** (Frame 9): "PORTFOLIO" fills the width between the margins, photo in the middle, offer text and buttons bottom-left, product pills bottom-right.
   - Slide-over effect: hero and first project sit in `.stack` (200svh) with a sticky `.pin`. The first project slides up over the hero from the very first scroll and is done after one screen of scrolling (rounded 48px top corners). Only on windows ≥1201px wide and without "reduce motion". Links to `#projects` scroll to the end of the stack.
2. **Projects**, in order of size: **01 The LightUp Agency** (landing page; text left, mockups right), **02 Reels & creatives**, **03 Olha's Beauty** (multi-page website; mockups left, text right).
   - Text block: tilted number badge, name in capitals, type subtitle, Client / The idea / What I did, tags, "Visit website" button. Text and pictures sit close together.
   - Reels page: title "REELS & CREATIVES" at 104px, centred, with lines, badge "02" at the left, one-line description, four big tilted phones with caption pills.
3. **How I work**: 2 rows of 3 burgundy cards, no timeline, spotlight hover animation kept.
4. **Services**, then 5. **Terms** (Services comes first; the menu has the same order).
6. **About**: photo 3:4 (head not cut), two paragraphs, straight left edge and uneven right edge.
7. **Contact**: centred heading "Let's work together", left text with WhatsApp and Instagram buttons (QR codes exactly as wide as the buttons), big circle photo close to the text on the right. No frame around the text.

## Effects
- **Star cursor**: a four-point star replaces the mouse cursor on mouse devices. Always beige (thin dark hairline edge), no size change on hover (it caused jumping), moves via `transform` with light smoothing. Phones and tablets keep the normal cursor.

## Copy rules
- **No full stop at the end of any text**, paragraph, button or caption (a dot is bad tone), in EN and RU, both in the HTML defaults and in `T`. Keep `?`, `!` and emoji. Dots between two sentences inside a paragraph are still there (ask her if she wants them changed).
- Russian copy must sound **natural**; she dictates corrections, so apply them exactly and keep EN and RU matching in meaning.
- Business facts on the site: landing pages from $300, multi-page websites from $500, **reels editing from $15** (complex reels with graphics from $30), ad creatives and presentations on request. The hero says "businesses and experts" (not "small businesses").
- How I work = 6 steps: Contact (her idea, a call), Offer, Structure (she writes texts only if the client needs), Moodboard (style, colours, draft of the first pages), Website, Revisions (up to 3 big rounds, then the project ends).
- Terms = 4 cards, each with **one heading and text** (no separate subtitle): "Everything agreed" / "2–3 rounds of revisions" / "100% prepayment" (can be split into 2–3 payments for long projects) / "Changes after the project" (paid separately, depending on the task).
- Olha's Beauty "What I did": AI content generation (images and video, mostly re-made existing visuals); the tag says "AI content generation".

## Checklist before saying "done"
- Measure every section at 1920×1080, 1470×825, 1440×900, 1366×768 and 1280×720, in **EN and RU**: no section taller than the window. Russian text is longer and breaks layouts first.
- Look at screenshots of the changed pages; check the phone layout (<860px) too.
- Do not touch v1 or v2. Keep memory notes (`design-rules-v3`) in sync with changes.

## Open items
- **Imprint and Privacy** links in the footer (two small pages, EN and RU). Needed because it is a business site for German and Swiss clients. Needs her full name, address and email. No cookie banner is needed (no cookies, no analytics, fonts are self-hosted).
- **GitHub**: v3 is linked to `git@github.com:1evasoroka1/Portfolio-website.git` (branch `main`, first commit "Initial" pushed). Never push without her go-ahead; she signs in herself (no passwords or tokens are entered by Claude).
