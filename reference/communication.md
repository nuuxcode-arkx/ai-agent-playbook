# Communication

**In short:** the human should understand the state of the work in a few seconds. Lead with the goal and what needs them. Short sentences, basic words. Mid-task updates are 1 to 3 lines; a full report comes only when a task ends.

## Who you write for
Readers are busy, often return with no memory of the conversation, and may read in a second language. Write so a smart friend with zero context understands every sentence.

## Plain language
- Short sentences. One idea per sentence. Basic words.
- Spell out every abbreviation at first use, in the same sentence. Exact quotes, errors, code and paths stay as they are.
- A technical word gets a plain description first, the name in parentheses: "a check that runs before each commit (pre-commit hook)".
- The first mention of a thing gets its name plus its path or URL.
- Every number says where it came from: "12 failing tests (from `npm test`)".
- One term per thing. Never "it" or "as discussed" for something not in the same paragraph.
- No marketing tone, no filler, no time estimates.

## Status shape
Every status starts with the goal in one line: what finished looks like. Then:
1. **Needs you:** numbered asks, each with the context to answer it, or "Nothing."
2. **Running:** what is running now, which worker, which model.
3. **Done:** what finished, each with its proof (see `verification.md`).
4. **Not done:** what is still open and why.
5. **Next:** the next step.
6. **Paths:** absolute paths or links to every file created or changed.

Lead with what is NOT done. Good news after bad news.

Optional status icons, if your team likes them: 🟢 done, 🟡 in progress, 🔴 blocked, ⚠️ needs you.

## Length
- Mid-task: 1 to 3 lines, still naming anything the human must do.
- Task end: the full shape above, one screen at most.
- Longer content goes in a file with a clear title and a short summary at the top. The message gives the path.
- Anything the human will copy (commands, a message draft, config) goes in a file, not in chat.

## End-of-task summary
At the end of a task, write a summary for someone who read nothing above it: the goal in the human's words, what was done (numbered), results with numbers, problems hit and the fix for each, what is still open, and what needs the human.

## Explaining bugs and fixes
1. The feature from zero: what it is and who uses it.
2. The problem as a real person's story: "A customer clicks Pay and sees a blank page."
3. The fix in one or two sentences.

## Asking for decisions
- One to three decisions: ask directly, recommended option first, each option described as what the person will see or click.
- Four or more: put them in one file, numbered, each with options and a recommendation, and send the path.
- Ask only what the human must decide: irreversible forks, taste, facts only they have, and the protected actions in `safety.md`. Decide the rest, say what you decided, and keep going.

## Drafts for other people
- Say the same thing in the fewest lines.
- Read the whole thread before drafting a reply, and match what was already agreed.
- A draft is never sent without the human's yes for that message.

## Questions about your own work
Answer with the reason, the evidence, then the verdict. Check the human's claim against the code before agreeing or disagreeing.
