# Screenshot OCR to Notion Automation Prompt

Process new screenshots into Notion. Work only with the configured screenshot inbox folder. Ensure a `Processed` subfolder exists and skip files newer than about 60 seconds.

## Destinations

- Raw screenshot log: `Screenshots Inbox`
- Media watch list: `Media Watchlist`
- Home appliances: `Home Appliances`
- Recipe list: `Recipes`
- Travel / location list: `Travel Places`
- Primary inbox: `Inbox Database`

## Setup

- If a Photos screenshot exporter script exists, run it first so Apple Photos screenshot items are copied into the inbox before OCR begins.
- List image files directly inside the inbox, excluding `Processed`.
- Stop if there are no new files.

## Workflow

1. OCR every new image file.
2. Group screenshots that clearly belong to the same reel, carousel, recipe, post, or capture sequence.
3. Keep one raw screenshot log page per image, but allow grouped downstream routing.

## For Each New Image

### 1. Transcribe faithfully

- Preserve headings, lists, tables, and code blocks implied by layout.
- Format message screenshots as `**Sender:** message`.
- Do not summarize during transcription.
- Mark unreadable text as `[illegible]`.
- If there is no readable text, describe the image in one sentence.
- Begin with a single H1 title.

### 1a. Enrich after transcription

Append:
- `## Summary`
- `## Research`

Research-link preferences:
- Movies / TV / books: prefer Wikipedia when useful
- Music: prefer Apple Music
- Books / comics: prefer Audible or publisher/store page
- Physical games / products: prefer Amazon or manufacturer page
- Products / places / apps / companies: prefer official site or docs

If the subject may have changed recently, verify via web search before linking.

### 2. Derive metadata

- Determine screenshot source from filename:
  - `Screenshot ...` = `Mac Screenshot`
  - `IMG_####` = `iPhone Screenshot`
- Parse captured date from filename or EXIF, not filesystem creation time

### 3. Classify

Choose one:
- `recipe`
- `travel`
- `media`
- `home_appliance`
- `other`

Tie-break preference:
`recipe` -> `travel` -> `media` -> `home_appliance` -> `other`

### 4. Log raw screenshot

Create a page in `Screenshots Inbox` with:
- title
- captured date
- source
- status
- full transcription
- summary
- research
- routing explanation

### 5. Route downstream

Search the destination first to avoid obvious duplicates.

#### If `media`

Create one row per distinct media item.

Status rules:
- `To Watch` for films, shows, anime, documentaries, podcasts, music, and video games
- `To Read` for books and comics
- `Maybe` is acceptable for board/card/tabletop games when intent is unclear

Populate:
- `Title`
- `Media Type`
- `Status`
- `Wikipedia URL`
- `Official URL`
- `Available On`
- `Recommended By`
- `Source Context`
- `Source Screenshot URL`
- `Captured`
- `Added Via Screenshot = __YES__`

Link rules:
- Books / comics: `Official URL` should be Audible or a publisher/store page
- Music: use `Media Type = Music` when available and prefer Apple Music in `Official URL`
- Tabletop games / physical products: prefer Amazon or manufacturer page in `Official URL`

Availability rules:
- Streamable video titles must always get verified current U.S. streaming availability
- If available on a subscribed service, say so explicitly
- If unreleased or not currently streamable, say that explicitly
- For books, use `Book / streaming availability not applicable.`
- For physical games/products, use `Physical product / streaming availability not applicable.` or `Tabletop game / streaming availability not applicable.`

If a screenshot references both a book/comic and its adaptation, create the row for the thing most directly being recommended and mention the adaptation in the page body if helpful.

#### If `home_appliance`

Populate as much as available:
- `Appliance Name`
- `Category`
- `Status`
- `Brand`
- `Model`
- `Serial Number`
- `Firmware Version`
- `App Version`
- `Location`
- `Product URL`
- `Support URL`
- `Source Screenshot URL`
- `Captured`
- `Added Via Screenshot = __YES__`
- `Network / Tech Notes`
- `Notes`

Link rules:
- Prefer manufacturer product page in `Product URL`
- Prefer manufacturer support/help/manual page in `Support URL`
- Use Amazon only if no strong manufacturer page exists

#### If `travel`

Use one row per distinct place when separable.

Populate:
- `Place Name`
- `Place Type`
- `Destination`
- `Address / Area`
- `Official URL`
- `Validation Status`
- `Date Verified`

Prefer official hotel, resort, museum, trail, park, or tourism pages.

#### If `recipe`

Consolidate related screenshots into one recipe when they clearly belong together.

Populate:
- `Recipe Name`
- `Status = To Try`
- `Source App`
- `Added Via Chat = __YES__`
- `Imported = __NO__`
- `Ingredients`
- `Instructions`
- `Source URL`
- `Chat Intake Notes`

#### If `other`

Create a page in `Inbox Database`.

Populate:
- `Title`
- `Type`
- `Status = Processing`
- `Capture Quality`
- `Category`
- `Date Captured`
- `Source`
- `Clarify?`
- `Next Step`
- `Notes`

If it is clearly a product, object, gadget, or shopping idea, include an Amazon or manufacturer link in the page body's `## Research` section.

### 6. Deduplicate

Move the processed file into `Processed` and verify the move succeeded.

## Failure Handling

- If OCR fails, still move the file to `Processed` and create a raw screenshot page marked unreadable.
- If downstream routing fails, create a fallback page in `Inbox Database`.

## Final Report

At the end, report:
- raw screenshots logged
- media items created
- home-appliance items created
- travel items created
- recipes created
- inbox items created
- failures
- files skipped as too recent
