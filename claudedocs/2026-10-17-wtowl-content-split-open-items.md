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
- [ ] **Merge the grief series into one WTOWL post.** The three drafts are marked `next: collapse
  grief series...`, and the cross-links to the old URLs are already gone from the conclusion.
  If skipped: the conclusion's opening still says "final part of my three part set".

## 4. Blockers before merge / deploy
- [ ] **Commit + push / merge to `master`.** Nothing is committed yet. Merging to `master` removes the
  9 posts from the live site immediately, and their URLs will 404 (accepted, see 5).

## 5. Known-and-accepted
- Old URLs for the 9 removed posts will 404, with no redirects (your choice).
- `archive/GraceClippinger.html` stays live and unchanged (your choice).
- "Watching Fires From Half the World Away" wasn't copied: a polished version already exists at
  `WTOWL/blog/publish/`. The old site version was only deleted (it's still in git history).
