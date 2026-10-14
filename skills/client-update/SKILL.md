---
name: client-update
description: Third step of the operator's daily routine for the client account. Builds the "what's new" page that the operator shows Jordan (the client) at the next meeting, with only the progress since the last meeting, written in first person in the operator's voice (never as if an AI made it), publishes it, and gives the operator the list of documents created with where they are. Use when the operator says "arma la presentación para Jordan", "la página de avances", "lo nuevo para la reunión" or runs /client-update.
---

# Update page for the client

Goal: a short page that the operator shares on screen at the next meeting, plus a list of the documents they should have open.

## Content rules (feedback from the operator)

- **Only what's new since the last meeting with Jordan.** Read the latest note in `docs/reuniones/` (and the transcript, if needed). Leave out anything already discussed there, even if it's important.
- Don't copy the new items into the long-lived content-plan page. It's the permanent plan.
- **First person, the operator's voice:**
  - "What was finished", "I drafted", "What I need from you", "Next for me";
  - Jordan is "you";
  - never "<operator> did…", "prepared by…", nor any mention of Claude, AI or automation.
- In **English**, plain, with large text (the client asked for it).
- Compliance, even on internal pages:
  - no "free" ("no-obligation", "no cost to you");
  - "Medicare Annual Enrollment Period", not "Open Enrollment";
  - the agency's approved disclaimer figures (number of organizations / products), exactly as in CLAUDE.md.
- No local file paths on the page. Jordan doesn't have the repo.

## 1. Gather what's new

- `git log --since="<date of the last meeting with Jordan>"` and the files created or changed.
- Items marked `[x]` in `docs/pendientes.md` after that date.
- Answers to things Jordan asked (figures, sources, documents found).
- New asks for Jordan: only the ones that weren't raised at the last meeting.

## 2. Build the page

- File: `docs/reuniones/AAAA-MM-DD-whats-new.html`, dated for the **next meeting** (usually the next business day).
- Reuse the style of the most recent page (`docs/reuniones/*-whats-new.html`): the agency website tokens, Fraunces + Inter, light and dark mode.
  - Copy its `<style>` block. Don't invent a new design.
- Structure:
  1. **Header:** eyebrow "Avery Benefits Group · Since our review on <date>", title "What's new", one line saying it only covers what changed.
  2. **Tiles:** one per item, each with a pill (Done / Drafted / Found / Needs you) linking to its section.
  3. **One section per item:** what was done and why it matters to Jordan, briefly. If there's text to approve (posts, scripts), include it so it can be read in the meeting.
  4. **What I need from you:** only the new asks.
  5. Footer: "Avery Benefits Group · Jordan Avery, Licensed Insurance Broker · <license line> · Internal update".
- Title (`<title>`): "Update <Mon D>".

## 3. Publish

- The first time for each new page: `Artifact` with `action: "quickstart"` (intent `other`), then `Artifact` publish with `file_path`, `icon` (e.g. "update") and a one-sentence `description`.
  - If the page already exists (same date), republish to the same file.
- Take one look in the browser before publishing (screenshot) and fix only what's visibly broken.
- Commit: the operator as author, no AI attribution, no push.

## 4. Report to the operator (in Spanish, starting with their name)

1. **Page link** and what it covers, in one line per section.
2. **Documents to have at hand during the meeting:** a list grouped by topic, with clickable paths (`[name](path)`) for every document created or updated since the last meeting:
   - PDFs and Word files for compliance;
   - newsletter HTML;
   - posts;
   - notes.

   Say which ones are attached in Gmail drafts, if any.
3. Remind them that the page is private and that, for Jordan to open it alone, they have to share it from the Share menu.
