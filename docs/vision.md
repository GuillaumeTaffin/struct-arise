# struct-arise: Vision

## The Problem

AI tools are transforming how developers work. Specifications matter more than ever: they guide AI agents, scope their context, and define what "done" looks like. Yet most teams still rely on unstructured markdown documents to capture requirements and design decisions.

Unstructured docs are easy to write and readable by both humans and AI, but they resist programmatic analysis. Worse, they drift. Documentation and code diverge over time, and keeping them synchronized is a manual, tedious process that rarely survives contact with real delivery pressure.

Today, verifying that code actually implements what the spec describes means feeding large amounts of files to an LLM and asking it to check. This is expensive, slow, and non-deterministic. It doesn't scale, and it can't be trusted as a gate in a CI pipeline.

## The Core Insight

If documentation is **structured data** rather than free text, spec-code traceability becomes a deterministic, programmatic problem, not an AI inference problem.

AI is excellent at helping humans produce structured artifacts from unstructured input. That's the enabler: let developers think and communicate in natural language, then use AI to distill that into structured specifications that machines can reason about.

Structured specs unlock bidirectional traceability:
- **Top-down** (spec → code): "Every business rule has a corresponding test."
- **Bottom-up** (code → spec): "This component implements these use cases."

## Why Now

The idea of structured, traceable specifications is not new. What is new is that it's finally practical, and that the role of the software engineer is shifting to make it necessary.

The software engineer's job has always been the same: solving problems. What changed over time is where the bottleneck sat. Before AI tools, the bottleneck was writing code. Code was the primary means of expression, and the practical challenge was finding the right balance between implementation speed and explicit design. The degenerate form of "best practices" was to stop thinking in terms of higher-level design, specs, and architectural links altogether, letting structure emerge entirely from the code after the fact. The best teams found a sweet spot: enough explicit documentation and architecture, combined with automated tools (architecture constraint checks, test conventions, naming standards) to enforce some structural intent without drowning in overhead. But even those teams were constrained by the fundamental asymmetry: code was fast to write, test, and refactor with IDEs; specifications were slow to produce, and even slower to restructure as things evolved.

Now, AI has collapsed that asymmetry. The cost of modifying documentation and the cost of modifying code have converged. Restructuring a specification, creating complex documentation structures, and maintaining the links between architectural layers can now happen at roughly the same speed as refactoring code. The bottleneck has shifted. It is no longer how fast you can implement; it is how fast you can keep the full picture (vision, specifications, architecture, code) coherent and aligned. Sustainable progress depends on maintaining that structured flow of intent from specs down to implementation. Without it, AI tools produce code fast but in the wrong direction.

This makes structured specifications **essential**, not just good practice. At the speed AI can implement, the quality of its output depends directly on the quality of the context it receives: clear requirements, explicit relationships between parts, well-defined boundaries. The engineer's central work becomes steering that context (defining the intent, the structure, the constraints) so that implementation stays aligned with the actual vision for the software. The role is the same; the leverage point has moved.

Concretely, AI makes this viable in three ways:

**Lower production cost.** AI doesn't just write faster; it helps developers rediscover relevant parts of their own specs. When reasoning about a change, AI can surface related business rules, affected use cases, or upstream dependencies that a human might lose sight of in a large spec base. It accelerates the design reflection itself, not just the writing.

**Viable incremental evolution.** The old documentation problem: the bigger it gets, the harder it is to know where to evolve it. With AI, the cost of restructuring stays low. A developer can start small (a bit of spec, a bit of implementation) and even when everything moves fast early on, rewriting and reorganizing the structured artifacts remains cheap. Stable parts emerge naturally as the system matures. There is no need to get the structure right upfront; it can be discovered and refined continuously.

**Spec-code co-evolution at implementation speed.** This is the critical shift. With AI assistance, evolving the spec transitively down to code, and evolving code back up to the spec, can happen as fast as just changing the code alone. The structured spec is no longer a drag on velocity; it moves with the implementation, in both directions, at the same pace.

## What struct-arise Is

**struct-arise** is an opinionated framework and application for creating, storing, and analyzing structured specifications.

At its core, it is a server that manages structured spec artifacts and exposes them through multiple interfaces: a CLI for developers, an AI connector via MCP for agent integration, and potentially a web UI for visualization and exploration.

It is also a proposed **methodology**: a way of working where developers design features through structured specs, then delegate implementation (to humans or AI agents) with clear, traceable requirements. The tool and the workflow reinforce each other.

## Design Principles

### Composable modeling perspectives

There is no single correct way to model a product. struct-arise does not impose one fixed ontology. Instead, it supports multiple **perspectives**, lenses that overlay the same product from different angles:

- **Business lens** (DDD-inspired): Product → Features → Use Cases → Business Rules → Domain Entities
- **Architecture lens** (C4-inspired): System Context → Containers → Components → Code
- **Code organization lens**: modules, file structure, test organization

These perspectives cross-reference each other. A business rule links to the component that implements it. A component links to the module that contains it.

### Opinionated but grounded

struct-arise reuses proven modeling primitives from DDD, BDD, and C4. It does not invent new paradigms; it combines existing ones into a coherent, structured format.

### Language-agnostic core

The specification model is independent of any programming language or framework. Specs describe *what* the system does and *how it is structured*, not *how it is coded*.

### Language-specific binders

Adapters for each ecosystem bridge the gap between specs and code. A Java binder might use annotations to mark which class implements which domain entity. Other ecosystems get their own idiomatic binders.

### Structured storage

Specifications are stored as schema-backed data (format TBD: JSON, XML, or similar), not plain text files. This enables validation, querying, and transformation by tools rather than by humans reading prose.

### Renderable

Structured data can always be transformed into human-readable formats (markdown, HTML, diagrams). The structured form is the source of truth; rendered views are derived artifacts.

## What It Enables

- **Semantic organization**: structured specs decompose a system by purpose and intent (features, use cases, business rules). This structure naturally pushes the codebase toward the same semantic boundaries, producing code that is organized by what it achieves rather than by technical layers alone. The result is a codebase that is inherently more evolvable, because changes to a goal are scoped to the code that serves that goal.
- **Deterministic analysis**: extract all business rules from the spec, check that each has a corresponding test annotation in code. No LLM required.
- **Visualization**: because relationships are explicit in the data, tools can render dependency graphs, coverage maps, and traceability matrices.
- **AI-augmented spec creation**: AI agents use struct-arise's API and tools to produce structured artifacts from unstructured developer input (conversations, rough notes, existing docs).
- **Scoped context for implementation**: developers or AI agents receive precisely the spec context relevant to their current task, no more, no less.
- **Validation hooks**: CI/CD integration to verify spec-code synchronization on every commit.
- **Composability**: starts at the single-project level, scales to cross-team and cross-project traceability.

## Inspirations and Positioning

struct-arise draws from:
- **Domain-Driven Design**: bounded contexts, aggregates, entities, ubiquitous language
- **Behavior-Driven Development**: scenarios, acceptance criteria, specification by example
- **C4 Model**: layered architecture views from system context down to code
- **Context engineering**: providing the right information to the right agent at the right time

It borrows the *concepts* from these disciplines, not their specific tooling. struct-arise is not Gherkin, not PlantUML, not a particular DDD framework. It is a structured layer that can express the ideas these tools encode, in a form that enables programmatic reasoning.

The first ecosystem binder will target **Java**, which offers rich existing tooling for annotations, BDD integration, architecture validation, and clean architecture enforcement.
