---
source_url: https://conventionalcomments.org/
fetched: 2026-04-27
license: Creative Commons CC BY 3.0
note: Structured extract of conventionalcomments.org. Label and decoration definitions reproduced verbatim from the spec.
---

# Conventional Comments

## Format String

```
<label> [decorations]: <subject>

[discussion]
```

## Labels (verbatim definitions)

1. **praise:** "Praises highlight something positive."
2. **nitpick:** "Nitpicks are trivial preference-based requests."
3. **suggestion:** "Suggestions propose improvements to the current subject."
4. **issue:** "Issues highlight specific problems with the subject under review."
5. **todo:** "TODO's are small, trivial, but necessary changes."
6. **question:** "Questions are appropriate if you have a potential concern but are not quite sure."
7. **thought:** "Thoughts represent an idea that popped up from reviewing."
8. **chore:** "Chores are simple tasks that must be done before acceptance."
9. **note:** "Notes are always non-blocking and highlight something to take note of."
10. **typo:** "Typo comments are like todo:, where the main issue is a misspelling."
11. **polish:** "Polish comments are like suggestion:, improving quality."
12. **quibble:** "Quibbles are very much like nitpick:."

## Decorations (verbatim definitions)

1. **(non-blocking):** "Should not prevent acceptance under review."
2. **(blocking):** "Should prevent subject acceptance until resolved."
3. **(if-minor):** "Resolve only if changes are minor or trivial."

## Examples Shown on the Page

- `suggestion: This is not worded correctly. Can we change this to match the wording of the marketing page?`
- `suggestion: Let's avoid using this specific function…`
- `issue (ux,non-blocking): These buttons should be red, but let's handle this in a follow-up.`
- `question (non-blocking): At this point, does it matter which thread has won?`
- `suggestion (security): I'm concerned implementing our own DOM purifying function…`
- `suggestion (test,if-minor): Missing unit test coverage…`
- `nitpick: \`little star\` => \`little bat\``
- `chore: Let's run the \`jabber-walk\` CI job…`
- `praise: Beautiful test!`

## Stated Purpose

Per the page: "Labeling comments encourages collaboration and saves hours of undercommunication and misunderstandings."

## License

Creative Commons CC BY 3.0
