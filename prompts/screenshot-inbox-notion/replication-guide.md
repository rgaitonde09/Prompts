# Screenshot Inbox to Notion Replication Guide

This guide explains how another user can recreate the screenshot-to-Notion workflow in their own workspace.

## What this system does

Each run:
- pulls recent screenshot items into a single inbox folder
- OCRs each screenshot
- logs the raw screenshot transcription
- classifies the screenshot into one of these buckets:
  - media
  - travel
  - recipe
  - home appliance
  - other
- enriches the item with links and research
- routes the result into the appropriate Notion database
- moves the source image into a `Processed` folder so it is not reprocessed

## Components

You need four pieces:

1. A cloud-backed screenshot inbox folder
2. A set of Notion databases
3. An optional Apple Photos exporter for iPhone screenshots
4. A scheduled Codex automation that runs the OCR and routing prompt

## Folder setup

Create a folder in iCloud Drive named:

`Screenshots Inbox`

Inside it, create:

`Processed`

Recommended setup:
- Mac screenshots save directly into `Screenshots Inbox`
- iPhone screenshots are exported into the same folder by a Shortcut or script

## Mac screenshot destination

On macOS:

1. Press `Cmd+Shift+5`
2. Open `Options`
3. Under `Save to`, choose `Other Location...`
4. Pick `iCloud Drive/Screenshots Inbox`

## iPhone screenshot export

You have two options.

### Option A: Shortcuts app

Build a Shortcut that:
- finds screenshots from the last day
- saves them to `iCloud Drive/Screenshots Inbox`
- runs on a daily schedule

### Option B: Apple Photos exporter script

This repo includes a working pattern in:

[photos_screenshots_to_inbox.py](../../photos_screenshots_to_inbox.py)

To adapt it:
- change the destination folder if needed
- keep the state file so exports are deduplicated across runs
- make sure it exports original capture time or preserves it in file metadata

## Notion databases to create

Create these databases in your Notion workspace.

### 1. Screenshots Inbox

Purpose:
- raw log of every screenshot processed

Recommended properties:
- `Title` or equivalent title property
- `Captured` (date)
- `Source` (select: `Mac Screenshot`, `iPhone Screenshot`)
- `Status` (select: `Reviewed`, `Unreviewed`)

Page body should hold:
- full transcription
- summary
- research
- routing notes

### 2. Media Watchlist

Purpose:
- books, movies, TV, anime, documentaries, podcasts, music, games

Recommended properties:
- `Title` (title)
- `Media Type` (select)
- `Status` (select)
- `Access URL` (url)
- `Audiobook URL` (url)
- `Wikipedia URL` (url)
- `Official URL` (url)
- `Available On` (text)
- `Recommended By` (text)
- `Source Context` (text)
- `Source Screenshot URL` (url)
- `Captured` (date)
- `Added Via Screenshot` (checkbox)

Recommended `Media Type` options:
- `Movie`
- `TV Show`
- `Book`
- `Anime`
- `Documentary`
- `Podcast`
- `Game`
- `Music`
- `Other`

Recommended `Status` options:
- `To Watch`
- `To Read`
- `To Listen`
- `In Progress`
- `Done`
- `Maybe`
- `Dropped`

### 3. Home Appliances

Purpose:
- device inventory and support capture

Recommended properties:
- `Appliance Name` (title)
- `Category` (select)
- `Status` (select)
- `Brand` (text)
- `Model` (text)
- `Serial Number` (text)
- `Firmware Version` (text)
- `App Version` (text)
- `Location` (text)
- `Product URL` (url)
- `Support URL` (url)
- `Source Screenshot URL` (url)
- `Captured` (date)
- `Added Via Screenshot` (checkbox)
- `Network / Tech Notes` (text)
- `Notes` (text)

### 4. Travel Places

Purpose:
- destinations, museums, parks, restaurants, hotels, attractions

Recommended properties:
- `Place Name` (title)
- `Place Type` (select or text)
- `Destination` (text)
- `Address / Area` (text)
- `Official URL` (url)
- `Validation Status` (select)
- `Date Verified` (date)

### 5. Recipes

Purpose:
- recipe captures consolidated from one or more screenshots

Recommended properties:
- `Recipe Name` (title)
- `Status` (select, e.g. `To Try`)
- `Source App` (select)
- `Added Via Chat` (checkbox)
- `Imported` (checkbox)
- `Ingredients` (text)
- `Instructions` (text)
- `Source URL` (url)
- `Chat Intake Notes` (text)

### 6. Primary Inbox

Purpose:
- anything not confidently routable elsewhere

Recommended properties:
- `Title` (title)
- `Type` (select: `Idea`, `Link`, `Note`)
- `Status` (select, e.g. `Processing`)
- `Capture Quality` (select or text)
- `Category` (text)
- `Date Captured` (date)
- `Source` (url or text)
- `Clarify?` (checkbox)
- `Next Step` (text)
- `Notes` (text)

## Link rules to bake into the workflow

These rules matter a lot because they keep the output useful.

### Media links

- Movies / TV / anime / documentaries:
  - `Access URL`: best current page to watch or start from when available
  - `Wikipedia URL`: use Wikipedia when available
  - `Official URL`: official studio, network, or franchise page if useful
  - `Available On`: verify current U.S. streaming

- Books / comics:
  - `Access URL`: prefer publisher, author, or strong store page
  - `Audiobook URL`: prefer Audible when available
  - `Official URL`: publisher, author, or canonical series page
  - `Available On`: use `Book / streaming availability not applicable.` if no streaming field is needed

- Music:
  - `Media Type`: `Music`
  - `Status`: use `To Listen`
  - `Access URL`: prefer Apple Music, Spotify, YouTube, or the strongest direct listening page
  - `Official URL`: artist, label, or release page when helpful
  - `Available On`: name the listening platform explicitly

- Tabletop games / toys / physical products:
  - `Access URL`: best product page to view or buy from
  - `Official URL`: prefer Amazon or manufacturer page
  - `Available On`: use `Physical product / streaming availability not applicable.` or similar

### Home appliance links

- `Product URL`: prefer manufacturer product page
- `Support URL`: prefer manufacturer support or manual page
- Use Amazon only if no solid manufacturer page exists

### Travel links

- prefer official park, museum, hotel, resort, or tourism pages
- if none exists, use a strong official location page

### Other bucket links

- if the item is still going to the primary inbox but is clearly a product or object, include an Amazon or manufacturer link in the page body research section

## Prompt / automation behavior

Your automation prompt should do all of the following:

1. Read only from `Screenshots Inbox`
2. Skip files newer than about 60 seconds
3. OCR every new image
4. Preserve a raw screenshot log entry for every file
5. Group obviously related screenshots before downstream routing
6. Enrich each transcription with:
   - `## Summary`
   - `## Research`
7. Parse capture time from filename or EXIF, not just filesystem creation time
8. Route into one of:
   - media
   - travel
   - recipe
   - home appliance
   - other
9. Search destination databases first to avoid obvious duplicates
10. Move processed files into `Processed`
11. Create a fallback primary-inbox item if routing fails after raw logging

The current example prompt lives outside this repo, in the owner's local Codex scheduled-task workspace (`screenshots-ocr-to-notion/SKILL.md`) — see `prompts/README.md` for why it isn't versioned here.

To reuse it in another workspace:
- replace the folder paths
- replace every Notion data source ID
- replace the owner-specific streaming subscriptions
- update any naming conventions

## Creating the scheduled automation

In Codex, create a cron automation that:
- runs daily
- uses the workspace containing your script and prompt
- executes the screenshot OCR prompt

Recommended schedule:
- nightly, after all devices have synced

Example timing:
- `2:00 AM` local time

## Suggested workflow for another user

1. Create the six Notion databases
2. Add the recommended properties
3. Create `Screenshots Inbox` and `Processed` in iCloud Drive
4. Point Mac screenshots at the folder
5. Add iPhone export via Shortcut or exporter script
6. Copy and adapt the exporter script if needed
7. Copy and adapt the automation prompt
8. Replace all Notion IDs with the new workspace's IDs
9. Create the scheduled Codex automation
10. Drop in a few test screenshots and run it manually once
11. Verify each category lands in the right database
12. Verify links match the rules above

## Validation checklist

Before declaring setup complete, confirm:
- raw screenshot pages are created
- processed files move into `Processed`
- books get Audible or publisher links
- music gets Apple Music links
- physical games/products get Amazon or manufacturer links
- movies and shows get current U.S. streaming info
- travel items get official location links
- recipes consolidate multi-slide captures correctly
- unclassified items fall back to the primary inbox

## Common pitfalls

- Using file creation time instead of screenshot capture time
- Forgetting to deduplicate Apple Photos exports
- Leaving `Available On` blank for streamable video titles
- Storing Amazon links for appliances when a stronger manufacturer page exists
- Creating duplicate media rows from multi-image carousels
- Failing to move processed files after success

## Optional improvements

- add a `Music` option to the `Media Type` select
- add database views such as `To Watch`, `To Read`, `Travel Queue`, and `Needs Clarification`
- add a second pass that revisits older items to refresh links or streaming availability
