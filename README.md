# GAB PoC: claude-code-action write-permission gate

Reproduces the exact authorization call made by `anthropics/claude-code-action@v1`
(`src/github/validation/permissions.ts` -> `repos.getCollaboratorPermissionLevel`)
for an actor that has no write access to the repository.

Authorized testing only, PDFtoMark test org.
