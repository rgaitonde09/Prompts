# Prompts

This folder stores reusable prompt assets for the second-brain workflow.

## Included

- `screenshot-inbox-notion/automation-prompt.md`
  - The screenshot OCR, enrichment, classification, and Notion-routing prompt.
- `screenshot-inbox-notion/replication-guide.md`
  - A setup guide for recreating the workflow in another Notion workspace.

## Suggested GitHub structure

If you later turn this folder into a GitHub repo or move it into one, this layout works well:

```text
prompts/
  README.md
  screenshot-inbox-notion/
    automation-prompt.md
    replication-guide.md
```

## Notes

- The live scheduled task currently reads from the prompt stored outside this repo at:
  - `/Users/rahulg/Documents/Claude/Scheduled/screenshots-ocr-to-notion/SKILL.md`
- The copy in this folder is the Git-friendly version for versioning and reuse.
