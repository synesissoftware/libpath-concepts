# libpath-concepts — compare and equate <!-- omit in toc -->

**Compare** (ordering) and **equate** (boolean sameness) for file-system path *forms*. Definitions are **provisional**.


## Table of Contents <!-- omit in toc -->

- [Why two operations](#why-two-operations)
- [Equate](#equate)
- [Compare](#compare)
- [Flags and working directory](#flags-and-working-directory)
- [Case folding](#case-folding)
- [Lexical vs canonical](#lexical-vs-canonical)
- [Port status](#port-status)
- [Examples (illustrative)](#examples-illustrative)


## Why two operations

Path strings can be:

* **equal** as values (same form under chosen rules) without being byte-identical; or
* **ordered** for use in sorted containers, without implying filesystem sameness.

Stdlibs often provide one string comparison or normalisation helper that conflates these. **libpath** (C/C++) splits them explicitly; the family target is both operations in every port (or documented deferral).


## Equate

**Equate** (provisional) answers: *under the selected rule set and options, do two path forms denote the same path value?*

* Result type: boolean (truthy/falsey in C);
* May consider separator equivalence (`/` vs `\` on Windows), case-folding options, and relative-path resolution against a **working directory context**;
* Does **not** consult the filesystem (no `realpath`, no existence checks);
* Implemented in **libpath** as `libpath_Equate_*` APIs.

Equate is the right choice for “are these two path strings the same path?” in tests and caches.


## Compare

**Compare** (provisional) answers: *under the selected rule set and options, what is the total order of two path forms?*

* Result type: three-way ordering (`<`, `=`, `>`);
* Establishes a deterministic ordering for collections and indexes;
* May use the same normalisation dimensions as equate (separators, case, relative resolution) but must satisfy strict weak ordering when documented;
* Implemented in **libpath** as `libpath_Compare_*` APIs.

Compare is **not** a substitute for equate when only sameness is needed.


## Flags and working directory

Both operations accept:

* **rule set** semantics (Unix vs Windows) via build configuration or explicit API;
* **option flags** (separator normalisation, case folding, …) — port-specific enumerations today;
* optional **working directory context** for resolving relative paths consistently (see **libpath** `libpath_WorkingDirectoryContext_t`).

Concepts will document flag matrices further when Phase 2 surveys stdlib behaviour and Phase 3 fixtures encode expectations.


## Case folding

On Windows, path comparison often ignores case for ASCII drive letters and path components; Unix rule sets are typically case-sensitive. **Equate** and **compare** must document which case-folding policy applies per rule set and flag set.

This is **lexical** case folding, not locale-aware filesystem collation.


## Lexical vs canonical

**libpath** compare/equate operate on **path form**, not OS-canonical or resolved entity paths:

* `foo/../bar` and `bar` may or may not equate depending on flags and util policy;
* dot-segment collapse is a separate util concern from raw parse classification;
* concepts may *mention* that stdlibs blur “clean path” with “same file” — that is why I/O is out of scope.


## Port status

| Port | Equate | Compare | Notes |
| --- | --- | --- | --- |
| **libpath** (C/C++) | ✅ | ✅ | Reference implementation |
| **libpath.Go** | ❌ stub | ❌ stub | Directories reserved |
| **libpath.Ruby** | ❌ | ⚠️ `compare_path` | String-oriented helper |
| **libpath.Rust** | ❌ | ❌ | Planned |
| **libpath.Python** | — | — | Greenfield |

Family Track S5 targets implement or explicitly defer with rationale.


## Examples (illustrative)

Under **windows** rules with default lexical policies (provisional illustrations only):

| Left | Right | Equate? | Compare |
| --- | --- | --- | --- |
| `C:\a\b` | `C:/a/b` | Often yes (separator normalisation) | Equal |
| `\a\b` | `C:\a\b` | No (different relativity) | Implementation-defined order |
| `a` | `a` | Yes | Equal |
| `a` | `b` | No | Left `<` right (typical) |

Exact outcomes are fixture-defined in Phase 3; ports must not guess beyond documented flags.


<!-- ########################### end of file ########################### -->
