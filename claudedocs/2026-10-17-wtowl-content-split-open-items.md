# Open items — moving personal posts to WTOWL (branch `old-post-archive`)

## 1. Needs manual verification
- [ ] **Site builds and nav looks right.** Not built locally (by request). After the push, check the
  GitHub Pages build and the masthead: the nav now has 5 buttons instead of 6 (`col-lg-2` +
  `justify-content-between`), so the spacing may look off. If skipped: a broken build on `master` goes live.
- [ ] **Skim the 8 new drafts in `~/Obsidian/projects/WTOWL/drafts/`.** Bodies are byte-identical to
  the old posts; only the frontmatter is new (`stage: draft`, `origin_*` fields).

## 2. Decisions I deliberately did not make
- [ ] **What a "personal" post means from now on.** There's no tag or page for it any more, so any
  minimal personal content would need tagging `technical` or go under `index.html` only.

## 3. Things Corey said he'd do
- [ ] **wiki-update contradicts itself on `# Inner Life`.** `~/.claude/skills/wiki-update/SKILL.md:27`
  says to skip that section, but line 82 says to focus on "any filled-in `# Inner Life` / `# Capture`
  content". (The post no longer mentions Inner Life, so this is a vault-only issue now.) Corey will decide later
  which way (skip vs. ingest) and fix the matching line. If skipped: private reflection can leak into wiki pages.
- [ ] **Review the updated Obsidian + Claude post in VS Code, then commit and push to `master`.**
  `_posts/2026-10-19-Obsidian-Claude-Note-Taking-Setup.md` (renamed from 2026-08-03; restructured
  to match the Edouard note) is uncommitted. Stage both paths so git records the rename. If skipped:
  the changes never go live.
- [ ] **Merge the grief series into one WTOWL post.** The three drafts are marked `next: collapse
  grief series...`, and the cross-links to the old URLs are already gone from the conclusion.
  If skipped: the conclusion's opening still says "final part of my three part set".

## 5. Known-and-accepted
- Old URLs for the 9 removed posts will 404, with no redirects (your choice).
- `archive/GraceClippinger.html` stays live and unchanged (your choice).
- "Watching Fires From Half the World Away" wasn't copied: a polished version already exists at
  `WTOWL/blog/publish/`. The old site version was only deleted (it's still in git history).
