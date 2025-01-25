---
layout: post
title: A brief rundown on my knowledge management system
published: true
tags: Technical
---

## Motivation

Years ago when I was still in university, I had the opportunity to learn from [Tiago Forte](https://fortelabs.com) back when he was first getting his start in becoming a sort of note taking guru. I was slammed with trying to complete two thesis (double major), some tough math classes, an RA gig in the freshmen dorms, and trying to apply for the peace corps (under the old system mind you). I was originally just supposed to just get his advice on how to put my peace corps application together but after simply admitting that I felt completely overwhelmed and unable to put in the time to do it. Tiago calmed me down over zoom and instead offered to give me a rundown on his task management system that he was trying to sell a course on. Since then I've watched him go from teaching David Allen's Get Things Done, to a class on Evernote, onto a more generalized Building a Second Brain course, and now I believe Personal Knowledge Management is what he terms his craft now.

I've found his framework extremely useful over the years. Even when implemented in doses and pieces it can help bring a lot of structure to what otherwise could be a quite chaotic life. My tools and workflow has morphed over time but I wanted to take the time to quickly give an overview of what I'm currently using.

Tiago is tool agnostic and in the end doesn't advocate a all or nothing approach but does give pointed advice when requested. To summarize my understanding of his work in my own words, Tiago advocates for the following systems when it comes to note taking and task management:

**CODE** - Capture, Organize, Distill, Express 

**Capture** - means pull in information into your notes, be greedy if it looks vaguely interesting clip it, save it, bookmark it. 

**Organize** - organize the captured material, that doesn't mean a file system necessarily it just means making the information more accessible.

**Distill** - extract what is more prescient or useful from your notes. Think highlighting, annotating, or summarizing the captured notes. 

**Express** - finally take the what you have from the previous exercises and create something new to either test your hypothesis before an audience and gather feedback. Basically contribute back to the ecosystem so someone can capture what you wrote. 

_now how to I organize_

**PARA** - Project, Area, Resources, Archive 

Pretty much all of your knowledge can be thought of as a project, area, or resource. A Project is a defined deliverable with a due date and stakeholder. That stakeholder could be you or someone else. A Project can have one or many tasks. Areas are things you're responsible for on a recurring basis. I have the basic facts sheet for taking care of my cats for sitters in my Areas, since it's a document that I just hand out to people for reference and is infrequently updated but frequently referenced. But creating such a document was a project. Basically the projects are temporal, while areas are recurring. The benefit of these distinctions to me feels shifty over time since as I'll explain later.  I try to take to heart Tiago's insistence that we use our brains for what it's useful for and our note taking system, usually a computer is good for. 

Resources are items we use to support the projects and areas, it's where captured items first land and often live for good. 

Archives, useful if you're dealing with physical notes. Basically the stuff you clean off your desk and shove away in a box. There if you need them but not immediately distracting. Not as applicable for digital notes unless you have so much text it's filling up your google drive. 

## What is Obsidian?
Obsidian is a text editor that creates a network of interconnected notes stored as plain text Markdown files on their local device. Emphasizing local data ownership, flexibility, and extensibility, 
### Main Features

1. **Markdown Support** : All notes in Obsidian are written in Markdown, a lightweight markup language that is easy to learn and powerful for formatting text. If you've used reddit it's the same stuff that let's you do inline text formatting. This ensures plain text files that are highly portable and compatible with other programs. No proprietary data file types like docx or whatever, it's just text. Markdown headers provide organization in the file itself along with bullet points or numbered lists if you like that more. Also it's easy to use source control like git with text files. 
    
2. **Bidirectional Linking** : create links between notes easily. These bidirectional links allow users to build a web of interconnected ideas, enabling the emergence of complex thought networks akin to a personal wiki.
    
3. **Plugins and Community Support** : Obsidian's extensive plugin ecosystem allows users to expand its functionality according to their needs, ranging from task management and calendar tools to advanced graph visualization. The active community continually develops new integrations and improvements.
    
4. **Customizability** : Users can customize Obsidian's interface and functionality extensively. Themes, custom CSS, and multiple panes. You can make it as pretty (or ugly) as you'd like kind of like your old myspace page. 
    
5. **Tags and Metadata** : Notes can be enriched with tags and additional metadata, improving their organization and searchability. This feature complements the linking system by providing multiple methods to structure and retrieve information. This is the killer feature of obsidian, not searching just search bar. 
    
8. **Local Storage** : All data is stored locally by default, which enhances privacy and ensures you have control over your notes. This is in contrast to cloud-based note-taking apps where the data resides on external servers. This also means if obisidian disappears your notes won't either, also you can make your notes available across networks however you'd like. For me Dropbox works just fine.
    
9. **Cross-Platform Availability** : Obsidian is available on multiple platforms including Windows, macOS, and Linux, with mobile apps for both iOS and Android, allowing for seamless access to your notes wherever you are. The Dropsync app gets my obsidian notes onto my phone and other devices reliably enough. 

And yeah it's free* though you can pay to have Obsidian store and sync your notes to your devices for you. 
## My Note-Taking System in Obsidian

### Principles

- **Ease of Use**: If a system isn't easy, it’s not useful. It should require minimal thought to engage with.

- **Plain Text Files**: All notes should be in text files to avoid vendor lock-in and ensure compatibility. Any worthwhile tool can parse text, notable right now large language models like gpt and llama. Also I often use scripts and other tools to generate text, the simple [MarkDownload](https://github.com/deathau/markdownload) turns my emails and webpages into markdown that then I use a cronjob to move over to my obisidian notes. 

- **Links and Tags Over Folders**: Utilize links and tags for organization. Computers excel at searching, so rely on them instead of manual folder structures. Physical files are nested to make finding things easier. But the search bar makes this not as essential. 
### Concepts

- **Tasks and Projects**: Life is composed of tasks that form projects. Projects have specific deliverables and deadlines. Recurring projects become areas of responsibility, supported by various resources.

- **Kanban Workflows**: Task workflows should fit into categories that minimize confusion and be visualized using kanban boards.

- **Focus Management**: Avoid distractions by only displaying tasks that require immediate attention. Protect your mental bandwidth more diligently than your computer's resources.

### My Workflow with Obsidian

#### Daily Journal

link to daily journal template [here](https://coreyclip.github.io/2025-my-obsidian-daily-notes-template/)

**The Daily Journal**:

- Use the Daily Notes plugin which generates a new note for each day.

- I use a template for this that automatically creates a fresh journal file each day.

**Tasks**
- I have an obsidian query block that queries tasks due this week. The tasks I create with the tasks plugin can also have tags, due dates, can be set to be recurring, and contain links to SOPs for the tasks or really whatever I may need as long as it's text. 
- links to pdfs and images are very much still text and markdown will render images. 

Journal Entries Header

- I have a Journal Entries section where I log entries throughout the day: tasks, notes, reminders, thoughts, etc. all go under this header. I use the Obsidian tasks plugin to create tasks which then appear in future journals. I also have a document with an Obsidian query that pulls up all of my tasks. 

- Tag entries by subject, type, and connections for effective organization. I am chaotic about this and it works for me. Sometimes I have tags that are similar in nature overgenerating results when I search for them but the time it takes to simply pick the right document vs stressing about the tags isn't a good trade off. 
#### Capture Section

**Manages Incoming Information:**

- Anything shared from your mobile device to Obsidian goes to "Capture" in the daily note. The capture section is at the bottom of the file since Obsidian appends text to the end of the file when something is shared with it. 
- On my phone pretty much every browser or app I have has an option to "share" something and then it's as simple as 

- Occasionally I'll review and organize captured data, tagging for easier future retrieval. I will sometimes do a search for a tag and then pull the information under the capture headers in my journals into one note. A simple `grep` command often works well for this though I'm sure there's a Obsidian way to do this. 

#### Note Creation and Organization


4. **Create and Tag New Notes:**

- I mainly create new notes from journal entries using markdown links, then tag and navigate to them. I first started using this workflow when I used to use logseq which is very similar to Obisidian but I abandoned due to it's lack of android support at the time of my switch. Logseq encourages this daily journal format and stresses tags and links to a greater degree than Obisidian. 

- Organize notes into "Project," "Resource," or "Area" folders for personal ease, though this structure is optional. I frankly just do this out of habit from when I was more attached to the idea of digital notes being similar to physical ones. But there is some utility in doing this since it makes it easier for scripts I wrote to then handle different broad buckets of notes differently. For example when doing some RAG procedures I just pull from the resources folder but I could just as well have done with via tags with a bit more code. 

## Conclusion

That's basically it. I rarely find it needs to be more complicated than this. In fact I probably overly complicated things with my commentary. Here's some links to some of the topics and features in Obsidian I mentioned but didn't explain:

https://help.obsidian.md/Linking+notes+and+files/Internal+links
https://publish.obsidian.md/tasks/Introduction
https://publish.obsidian.md/tasks/Queries/About+Queries
https://help.obsidian.md/Plugins/Daily+notes