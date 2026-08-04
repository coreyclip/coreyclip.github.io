---
layout: post
title: Obsidian + Claude 
published: true
tags: technical
---

# My implimentation of the Karpathy LLM Wiki Method
Your brain is brilliant at thinking and terrible at storage. It's a lousy calendar it forgets appointments, or worse, nags you about them at 2am. It's a lossy notebook an idea you had on a walk or in the middle of a conversation is gone an hour later. Every open loop you try to hold in your head is a background process quietly eating the attention you'd rather spend on the actual work.

The solution is as old as written language: get it out of your head and onto something durable. Write it down. Once a thought lives somewhere reliable, your mind is free to stop guarding it and go back to thinking. 

The catch is that externalizing usually just moves the problem. Now you have a pile of notes, and the pile itself needs organizing, searching, and maintaining, and the chore is what adds friction to using your notes. What's new is that the maintenance can now be handed off without hiring an assistant. In this post I'll go over my setup where I capture thoughts and pieces of information, and a large language model does the tedious upkeep of turning that pile into something you can actually use.

The way I do this is by pairing habits and concepts from Tiago Forte's Second Brain with Claude Code running inside my Obsidian vault. In this post I'll detail the habits and workflows that work well for me along with the precise software and plugins I use to make my notes act as an active assistant in making my life work. But first here's a quick reminder of what Obsidian is. 

# What Obsidian is

[Obsidian](https://obsidian.md) is a free and open source note-taking app that stores your notes in text files on your computer. Critically, this is what makes it different from other popular proprietary note platforms like OneNote, Evernote, or Notion. Your notes are just **plain Markdown text files in a folder on your own computer.** There's no proprietary database, no lock-in, no cloud you're forced into. You could open every note in Notepad and lose nothing. 

That plainness is the whole reason it works with an LLM. Because the notes are ordinary text files, *any* program can read and write them, including Claude. The vault isn't a walled garden that becomes vendor-locked; it's a directory a tool can just reach into.

On top of that foundation Obsidian adds the things that make a big pile of notes navigable:

- **Wikilinks** — type `[[Note name]]` and notes connect into a browsable graph, wiki-style.
- **Backlinks** — every note automatically shows what else points to it, so structure emerges without you filing anything.
- **Tags and properties** — `#tags` and frontmatter fields for slicing across folders and giving notes machine-readable metadata.
- **Local-first and cross-platform** — desktop, mobile, and a web clipper, all syncing the same folder. Your data stays yours.

It's free for personal use, and because the storage is just files, nothing you build here is trapped if you ever leave. And critically, the links and tags Obsidian supports are easily read and used by an LLM running within the directory/folder.

You could replicate my setup with another note taking app that critically stores the notes in something like txt, md, or org file formats. But the crucial aspect is that weather it's claude code or some other LLM running *on your filesystem* it needs to be able to access the text. 

# Give the LLM your Obsidian vault and give it a framework for how to work with it

The piece that makes this more than simply notes comes from [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). He noticed that the usual way people bolt an LLM onto their notes is wasteful: the model re-reads your raw documents on every single question, "rediscovering knowledge from scratch" each time it's asked. This is what happens if you don't give the LLM a framework for how to utilize your notes *and* don't specifically tell the LLM to make it's own notes. Asking the LLM to tell you about something you wrote last week or create a outline of all your meetings with xyz person, takes longer than necessary and consumes way more tokens becase the LLM combs the entire directory of notes to find that information. Also the larger the scope the less precise the LLM is (aka it hallucinates as people say). 

Andrei's alternative is to let the LLM *maintain a wiki.* Instead of re-searching your notes on demand, the model incrementally builds and keeps up a set of interlinked Markdown pages — synthesizing knowledge once and reusing it, so it compounds over time instead of resetting. As he puts it, **"Obsidian is the IDE; the LLM is the programmer."** You browse, curate, and dump raw notes; the model does more of the writing and cross-linking.

*"The tedious part of maintaining a knowledge base is not the reading or the thinking, it's the bookkeeping."* That bookkeeping filing, tagging, updating cross-references, catching stale claims is exactly what an LLM is optimized to do and you are not. The human keeps the two interesting jobs (deciding what to feed it, and asking good questions); the model takes everything tedious in between. 

My setup is my own adaptation of Andrei's idea, wired into Claude and organized with the [PARA method](https://fortelabs.com/blog/para/) from Tiago Forte. I still stick to PARA because in the end the vault and even the wiki that Claude creates is meant to be viewable and comprehensible to both me and the LLM. Digitally organizing things into projects (items with a beginning and end), areas (ongoing responsibilities with no end), resources (reference material to support projects, areas, and things I just find interesting), and archives (finished or expired projects and areas) helps me at a glance know what shape my life looks like and signals to the LLM what my priorities and interests are. 

# The Day to Day

Capture everything cheaply into a single daily note, route it into durable folders or flesh them out later, and let Claude synthesize what you've captured into an evergreen knowledge base you actually reuse. I use Obsidian's daily note plugin to automatically create a note for each day and do 90% of my journaling there. It acts as the default note for everything that happened that day. Not only does it eliminate the need to think about which note a piece of info goes into, it also lets the LLM quickly find where new information is, since it can tell what day or time it is. I utilize linking and tags to give context to notes when needed but most of the time I just write markdown headers to give context. 

Along with just typing things. I also greedly and readily paste links, utilize the obisidian webclipper extension to take in whatever I come across online that *might* be useful. The capture heading in my journal template is essentially just a basket that get's sorted later. 

Prior to wiring Claude into all of this, I would organize the notes linked out from the daily notes into projects, areas, resources, and archives as a kind of Sunday ritual or in my mornings, but now I mostly let Claude do that and review periodically.

What this serves as is a reusable and growing set of documents that make working with a large language model useful for the purposes you determine. The tools matter far less than the habits that make them worth having, so I'll spend the first half of this post on the habits, and the second half on the concrete setup including a full install guide for Linux, macOS, and Windows so you can get running regardless of your machine.

This is [my earlier post on knowledge management](/My-Obsidian/), if you want to see a more detailed explanation of the obisian setup I currently use.

# The wiki layer, adapted to PARA

The wiki is the part that makes the system compound and makes the LLM operate **better** within your notes. I'll explain what each part does, but note that 95% of this is maintained by the LLM itself, though it's meant to be human-readable in case you need to essentially debug the setup by correcting mistakes or updating knowledge. Though unless it's a small change I mostly just tell Claude to update its wiki. By and large you can simply ask a LLM why it did something or tell it how to operate, and critically tell it to update it's CLAUDE.md or whatever equivalent document it uses as it's central guidance. 

The LLM maintains a persistent, interlinked set of Markdown pages instead of re-searching your raw notes on every query. Here's the structure underneath it. Karpathy's approach has three layers and three operations:

- **Raw sources** are immutable — articles, PDFs, your own notes. The LLM reads them but never edits them unless told to do so. This is your Obsidian vault and the aforementioned PARA and daily notes. 
- **The wiki** is LLM-owned markdown: *entity* pages (a person, a project, a company), *concept* pages, and cross-references between them.
- **The schema** is a config file documenting the conventions and workflows. This is the single most important file, because it's how the LLM knows the rules.

The operations are **ingest** (read a new source, note the takeaways, update the relevant pages, append a line to a log), **query** (search the pages and answer with citations — a good answer often *becomes* a new page), and **lint** (a periodic health check for contradictions, stale claims, orphaned pages, and missing links).

PARA and the wiki are complementary rather than competing, and they map onto each other cleanly:

| Karpathy's layer | In a PARA vault |
|---|---|
| Raw sources | Your **daily notes** plus the `Capture` inbox in the daily notes. |
| The wiki | A dedicated **`Wiki/` folder** of evergreen notes the LLM owns, kept *separate* from PARA so ownership stays clean. |
| The schema | A `CLAUDE.md` (or a Claude skill's instructions) documenting the conventions. |
| index / log | A wiki index note plus an append-only ingest log. |
| Ingest / query / lint | The routing and wiki-update workflows you run with Claude. |

The mental model I keep: **PARA answers "where does this note live and what's it for?"** — one home per note with the exception of journal notes whose content is decomposed into PARA categories. **The wiki answers "what do I actually know about X?"** — synthesized, interlinked, LLM-maintained. Routing feeds both: a captured item lands in its PARA home *and* triggers a wiki update if it's worth synthesizing.

Obsidian gives you three ways to connect notes, and each does a different job here. **Links** (`[[Note name]]`, the clickable wikilinks) are the knowledge graph — Karpathy's cross-references, where an "orphaned page" is just a note nothing links to. **Tags** (`#tag`, nestable like `#status/active`) are cross-cutting facets that span folders — the dimensions a single home can't capture. **Properties** (frontmatter at the top of a note) are the machine-readable hooks the LLM leans on: a page's `type`, `status`, `aliases`, and dates. Folders give location, properties give role, tags give facets, and links give the graph — the same vault, read two ways.

# Setup: install and connect, by OS

There are three moves: install Obsidian, install Claude, and wire them together. The only step that genuinely branches by operating system is how Claude reaches your vault, so read that part carefully.

## 1. Install Obsidian

Download it from [obsidian.md](https://obsidian.md), or use a package manager:

| OS | Command |
|----|---------|
| **macOS** | `brew install --cask obsidian` |
| **Windows** | `winget install Obsidian.Obsidian` (or `scoop install obsidian`) |
| **Linux** | `flatpak install flathub md.obsidian.Obsidian` (or the AppImage / Snap) |

## 2. Choose which Claude connects to your vault

This is the decision that trips people up. Pick by OS:

| You're on | Use | Why |
|-----------|-----|-----|
| **macOS / Windows** | **Claude Desktop** | Pairs with the `mcp-tools` Obsidian plugin for a one-click, secure vault connection. |
| **Linux** | **Claude Code** (CLI) | Claude Desktop isn't offered on Linux, and you probably prefer the terminal anyways :). Claude Code reads and writes files in whatever folder you run it from, so you run it *inside your vault*. |

I myself run Linux and use the Obsidian shell plugin to run Claude directly in Obsidian.

Then install it:

- **Claude Desktop** (macOS / Windows): download from [claude.ai/download](https://claude.ai/download).
- **Claude Code** (Linux, or power users on any OS): the native installer is simplest and needs no Node —
  - macOS / Linux: `curl -fsSL https://claude.ai/install.sh | bash`
  - Windows (PowerShell): `irm https://claude.ai/install.ps1 | iex`
  - or via npm: `npm install -g @anthropic-ai/claude-code`

  Then run `claude` from inside your vault folder.

## 3. Wire the bridge

**macOS / Windows — Claude Desktop plus the MCP Tools plugin:**

1. In Obsidian, go to Settings → Community plugins, then install and enable **Local REST API**. Open its settings and copy the **API key** it generates — MCP Tools needs it.
2. Install and enable the **MCP Tools** plugin (`mcp-tools`, by Jack Steam).
3. In MCP Tools' settings, click **Install Server**. It downloads a small local MCP server and configures Claude Desktop for you.
4. **Fully quit and reopen Claude Desktop** — quit it, don't just close the window. Your vault's tools (search, read/write notes, templates) now appear in Claude Desktop.

MCP Tools is desktop-only — it won't run on Obsidian mobile, and it targets Claude Desktop specifically. But you can start a Claude session in your vault, run /remote-control, and interact with Claude and the vault via a remote session. This requires the machine running Claude to be on and not asleep. I usually just leave my laptop at home running the session and use the Claude mobile app along with the Obsidian mobile app.

**Linux — Claude Code pointed at the vault:**

1. Open a terminal in your vault folder: `cd ~/path/to/Vault`.
2. Run `claude`. It can now read and write the notes in that folder directly, with no plugin required. Ask it to route your daily note and it edits the files in place. This alone is enough to start.

# Your first week

Start here:

1. Enable the **Daily Notes** core plugin, point new notes at a `Journals/` folder, and set a template so every day opens with the same headings. A minimal one:

~~~markdown
# {{date}}

# Tasks
```tasks
not done
due before tomorrow
```

# Journal Entries

# Capture
~~~

2. Create the four PARA folders: `Projects/`, `Areas/`, `Resources/`, `Archives/`.
3. Install the **Tasks** community plugin (the `tasks` block above needs it).
4. **Run the capture habit for a full week** — everything into the daily note, no organizing unless you're into that kind of thing.
5. Once capturing is automatic, start the batch jobs: ask Claude to route the week's daily notes into PARA, then to summarize your captured links. Ask it what it thinks about your notes and what kind of notes it would create for the PARA structure. Chat with Claude, discuss, push back, etc.
6. Add the wiki layer last, once you have enough captured material to make synthesizing worthwhile. To do this, all you really need to do is tell Claude you want to replicate the wiki strategy in [this gist by Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). You can literally just copy all the text in that gist, paste it into Claude, and tell it to get to work — though I'd advise adding to the prompt that you want to chat about what you're creating and how it could support your day to day and tell it to write what it's understanding is to it's CLAUDE.md file. Then go review the CLAUDE.md file and either tell claude how to change it or update it or just manually edit the text yourself. 

# Tips and gotchas

- **Habit before tools.** If capture isn't easy this all just doesn't really come together. Nail the daily dump first; everything else is optimization.
- **Batch, don't organize live.** Deciding "where does this go?" mid-thought is exactly the friction that kills these systems. Let it pile up in the daily note and sort in batches.
- **Let Claude propose, and you approve.** For routing and wiki updates, have Claude show you a plan before it writes anything. You stay in control, and it learns your structure over time.
- **Keep the daily note as the source of truth.** The routed notes and the wiki are *derived*; the chronological journal is the ground truth you can always rebuild from.
- **Mind your privacy.** When you let Claude read your vault, the notes it accesses are sent to Claude to process. Keep genuinely sensitive material — passwords, medical, financial — out of the vault entirely, or in a separate vault you never connect. If you need to work with private material, a local model setup (something like Ollama) is the direction to look, though it's more technical.


# What to do with this

Here are some things I use my Claude + Obsidian setup for:
- Generating summaries of my week or month to support the [weekly or monthly reviews that Tiago Forte recommends](https://fortelabs.com/the-weekly-review-is-an-operating-system-8e8e04f885ab).
- Pulling together notes on topics into complete reference documents or presentations.
- Logging and tracking language learning from words or phrases I capture during my day-to-day life.
- Triaging the `# Capture` inbox for me: Claude opens each link I dumped during the day, summarizes it, and either files it as a resource note or turns it into a follow-up task, so I never sit down to a wall of unread bookmarks.
- Drafting blog posts, this one included, from notes I'd already captured in the vault rather than starting from a blank page.
- Turning a pile of research into a finished artifact: feed in a stack of articles and get back a cited report, a slide deck, or even an audio rundown I can listen to.
- Building and maintaining topic wikis I keep growing over time, like a digital-marketing knowledge base I've forked across a few markets and languages.
- Prepping talks and workshop decks by having Claude pull the relevant wiki pages together into an outline I can build a presentation from.
