# libpath-concepts - Contributing <!-- omit in toc -->


## Table of Contents <!-- omit in toc -->

- [Scope](#scope)
- [Provisional nomenclature](#provisional-nomenclature)
- [Documentation style](#documentation-style)
- [Fixtures (Phase 3)](#fixtures-phase-3)
- [Pull requests](#pull-requests)


## Scope

Contributions should stay within **file-system path form** semantics:

* parsing, classification, and decomposition under selectable Unix or Windows rule sets;
* compare and equate definitions;
* comparative notes and shared test fixtures.

Do **not** add URL/URI locators, I/O APIs, or language-runtime implementation code to this repository.


## Provisional nomenclature

Prescribed terms in **docs/glossary.md** and related documents are **provisional** until the operator ingests stored web material and explicitly seals naming. When proposing new terms:

* label them **provisional** / **proposed**;
* prefer alignment with existing **libpath** family vocabulary;
* do not rename sealed concepts without operator agreement.


## Documentation style

Follow Synesis **freelibs** markdown conventions: `*` body lists, double blank lines before `##`/`###`, `<!-- omit in toc -->` titles on top-level project files, and the standard end-of-file marker.


## Fixtures (Phase 3)

When fixture files arrive:

* prefer JSON or YAML with lexicographic keys;
* document schema in **fixtures/schema.md** before bulk cases;
* keep fixtures as data, not embedded port test harnesses.


## Pull requests

Open pull requests at https://github.com/synesissoftware/libpath-concepts. For large semantic changes, discuss in an issue first so nomenclature stays coherent across ports.


<!-- ########################### end of file ########################### -->
