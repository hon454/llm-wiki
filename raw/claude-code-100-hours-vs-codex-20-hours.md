# Claude Code (~100 hours) vs. Codex (~20 hours)

Source: Reddit r/ClaudeCode
URL: https://www.reddit.com/r/ClaudeCode/comments/1sk7e2k/claude_code_100_hours_vs_codex_20_hours/
Date: 2026 (exact date unknown)

---

Since some people keep asking about the differences, I hit my CC limits Friday morning, so decided to try Codex over the weekend. I've put ~20 hours into it. Not vibe coding, co-developing.

If you just want to know about both, skip to 'Claude Experience' and 'Codex Experience'.

## My Experience:

I'm a 14 year engineer with time in MAG7 and now at another major tech firm. Principal/Staff Eng Manager equivalent. Experience is all platform level with heavy distributed systems experience.

## Dev stack/App Structure:

VSCode Extensions in a 80k LOC python/typescript project with ~2800 tests. It's a data analysis application where a user uploads some pdf/csv/xml files from different sources, they're parsed and normalized into a structured data model backed by postgres. It connects to a backend live data provider over websocket which streams current data into the data model. The server side updates certain analyses from the data stream and SSEs to the web UI. All strongly architected - not just 'vibed'.

## Shared Agentic Workflow:

Plan mode first with a fairly thorough and scoped prompt. plan-review skill when a plan is drafted, which runs 8 subagents (architecture, coding standards, ui design, performance and some others). Each subagent has tightening prompts and explicit reference documents from earlier 'research' sessions (for example, 'postgres_performance.md', 'python_threading.md'. 'software_architecture.md'): Architecture review specialist is prompted to review, for example, SOLID, DRY, KISS, YAGNI with specific references for each concept.

Do code. Each phase of the plan is committed separately and a code-review skill (basically a reuse of the plan subagent specialists) is run on each commit and I manually review feedback and add comments and steer.

CLAUDE.md ~100 lines. TDD, Git Workflow, a few key devex conventions and common project tool use like Docker commands.

## Claude Experience (Opus 4.6):

It feels like an engineer on a time crunch who's just trying to get the feature built and not really worried about adding hacks, patches, spewing helper functions instead of revisiting the core architecture.

Interactive. Needs much more babysitting.

Speeds towards getting things working. It doesn't really 'take it's time' or think before acting.

Despite aggressive manual management of context (I think the 1MM context is a noob trap and you need to keep it under a quarter of that), it frequently blatantly ignores CLAUDE.md. Like, almost at least once a session I'll see it do this.

Semi-frequently will leave a task half-done. Like, if it's migrating a test suite (I have 8 suites) from one async pattern to another, I'll find that it did it for most of the tests but left a few on old patterns.

Weirdly, it almost never thinks to add new files for new functionality. It loves just adding functions to existing files instead of following strong OO and factoring (I came from C/C++ and prefer to keeps each files <600 lines ish)

Loves to change tests to match what it thinks the goal of the work is. I've done a lot of work to tell it 'after implementing a change, if tests break, stop and prompt me, don't blindly fix it'. In general, the tests is writes are 95% useful and 5% pinning broken behavior. This compounds over time.

## Codex Experience (GPT-5.4)

It feels like a junior-ish senior (5-6 years experience). It will frequently stop, pull back and rework code to be cleaner without be having to interact with it.

It's a LOT slower than Claude. Like 3-4x slower for the same task.

It's more thoughtful and deliberate. It doesn't just extend 'god classes' like Claude does. It automatically factors things to be a lot tighter. It will revisit it's assumptions and rework stuff halfway through to clean it up.

A few times I've seen it do things I hadn't thought of, which are additive.

I have never seen it ignore AGENTS.md. It won't event let me override directives mid session.

At this point I'm actually just firing it off and coming back when it's done to review the work. It's demonstrated competence so I don't feel the need to be watching the output line by line to wait for it to go off the rails.

## Overall

Codex Pro x5 seems to have similar usage caps to Claude x20.

Codex is noticeably slower, less interactive and more deliberate. Claude is faster, interactive (needs babysitting) and more 'get it done'.

I get more done in a session with Claude, but Codex work is better. So with Claude I can prototype and build extremely quickly, but I have to guide a lot of refactorings every few days. I do still do this with codex as the app evolves, but it's less 'go and see what crap I have to cleanup' and more 'the app has grown and it's time to refactor'

If I wanted a 'vibe code' experience for a low to moderate complexity project, Claude is great and I'll get it done faster. If I want to build enterprise software, I'd lean Codex.

So, both useful. But I think Claude requires a skilled, focused driver more than Codex does. Note: both are going to give crap output if you don't know SWE at all.
