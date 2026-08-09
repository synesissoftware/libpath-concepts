# libpath-concepts — path parts <!-- omit in toc -->

The **path descriptor** (provisional) decomposes a path string into analysable parts. All field names below are **provisional** until nomenclature is sealed.


## Table of Contents <!-- omit in toc -->

- [Overview](#overview)
- [Fields](#fields)
- [Invariants](#invariants)
- [Volume vs root (Windows)](#volume-vs-root-windows)
- [Entry name, stem, and extension](#entry-name-stem-and-extension)
- [Mapping to port APIs](#mapping-to-port-apis)
- [Mapping to recls](#mapping-to-recls)


## Overview

Parsing produces a structured **path descriptor** from an input string under a selected **rule set** and optional parse flags (reference directory, home/tilde recognition, separator-run policy, …).

The fullest establishable form is used for analysis:

1. If the path is **absolute**, it is interpreted without a reference directory;
2. If **relative** and a reference directory is supplied, the path is interpreted relative to that directory;
3. If **relative** without a reference directory, the path is interpreted as given.

This matches the contract documented in **libpath.Go** `PathDescriptor` and **libpath** `libpath_PathDescriptor_t`.


## Fields

| Provisional field | Meaning |
| --- | --- |
| **input** | Original string passed to parse |
| **full path** | Fullest establishable path string used for analysis |
| **location** | Directory span before the entry name — root (if any) plus directory |
| **root** | Complete root span when established (e.g. `/`, `C:\`, `\\server\share\`) |
| **volume** | Windows drive or UNC volume prefix; may exist without a complete root |
| **directory** | Path after root, before entry name (excludes root and entry) |
| **directory parts** | Directory split on separators (root not included in the count) |
| **entry name** | Final component after the last separator; empty if trailing separator |
| **stem** | Entry name without final extension segment |
| **extension** | Final extension segment of entry name (if any) |
| **classification** | Enumerated path form kind — see [classification.md](./classification.md) |


## Invariants

These relations hold for successful parses (provisional, family target):

* `location` = `root` + `directory` (concatenation of established spans);
* `full path` = `location` + `entry name` (when entry name is non-empty) or equals `location` when entry name is empty;
* **Trailing separator** ⇒ **entry name** is empty (stricter than many “basename” APIs);
* **root** is populated only when a **complete** root is established; contrast **volume** on Windows;
* **directory parts** count excludes the root; dot-directory parts (`.`, `..`) may be counted separately in port metadata.


## Volume vs root (Windows)

On Windows:

* **volume** — e.g. `C:` or `\\server\share` — may be present for drive-letter-relative paths;
* **root** — e.g. `C:\` or `\\server\share\` — requires the complete rooted prefix.

**libpath** (C) exposes `volumePart` separately from `rootPart` when built for Windows. **libpath.Go** currently folds volume into root in some cases — a known family gap to close against this model.


## Entry name, stem, and extension

* **entry name** corresponds to “basename” with the trailing-separator rule above;
* **stem** / **extension** split the entry name on the last `.` (family convention; edge cases for leading dots are port-defined);
* A path that ends with a separator has no entry name, hence no stem or extension.


## Mapping to port APIs

| Provisional term | **libpath** (C) | **libpath.Go** | **libpath.Ruby** |
| --- | --- | --- | --- |
| path descriptor | `libpath_PathDescriptor_t` | `PathDescriptor` | `ParsedPath` |
| input | `input` | `Input` | (input string) |
| full path | `fullPath` | `FullPath` | — |
| location | `locationPart` | `Location` | `directory_path` (related) |
| root | `rootPart` | `Root` | — |
| volume | `volumePart` (Windows) | (gap) | volume helpers |
| directory | `directoryPart` | `Directory` | — |
| directory parts | slices API | `DirectoryParts` | — |
| entry name | `entryNamePart` | `EntryName` | `file_full_name` |
| stem | `entryStemPart` | `Stem` | — |
| extension | `entryExtensionPart` | `Extension` | — |


## Mapping to recls

**recls** **DirectoryPath** ≈ concepts **location** — the path of the directory containing an entry, including established root, excluding the final entry name component.

When **recls** and **libpath** are used together, ports should document any deliberate naming aliases so concepts glossary terms remain the semantic anchor.


<!-- ########################### end of file ########################### -->
