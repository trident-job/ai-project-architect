# Agentic Task Delegation

A methodology for delegating bounded mechanical tasks to agentic tools while retaining strategic oversight in conversation.

The examples below use Claude's Cowork feature, but the pattern applies to any AI workflow where a separate agent or tool can perform filesystem operations independently: Claude Code, custom scripts, or equivalent features in other AI platforms.

---

## When to Use This Pattern

The pattern works well when a task is:

- Mechanical but detail-intensive (reading many files, renaming, sorting, building indexes)
- Clearly bounded to a specific folder or set of files
- Describable without needing the full project context
- Something that would consume significant context window and conversation time if done inline

Examples: organizing source documents into case folders, sorting and identifying meaningful data in archival collections, categorizing and renaming financial documents, processing batches of emails or screenshots, building inventories of large file collections, extracting structured data from unstructured sources.

The pattern does NOT work well for tasks requiring strategic judgment or project context, drafting that needs voice calibration, work that depends on decisions not yet made, or anything where the "right answer" requires understanding the broader project.

---

## The Workflow

### 1. Scope the Task (Claude + Human, in conversation)

Identify what needs doing, what the inputs are, and what the output should look like. Make key decisions before writing the prompt. Cowork agents can't make judgment calls about where things belong or how they relate to other parts of the project.

Questions to resolve: What folder(s) does Cowork need access to? What's the output structure? Are there files that belong elsewhere? What documentation should Cowork produce? Are there files Cowork should NOT touch or open?

### 2. Draft the Prompt (Claude, saved to project)

Write detailed task instructions as a standalone text file. The prompt must contain everything the Cowork agent needs to do the job without any other context. See "What Makes a Good Prompt" below.

### 2b. Optional: Prompt Review by Cowork

For complex delegations, consider having Cowork review and critique the prompt before running it. Start a Cowork session, provide the draft prompt, and ask for feedback on feasibility, efficiency, and potential issues.

Cowork brings a distinct operational perspective, particularly around parallel agent architecture, batch sizing, filesystem operations, and verification strategies. A prompt that looks complete to the Chat instance that wrote it may have significant improvements that Cowork identifies immediately.

The general principle: Claude in Chat, Claude Code, and Cowork are all capable peers with different operational contexts, not subordinates receiving instructions. Treat delegation prompts as proposals that the receiving instance can improve.

### 3. Hand Off to Cowork (Human)

Switch to Cowork. Point it at the specific folder it needs to work in, not a parent directory. Provide the prompt. Start a fresh Cowork session for each task.

### 4. Monitor (Human + Claude)

Claude doesn't need filesystem access during this step. The human can relay progress updates, screenshots of Cowork's plan, or questions that arise. Claude advises from the web or mobile interface if needed.

Watch for: Cowork requesting permissions it shouldn't need (especially delete permissions when the prompt says not to delete), subagents creating extraneous files, and error messages (especially image dimension limits).

### 5. Move Output Into Place (Human)

After Cowork finishes, move the organized output from the staging area into the project's file structure. Cowork creates output in an Organized/ subfolder within its working directory. Move the contents to the final destination.

### 6. Review and Integrate (Claude, back on desktop)

Claude reviews what Cowork produced: checks file names, reads indexes and narrative files, verifies nothing is missing or misfiled. Then Claude does the integration work that requires project context: writing case files, updating cross-references, adding to lessons or other project-level documents.

### 7. Clean Up (Human + Claude)

Delete the original source files once the organized versions are confirmed. Remove the staging folder. Completed Cowork prompts can stay as templates or be removed.

---

## What Makes a Good Prompt

The prompt is the entire interface between the project's knowledge and the Cowork agent. It needs to be thorough because Cowork has no access to the project's history, conventions, or strategic context.

A strong prompt follows this structure:

1. One-line task summary
2. Background context (what the files are, why they matter)
3. Directory structure the agent will find
4. Output structure to create
5. File-by-file handling instructions where needed
6. Naming conventions
7. Documentation to produce (INDEX.txt, narrative files)
8. Format specifications for those documents
9. Rules and constraints (especially "do not delete")
10. Technical notes (how to read unusual file types)

**Be explicit about everything.** Cowork doesn't infer from context. If files from 2023 shouldn't be mixed with files from 2024, say so and explain why.

**Specify the output structure before the file handling instructions.** The agent needs to know where things go before it starts sorting them.

**Include background context.** Even though the agent doesn't need the full project history, knowing what the files represent helps it make better naming and description decisions.

**Ask for documentation.** INDEX.txt files and narrative summaries are the bridge between Cowork's mechanical work and Claude's integration work.

**Always say "do not delete any files."** Cowork should copy to the organized structure, leaving originals in place until confirmed correct.

**Include technical notes for unusual file types.** Instructions for reading .eml files (multipart MIME boundaries, quoted-printable encoding, where to find the plain text body) prevent errors.

---

## Known Issues and Workarounds

**Image dimension limits.** Cowork hits an error when processing multiple images larger than 2000px. Resize images before handing off. On macOS: `sips --resampleWidth 1000 *.png`

**Subagent file creation.** Cowork sometimes uses subagents that create extraneous files in the source directory. If Cowork requests delete permission for these, deny it unless you can verify exactly what it wants to delete.

**Copy vs move.** Because the prompt says "do not delete," Cowork copies files rather than moving them. Originals remain in the source directory. This is intentional. Delete originals after confirming the organized versions are correct.

**Session isolation.** Start a fresh Cowork session for each task. Leftover context from a previous task can cause unexpected behavior.

**Folder scope.** Point Cowork at the specific folder it needs, not a parent directory. Broader access lets it get distracted by other folders.

---

## What Stays in Chat vs. What Goes to Cowork

Cowork handles the mechanical work. Chat handles everything that requires project context: strategic decisions about filing and organization, voice-calibrated drafting, case file writing and updates, lessons and pattern documentation, and quality review of Cowork output.

The division: Cowork reads, renames, sorts, and documents. Claude in Chat judges, writes, connects, and integrates.
