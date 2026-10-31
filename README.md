# GAB PoC: claude-code-action write-permission gate (authorized test org)

Reproduces the exact authorization call made by `anthropics/claude-code-action@v1`
(`src/github/validation/permissions.ts` -> `checkActorWritePermissions` ->
`repos.getCollaboratorPermissionLevel`) for actors that do not have write access.

Run: https://github.com/PDFtoMark/gab-poc-claude-gate/actions/runs/41167527290

Observed output:

    lazyhacker6 -> HTTP 200 permission=read  (write gate => false)
    Ety60       -> HTTP 200 permission=admin (write gate => true)

`checkActorWritePermissions` only accepts `admin` or `write`; any other value is
returned as `false`, and `run.ts` then throws
"Actor does not have write permissions to the repository" before the trigger
phrase is parsed, before `prepare Tag Mode` runs, and before any Claude Code CLI
execution. A 404 for a non-user actor follows the `allowed_bots` path, which is
empty in the target workflow, and therefore also fails closed.

Authorized testing only, PDFtoMark test org.
