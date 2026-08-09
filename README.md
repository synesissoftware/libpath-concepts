# libpath-concepts <!-- omit in toc -->

Shared vocabulary, semantics, and test material for the **libpath** family — file-system path *forms* under selectable Unix or Windows rule sets.


[![License](https://img.shields.io/badge/License-BSD_3--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)
[![GitHub release](https://img.shields.io/github/v/release/synesissoftware/libpath-concepts.svg)](https://github.com/synesissoftware/libpath-concepts/releases/latest)
[![Last Commit](https://img.shields.io/github/last-commit/synesissoftware/libpath-concepts)](https://github.com/synesissoftware/libpath-concepts/commits/master)


## Table of Contents <!-- omit in toc -->

- [Introduction](#introduction)
- [Four pillars](#four-pillars)
- [Documentation](#documentation)
- [Provisional nomenclature](#provisional-nomenclature)
- [Project Information](#project-information)
  - [Where to get help](#where-to-get-help)
  - [Contribution guidelines](#contribution-guidelines)
  - [Related projects](#related-projects)
  - [Programme strategy](#programme-strategy)
  - [License](#license)


## Introduction

**libpath-concepts** is a public knowledge base for the **libpath** multi-language programme. It is **not** a runtime library: language APIs and implementations live in the [**libpath** ports](#related-projects).

The family treats file-system paths as cross-platform *values* with an explicit part model, relativity lattice, and selectable OS rule sets (Unix or Windows). Filesystem enumeration and I/O are and will always remain out of scope — that work belongs to **recls** and the operating system.

Scope is **file-system paths only**. URL/URI, registry keys, and other non–file-system locators are excluded.


## Four pillars

| Pillar | Role | Status |
| --- | --- | --- |
| **Describe** | OS path models (Unix vs Windows), relativity, and decomposition | ✅ Phase 1 — [docs/](./docs/) |
| **Prescribe** | Rich, family-aligned nomenclature (clearly labelled **provisional**) | ✅ Phase 1 — [docs/glossary.md](./docs/glossary.md) |
| **Compare** | Cross-language survey of popular path libraries | ⏳ Phase 2 — [docs/stdlib-survey.md](./docs/stdlib-survey.md) (planned) |
| **Supply** | Language-independent unit-test fixtures for ports | ⏳ Phase 3 — [fixtures/](./fixtures/) (planned) |


## Documentation

| Document | Contents |
| --- | --- |
| [docs/rationale.md](./docs/rationale.md) | Scope, audience, and relationship to ports |
| [docs/os-models.md](./docs/os-models.md) | Unix and Windows path *form* models |
| [docs/parts.md](./docs/parts.md) | Path descriptor part model |
| [docs/classification.md](./docs/classification.md) | Relativity lattice and classification taxonomy |
| [docs/compare-and-equate.md](./docs/compare-and-equate.md) | Ordering vs boolean sameness |
| [docs/glossary.md](./docs/glossary.md) | Provisional term definitions |


## Provisional nomenclature

All prescribed terms in this repository are **provisional** until the operator supplies stored web material on this topic and explicitly seals decisions. See [docs/glossary.md](./docs/glossary.md) and the standing reminder in the [programme strategy](#programme-strategy) documents.


## Project Information


### Where to get help

[GitHub Page](https://github.com/synesissoftware/libpath-concepts "GitHub Page")


### Contribution guidelines

See [CONTRIBUTING.md](./CONTRIBUTING.md). Defect reports, clarifications, and pull requests are welcome on https://github.com/synesissoftware/libpath-concepts.


### Related projects

Runtime **libpath** ports (implementations):

* [**libpath**](https://github.com/synesissoftware/libpath/) — C/C++;
* [**libpath.Go**](https://github.com/synesissoftware/libpath.Go/) — Go;
* [**libpath.Python**](https://github.com/synesissoftware/libpath.Python/) — Python;
* [**libpath.Ruby**](https://github.com/synesissoftware/libpath.Ruby/) — Ruby;
* [**libpath.Rust**](https://github.com/synesissoftware/libpath.Rust/) — Rust.

**libpath** is used in **[recls](https://github.com/synesissoftware/recls)** and related libraries.


### Programme strategy

Internal programme documents (specifications, gap analysis, delivery plan) live in the Synesis **freelibs** workspace at [`.management/strategy/libpath/`](../../.management/strategy/libpath/):

* [concepts-development-spec.md](../../.management/strategy/libpath/concepts-development-spec.md) — build specification for this repository;
* [family-gap-analysis-and-plan.md](../../.management/strategy/libpath/family-gap-analysis-and-plan.md) — port gap analysis and staged plan.


### License

**libpath-concepts** is released under the 3-clause BSD license. See [LICENSE](./LICENSE) for details.


<!-- ########################### end of file ########################### -->
