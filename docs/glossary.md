# libpath-concepts — glossary <!-- omit in toc -->

Provisional definitions for the **libpath** family path-form model.


## Table of Contents <!-- omit in toc -->

- [Status](#status)
- [Core terms](#core-terms)
- [Part fields](#part-fields)
- [Relativity and classification](#relativity-and-classification)
- [Operations](#operations)
- [Related external names](#related-external-names)


## Status

**All terms in this glossary are provisional** until the operator supplies stored web material on this topic and explicitly seals nomenclature. Do not treat this document as a final standard.


## Core terms

| Term (provisional) | Definition |
| --- | --- |
| **path** | A file-system path string interpreted under a selected **rule set** |
| **path descriptor** | Structured decomposition of a **path** into parts and **classification** |
| **rule set** | Unix or Windows semantics applied to analysis (selectable vs host) |
| **parse flags** | Options moderating parse behaviour (reference directory, home recognition, separator runs, …) |
| **file-system path** | A path referring to file-system naming — not URL/URI or other locators |


## Part fields

| Term (provisional) | Definition |
| --- | --- |
| **input** | Original string supplied to parse |
| **full path** | Fullest establishable path string used for analysis |
| **location** | **Root** (if any) plus **directory** — the directory span before **entry name**; ≈ **recls** `DirectoryPath` |
| **root** | Complete root span when established (e.g. `/`, `C:\`, `\\server\share\`) |
| **volume** | Windows drive or UNC volume prefix; may exist without a complete **root** |
| **directory** | Path after **root**, before **entry name** |
| **directory parts** | **Directory** split on separators; **root** not included in part count |
| **entry name** | Final path component; empty when input has a trailing separator |
| **stem** | **Entry name** without its final extension segment |
| **extension** | Final extension segment of **entry name**, if any |


## Relativity and classification

| Term (provisional) | Definition |
| --- | --- |
| **absolute** | Anchored as a full path from the file-system root for the rule set (on Windows: drive-rooted or UNC-rooted, not slash-rooted alone) |
| **rooted** | Has a root prefix; on Windows includes slash-rooted paths |
| **relative** | No established **root** |
| **volume-relative** | Windows: has **volume** (`C:`) without complete **root** |
| **slash-rooted** | Begins with `/` or `\` |
| **classification** | Enumerated kind of path form (see [classification.md](./classification.md)) |
| **home-rooted** | Leading `~` form when home recognition is enabled |


### Classification kinds (provisional)

| Kind | Short meaning |
| --- | --- |
| **InvalidSlashRuns** | Invalid repeated separator runs |
| **InvalidChars** | Illegal characters for the rule set |
| **Invalid** | Generic invalid |
| **Empty** | Empty input |
| **Relative** | Relative path |
| **SlashRooted** | Leading separator path |
| **DriveLetterRelative** | `C:…` without root separator |
| **DriveLetterRooted** | `C:\…` or `C:/…` |
| **UncIncomplete** | Incomplete UNC prefix |
| **UncRooted** | Complete UNC-rooted path |
| **HomeRooted** | Home/tilde form |


## Operations

| Term (provisional) | Definition |
| --- | --- |
| **equate** | Boolean sameness of two path forms under rule set and options |
| **compare** | Three-way ordering of two path forms under rule set and options |
| **lexical form** | Path value as analysed without filesystem resolution |
| **canonical** (util) | Dot-segment normalisation and related string transforms — util layer, not I/O |


## Related external names

Mapping aids for readers familiar with other ecosystems (not synonyms for sealed terms):

| External name | Concepts anchor (provisional) |
| --- | --- |
| `std::filesystem::path` | Mixes form and host I/O; compare **path descriptor** parts only |
| `path/filepath` (Go) | Host-centric; see Phase 2 survey |
| `pathlib.Path` | Host-centric object; see Phase 2 survey |
| `Pathname` (Ruby) | Rich util; **libpath.Ruby** maps toward concepts |
| `std::path::Path` (Rust) | Prefix/component model; family aligns toward **path descriptor** |
| recls `DirectoryPath` | ≈ **location** |
| basename / dirname | Approximate **entry name** / parent — trailing-separator rules differ |


<!-- ########################### end of file ########################### -->
