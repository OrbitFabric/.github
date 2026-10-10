<p align="center">
  <img src="https://raw.githubusercontent.com/OrbitFabric/orbitfabric/main/assets/brand/orbitfabric-logo-horizontal-light.png" alt="OrbitFabric" width="720">
</p>

<p align="center">
  <strong>One mission contract. Different systems remain themselves.</strong>
</p>

# OrbitFabric

**OrbitFabric is an open-source, model-first Mission Data Fabric for small spacecraft.**

A spacecraft mission repeats the same concepts across flight software, ground operations, interface definitions, verification, simulation, documentation and external engineering tools. Those independently maintained representations can drift, even when everyone believes they are describing the same mission.

OrbitFabric makes mission meaning explicit, machine-readable and traceable, while preserving the authority of every system over its own implementation and runtime behavior.

> Define mission meaning once. Keep its identity and provenance visible as different engineering systems realize, inspect and verify it.

## Four roles in one engineering ecosystem

| Project | Primary responsibility | What it does **not** own |
| --- | --- | --- |
| **[1. OrbitFabric Core](https://github.com/OrbitFabric/orbitfabric)** | Semantic authority: validate, exercise and export the Mission Data Contract | Native flight/ground implementations |
| **[2. OrbitFabric Studio](https://github.com/OrbitFabric/orbitfabric-studio)** | Human engineering workbench: explore and understand Core-owned mission facts | A second interpretation of mission semantics |
| **[3. Integration Adapters](#3-integration-adapters)** | Explicit projection and traceability toward independently owned target systems | Target-native architecture or runtime truth |
| **[4. Reference Mission](https://github.com/OrbitFabric/orbitfabric-reference-mission)** | Shared engineering proving ground: one coherent mission, reproducible scenarios and Engineering Stories | A real spacecraft, flight stack or mission simulator |

This order describes the ecosystem's **architectural roles and presentation priority**, not a sequential execution pipeline. Studio and adapters serve different purposes and consume explicit Core-owned surfaces. The Reference Mission exercises those boundaries; it does not sit downstream of Studio or of every adapter.

```text
Mission Model (the Mission Data Contract)
        +
separately authored Operational Scenarios
                     |
                     v
              ORBITFABRIC CORE
               semantic authority
                  /         \
                 /           \
      Core-owned facts      Integration Input Set
      + Interface Manifest  + explicit Projection Profile
               |                       |
               v                       v
        ORBITFABRIC STUDIO       INTEGRATION ADAPTERS
        human understanding     target-specific projection
                                       |
                                       v
                             INDEPENDENT NATIVE SYSTEMS
                             flight / ground / tooling
                             native runtime + evidence

       REFERENCE MISSION + ENGINEERING STORIES
       provide shared context and reproducible,
       evidence-bounded proofs across these boundaries
```

---

## The Mission Data Contract

The Mission Data Contract is expressed through a structured **Mission Model**, covering mission-level concerns such as spacecraft identity, subsystem topology, operational modes, commands, telemetry, events, faults, payload lifecycle, data products, storage, downlink assumptions, commandability and autonomy.

The **Mission Model is the semantic source of truth**. Operational Scenarios are authored separately and exercise that model; they do not become a second semantic authority.

Core validates and interprets the model, exports documented machine-readable surfaces and generates contract-facing artifacts. Scenario evidence additionally consumes the separately authored Scenarios. Target-specific artifacts also require an explicit Projection Profile and an external integration implementation.

The fundamental separation is:

```text
mission meaning != native system implementation
declared intent != projected behavior
projected behavior != executed behavior
executed behavior != evidence-supported observation
```

---

## 1. OrbitFabric Core

[**OrbitFabric Core**](https://github.com/OrbitFabric/orbitfabric) is the foundation of the ecosystem and the **sole OrbitFabric authority for Mission Data Contract interpretation**.

Core loads and validates the Mission Model, performs semantic linting, executes deterministic host-side Scenario evidence and produces generated documentation and contract-facing outputs. It also exposes structured facts for consumers that must not reconstruct mission meaning themselves.

Important Core-owned surfaces and boundaries include:

- **Mission Snapshot, Entity Index and Relationship Manifest** for structured mission understanding;
- **Scenario Declaration and scenario evidence** for exact source identities and bounded verification;
- **Core Interface Manifest** for capability and contract compatibility negotiation;
- **Core Integration Input Set** for coherent downstream integration inputs;
- **generic integration contracts** describing Projection Profiles, external adapter execution and Integration Results;
- **provider-neutral adapter lifecycle contracts and management**, separate from provider-specific release acquisition.

Core owns the semantics and the contracts. It does **not** implement the native architecture of F Prime, COSMOS, cFS or any other external system. Nor is it itself a flight software framework, ground segment, mission control system, operator console or spacecraft dynamics simulator.

This is a deliberate constraint: if another component must invent mission meaning to do its job, the boundary should be investigated rather than silently bypassed.

[Explore Core](https://github.com/OrbitFabric/orbitfabric) · [Core documentation](https://orbitfabric.github.io/orbitfabric/)

---

## 2. OrbitFabric Studio

[**OrbitFabric Studio**](https://github.com/OrbitFabric/orbitfabric-studio) is the local-first, human-facing engineering workbench.

> OrbitFabric Core exposes the Mission Data Contract. OrbitFabric Studio exposes the mission to the human.

Studio makes explicit mission facts easier to **find, navigate, relate and understand**. Its public preview includes Mission Atlas, entity exploration and Entity X-Ray, relationship context and maps, validation findings and operational-logic views.

Studio consumes structured Core-owned information. Its compatibility boundary begins with the **Core Interface Manifest** and the capabilities each view requires. The mission's semantic identities, relationships and operational constraints are not reconstructed from filenames, YAML conventions or UI heuristics.

The design rule is concise:

```text
Core owns the fact.
Studio makes the fact understandable.
```

Studio is not intended to become another Mission Model interpreter, a generic JSON dashboard, a mission-control console or a source of inferred runtime truth. Its interface and presentation can evolve without displacing the semantic authority underneath.

Studio is currently published as a **development preview**; newer work in its repository does not automatically imply released product capabilities.

[Explore Studio](https://github.com/OrbitFabric/orbitfabric-studio) · [Studio release and development status](https://github.com/OrbitFabric/orbitfabric-studio#public-preview-scope)

---

## 3. Integration Adapters

**Integration Adapters** carry explicit OrbitFabric mission intent toward target-native engineering representations. They do not translate by privately reinterpreting the original Mission Model.

The generic integration boundary is:

```text
OrbitFabric Core
    -> coherent Core Integration Input Set
    + explicit Projection Profile
    -> external Integration Package / Adapter
    -> Integration Result + target-native artifacts
    -> independently owned target tools / systems
```

The Projection Profile expresses target-specific projection intent. An adapter owns its projection, mappings, diagnostics, coverage and provenance. **The downstream system remains authoritative** for native implementation, execution and observed runtime behavior.

### Current public adapter portfolio

| Adapter | Target boundary |
| --- | --- |
| **[F Prime](https://github.com/OrbitFabric/orbitfabric-fprime-adapter)** | Projects explicit mission contracts into F Prime / FPP engineering surfaces while preserving native component architecture, deployment, dictionary and runtime ownership |
| **[OpenC3 COSMOS](https://github.com/OrbitFabric/orbitfabric-openc3-cosmos-adapter)** | Projects selected command, ground-operation and verification intent into COSMOS-native surfaces without taking ownership of COSMOS execution |
| **[EDS / cFS](https://github.com/OrbitFabric/orbitfabric-eds-cfs-adapter)** | Carries contracts through Electronic Data Sheets toward NASA EdsLib / cFS integration, retaining native cFS/EdsLib authority |
| **[OpenOBSW / OpenSVF](https://github.com/OrbitFabric/orbitfabric-openobsw-opensvf-adapter)** | Connects mission contracts with OpenOBSW integration and OpenSVF validation workflows while keeping those systems independent |

These are the four OrbitFabric-maintained public adapters, **not the architectural limit**. Compatible community, private and experimental integrations can exercise the same documented contract boundaries.

**Integration Coverage** describes exactly what an adapter projects and what it does not. It distinguishes source semantic surface, target applicability and adapter-declared scope instead of turning a partial mapping into a blanket “supported” claim. Native results and retained evidence bound the integration claim.

### Adapter development and distribution support

These are supporting components, **not additional mission-semantic authorities**:

- [**Adapter Developer Template**](https://github.com/OrbitFabric/orbitfabric-adapter-template): executable starting point for compatible adapters, tests and packaging.
- [**Adapter Catalog**](https://github.com/OrbitFabric/orbitfabric-adapter-catalog): version-controlled data describing exact releases and acquisition source bindings; not a package registry.
- [**GitHub Release Source**](https://github.com/OrbitFabric/orbitfabric-github-release-source): the current provider-specific source for retrieving and verifying exact GitHub-published adapter releases.
- **Core Adapter Manager**: Core-owned, provider-neutral installed-state and lifecycle boundary, including installation, verification, execution and removal.

A managed path connects **project desired state**, exact Catalog selection, a verified Release Source handoff and the Core Adapter Manager. That lifecycle is distinct from the semantic projection contract. It does not require every compatible adapter to be public, catalogued or hosted on GitHub.

The upstream projects are independently maintained. OrbitFabric integration does not imply their endorsement.

---

## 4. OrbitFabric Reference Mission

[**OrbitFabric Reference Mission**](https://github.com/OrbitFabric/orbitfabric-reference-mission) is the shared engineering proving ground for Core, Studio and the integration ecosystem.

It provides a **realistic but synthetic** small-spacecraft Mission Model, separately authored operational Scenarios, a progressive tutorial, generated evidence and the common mission context used to exercise OrbitFabric's architectural claims.

It serves two complementary purposes:

**Reference Mission:** one coherent canonical mission context in which Core semantics, scenarios, generated surfaces and Studio exploration can be inspected without constructing a new example for every feature.

**Engineering Stories:** reproducible, question-driven investigations that select a bounded mission slice and test it against actual tools, independently owned engineering systems or runtime boundaries.

The Reference Mission is **not** a real spacecraft configuration, flight software, ground segment or spacecraft simulator.

[Explore Reference Mission](https://orbitfabric.github.io/orbitfabric-reference-mission/) · [Repository](https://github.com/OrbitFabric/orbitfabric-reference-mission)

### Published Engineering Stories

Every Engineering Story has two required layers: a **Front Story** for the engineering question, investigation and conclusions, and a **Reference Project** for inspectable and reproducible artifacts. A separate **Technical Deep Dive is optional**, when the mechanics and evidence merit their own document.

**[R1: One Mission Contract Across Flight and Ground](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/)**

R1 tests whether one mission-level semantic root can survive independently owned flight and ground engineering paths. A concrete stop-acquisition slice passes through **F Prime** and **OpenC3 COSMOS** and converges in an accepted live command/telemetry verification loop. R1 also provides a [Technical Deep Dive](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/technical-deep-dive/).

**[R2: One Intent, Two Executions](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r2-pwnsat/)**

R2 asks whether mission intent can remain accountable through an external runtime that OrbitFabric does not control. Using the existing `obc.request_health_check` operation and **PWNSAT/FlatSat**, it separates authorized invocation, frozen command representation, controlled presentation, runtime occurrences and native observation.

**R2 is published as a bounded offline Engineering Story:** projection lineage and the software-side evidence/falsification harness are reproducible; **physical FlatSat execution has not yet been run**, and no physical execution-multiplicity result is claimed. R2 has a Reference Project but does not currently require a separate Technical Deep Dive.

[Browse all Engineering Stories](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/)

---

## Engineering discipline and evidence

OrbitFabric's architectural boundaries are made explicit and testable. Important practices include:

- clear ownership of meaning, projection, native behavior and observation;
- explicit contracts, compatibility rules and stable-versus-candidate classifications;
- exact dependency baselines and provenance;
- deterministic evidence and retained artifacts;
- Integration Coverage with explicit limitations and non-claims;
- bounded acceptance and fail-closed handling when available evidence is insufficient.

An Integration Result does not by itself prove a target behaved as intended. A native telemetry observation does not automatically prove an authorized invocation executed exactly once. **Claims stop where the evidence stops.**

Architecture decisions, conformance tests and reproducible Engineering Stories are kept visible so conclusions can be challenged rather than taken on trust.

---

## Open source and participation

OrbitFabric is designed for spacecraft software and systems architects, flight and ground engineers, verification teams, payload integrators, research laboratories, small-spacecraft projects and developers of engineering tools.

Its public reference material follows a clean-room approach: synthetic or publicly usable inputs rather than private mission configurations, employer-owned code or proprietary interfaces.

Contributions and external integrations are welcome. An integration can preserve both a shared mission meaning and the independence of its target ecosystem.

## Where to start

| Goal | Entry point |
| --- | --- |
| **Understand the semantic foundation** | [OrbitFabric Core](https://github.com/OrbitFabric/orbitfabric) · [documentation](https://orbitfabric.github.io/orbitfabric/) |
| **Explore a mission visually** | [OrbitFabric Studio](https://github.com/OrbitFabric/orbitfabric-studio) |
| **Integrate a native toolchain** | [F Prime](https://github.com/OrbitFabric/orbitfabric-fprime-adapter) · [OpenC3 COSMOS](https://github.com/OrbitFabric/orbitfabric-openc3-cosmos-adapter) · [EDS/cFS](https://github.com/OrbitFabric/orbitfabric-eds-cfs-adapter) · [OpenOBSW/OpenSVF](https://github.com/OrbitFabric/orbitfabric-openobsw-opensvf-adapter) |
| **Explore the shared example mission** | [Reference Mission](https://orbitfabric.github.io/orbitfabric-reference-mission/) |
| **Follow a verified flight/ground proof** | [R1 Engineering Story](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/) |
| **Explore external runtime accountability** | [R2 Engineering Story](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r2-pwnsat/) |
| **Build or distribute an adapter** | [Adapter Template](https://github.com/OrbitFabric/orbitfabric-adapter-template) · [Catalog](https://github.com/OrbitFabric/orbitfabric-adapter-catalog) · [GitHub Release Source](https://github.com/OrbitFabric/orbitfabric-github-release-source) |

---

**OrbitFabric makes mission meaning explicit so the systems around it can evolve without silently redefining what the mission means.**
