# libpath-concepts — rationale <!-- omit in toc -->

Why **libpath-concepts** exists and how it relates to the **libpath** ports.


## Table of Contents <!-- omit in toc -->

- [Problem](#problem)
- [What concepts is](#what-concepts-is)
- [What concepts is not](#what-concepts-is-not)
- [Audience](#audience)
- [Relationship to recls](#relationship-to-recls)
- [Standing reminder](#standing-reminder)


## Problem

Standard library path APIs are usually **host-centric convenience** layers. They mix lexical path *form* with filesystem *entity* resolution (`realpath`, existence checks, working-directory mutation). That is appropriate for application scripting but weak as a cross-platform contract for libraries such as **recls** that must reason about path values under explicit OS rules.

The **libpath** family provides an I/O-free, OS-faithful model of file-system path *forms* — parsing, classifying, comparing, and equating them under selectable Unix or Windows semantics. **libpath-concepts** captures the shared semantics those ports must realise.


## What concepts is

A public knowledge base that:

* **describes** how Unix and Windows path forms differ;
* **prescribes** a rich nomenclature for classification and decomposition (currently **provisional**);
* **compares** popular language path libraries (Phase 2);
* **supplies** language-independent test fixtures (Phase 3).

Concepts is documentation and data. Implementations live in sibling port repositories.


## What concepts is not

* **Not** a parse library or language binding;
* **Not** a replacement for OS documentation;
* **Not** a hybrid “platform-independent” path dialect — selectable Unix or Windows **rule sets** only;
* **Not** a place for filesystem I/O, existence, or canonicalisation that requires touching the OS (though docs may *mention* why stdlibs blur form vs entity).


## Audience

| Audience | Use |
| --- | --- |
| General programmers / library authors | Description, glossary, comparative KB |
| **libpath.*** maintainers | Prescribed nomenclature + shared fixtures |
| Implementing agents | Spec in **.management/strategy/libpath/**, then product docs here |


## Relationship to recls

**recls** enumerates and classifies file-system entries. Its path types (notably **DirectoryPath**) align with the concepts **location** part — the directory span before the final entry name, including root when established. Concepts documents that mapping explicitly in [parts.md](./parts.md).


## Standing reminder

Before sealing final nomenclature, the operator must supply stored web material on this topic. Until then, all prescribed names remain **provisional**.


<!-- ########################### end of file ########################### -->
