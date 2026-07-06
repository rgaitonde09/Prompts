# Screenshot Inbox to Notion Replication Guide

This is the Git-friendly copy of the setup guide for recreating the screenshot inbox workflow in another Notion workspace.

The full guide in this project lives at:

- [screenshot-inbox-notion-replication-guide.md](/Users/rahulg/Documents/Claude/Projects/second%20brain/screenshot-inbox-notion-replication-guide.md)

## Quick Summary

To replicate this workflow, another user needs:

1. A cloud-backed screenshot inbox folder
2. A `Processed` subfolder
3. A set of Notion databases for raw logs and routed destinations
4. An optional Apple Photos export step for iPhone screenshots
5. A scheduled automation that OCRs, enriches, classifies, routes, and archives processed images

## Required Notion Databases

- `Screenshots Inbox`
- `Media Watchlist`
- `Home Appliances`
- `Recipes`
- `Travel Places`
- `Inbox Database`

## Key Rules

- Books: Audible or publisher/store page
- Music: Apple Music
- Movies / TV: verify current U.S. streaming
- Physical products / games: Amazon or manufacturer
- Home appliances: manufacturer product and support pages
- Travel: official place or tourism page
- Anything ambiguous: send to primary inbox

## Recommended Next Step

When moving this into another workspace:
- replace all local paths
- replace all Notion data source IDs
- replace the user's subscribed streaming services
- test with a few screenshots before turning on the nightly run
