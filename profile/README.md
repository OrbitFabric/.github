<p align="center">
  <img src="https://raw.githubusercontent.com/OrbitFabric/orbitfabric/main/assets/brand/orbitfabric-logo-horizontal-light.png" alt="OrbitFabric" width="720">
</p>

<p align="center">
  <strong>One mission contract. Different systems remain themselves.</strong>
</p>

# OrbitFabric

**A model-first Mission Data Fabric for small spacecraft.**

A mission's meaning is scattered across systems that were never designed to share one model. A command appears in mission engineering, flight software, ground operations and verification. Telemetry, modes, faults and data products acquire different representations in every toolchain. Each system needs its own design and runtime authority, yet those representations must still describe the same mission.

OrbitFabric addresses that boundary: **define mission meaning explicitly, preserve its identity as other systems consume it, and make the resulting engineering work traceable.**

It does not ask flight software, ground software, verification tools or simulators to become OrbitFabric. It gives them a common semantic reference without taking over what they natively own.

> **One semantic root. Multiple native realizations. Evidence that keeps the distinctions honest.**

## The ecosystem at a glance

Four complementary roles organize the project:

| | Project | Why it exists |
| --- | --- | --- |
| **01** | [**Core**](https://github.com/OrbitFabric/orbitfabric) | Establish the Mission Data Contract and provide authoritative, machine-readable mission facts |
| **02** | [**Studio**](https://github.com/OrbitFabric/orbitfabric-studio) | Help engineers see, navigate and understand the mission as a connected system |
| **03** | [**Integration Adapters**](#3-integration-adapters) | Project explicit mission intent into independently owned native engineering environments |
| **04** | [**Reference Mission**](https://github.com/OrbitFabric/orbitfabric-reference-mission) | Make the architecture inspectable through one coherent mission and reproducible Engineering Stories |

These are **architectural responsibilities and the intended reading order**, not four stages of a mandatory execution pipeline. Studio and adapters are distinct consumers of Core-owned surfaces; the Reference Mission provides the common context in which the ecosystem is exercised.

```text
                 MISSION DATA CONTRACT
                 structured Mission Model
                           |
                           v
                    ORBITFABRIC CORE
                   semantic authority
                  /                  \
                 /                    \
          Core-owned facts      Integration Input Set
          + Interface Manifest  + explicit Projection Profile
                 |                      |
                 v                      v
          ORBITFABRIC STUDIO      INTEGRATION ADAPTERS
          see and understand      explicit native projection
                                        |
                                        v
                                INDEPENDENT SYSTEMS
                                flight / ground / tools
                                native runtime and evidence


          REFERENCE MISSION + ENGINEERING STORIES
          shared mission context, experiments and proofs
```

Operational Scenarios are authored separately from the Mission Model. They exercise the same contract and provide declared test intent; they are not a second source of mission semantics. Target-native execution and observation remain outside Core's authority.

---

## 1. OrbitFabric Core

**[Core](https://github.com/OrbitFabric/orbitfabric)** is the foundation of OrbitFabric. It owns the interpretation of the **Mission Data Contract**, expressed as a structured, machine-readable **Mission Model**.

That model describes mission-level entities and constraints: spacecraft and subsystems, commands and telemetry, operational modes, events and faults, payload behavior, packets, data products, storage, contacts, downlink priorities, commandability and autonomy.

Core turns the contract into something engineering teams and other tools can use:

- **Validate and lint:** check structural consistency and mission-level semantic relationships.
- **Exercise declared intent:** consume separately authored Operational Scenarios and produce deterministic host-side evidence. This is not spacecraft runtime simulation.
- **Document and export:** generate mission documentation, contract-facing flight/ground bindings and artifacts.
- **Expose structured facts:** publish surfaces such as Mission Snapshot, Entity Index, Relationship Manifest, Scenario Declaration, Core Interface Manifest and the coherent Core Integration Input Set.

Core also defines generic integration contracts and the provider-neutral lifecycle boundaries for installed external adapters. Those responsibilities do not make Core an adapter implementation or a native runtime.

**The Mission Model is the semantic source of truth.** Derived files remain derived. A target system may implement mission intent, but it does not silently redefine the contract that intent came from.

[Explore Core](https://github.com/OrbitFabric/orbitfabric) · [Architecture](https://github.com/OrbitFabric/orbitfabric/blob/main/docs/ARCHITECTURE.md) · [Documentation](https://orbitfabric.github.io/orbitfabric/)

---

## 2. OrbitFabric Studio

**[Studio](https://github.com/OrbitFabric/orbitfabric-studio)** is the local-first engineering workbench for understanding an OrbitFabric mission.

> **Core exposes the Mission Data Contract. Studio exposes the mission to the human.**

An engineer opening an unfamiliar mission should not have to reconstruct its architecture by jumping among YAML files, command-line outputs, generated dictionaries and diagrams. Studio aims to provide a coherent mental model from which that engineer can ask:

- What exists in this mission?
- How are its parts connected?
- What can happen in a given operational context?
- Which facts are declared, which were exercised, and where did they come from?

The **public development preview** provides mission orientation through Mission Atlas, entity exploration and Entity X-Ray, relationships and context, validation findings, and operational logic. The broader product vision develops complementary mission-understanding lenses while keeping source facts and provenance explicit.

Studio consumes Core-owned structured information and negotiates required capabilities through the Core Interface Manifest. It can organize and visualize facts; it **cannot invent missing mission relationships, reinterpret Mission Model semantics, or treat UI state as runtime truth**.

Studio is not a generic YAML editor, graph toy, dashboard or mission-control console. Its purpose is to make a real engineering question easier to investigate, with the evidence still reachable.

[Explore Studio](https://github.com/OrbitFabric/orbitfabric-studio) · [Vision](https://github.com/OrbitFabric/orbitfabric-studio/blob/main/docs/VISION.md) · [Public preview scope](https://github.com/OrbitFabric/orbitfabric-studio#public-preview-scope)

---

## 3. Integration Adapters

**[Integration Adapters](https://github.com/OrbitFabric/orbitfabric-adapter-template)** connect OrbitFabric intent to systems that rightly have their own architecture and engineering language.

This is not a promise that one mission model automatically generates complete flight software or ground systems. The adapter consumes a **Core Integration Input Set** and an explicit **Projection Profile**; it owns target-specific mapping, materialization, diagnostics and traceability.

```text
Core-owned mission inputs
       + Projection Profile
                |
                v
     target-specific adapter
                |
                +--> native artifacts and mappings
                +--> declared coverage and diagnostics
                +--> Integration Result and provenance
                |
                v
     independently owned target system
        native acceptance / runtime evidence
```

A projection may be complete within its declared scope, partial or deliberately not implemented. **Projection is not execution; generated artifacts are not proof of target behavior.** Native tools retain their own validity rules, identifiers, execution and observations.

### Four public integration paths

| Adapter | What it connects, and where authority remains |
| --- | --- |
| [**F Prime / FPP**](https://github.com/OrbitFabric/orbitfabric-fprime-adapter) | Mission contract identities to FPP declarations and native F Prime projects. F Prime owns component architecture, topology, dictionaries, build and runtime. |
| [**OpenC3 COSMOS**](https://github.com/OrbitFabric/orbitfabric-openc3-cosmos-adapter) | Selected Scenario and verification intent to COSMOS procedures and suites. COSMOS owns native interpretation, command/telemetry transport and execution. |
| [**EDS / cFS**](https://github.com/OrbitFabric/orbitfabric-eds-cfs-adapter) | Mission contracts to CCSDS Electronic Data Sheets and the NASA EdsLib / cFS integration lane. The selected cFS mission owns target allocation; native tools own realization. |
| [**OpenOBSW / OpenSVF**](https://github.com/OrbitFabric/orbitfabric-openobsw-opensvf-adapter) | Mission contracts to OpenOBSW-facing integration and OpenSVF verification surfaces. Both systems preserve independent implementation and validation authority. |

The adapters are **examples of distinct integration boundaries**, not the exhaustive scope of OrbitFabric. Community and private adapters can use the same documented contracts. Integration Coverage and retained evidence make supported and unsupported target behavior explicit.

### Building and distributing adapters

Adapter development and release acquisition are a separate support layer:

- [**Adapter Developer Template**](https://github.com/OrbitFabric/orbitfabric-adapter-template) demonstrates a working repository and contract-consumption pattern.
- [**Adapter Catalog**](https://github.com/OrbitFabric/orbitfabric-adapter-catalog) records exact adapter releases and acquisition bindings; it is not a package registry or endorsement list.
- [**GitHub Release Source**](https://github.com/OrbitFabric/orbitfabric-github-release-source) resolves and verifies published release material from GitHub.
- **Core Adapter Manager** owns the provider-neutral install, verify, execute and remove lifecycle.

Release identity and installation are not the same as mission semantics or target-runtime evidence. Keeping them separate is intentional.

---

## 4. OrbitFabric Reference Mission

**[Reference Mission](https://github.com/OrbitFabric/orbitfabric-reference-mission)** is the public engineering environment in which OrbitFabric can be learned, challenged and demonstrated against **one coherent mission context**, rather than a disconnected set of illustrative examples.

It has two complementary parts.

**The Reference Mission** is a realistic but synthetic small-spacecraft Mission Model, with Operational Scenarios, a progressive tutorial, generated contract evidence and a common context for Core and Studio engineering work. It is not a real spacecraft configuration, onboard application or spacecraft simulator.

**Engineering Stories** take a concrete question and follow it across real software boundaries using a reproducible Reference Project. Each Story has a reader-facing narrative and inspectable engineering material. A separate Technical Deep Dive is optional.

### Engineering Stories

**[R1: One Mission Contract Across Flight and Ground](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/)**

Can one mission-level semantic root survive across independently owned flight and ground systems? R1 takes a concrete stop-acquisition intent through **F Prime** and **OpenC3 COSMOS** and demonstrates convergence in a live command/telemetry verification loop. [Technical Deep Dive](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/technical-deep-dive/).

**[R2: One Intent, Two Executions](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r2-pwnsat/)**

What can we legitimately say about execution when one authorized mission operation crosses into an external runtime? R2 uses **PWNSAT/FlatSat** to investigate the distinction between command intent, projected representation, controlled presentation, runtime observations and evidence-supported execution claims.

The Stories are not feature showcases built to guarantee a favorable outcome. They are engineering investigations whose conclusions remain bounded by what was actually demonstrated.

[Explore the Reference Mission](https://orbitfabric.github.io/orbitfabric-reference-mission/) · [Browse Engineering Stories](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/) · [Reference Mission repository](https://github.com/OrbitFabric/orbitfabric-reference-mission)

---

## What holds the ecosystem together

OrbitFabric is built around a small number of deliberate commitments:

**Semantic ownership.** One Mission Data Contract remains authoritative. Studio, adapters and reference experiments consume it without becoming competing interpreters.

**Native independence.** F Prime remains F Prime. COSMOS remains COSMOS. cFS, OpenOBSW, OpenSVF and other external systems keep their own implementation, configuration and runtime responsibility.

**Traceability over magic.** An integration records source identity, explicit projection choices, target mapping, coverage, diagnostics and provenance. Unsupported parts do not disappear behind a successful artifact generation.

**Evidence over inference.** Declared intent, projected output, target execution and observation are different facts. A successful integration step does not establish that a spacecraft or external runtime behaved as expected.

**Reproducible engineering.** Public examples, exact baselines, contracts, validation and Engineering Stories are intended to be inspected and challenged. Public material uses synthetic or otherwise publicly usable inputs rather than proprietary mission information.

These commitments are as important as individual features. They allow the components of the ecosystem to evolve independently without silently changing what the mission means.

---

## Where to start

| If you want to... | Start here |
| --- | --- |
| Understand the Mission Data Contract and semantic authority | [Core](https://github.com/OrbitFabric/orbitfabric) and its [documentation](https://orbitfabric.github.io/orbitfabric/) |
| See an unfamiliar mission as an engineering system | [Studio](https://github.com/OrbitFabric/orbitfabric-studio) |
| Connect mission intent to a native toolchain | [F Prime](https://github.com/OrbitFabric/orbitfabric-fprime-adapter), [OpenC3 COSMOS](https://github.com/OrbitFabric/orbitfabric-openc3-cosmos-adapter), [EDS/cFS](https://github.com/OrbitFabric/orbitfabric-eds-cfs-adapter), [OpenOBSW/OpenSVF](https://github.com/OrbitFabric/orbitfabric-openobsw-opensvf-adapter) |
| Follow a structured mission tutorial or inspect scenarios | [Reference Mission](https://orbitfabric.github.io/orbitfabric-reference-mission/) |
| Examine an accepted flight/ground integration proof | [Engineering Story R1](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r1-flight-ground/) |
| Explore the limits of execution accountability | [Engineering Story R2](https://orbitfabric.github.io/orbitfabric-reference-mission/engineering-stories/r2-pwnsat/) |
| Build or install a compatible adapter | [Adapter Template](https://github.com/OrbitFabric/orbitfabric-adapter-template), [Catalog](https://github.com/OrbitFabric/orbitfabric-adapter-catalog) and [Release Source](https://github.com/OrbitFabric/orbitfabric-github-release-source) |


---

**Make mission meaning explicit. Let native systems remain themselves. Know what the evidence actually proves.**
