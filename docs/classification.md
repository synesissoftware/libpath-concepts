# libpath-concepts — classification <!-- omit in toc -->

Relativity lattice and classification taxonomy for file-system path forms. All kind names are **provisional**.


## Table of Contents <!-- omit in toc -->

- [Relativity lattice](#relativity-lattice)
- [Absolute vs rooted vs volume-relative (Windows)](#absolute-vs-rooted-vs-volume-relative-windows)
- [Classification kinds](#classification-kinds)
- [Invalid and empty forms](#invalid-and-empty-forms)
- [Home and tilde](#home-and-tilde)
- [Port mapping notes](#port-mapping-notes)


## Relativity lattice

Path **relativity** (provisional) describes how a path string anchors to the file-system naming hierarchy **without** filesystem I/O.

```text
                    ┌─────────────────────────────────────┐
                    │            Invalid forms             │
                    │  (slash runs, bad chars, generic)    │
                    └─────────────────────────────────────┘
                                      │
                    ┌─────────────────┴─────────────────┐
                    │              Empty                 │
                    └─────────────────┬─────────────────┘
                                      │
          ┌───────────────────────────┼───────────────────────────┐
          │                           │                           │
    ┌─────▼─────┐              ┌──────▼──────┐            ┌───────▼────────┐
    │ Relative  │              │ HomeRooted  │            │ SlashRooted    │
    │           │              │  (~…)       │            │ (/ or \ lead)  │
    └─────┬─────┘              └─────────────┘            └───────┬────────┘
          │                                                       │
          │              Windows-only branches                    │
          │     ┌────────────────────┬────────────────────┐       │
          │     │                    │                    │       │
          │  ┌──▼──────────────┐  ┌──▼────────────┐  ┌───▼──────────────┐
          │  │ DriveLetter     │  │ DriveLetter   │  │ UncIncomplete    │
          │  │ Relative (C:…)  │  │ Rooted (C:\…) │  │ (\\server …)     │
          │  └─────────────────┘  └───────────────┘  └───┬──────────────┘
          │                                               │
          │                                        ┌──────▼──────┐
          │                                        │ UncRooted   │
          │                                        │ (\\s\sh\…)  │
          │                                        └─────────────┘
          └────────────────────────────────────────────────────────
```

On **Unix**, slash-rooted paths are also **absolute**. On **Windows**, slash-rooted paths are **not** absolute (but are **rooted**).


## Absolute vs rooted vs volume-relative (Windows)

| Predicate (provisional) | Unix | Windows |
| --- | --- | --- |
| **absolute** | Begins with `/` | Drive-rooted (`C:\…`) or UNC-rooted (`\\server\share\…`) |
| **rooted** | Same as absolute | Absolute forms **or** leading `\` / `/` (slash-rooted) |
| **volume-relative** | N/A | Has drive letter (`C:…`) without complete root |

**libpath** (C) exposes `libpath_ParseResult_IsPathAbsolute` and `libpath_ParseResult_IsPathRooted` with the Windows distinction above documented in **parse/api.h**.

**Critical family rule:** on Windows, `\foo` is **rooted** but **not** **absolute**. Stdlibs and ports that treat “non-empty root” as absolute are misaligned with the C reference.


## Classification kinds

Shared target enumeration (names provisional; align with **libpath.Go** `Classification`):

| Kind | Rule set | Summary |
| --- | --- | --- |
| **InvalidSlashRuns** | both | Disallowed or unnormalised repeated separator runs |
| **InvalidChars** | both | Characters illegal in paths for the rule set |
| **Invalid** | both | Generic invalid |
| **Empty** | both | Empty input |
| **Relative** | both | No root anchor |
| **SlashRooted** | both | Leading `/` or `\` |
| **DriveLetterRelative** | windows | `C:…` without `\`/`/` after drive |
| **DriveLetterRooted** | windows | `C:\…` or `C:/…` |
| **UncIncomplete** | windows | UNC prefix without complete `\\server\share\` root |
| **UncRooted** | windows | Complete UNC root |
| **HomeRooted** | both | Leading `~` when home recognition enabled |


## Invalid and empty forms

* **Empty** — zero-length input (distinct from `"."` or root-only paths);
* **InvalidChars** — e.g. `|`, `<`, `>`, `?`, `*` on Windows; NUL and other platform rules;
* **InvalidSlashRuns** — policy-dependent; may be rejected or normalised per parse flags;
* Invalid paths may still populate **input** and error offsets in port APIs.


## Home and tilde

**HomeRooted** is a distinct concern from absolute/relative:

* Triggered when parse flags recognise `~` / `~/…` / `~user/…` (port-defined);
* Not equivalent to slash-rooted or drive-rooted forms;
* Expansion to a concrete directory is **out of scope** for concepts I/O — only the lexical classification is in scope.


## Port mapping notes

| Concepts kind (provisional) | **libpath.Go** | **libpath** (C) | **libpath.Ruby** symbols |
| --- | --- | --- | --- |
| Relative | `Relative` | (predicates) | `:relative` |
| SlashRooted | `SlashRooted` | rooted, not abs (Win) | `:rooted` (Win) |
| DriveLetterRelative | `DriveLetterRelative` | — | `:drived` |
| DriveLetterRooted | `DriveLetterRooted` | absolute (Win) | `:absolute` (Win) |
| UncRooted | `UncRooted` | absolute (Win) | `:absolute` (Win) |
| HomeRooted | `HomeRooted` | — | `:homed` |
| Empty | `Empty` | — | — |

**libpath** (C) today exposes **absolute** / **rooted** predicates rather than the full enum; family work will expose or document the mapping (Track S4 in the family plan).

**libpath.Rust** aligns with the Go-style enum; naming may converge on **path descriptor** (provisional).


<!-- ########################### end of file ########################### -->
