<p align="center">
  <img src="https://raw.githubusercontent.com/OrbitFabric/orbitfabric/main/assets/brand/orbitfabric-logo-horizontal-light.png" alt="OrbitFabric" width="720">
</p>

<p align="center">
  <strong>One mission contract. Different systems remain themselves.</strong>
</p>

# OrbitFabric

OrbitFabric is an open-source engineering ecosystem built around a model-first **Mission Data Fabric for small spacecraft**.

Its purpose is to make mission semantics explicit, machine-readable, reusable and traceable across software, tooling, documentation, verification and external integrations without forcing those systems to share one implementation model.

The idea is simple:

> Define mission meaning once, then let each engineering system remain authoritative for the part it actually owns.

A spacecraft mission often describes the same concepts independently in many places:

```text
flight software
ground software
interface definitions
test fixtures
simulation and verification inputs
documentation
operational procedures
payload integration
storage and downlink planning
external engineering tools
```

Every independent representation is another opportunity for meaning to drift.

OrbitFabric introduces an explicit **Mission Data Contract** so those representations can remain connected to one semantic root.

---

## The Mission Data Contract

The Mission Data Contract is expressed through a structured **Mission Model**.

It can describe mission-level concepts such as:

```text
spacecraft identity
subsystems
operational modes and transitions
telemetry
commands
events
faults and recovery intent
packets and policies
payload contracts
data products
storage and retention intent
contact and downlink assumptions
commandability
autonomy
```

The Mission Model remains the semantic source of truth.

Operational Scenarios are authored separately from the Mission Model. They exercise the Mission Data Contract as deterministic evidence inputs; they are not an additional Mission Model domain and do not become a competing semantic source of truth.

Generated documentation, runtime-facing bindings, ground-facing artifacts, machine-readable inspection surfaces and the coherent Core Integration Input Set are derived from the Mission Model. Scenario evidence combines the Mission Model with separately authored Scenarios. Target-specific integration outputs additionally depend on an explicit Projection Profile and the selected adapter.

---

## OrbitFabric Core

At the center of the ecosystem is [**OrbitFabric Core**](https://github.com/OrbitFabric/orbitfabric).

Core is the semantic authority of OrbitFabric.

It loads and validates the Mission Data Contract, performs semantic linting, executes deterministic host-side scenario evidence, generates contract-facing artifacts and exports machine-readable surfaces that downstream tools can consume without reconstructing mission meaning themselves.

Core currently provides capabilities including:

```text
Mission Model loading and validation
semantic linting
deterministic scenario evidence
generated mission documentation
runtime-facing contract bindings
ground-facing contract artifacts
Mission Snapshot
Entity Index
Relationship Manifest
Core Interface Manifest
Core Integration Input Set
integration contract surfaces
provider-neutral adapter lifecycle boundaries
```

The architectural rule is:

```text
The Mission Model is the semantic source of truth.

OrbitFabric Core owns Mission Data Contract interpretation.

Downstream systems consume explicit Core-owned facts
instead of independently rebuilding mission semantics.
```

Core is deliberately **not** a flight software framework, a ground segment, a mission control system, an operator console or a spacecraft dynamics simulator.

Its role is narrower and more foundational: preserve coherent mission meaning across those boundaries.

[Explore OrbitFabric Core](https://github.com/OrbitFabric/orbitfabric) · [Core documentation](https://orbitfabric.github.io/orbitfabric/)

---

## The ecosystem in one view

```text
                         MISSION AUTHORING
        +--------------------------------------------------+
        | YOUR MISSION                                     |
        | or ORBITFABRIC REFERENCE MISSION                 |
        |                                                  |
        | Mission Model          Operational Scenarios     |
        | semantic contract      evidence inputs           |
        +------------------------+-------------------------+
                                 |
                                 v
                    +-----------------------------+
                    |      ORBITFABRIC CORE       |
                    |                             |
                    | semantic authority          |
                    | validation + lint           |
                    | scenario evidence           |
                    | generated artifacts         |
                    | Core-owned facts            |
                    | Core Interface Manifest     |
                    | Integration Input Set       |
                    | adapter lifecycle semantics |
                    +------------+----------------+
                                 |
              +------------------+---------------------------+
              |                                              |
              | Core Interface Manifest +                    | Core Integration Input Set
              | Core-owned structured facts                  | + explicit target-specific
              v                                              |   Projection Profile
       +-------------------+                                 v
       | ORBITFABRIC       |                    +---------------------------+
       | STUDIO            |                    |    INTEGRATION ADAPTERS   |
       |                   |                    |                           |
       | understand        |                    | explicit target-specific  |
       | inspect           |                    | projection + traceability |
       | navigate          |                    | extensible boundary       |
       | explain           |                    +-------------+-------------+
       +-------------------+                                  |
                                  +----------------------------+----------------------------+
                                  |             |              |              |             |
                                  v             v              v              v             v
                              F Prime        OpenC3          EDS / cFS     OpenOBSW /     OTHER /
                              Adapter        COSMOS          Adapter       OpenSVF        COMMUNITY
                                             Adapter                        Adapter        ADAPTERS
                                  |             |              |              |             |
                                  v             v              v              v             v
                              F Prime        OpenC3          EDS / cFS     OpenOBSW /     target-owned
                              native         COSMOS          native        OpenSVF        systems /
                              system         native          systems       native         tooling
                                             system                         systems

                         Each adapter path emits an Integration Result and,
                         where applicable, target-native artifacts. Native
                         acceptance and verification can retain evidence for
                         the behavior actually demonstrated.


                 ADAPTER LIFECYCLE / DISTRIBUTION SUPPORT

       +----------------------------+
       | Adapter Developer Template |
       |                            |
       | bootstrap compatible       |
       | adapters and releases      |
       +----------------------------+

       +----------------------------+     +----------------------------+
       | Adapter Project Lock       |     | Adapter Catalog            |
       |                            |     |                            |
       | project desired exact      |     | exact release identities   |
       | adapter state              |     | + source bindings          |
       +-------------+--------------+     +-------------+--------------+
                     \                              /
                      +-------------+--------------+
                                    |
                                    v
                         exact release selection
                                    |
                                    v
                      +-------------------------------+
                      | Provider-specific             |
                      | Release Sources               |
                      |                               |
                      | GitHub Release Source         |
                      |   current implementation      |
                      | additional providers          |
                      |   are an extension point      |
                      +---------------+---------------+
                                      |
                                      v
                           verified exact release
                           ResolvedAdapterRelease
                                      |
                                      v
                           +----------------------+
                           | Core Adapter Manager |
                           |                      |
                           | install / verify     |
                           | execute / remove     |
                           +----------+-----------+
                                      |
                                      v
                           Installed Adapter State
                                      |
                                      +--> adapter execution
                                           in the lane above
```

The four named public adapters are the current OrbitFabric-maintained portfolio, not the architectural limit of the ecosystem. The adapter boundary is deliberately extensible: community-maintained, private, experimental and future target integrations can use the same Core-owned contracts without moving target-specific semantics into Core.

The semantic/integration path and the adapter lifecycle/distribution path are deliberately separate. For the managed project path, Adapter Project Lock expresses desired exact state, the Adapter Catalog identifies exact releases and source bindings, provider-specific Release Sources acquire and verify exact release material, and Core Adapter Manager owns the provider-neutral installed lifecycle. GitHub Release Source is the current provider implementation, not a GitHub-specific constraint on the architecture. Compatible private, experimental and uncatalogued adapters remain valid outside the canonical Catalog path.

The ecosystem is intentionally separated by ownership:

```text
Core owns mission semantics.

Studio makes Core-owned facts understandable to humans.

Adapters own explicit target-specific projection and traceability.

External systems remain authoritative for their native
architecture, implementation and runtime behavior.

Evidence records what was actually demonstrated.
```

---

## OrbitFabric Studio

[**OrbitFabric Studio**](https://github.com/OrbitFabric/orbitfabric-studio) is the local-first engineering workbench for seeing and understanding an OrbitFabric mission.

Its product thesis is deliberately human-centered:

> OrbitFabric exposes the Mission Data Contract. OrbitFabric Studio exposes the mission to the human.

Studio first consumes the Core Interface Manifest to negotiate the Core capabilities it requires, then consumes structured Core-owned facts and organizes them into complementary engineering lenses for mission structure, entity inspection, relationships, operational logic, validation findings, scenarios, evidence and provenance.

The authority boundary is strict:

```text
Core owns the fact.
Studio makes the fact understandable.
```

Studio may organize, navigate, label, correlate and visualize explicit mission facts.

It must not become a second Mission Model interpreter, privately recover missing semantics from YAML, invent relationships, manufacture mission-readiness scores or present engineering UI state as mission truth.

[Explore OrbitFabric Studio](https://github.com/OrbitFabric/orbitfabric-studio)

---

## OrbitFabric Reference Mission

[**OrbitFabric Reference Mission**](https://github.com/OrbitFabric/orbitfabric-reference-mission) is the shared public engineering environment for the ecosystem.

It is a realistic but synthetic small-spacecraft mission used as:

```text
a progressive Mission Data Contract tutorial
an executable scenario evidence set
a Core integration/reference mission
a Studio engineering acceptance mission
an ecosystem convergence environment
a source for reproducible Engineering Stories
```

The Reference Mission is not flight software, a real spacecraft configuration or a spacecraft simulator.

Its role is to provide one coherent mission context in which architectural claims can be demonstrated against the same semantic root instead of through unrelated examples.

[Explore the Reference Mission](https://orbitfabric.github.io/orbitfabric-reference-mission/)

---

## Integration model

OrbitFabric integrations are based on explicit boundaries rather than hidden translation rules.

The generic pattern is:

```text
Mission Model
    |
    v
OrbitFabric Core
    |
    v
coherent Core Integration Input Set
    |
    + explicit Projection Profile
    |
    v
target-specific Adapter
    |
    + target-native artifacts
    + mappings
    + diagnostics
    + provenance
    + Integration Result
    |
    v
native target tooling / runtime / verification
```

An adapter does not gain authority to reinterpret Mission Data Contract semantics.

A production integration consumes documented Core-owned surfaces. It does not silently fall back to reparsing raw Mission Model YAML when a required semantic surface is missing or incompatible.

The Projection Profile expresses explicit target-specific intent. The adapter owns the projection. The target ecosystem remains authoritative for native realization.

This separation is fundamental:

```text
mission meaning != target realization

declared behavior != observed behavior

projected constraint != runtime enforcement

static semantic agreement != deployment readiness
```

---

## Current integration adapters

The current public OrbitFabric-maintained adapter portfolio deliberately exercises materially different integration boundaries. These adapters are concrete implementations of an extensible adapter contract, not an exhaustive list of systems that OrbitFabric can integrate with.

### F Prime

[**orbitfabric-fprime-adapter**](https://github.com/OrbitFabric/orbitfabric-fprime-adapter)

Projects explicit OrbitFabric mission contracts toward native F Prime and FPP engineering surfaces while preserving F Prime ownership of component architecture, instances, topology, scheduling, generated dictionaries and runtime behavior.

### OpenC3 COSMOS

[**orbitfabric-openc3-cosmos-adapter**](https://github.com/OrbitFabric/orbitfabric-openc3-cosmos-adapter)

Projects selected mission and verification intent toward OpenC3 COSMOS-native procedures and verification surfaces while preserving COSMOS ownership of its target model, interfaces, telemetry processing, procedure execution and runtime behavior.

### EDS / cFS

[**orbitfabric-eds-cfs-adapter**](https://github.com/OrbitFabric/orbitfabric-eds-cfs-adapter)

Carries OrbitFabric contracts through CCSDS Electronic Data Sheets into the NASA EdsLib / cFS integration lane.

The adapter owns deterministic projection and traceability. The selected cFS mission owns mission-specific target allocation. EdsLib, MissionLib, cFE and cFS remain authoritative for native realization and runtime behavior.

### OpenOBSW / OpenSVF

[**orbitfabric-openobsw-opensvf-adapter**](https://github.com/OrbitFabric/orbitfabric-openobsw-opensvf-adapter)

Connects OrbitFabric mission contracts with OpenOBSW integration surfaces and OpenSVF validation workflows while preserving the independent authority of both downstream systems.

F Prime, OpenC3 COSMOS, NASA cFS/EdsLib, OpenOBSW and OpenSVF are independent upstream projects. OrbitFabric integrations do not imply endorsement by those projects.

---

## Integration Coverage

OrbitFabric-maintained adapters do not describe maturity through vague claims such as "supported" or a single percentage.

They explicitly distinguish:

```text
OrbitFabric Semantic Surface
        |
        v
Target Applicable Surface
        |
        v
Adapter Declared Scope
```

Capabilities are classified using explicit dispositions such as:

```text
FULL
PARTIAL
OUT_OF_SCOPE
NOT_IMPLEMENTED
TARGET_UNSUPPORTED
NOT_APPLICABLE
NOT_ANALYZED
```

This matters because a focused adapter can be complete within its declared purpose without pretending to implement every feature of the downstream system.

Likewise, a downstream limitation is recorded as a limitation of the projection or target rather than silently weakening the upstream Mission Data Contract.

The rule is simple:

> Integration claims stop where the retained evidence stops.

---

## Adapter ecosystem support

The adapter portfolio is supported by several deliberately separated projects. These support adapter development, discovery and acquisition; they do not sit inside the semantic projection path and they do not transfer target-specific authority into Core.

### Adapter Developer Template

[**orbitfabric-adapter-template**](https://github.com/OrbitFabric/orbitfabric-adapter-template)

An executable starting point for developers building new OrbitFabric-compatible adapters.

It demonstrates repository structure, contract consumption, packaging, testing, release boundaries and Integration Coverage without becoming an alternative specification to Core.

### Adapter Catalog

[**orbitfabric-adapter-catalog**](https://github.com/OrbitFabric/orbitfabric-adapter-catalog)

The canonical version-controlled index of exact adapter releases and their acquisition source bindings.

The Catalog is a data product, not a package registry and not an endorsement database.

Community-maintained adapters may be catalogued when they publish compatible exact release identities and artifacts. Private, experimental and uncatalogued adapters remain valid.

### GitHub Release Source

[**orbitfabric-github-release-source**](https://github.com/OrbitFabric/orbitfabric-github-release-source)

The first provider-specific acquisition component for OrbitFabric adapters.

It resolves and verifies exact adapter releases published through GitHub and hands verified release material into Core's provider-neutral lifecycle boundary. Additional provider-specific Release Sources can be introduced behind the same resolved-release handoff when there is evidence to justify them; GitHub is the current implementation, not an architectural requirement.

Keeping this component outside Core prevents GitHub-specific acquisition behavior from becoming part of the Mission Data Contract or Core semantic authority.

### Core Adapter Management

Adapter lifecycle semantics themselves remain Core-owned.

Core distinguishes logical adapter identity, exact release identity, project desired state, installed state and provider-specific acquisition metadata.

This separation allows distribution mechanisms to evolve without redefining what an adapter means or how mission semantics are owned.

---

## Engineering Stories

The Reference Mission also hosts **Engineering Stories**: reproducible technical investigations built around concrete engineering questions.

A complete Engineering Story is intentionally layered:

```text
Front Story
    Why does this matter?
    What problem did we explore?
    What did we learn?

Technical Deep Dive
    How exactly does it work?
    Where are the ownership boundaries?
    What evidence supports the claim?

Reference Project
    Can I inspect it?
    Can I run it?
    Can I reproduce the result?
```

The first completed Story is:

### R1: One Mission Contract Across Flight and Ground

R1 asks whether one canonical mission-level semantic root can drive independently owned flight and ground engineering paths without forcing either side to adopt the other's implementation model.

The experiment follows one concrete command-and-telemetry slice through native F Prime and OpenC3 COSMOS paths and into a live flight-ground verification loop.

The result is deliberately narrow and evidence-backed:

> One mission-level semantic root can remain authoritative over meaning while native flight and ground systems retain their own architecture, implementation ownership and runtime behavior, and the two paths can still be verified to converge on one real command/telemetry loop.

[Read the R1 Engineering Story](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/) · [Technical Deep Dive](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/technical-deep-dive/)

---

## Engineering discipline

OrbitFabric is developed as an engineering system, not as a collection of loosely related utilities.

Important architectural boundaries are made explicit before they become product behavior.

The project uses:

```text
explicit contracts
clear semantic ownership
stable vs candidate surface classification
exact dependency baselines
compatibility rules
fail-closed behavior
deterministic evidence
retained provenance
native downstream acceptance
controlled releases
bounded claims
```

A surface does not become stable merely because it exists.

A generated artifact does not become authoritative merely because another system consumes it.

An integration does not claim semantics that were not actually projected.

A native target behavior is not inferred from a schema if runtime evidence says otherwise.

A successful static mapping is not presented as proof of a functioning deployment.

Architectural investigations, decisions, implementation gates and acceptance evidence are maintained through a structured engineering process so that significant changes are reasoned about, falsified where possible and demonstrated before they become part of the public product surface.

That rigor is intentionally visible in the repositories through contracts, compatibility documentation, Integration Coverage, exact version baselines, reproducible examples and retained CI evidence.

---

## Open source

OrbitFabric is an open-source engineering project.

The public repositories are intended to be inspected, reproduced, challenged and extended.

Public examples and reference material follow a clean-room approach: synthetic or publicly usable material is used instead of proprietary mission data, private packet definitions, employer-owned code or customer information.

Community integrations are welcome.

Extending OrbitFabric does not require moving target-specific semantics into Core. New systems can remain authoritative for their own native behavior while participating through explicit contracts and evidence-backed integration boundaries.

---

## Who OrbitFabric is for

OrbitFabric is intended for people working where mission meaning crosses software or organizational boundaries, including:

- spacecraft software and systems architects;
- flight software engineers;
- ground software and operations engineers;
- verification and validation engineers;
- payload and subsystem integrators;
- university CubeSat and small-spacecraft teams;
- research laboratories;
- small space companies and technical teams;
- embedded engineers entering the space domain;
- developers building engineering tools around mission semantics.

OrbitFabric may be especially relevant when the same mission information currently exists independently in code, dictionaries, documents, tests, procedures and integration scripts and those representations need to remain traceable to one explicit semantic root.

---

## Where to start

| If you want to... | Start with |
| --- | --- |
| Understand OrbitFabric semantics and the Mission Data Contract | [OrbitFabric Core](https://github.com/OrbitFabric/orbitfabric) |
| Install and exercise Core | [Core documentation](https://orbitfabric.github.io/orbitfabric/) |
| See and navigate a mission interactively | [OrbitFabric Studio](https://github.com/OrbitFabric/orbitfabric-studio) |
| Explore a coherent synthetic mission | [Reference Mission](https://github.com/OrbitFabric/orbitfabric-reference-mission) |
| Follow an end-to-end flight/ground engineering proof | [R1 Engineering Story](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/) |
| Integrate with F Prime | [F Prime Adapter](https://github.com/OrbitFabric/orbitfabric-fprime-adapter) |
| Integrate with OpenC3 COSMOS | [OpenC3 COSMOS Adapter](https://github.com/OrbitFabric/orbitfabric-openc3-cosmos-adapter) |
| Integrate with NASA cFS / EdsLib | [EDS-cFS Adapter](https://github.com/OrbitFabric/orbitfabric-eds-cfs-adapter) |
| Integrate with OpenOBSW / OpenSVF | [OpenOBSW/OpenSVF Adapter](https://github.com/OrbitFabric/orbitfabric-openobsw-opensvf-adapter) |
| Build a new adapter | [Adapter Developer Template](https://github.com/OrbitFabric/orbitfabric-adapter-template) |
| Browse exact adapter release identities | [Adapter Catalog](https://github.com/OrbitFabric/orbitfabric-adapter-catalog) |
| Understand GitHub-based adapter acquisition | [GitHub Release Source](https://github.com/OrbitFabric/orbitfabric-github-release-source) |

---

**OrbitFabric makes the mission contract explicit so that the systems around it can evolve without silently redefining what the mission means.**
