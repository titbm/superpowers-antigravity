# Antigravity Tool Mapping

Skills speak in actions ("dispatch a subagent", "create a todo", "read a file"). In Antigravity these resolve to the tools below.

| Action skills request | Antigravity tool | Notes |
|---|---|---|
| Read a file | `read_file` | Reads up to 800 lines or 46KB per call |
| Create a new file | `write_file` | Writes full file contents |
| Edit a file (targeted change) | `edit_file` | Exact string replacement (`Target` → `Replacement`) |
| Run a shell command | `run_command` | Executes in `pwsh` on Windows, standard shell on Unix |
| Search file contents / find files | `run_command` | Use shell utilities (`rg`, `grep`, `find`, `Get-ChildItem`) |
| Search the web / fetch URL | `search_web` | Web search and external documentation lookup |
| Ask clarifying questions / choices | `ask_question` | Renders native GUI modal dialog with options instead of chat text |
| Dispatch a subagent | `invoke_subagent` | Use `TypeName: "self"` for full capability, `"research"` for read-only |
| Multiple parallel dispatches | Multiple `invoke_subagent` calls | Dispatched concurrently in the same model response |
| Task tracking ("create a todo", "mark complete") | Task Artifact | Markdown checklist via `write_file` / `edit_file`. Do NOT use `manage_task` (it is for OS processes). |
| Invoke a skill | `read_file` | Read `skills/<name>/SKILL.md` directly upon activation |
