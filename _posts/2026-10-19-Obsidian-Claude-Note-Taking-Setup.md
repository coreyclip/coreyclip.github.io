---
layout: post
title: Obsidian + Claude 
published: true
tags: technical
---
# My BASB + LLM Wiki System

*This is an updated version of the guide I first wrote back in August.*

Here is an overview of how I manage **all** of my digital knowledge, and hence much of my life. Our minds are best for thinking; externalizing knowledge into notes helps us think and keep track of what we need to do. Text files are small, so keep a lot of them. Make creating notes, capturing information into them, and organizing them as simple as possible. LLMs are efficient at transforming and regurgitating text, and the more of *your* text you give one to live within, the better it's able to assist you, rather than giving you the generic answer it would give anyone. Computers are very good at searching through text, and LLMs are very good at organizing text files. So spend more time writing and copy-pasting, and leave the organizing for later. The goal is an extensive body of text that both you and your LLM live within to answer questions, cultivate more knowledge, and extend what you can do.

**Key ideas / tools**
- **Tiago Forte's Building a Second Brain (BASB):** note taking as the cultivation of a second body of knowledge outside of your mind. Be greedy when grabbing and storing information, review your notes periodically to come up with new ideas, and organize knowledge by how actionable it is. ([Overview of BASB](https://fortelabs.com/blog/basboverview/)) Tiago Forte himself has landed in the same place as I have regarding pairing LLMs with his notes system. In 2026 he [wrote](https://fortelabs.com/blog/introducing-the-ai-second-brain/) that "Personal Context Management is replacing Personal Knowledge Management," with agent tools like Claude Code finally making the second brain something that does work for you rather than just storing it.

- **Obsidian:** a free, local-first note-taking app that stores everything as plain markdown files in a folder on your computer. Because the notes are just local text, you can do pretty much anything with them, nothing is vendor-locked, and any outside tool (a script, a search command, an LLM) can read and write them with almost no friction. Simple `[[wikilinks]]` and inline `#tags` let you link and categorize on the fly.
- **An LLM in the vault:** any AI agent that can work with your file system can simply be started from inside your Obsidian vault. I use Claude Code. Many of the chores Tiago recommends, and more, can then be done with more regularity and speed.

**The short version:** I write everything into one daily note, and once a day or so Claude sorts it into PARA folders. On a slower cadence it also distills what I've written into a wiki that it maintains itself. The daily note records what happened. The wiki holds what I now know. Claude does the bookkeeping, which is the part that makes most note systems die. The maintained note set is then a powerful reference tool for myself and claude code. 

---

## Part 1: How it works

### Three ideas

1. **One capture surface.** Everything starts in a single **daily note**: ideas, links, meeting notes, tasks, vocabulary, half-thoughts. I don't decide where something belongs at the moment I write it; that decision is exactly the friction that kills note-taking habits. I also greedily paste links and use the Obsidian Web Clipper on anything I come across that *might* be useful. The `# Capture` heading in my template is just a basket that gets sorted later.

2. **PARA for where things live.** Anything worth keeping is later *routed* out of the daily note into one of four folders, following Tiago Forte's [PARA method](https://fortelabs.com/blog/para/):
   - **Projects:** active work with an end. A product launch, a blog post, a move.
   - **Areas:** ongoing responsibilities with no end date. Health, finances, routine housekeeping, the garbage truck schedule.
   - **Resources:** reference material and interests. Clipped articles, guides, research.
   - **Archives:** anything finished or dormant.

   This organizes notes by how actionable they are. An active project is something I check and interact with regularly. Areas are periodic. Resources support projects and areas or are things to read later. Archives are, well, archives: rarely touched but kept just in case. At a glance it tells me what shape my life is in, and it tells the LLM what my priorities and interests are.

3. **An LLM-maintained wiki for what I know.** On top of PARA sits a `Wiki/` folder of interlinked pages that Claude writes and maintains from everything in the vault. This follows [Andrej Karpathy's LLM-wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). His key point is that people abandon wikis because the upkeep costs more than they're worth: *"The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping."* An LLM doesn't get bored and doesn't forget cross-references, so the upkeep cost drops consdierably. As he puts it, **"Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."** The wiki then becomes the LLM's own notes, where the structure tells it how the people, projects and ideas in your life relate. Context and memory for the LLM end up stored in your file system, in a form both of you can read.

The loop is always the same: **capture cheaply → route in batches → distill into the wiki.**

### How the layers fit

```
 Daily note (raw, chronological, mine)
      │  route: split each entry to its home
      ▼
 PARA folders (projects / areas / resources / archives) — mine
      │  ingest: extract people, projects, themes
      ▼
 Wiki/ (entities, concepts, sources, syntheses) — Claude's
      ▲
      └── schema file: the rules Claude follows when it writes here
```

**PARA answers "where does this note live?"** Each note has exactly one home. **The wiki answers "what do I actually know about X?"** That knowledge is synthesized, cross-linked and dated. The two don't compete, because the wiki is built *from* the PARA notes. Folders give location, properties (frontmatter) give each note a machine-readable role, tags give facets that cut across folders, and links give the graph.

Without a structure like this, asking an LLM about something you wrote last week, or for an outline of all your meetings with someone, means it combs the entire vault every time, "rediscovering knowledge from scratch." That's slower, burns far more tokens, and the wider the search the less precise the answer.

### Inside the wiki

The wiki has four page types, each in its own folder:

| Page type | What it holds | Example |
|---|---|---|
| **Entity** | One person, organization, project or place. It grows a dated timeline over time. | A co-founder; a beta customer; my startup |
| **Concept** | A theme (broad) or a specific event or decision (narrow). Narrow pages link up to their broad parent. | "Chinese language learning" → "Character deck build, Sept 2026" |
| **Source** | One page per ingest run, saying which raw notes were read and what changed | "Journal window 09-25 → 10-04" |
| **Synthesis** | The answer to a question, saved so later work builds on it | Weekly reviews; lint reports |

Two files hold the wiki together:
- **`index.md`** catalogs every page in one line each. Claude reads it first on every task, so it never has to search blindly. 
- **`log.md`** is an append-only record of every operation, each starting with the same greppable line (`## [2026-10-13] lint | …`). It's the audit trail, and it's how Claude knows where it left off.

*These files are mostly for the AI, but they're worth understanding so you can debug the wiki when something looks off.*

The other important file is the **schema**: a plain markdown file stating the page types, frontmatter fields and rules. That one file turns a general chatbot into a consistent wiki maintainer. The rules that matter most:
- **Raw notes are never edited by the LLM (unless told to).** It reads my journals and PARA notes and writes only inside `Wiki/`. In practice the daily note is still mostly just me writing, while the notes under PARA have become roughly 50/50: things I wrote myself with amendments from Claude, and syntheses Claude wrote that I reviewed and edited.
- **Timelines are append-only and newest-first.** Nothing gets silently rewritten, and every claim traces back to a source page, then to the journal entry it came from.
- **Every edit updates the page's `last_updated` field and adds a log entry.** That's how staleness gets detected later.

I bootstrapped my wiki in April 2026 by having Claude backfill my journals going back to mid-2024. Six months later it's about 250 pages. When something is wrong I can fix a page myself, but usually I just tell Claude what to change, and to update its `CLAUDE.md` if the mistake came from a rule.

### Why a wiki instead of "just search my notes" (RAG)

There are three ways to let an LLM answer questions from your notes:

1. **RAG (retrieval-augmented generation):** chop every note into chunks, embed them in a vector database, and at question time pull the chunks that look most similar to the question.
2. **Let the agent search raw notes:** what Claude Code does out of the box. It greps and reads your files fresh on every question.
3. **A curated wiki:** the LLM reads new notes once, writes what it learned into interlinked pages, and answers from those pages.

The core difference is *when* the work happens. RAG and raw search do it at question time, every time. The wiki does it when notes come in, then reuses the result.

| | RAG | Curated wiki |
|---|---|---|
| What it returns | Chunks of what was *written* | Pages of what was *learned* (fifty mentions of a project become one page with a timeline) |
| "How did X evolve?" | Scattered chunks that mention X | Already answered by the timeline on X's page |
| When it's wrong | Hard to see why a chunk was picked; rephrasing the question changes the results | Open the page, follow its source link back to the journal entry, fix one file |
| Infrastructure | Vector store, embedding model, reindexing | Plain markdown in any editor |
| Where the cost goes | Every query | Every ingest and lint, which you review |
| Main risk | Misses the right chunk | Summaries can miss details, and a bad ingest can bake in an error (source links are the safeguard) |

RAG still wins when the corpus is too big to curate, or when nobody (human or LLM) will keep the wiki maintained. For exact quotes, plain keyword search beats both, and since my raw notes stay in the vault, Claude can still grep them. Karpathy notes that the index-first approach works well at moderate scale (~100 sources, ~hundreds of pages); beyond that, adding a search tool on top is the natural next step. My wiki is about 250 pages built from a few thousand notes, ingested in weekly batches (about 80 source pages given that I process my daily notes into large weekly and monthly files), so for a personal vault (or a small team's), the wiki is the better fit.

### The rest of the CODE loop

Tiago's method is [Capture, Organize, Distill, Express](https://fortelabs.com/blog/basboverview/). Everything above leans on the first two. Recently I've handed more of the loop to Claude, each piece as its own skill: one to finish organizing, two for distilling and reusing what I've made:

- **PARA triage.** Loose notes that pile up outside the folders get a proposed home using Tiago's questions in order: is it for a project? an area? a resource? or done? Claude writes the proposals as a form in my daily note; I edit it, and it applies only what I approved.
- **[Progressive summarization](https://fortelabs.com/blog/progressive-summarization-a-practical-technique-for-designing-discoverable-notes/).** Claude proposes the bold layer (I aim for no more than 10-20% of the note) and drafts a summary at the top of the note. The highlight layer stays mine, because the point of that layer is *my* sense of what resonates. Claude never rewords the source.
- **[Intermediate packets](https://fortelabs.com/blog/just-in-time-pm-4-intermediate-packets/).** Reusable work (runbooks, checklists, templates, prompts) gets a `packet` property, and an Obsidian Base turns those into a catalog. Before Claude starts new research, it checks whether I've already made something it can reuse.

### What it looks like day to day

**Throughout the day:**
- Open today's note.
- Write everything there without worrying about organizing. I use a few tags and inline links so Obsidian's search works a bit more cleanly, and that's the extent of it.

**At the end of the day or the week:**
- **Capture processing:** each link or idea in the capture inbox gets summarized and filed as a Resource note or turned into a follow-up task, so I never sit down to a wall of unread bookmarks.
- **Routing:** Claude proposes where each piece of the daily note should go, I approve or adjust, and it does the filing. This used to be my Sunday ritual.
- **Wiki update:** Claude finds the journals it hasn't ingested yet and drafts a plan: new pages, updates to existing ones, people it will deliberately skip. I approve or adjust the plan. Then it writes the updates plus a weekly summary.
- **Lint:** a health check for broken links, index entries with no file, pages nothing links to, and stale pages.
- **Weekly review:** a summary of my week for the [weekly review Tiago recommends](https://fortelabs.com/the-weekly-review-is-an-operating-system-8e8e04f885ab), catching missed tasks and re-ordering my task list before I plan the next week.

All of these are saved as **Claude skills**, which are reusable instruction files (mine are `capture-processor`, `journal-router`, `wiki-update` and `weekly-review`). I don't re-explain the process each time; I just say "update the wiki" or "process my captures". A few others run on a schedule: systemd timers call `claude -p "/skill"` headless, and each run writes only to its own output note, like a weekly newsletter on topics I follow.

### What I get out of it

- **Questions get answered from the index.** "What did we decide about pricing and when?" or "Who did I meet at that event and what did they want?" Claude reads the index, opens two or three pages, and answers with links back to the original notes.
- **Stretching what I can do.** Because Claude already lives in my knowledge base, I don't have to explain much up front. "Build Anki flashcards from the Chinese words I wrote down this week." "Pull everyone I signed up at last week's event who works in a particular industry into a CSV." "Turn these notes into a blog outline in my style." This post was drafted that way, from notes already in the vault.
- **Refined artifacts from piles of research.** Feed in a stack of articles and get back a cited report, a slide deck, or an audio rundown I can listen to. For talks and workshops, Claude pulls the relevant wiki pages into an outline I build the deck from.
- **Topic wikis that keep growing,** like a digital-marketing knowledge base I've forked across a few markets and languages.

### What to keep in mind

- **The habit matters more than the tools.** If capturing information isn't automatic and easy, the system breaks down. If Obsidian and its plugins are a pain for you, it's time for a different app. Anything that stores notes as plain text (md, txt, org) can work, as long as the LLM can reach the files. 
- **Batch, don't organize live, and let Claude propose while you approve.** Have it show you a plan before it writes anything. You stay in control, and it learns your structure over time. LLMs are best at following patterns, not deciding what you want. Give it a pattern *you* chose, then hold it to that pattern. 
- **Keep the daily note as the source of truth.** The routed notes and the wiki are *derived*; the chronological journal is the ground truth you can always rebuild from.
- **Drift is mostly bookkeeping.** My lints rarely find wrong facts. They find an index entry whose page was moved, or a link to a page that was archived. A periodic check of the index against the files on disk catches nearly all of it.
- **Decide privacy boundaries up front.** Whatever Claude reads is sent to Anthropic. Same can be said for OpenAI or Google's models. And even if you trust these entities, prompt injection and other security vulnerabilities are out there and not adequately "solved" so to speak. If the model has access to the internet, I default to considering anything that touches it have the possibility of being made public. Keep genuinely sensitive material (passwords, medical, financial) out of the vault entirely, and set up Claude's permission rules. Or consider running a local model. 
- **Start smaller than you think.** Daily notes plus PARA plus a single "route my week" request already gives most of the value. Add the wiki once there's enough material to distill.

---

## Part 2: Setting it up

### 1. Install Obsidian
Download it from [obsidian.md](https://obsidian.md) and create a vault, which is just a folder on your computer. Your notes stay as plain markdown files that you own.

### 2. Create the structure
```
Vault/
├── Journals/daily/     ← daily notes, named YYYY-MM-DD.md
├── Projects/
├── Areas/
├── Resources/
├── Archives/
├── Templates/          ← used by daily notes
└── Wiki/
    ├── entities/  concepts/  sources/  syntheses/
    ├── index.md
    ├── log.md
    └── schema.md
```

### 3. Daily note template
Turn on the core **Daily Notes** plugin, point it at `Journals/daily/`, and set this as the template. Also install the **Tasks** community plugin, which the query block under ## Urgent & due needs.

~~~markdown
# {{date}}

## Urgent & due
```tasks
not done
(due before tomorrow) OR ((tags include #deadline) AND (due before in 8 days))
sort by due
short mode
```

# Journal Entries

# Capture
~~~

### 4. Connect Claude to the vault
- **Claude Code** (the command-line tool) is what I use. Install it from [claude.com/claude-code](https://claude.com/claude-code) (or `curl -fsSL https://claude.ai/install.sh | bash` on macOS/Linux, `irm https://claude.ai/install.ps1 | iex` in Windows PowerShell), `cd` into your vault and run `claude`. It reads and writes the files in that folder directly, with no plugin needed. I usually run it inside Obsidian through the [Terminal](https://github.com/polyipseity/obsidian-terminal) community plugin.
- **Claude Desktop** on macOS/Windows also works, connected through **Local REST API** and **MCP Tools**. Desktop now has a [Linux beta](https://itbrief.co.uk/story/anthropic-launches-claude-desktop-beta-for-linux-users) for Ubuntu and Debian too.
- claude.ai on its own can't reach your files. The exception is Remote Control (below), which drives a Claude Code session running on your machine.

On the go, I leave a Claude Code session running in the vault on my laptop at home, run `/remote-control`, and talk to it from the Claude mobile app alongside Obsidian mobile. The laptop has to stay on and awake. 

### 5. Write a `CLAUDE.md` at the vault root
Claude Code reads this file automatically at the start of every session. A starter version:

```markdown
# Vault rules
- This vault uses PARA: Projects/, Areas/, Resources/, Archives/.
  Never create new top-level folders; file notes under the right category.
- Daily notes live in Journals/daily/YYYY-MM-DD.md. They are my raw record.
  Read them; don't rewrite them.
- Wiki/ is yours to maintain. Follow Wiki/schema.md exactly.
- Before any batch of writes, show me a plan and wait for approval.
- Off-limits: [list any folders or note sections Claude must never read]
```

### 6. Write `Wiki/schema.md`
This is the most important file. Here's a minimal version:

```markdown
# Wiki schema
Page types (one folder each):
- entities/  person | organization | project | place
  Sections: summary, Key Details, Timeline (newest first), Related
- concepts/  scope: broad (a theme) or narrow (an event/decision, with parent_concept)
  Sections: summary, Current State / Evidence (newest first), Related
- sources/   one page per ingest run: which raw notes, what changed
- syntheses/ saved answers to questions
Every page has frontmatter: type, sources, last_updated (YYYY-MM-DD).

Rules:
- Read index.md first, always. Add every new page to it.
- Append one entry to log.md per operation: ## [YYYY-MM-DD] op | title
- Timelines and Evidence are append-only. Never reorder or delete.
- Check for an existing page (and aliases) before creating one.
- Never edit files outside Wiki/.

Operations: ingest (journals → pages), query (index → pages → answer),
lint (index vs files, broken links, orphans, stale pages).
```

Karpathy's [gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) is worth reading in full. You can also just paste it into the AI agent running in your vault and ask to discuss implementing the pattern. Tell it to write its understanding into its `CLAUDE.md`, then review that file and correct it.

### 7. How to get started
1. **Week 1:** capture only. Put everything in the daily note for each day.
2. **End of week 1:** ask Claude: *"Read this week's daily notes and propose where each item should be filed in PARA. Show me the plan first."*
   - You can also import all of your notes from wherever else they live. I just dropped mine, which used to be a bunch of .org files, into the folder and asked Claude to organize them after it had processed my first week; that week's notes helped it understand what belonged in my PARA structure.
3. **End of week 2:** start the wiki. *"Read Wiki/schema.md, then ingest this fortnight's daily notes into the wiki. Propose new entity and concept pages before writing."* Go one week of journals at a time at first so you can check its work.
4. Once a request feels routine, ask Claude to save it as a skill. Read the file it writes and adjust it. Skills live in the vault's `.claude/skills/` folder, or in `~/.claude/skills/` if you want them available everywhere. Most of mine are in the latter.
5. Run a lint every week or two.

### Further reading
- Tiago Forte, [Building a Second Brain](https://www.buildingasecondbrain.com/).
- Andrej Karpathy, [the LLM wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).
- [Obsidian Help](https://help.obsidian.md): Daily Notes, Templates, Properties.
- [My earlier post on knowledge management](/My-Obsidian/), for more on how I use Obsidian itself.
