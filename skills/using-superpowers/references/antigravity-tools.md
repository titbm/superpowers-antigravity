# Antigravity Tool Mapping

Skills speak in actions ("dispatch a subagent", "create a todo", "read a file"). In Antigravity these resolve to the tools below.

| Action skills request | Antigravity tool | Notes |
|---|---|---|
| Read a file | `read_file` | Reads up to 800 lines or 46KB per call |
| Create a new file | `write_file` | Writes full file contents |
| Edit a file (targeted change) | `edit_file` | Exact string replacement (`Target` → `Replacement`) |
| Run a shell command | `run_command` | Executes in `pwsh` on Windows, standard shell on Unix. On Windows, run bundled scripts with `bash <path>`. For long-running servers/daemons, set `IsDaemon: true` and `WaitMsBeforeAsync: 500`. |
| Search file contents / find files | `run_command` | Use shell utilities (`git grep`, `Select-String`, `Get-ChildItem`) |
| Search the web / fetch URL | `search_web` | Web search and external documentation lookup |
| Ask clarifying questions / choices | `ask_question` | Renders native GUI modal dialog with options instead of chat text |
| Invoke a skill | `read_file` | Read skill's `SKILL.md` using the exact path from Available skills list in system prompt |
| Dispatch a subagent | `invoke_subagent` | Pass `TypeName` ("self" for full capability, "research" for read-only), `Role` (brief title, e.g. "Implementer"), and `Prompt`. See **Subagent Mapping** below. System wakes parent reactively; do not poll. |
| Multiple parallel dispatches | Multiple `invoke_subagent` calls | Dispatched concurrently in the same model response |
| Task tracking ("create a todo", "mark complete") | Task Artifact | Markdown checklist via `write_file` / `edit_file`. Do NOT use `manage_task` (it is for OS processes). |

## Subagent Mapping (`invoke_subagent`)

When skills specify which model or agent to dispatch, map their exact wording directly:

| What the skill asks for | `Model` | `TypeName` | What it does |
|---|---|---|---|
| **"fast, cheap model"** / **"least powerful model"** | `"flash"` | `"self"` | Routine implementation tasks, isolated 1-2 file edits |
| **"standard model"** / **"general-purpose"** | `"flash"` | `"self"` | Multi-file integration, regular features, standard bugfixes |
| **"most capable available model"** | `"pro"` | `"self"` | Architecture, difficult bugs, final whole-branch review |
| **"research"** / **"explore"** (codebase search) | `"flash"` | `"research"` | Read-only grepping, finding symbols and references |

### Actual Model Launched by Backend Based on Parent Session

| Parent Session | Subagent `"flash"` | Subagent `"pro"` |
|---|---|---|
| **PRO** *(Argon, Opus)* | **Flash** model | **PRO** model |
| **FLASH** *(Gemini Flash)* | **Flash** model | **Flash (High Thinking)** model |
