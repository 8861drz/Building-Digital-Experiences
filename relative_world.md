# Relative Architecture — A World View

## Introduction

<!-- DOC 1: World View -->

Every business — regardless of size or industry — operates as a participant in a larger ecosystem of participants. It receives intent, applies its own rules, produces outcomes, and communicates those outcomes to others who may react or not as they see fit. Its partners do the same. So do its partners' partners.

Most businesses don't think of themselves this way. Small businesses see a collection of daily problems. Large enterprises see organizational silos. Startups see a product to build. But the ecosystem exists whether it is recognized or not — and the cost of not recognizing it is paid eventually. In the large enterprise it arrives as untouchable legacy systems and impossible integration projects. In the startup it arrives faster and hits harder — a monolith built in year one that the business has outgrown by year three.

Relative Architecture is simply a precise description of how commercial activity has always worked — given a name and a set of tools so that business, product, and engineering can work from the same model from the beginning. Startups especially should start here. It is far easier to honor the ecosystem from the first line of code than to rediscover it after the monolith is already built.

This document builds the model from first principles — starting with what an application fundamentally is, through the nature of business ecosystems, to the organizational structure that makes it all governable. It is intended to be as readable to a CEO as to a developer. That shared readability is not incidental. It is the point.

---

## The Evolution That Makes This Necessary

<!-- DOC 1: World View -->

Understanding why Relative Architecture matters now requires understanding how business systems have evolved — and what that evolution has done both to the definition of an application and to the people inside those businesses.

---

### Era 1 — Internal Systems

The application exists to serve internal operators. An order entry clerk enters the order. A loan officer submits the application. A booking agent makes the reservation. The system is a tool for employees executing a process on behalf of customers.

The boundary of the application is the walls of the organization. Participants are internal, controlled, and trained. Tight coupling between the system and its operators is not just acceptable — it is by design. The process lives in the people. The system supports them.

The operator's role is **execution**. They are the human face of the business process.

> **Era 1 definition:** An application captures intent, enforces rules, and produces an outcome.

---

### Era 2 — Consumer Facing Systems

The consumer enters their own order. Submits their own application. Books their own reservation. The system now faces outward. The internet made this possible and competitive pressure made it necessary.

The operator shifts from executing the process to managing exceptions. The routine work moves to the consumer. The human stays in the loop for edge cases, escalations, and judgment calls the system cannot handle.

But the application remains tightly coupled — now to a specific consumer surface. A website. A mobile app. Designed for one type of participant, one interaction pattern, one assumption about who is on the other side. The contract is still implicit, buried in the interface.

The operator's role becomes **exception management**. They handle what the consumer-facing system cannot.

> **Era 2 definition:** An application captures intent from consumers directly, enforces rules, and produces an outcome — with operators managing the process rather than executing it.

---

### Era 3 — Ecosystem Participation

Anyone who honors the contract can participate. A consumer through a mobile app. A partner through an API. An automated system reacting to an event. Another business integrating your capability into their own process. The surface is irrelevant. The contract is everything.

The operator is no longer executing transactions or managing individual exceptions. They are **stewarding the process** — setting the rules, designing exception handling, making judgment calls on what the automation cannot resolve. The routine flows through the ecosystem automatically because the contract governs it.

The application now has two obligations beyond producing its own outcome. It must expose its capabilities as explicit contracts that any participant can invoke. And it must publish its outcomes as events so the ecosystem can react — other participants subscribing to what matters to them and ignoring the rest.

This is the era Relative Architecture is designed for. And it is the era most established businesses are struggling to reach — because they have Era 1 systems adapted for Era 2 surfaces, and getting to Era 3 requires not just new technology but a fundamentally different way of thinking about what an application is.

The tight coupling that was appropriate in Era 1 becomes the obstacle in Era 3. A capability buried in a UI designed for one participant cannot be consumed by any other. The contract has to be extracted from the surface and made explicit before ecosystem participation is possible.

> **Era 3 definition:** An application captures intent from any participant who honors its contract, enforces rules, produces an outcome, publishes that outcome as an event for the ecosystem to react to, and subscribes to external events that shift its own context and available actions.

This final definition is where the full model lives. Everything that follows is an unpacking of what it means and how to realize it.

---

### The People Who Lived Through It

The evolution created a talent problem that most organizations don't recognize they have — and a solution hiding in plain sight.

The domain analyst role the model demands sounds like a rare combination. Deep business knowledge. Process fluency. Enough technical literacy to express domain knowledge as contracts. Where do you find that person?

You already have them.

The person who lived through all three eras in a domain is exactly that candidate.

They were the **Era 1 operator**. They executed the process manually. They know every rule, every edge case, every exception that happens on a Tuesday. They know why the underwriting rules exist, not just what they are. They know which regulatory constraints are hard and which are interpretive. That knowledge took years to accumulate and cannot be found in any document.

They managed the **Era 2 transition**. They watched consumers take over execution and shifted to managing exceptions. They learned to articulate the process to a development team. They became the person developers came to when requirements were ambiguous. They developed an instinct for when a system was modeling the domain correctly and when it was subtly wrong.

They are living through **Era 3** — often frustrated. Frustrated that the system treats every participant like a consumer with a browser. That integrations require heroic custom work. That the rules are buried in code nobody can read. That partners can't connect without months of project work.

That frustration is diagnostic. It means they already understand the model intuitively. They can feel the gap between what the ecosystem should be and what the tightly coupled system allows. They just haven't had the language to express it or the organizational structure to act on it.

Give them the language — contracts, capabilities, process traces, ecosystem participation — and they recognize it immediately as a precise description of what they already know. They don't need to learn the domain. They need to learn the expression.

This is not a unicorn hire. It is **developing the people who already have the hardest half of the job** and giving them the tools for the other half. The domain analyst pipeline already exists inside most organizations — in the people who have been closest to the business process the longest, who carry the most institutional knowledge, who are often underutilized because no role has ever honored what they know at the right level.

The evolution of the system created the domain analyst. The organization just hasn't recognized it yet.

---

## The Core Model

<!-- DOC 1: World View -->

The Era 3 definition of an application is the foundation of the model:

> An application captures intent from any participant who honors its contract, enforces rules, produces an outcome, publishes that outcome as an event for the ecosystem to react to, and subscribes to external events that shift its own context and available actions.

Each element carries distinct meaning:

- **Intent** lives at the boundary — what the participant expressed, raw and uninterpreted
- **Rules** are the domain — what is valid, what is allowed, what must follow from what
- **Outcome** is the meaningful result — the loan is approved, the application is declined, the review is required. A business meaningful state change that completes or advances the intent.
- **Published event** is the deliberate communication of that outcome to the ecosystem — other participants react or ignore as they see fit
- **Subscribed events** are the external signals the application monitors — outcomes from other participants that shift what this application knows and what actions are available

These concerns change for different reasons. Intent changes when UX changes. Rules change when the business changes. Events change when the ecosystem changes. Tangling them means every change touches everything.

---

## The Actor Model

<!-- DOC 1: World View — concept only -->

The fundamental primitive is an **participant with a boundary**.

A participant:

- Receives intent from its boundary
- Applies its own rules to determine what is valid and what must follow
- Consults other participants when it cannot fulfill intent alone
- Produces effects, some of which are published as events into the ecosystem
- Subscribes to external events that shift its context and available actions

There is no privileged center. No layer that is more "real" than another. What looks like layers or hierarchy is a perspective imposed from outside — not a property of the system itself.

---

## The Ecosystem Event Space

<!-- DOC 1: World View — concept only -->

Participants interact in a shared event space. Two flows exist simultaneously:

**Inbound (reactive):** World publishes → participant subscribes → context updates → available intent shifts

**Outbound (proactive):** Participant expresses intent → rules applied → effects produced → some effects published as events → world reacts

This makes every participant a **peer in an event space** — not a terminal endpoint. They sit in the middle of a flow, consuming from the world and contributing back to it.

---

## 
## Relative Architecture — Everything is Relative

<!-- DOC 1: World View -->

The model is **self-similar at every scale**. This is what makes it _relative_:

- Zoom in — inside an application you see participants collaborating across an event space
- Zoom out — that entire application is just one participant in a larger ecosystem

The same primitives repeat at every level:

- A function inside a service
- A service inside an application
- An application inside an enterprise
- An enterprise inside an industry ecosystem

Every participant's view of the system is relative to where it stands. What looks like infrastructure from one perspective is someone else's entire domain. There is no absolute top, no absolute bottom, no privileged center.

---

## Proactive Architecture — The Loan Application

<!-- DOC 1: World View — scenarios; move key observations to DOC 2 -->

The most common form of ecosystem interaction is proactive — a participant expresses intent and the ecosystem orchestrates toward an outcome. The loan application is a natural example because everyone understands it and it touches every concept in the model simultaneously.

---

**Process: Consumer Loan Application**

_Initiating participant:_ Consumer — intent to borrow

_Process owner:_ Loan Origination — accountable for the outcome from first intent to final decision

_Outcome:_ Approved, Declined, or Manual Review Required

---

**Scenario: Approved**

Consumer submits application → **Loan Origination** receives intent, owns the process from here

Context assembled once by Loan Origination — consumer identity, requested amount, supporting information — passed forward to each participant as needed. No participant calls back to a central record to assemble what they need.

Loan Origination consults → **Identity Verification** — is this person who they claim to be → `IdentityVerified` returned

Loan Origination consults → **Credit Assessment** — what is their credit position → `CreditAssessmentCompleted` returned

Loan Origination consults → **Regulatory Compliance** — does this loan meet lending rules → `ComplianceConfirmed` returned

Loan Origination applies its own underwriting rules → decision: approved

Loan Origination publishes `LoanApproved` → process ownership ends here

Ecosystem reacts independently:

- **Loan Funding** domain is subscribed → begins disbursement process
- **Consumer Notification** domain is subscribed → notifies consumer of approval and terms
- **Audit Trail** domain is subscribed → records the complete decision trail
- **Regulatory Reporting** domain is subscribed → files required disclosures

Consumer surface updates → journey complete

---

**Scenario: Declined**

Same process through Credit Assessment → poor score returned

Loan Origination applies underwriting rules → decision: declined

Loan Origination publishes `LoanDeclined`

Ecosystem reacts:

- **Consumer Notification** notifies consumer with reason
- **Audit Trail** records decision
- Funding domain receives no event — takes no action

---

**Scenario: Manual Review Required**

Credit Assessment returns borderline score

Loan Origination applies underwriting rules → cannot decide automatically → publishes `ManualReviewRequired`

**Operator surface** receives event → review item appears in credit processor's queue with full application context and assessment

Process pauses — waiting for human intent

Credit processor reviews → approves or declines → intent published back into the ecosystem

Process resumes from human decision → follows approved or declined path above

---

**Key observations:**

**Orchestration is visible.** Loan Origination owns every decision point. The sequence is explicit. Accountability is unambiguous. If something goes wrong the owner is clear.

**Each participant does one thing.** Credit Assessment assesses credit. It has no knowledge of what comes before or after it. Identity Verification verifies identity. Regulatory Compliance checks compliance. Each is sovereign in its own concern.

**Context travels with the process.** No participant calls back to a central record mid-process. Loan Origination assembled what was needed and passed it forward.

**The proactive outcome triggers choreographed reactions.** Once `LoanApproved` is published Loan Origination's accountability ends. The ecosystem reacts independently. Loan Origination does not orchestrate funding or notification — it publishes the outcome and trusts the ecosystem to respond.

**Three scenarios share one process trace.** The happy path, the decline, and the manual review are all expressions of the same process. The branching is owned by Loan Origination at each decision point — not distributed across participants.

---

## Reactive Architecture — The Earthquake Example

<!-- DOC 1: World View -->

The ecosystem doesn't just fulfill intent. It maintains coherence with a changing world.

**Trigger:** External feed publishes `NaturalDisasterDetected` — major earthquake, San Francisco Bay Area

1. **Risk Management** is subscribed → applies its rules → publishes `RegionalLendingRiskElevated`
2. **Loan Origination** is subscribed → flags incoming applications, holds in-flight applications → publishes `LoanApplicationHeld` per affected application
3. **Consumer Notification** is subscribed → notifies affected consumers
4. **Operator Surface** receives held events → credit processors see updated queue
5. **Regulatory Compliance** is subscribed → confirms or flags suspension against fair lending obligations
6. **Audit Trail** receives everything throughout

**Key observations:**

- No orchestrator — each domain reacted independently according to its own rules
- No domain reached into another domain's state
- The coordinated business response emerged from local reactions to shared events
- Time is visible — a subsequent `RegionalLendingRiskNormalized` event resumes held applications

---

## Functional Domains vs Data Domains

<!-- DOC 1: World View -->

Most enterprises organize around **data ownership** — the customer database, the order table. Domains are defined by what they store. Integration is defined by who reads whose data.

The model demands **functional domains** — organized around capability and process ownership. Data becomes a consequence of process, not the definition of the domain.

Benefits:

- Maps naturally to a team with clear accountability
- Integration becomes contract-based collaboration, not data access
- Failure accountability is unambiguous — the process has a home
- Domains can evolve independently without coordinating every change

---

## Business Process Ownership

<!-- DOC 1: World View -->

A business process has a **home** — the participant that owns the outcome, holds in-flight state, and is accountable for completion.

When consulting another participant:

- You are requesting a contribution to your process
- The other participant fulfills their part according to their own rules and returns
- Accountability never leaves the owning domain
- The consulted participant may itself consult multiple others — this is invisible to you by design

**Business process integration** is the discipline of coordinating across ownership boundaries without violating them.

---

## The Technical Platform

<!-- DOC 1: World View — concept only; detail to DOC 2 -->

Within an organization, the same model applies — but physical separation no longer enforces boundary discipline. The platform must do it instead.

**Infrastructure primitives:**

- **Bus** — publish, subscribe, done. Producers don't know consumers. Consumers don't know producers.
- **Gateway** — unified entry point for request/response. Callers invoke capability by name, not by location.
- **Service registry** — resolves capability to implementation at runtime
- **Identity and trust fabric** — baked into infrastructure, not bolted onto each domain
- **Stub/mock infrastructure** — any dependency can be stubbed against its contract, enabling domain-independent development

The platform makes the right thing the easy thing. Boundary discipline is enforced by infrastructure, not by policy.

---

## User Roles — Consumer vs Operator

<!-- DOC 1: World View -->

"User" is not a monolith. Two fundamentally different participants share the same domain:

**Consumer** — has a goal external to the application. The application is a means to an end. Their experience is a journey toward an outcome. Episodic engagement, limited visibility, limited control.

**Operator** — has a goal internal to the application. Their job _is_ the process. Continuous engagement, full pipeline visibility, manages exceptions, makes judgment calls the rules couldn't anticipate.

Same domain. Same events. Completely different participants with different vocabularies, different views of state, and different trust relationships.

Treating them as the same participant with different permissions misses something fundamental — they are different surfaces of the same domain, each attached to the domain's event space in their own way.

---


## The Ecosystem of Participants

<!-- DOC 1: World View -->

At the largest scale each organization is a participant in a broader ecosystem of participants. Your organization exposes capabilities that others can consume, publishes events that others may react to, and consumes capabilities from others in return. From the outside your organization looks like a single participant with a contract — regardless of how sophisticated your internal ecosystem is.

External participants are **black boxes**. What lives behind their boundary is entirely their concern. It could be a single developer with a focused API. It could be a small business managing a targeted capability. It could be a large enterprise operating their own full internal ecosystem with hundreds of domains and their own technical platform. You don't know. You don't need to know. The contract is all that matters.

This is the model's most liberating property. You can reason about your ecosystem in isolation, treating everything outside your boundary as a black box with a contract. The fact that those black boxes range from trivially simple to enormously complex doesn't complicate your reasoning — it is hidden behind the boundary by design.

The business architect's first move for any new requirement is to look outward before looking inward. The broader ecosystem may already have what you need. Build vs buy vs partner is resolved at the capability map level — before any engineering conversation begins.

**The business architect is the organization's interface to the world** — ensuring that interface is clean, consistent, trustworthy, and composable in both directions.

---
---

## The Organizational Model

<!-- DOC 1: World View -->

### Three Lanes

The model demands three distinct lanes of responsibility. They can be inhabited by different people or the same person wearing different hats — but they are always distinct concerns.

**Business** — owns domain knowledge and authority. Knows what a loan approval means in the real world. Knows the rules, the edge cases, the regulatory constraints, the exceptions. Validates that contracts reflect reality. Signs off on process traces. Does not think naturally in terms of contracts and events — thinks in terms of what we do and how we do it.

**Product / Domain Architecture** — the middle lane. Listens to the business, understands the domain well enough to know when the business is being imprecise, and has enough technical literacy to translate that understanding into contracts engineering can act on without interpretation. Produces the capability map. Facilitates scenario testing. Accepts OpenAPI specs on behalf of the business. Reports to the business — not to IT. Their deliverable is contracts not features.

**Engineering** — given a precise contract, implements it with full technical sovereignty. Framework, language, data model, cloud infrastructure — all engineering decisions made inside the boundary. Does not make domain decisions. Does not interpret ambiguous requirements. Wants the contract and nothing else.

The handoff is the contract. Clean. Unambiguous. One direction.

**Business writes the contracts. Engineering implements them.**

---

### The C-Suite Expression

The three lanes naturally map to executive accountability:

**Chief Operating Officer** — the natural owner of the whole model. Already accountable for how the business operates. By owning all three lanes the COO eliminates the business-IT divide permanently. The capability map and process traces are a precise description of how the business runs — that belongs under operations, not under a separate IT organization.

**Chief Product Officer** — owns the middle lane at the executive level. Accountable for the fidelity of contracts to business reality. Owns the capability map as a strategic artifact. Their organization is the domain modelers and product managers working at the contract level. Oriented toward the business. Reports to the COO.

**CIO / CTO** — owns engineering. Accountable for implementation quality, platform reliability, delivery velocity, infrastructure cost. Given good contracts they have full sovereignty. No longer in the business of interpreting requirements or making domain decisions. Reports to the COO as a peer of the domain leads.

**Chief Domain Officer** — or equivalent. Owns the business knowledge itself. The subject matter experts who are the authority on what the business actually does. Validates that contracts reflect reality. Signs off on capability definitions. Makes this knowledge a deliberate strategic asset rather than tribal knowledge scattered across departments.

Within each domain all three lanes are represented — domain experts, product people modeling the contracts, engineering implementing them — all reporting through the COO's organizational structure, all aligned on the same contracts.

---

### The Business as an Ecosystem

At the highest level every company regardless of industry consists of three functions:

**Sales and Marketing** — generates intent from the market. Acquires customers. Creates demand. Produces the raw material that operations acts on. Publishes events into the ecosystem — prospect acquired, customer converted, contract signed.

**Operations** — the ecosystem itself. Captures intent, applies business rules, orchestrates processes, coordinates capabilities, produces outcomes. The capability map, the process traces, the domain contracts, and the three lanes all live here.

**Finance** — the ledger of record for outcomes. Every meaningful business event that operations produces has a financial expression. Revenue recognized. Cost incurred. Liability created. Asset acquired. The general ledger is a specialized subscriber to the operations event space — capturing the financial dimension of every outcome.

The flow is clean:

**Sales and Marketing publishes intent → Operations orchestrates and produces outcomes → Finance records the financial expression of those outcomes**

Everything ends up in the general ledger because every business outcome has financial meaning. Finance doesn't need to understand the operational process. It subscribes to the outcomes and records them.

---

### External Actors Are Complete Businesses

Every external participant your operations ecosystem relies on — the credit bureau, the loan approval company, the payment processor — is itself a complete business with its own sales and marketing, its own operations ecosystem, its own general ledger.

From your ecosystem they look like a single capability node with a contract. From their perspective they are a complete business operating their own relative architecture. They have customers, processes, and financial outcomes of their own.

This scales infinitely. Every node in your ecosystem that is an external participant is a complete business. Every node in their ecosystem is a complete business. The ecosystem of ecosystems extends as far as the economy itself.

The economy is an ecosystem of businesses — each with sales and marketing generating intent, operations producing outcomes, and finance recording the financial expression of those outcomes. All connected through contracts, events, and trust relationships.

---

### The Ultimate Competitive Advantage

The COO who understands this model sees their operations ecosystem as a node in a larger economic ecosystem. They govern their contracts with the same discipline they expect from external partners. They make their capability map legible to partners and customers as well as internal teams.

That COO is not just running a business efficiently.

They are positioning their business as a **platform that others want to connect to**.

A business that can articulate precisely what it does, what it promises, and what it publishes to the world — in a form that is stable, trustworthy, and composable — becomes a preferred participant in every ecosystem it participates in.

That is the organizational expression of relative architecture made real.

---

## Summary Statement

<!-- DOC 1: World View -->

An application is an ecosystem of participants — human or system — each with a distinct set of concerns. The ecosystem has no privileged center. Layers are a perspective, not a property. The fundamental primitive is the participant and its boundary.

Time is real. Trust is federated. Process ownership is unambiguous. Identity travels as assertion, not credential.

The architecture's job is to honor the boundaries between participants, keep the contracts at those boundaries honest, and let each participant be coherent on its own terms.

Everything else is an implementation detail in service of that.
