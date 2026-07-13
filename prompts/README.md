# Prompts

This folder stores reusable prompt assets for the second-brain workflow.

## Included

- `screenshot-inbox-notion/automation-prompt.md`
  - The screenshot OCR, enrichment, classification, and Notion-routing prompt, including media `Access URL` / `Audiobook URL` enrichment rules.
- `screenshot-inbox-notion/replication-guide.md`
  - A setup guide for recreating the workflow in another Notion workspace.
- `wine-sommelier-value/standalone-prompt.md`
  - A reusable sommelier and wine value-analysis prompt for label photos, wine lists, and pricing decisions.

## Suggested GitHub structure

If you later turn this folder into a GitHub repo or move it into one, this layout works well:

```text
prompts/
  README.md
  screenshot-inbox-notion/
    automation-prompt.md
    replication-guide.md
  wine-sommelier-value/
    standalone-prompt.md
```

## Notes

- The live scheduled task currently reads from the prompt stored outside this repo at:
  - `/Users/rahulg/Documents/Claude/Scheduled/screenshots-ocr-to-notion/SKILL.md`
- The copy in this folder is the Git-friendly version for versioning and reuse.
- For RG Brain, any new standalone prompt created in this workspace should be added here and then published to `https://github.com/rgaitonde09/Prompts`.
