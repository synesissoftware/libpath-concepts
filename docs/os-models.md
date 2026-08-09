# libpath-concepts — OS path models <!-- omit in toc -->

Unix and Windows file-system path *form* models under **libpath** rule sets. Terminology here is **provisional**.


## Table of Contents <!-- omit in toc -->

- [Rule sets](#rule-sets)
- [Unix model](#unix-model)
- [Windows model](#windows-model)
- [Cross-host analysis](#cross-host-analysis)
- [Separators and normalisation notes](#separators-and-normalisation-notes)
- [What is out of scope](#what-is-out-of-scope)


## Rule sets

A **rule set** (provisional term) selects which OS path semantics apply to analysis:

| Rule set | Typical use |
| --- | --- |
| **unix** | POSIX-style paths: `/` separator, single rooted form |
| **windows** | Drive letters, UNC, `\` and `/` separators, richer relativity lattice |

**libpath** ports may compile for a host OS or analyse paths under an explicit, selectable rule set (including cross-host). Concepts does **not** define a third hybrid syntax.


## Unix model

Under the **unix** rule set:

* The path-name separator is `/` (forward slash).
* A path is **slash-rooted** (and, on Unix, **absolute**) when it begins with `/`.
* There is no separate volume concept; the “root” is the leading slash when present.
* Relative paths do not begin with `/` (except via `~` home expansion when that feature is enabled — see [classification.md](./classification.md)).
* `.` and `..` are directory parts with conventional meaning in higher-level util; the parse model records them lexically.

Examples (provisional illustrations):

| Input | Classification (typical) | Notes |
| --- | --- | --- |
| `abc/def` | Relative | No leading slash |
| `/abc/def` | SlashRooted | Absolute on Unix |
| `/` | SlashRooted | Root only; empty entry name |
| `abc/` | Relative | Trailing separator ⇒ empty entry name |


## Windows model

Under the **windows** rule set:

* Both `\` and `/` may act as path-name separators (libpath treats them equivalently for parsing).
* **Absolute** and **rooted** are **distinct** (see [classification.md](./classification.md)):
  * **Drive-letter-rooted** (`C:\`, `C:/`) and **UNC-rooted** (`\\server\share\`) are absolute;
  * **Slash-rooted** (`\foo`, `/foo`) is rooted but **not** absolute;
  * **Drive-letter-relative** (`C:foo`) has a volume (`C:`) but no established root.
* **Volume** (provisional) is the drive letter or UNC server/share prefix; it may exist without a complete **root**.
* **Root** (provisional) is the complete root span when established (e.g. `C:\`, `\\server\share\`).
* UNC paths have complete and incomplete forms (`\\server` vs `\\server\share\`).
* Device paths, long-path prefixes (`\\?\`), and similar extended forms are recognised in mature ports; Phase 1 documents the core lattice — extended forms are scheduled in port TODOs and future fixture expansion.

Examples (provisional illustrations):

| Input | Classification (typical) | Absolute? | Rooted? |
| --- | --- | --- | --- |
| `abc` | Relative | No | No |
| `\abc` | SlashRooted | No | Yes |
| `C:abc` | DriveLetterRelative | No | No |
| `C:\abc` | DriveLetterRooted | Yes | Yes |
| `\\srv\share\file` | UncRooted | Yes | Yes |


## Cross-host analysis

A Windows path string may be parsed under **windows** rules on a Unix host (and vice versa). This supports tools that must reason about foreign path forms without implying that the path is valid on the current host filesystem.

Reference behaviour for absolute vs rooted on Windows is documented in **libpath** (C/C++): `libpath_ParseResult_IsPathAbsolute` vs `libpath_ParseResult_IsPathRooted`.


## Separators and normalisation notes

* **Lexical form** is what the input string contains after parse-time normalisation flags; concepts does not equate lexical form with OS **canonical** or **resolved** paths.
* Trailing separators affect **entry name** (always empty when a trailing separator is present) — see [parts.md](./parts.md).
* Repeated slash runs may be invalid or collapsible depending on rule set and parse flags.


## What is out of scope

* URL/URI and other non–file-system schemes;
* Registry paths, `file://` URLs, and ADS/stream notation as first-class concepts;
* Filesystem operations (`stat`, `realpath`, directory iteration).


<!-- ########################### end of file ########################### -->
