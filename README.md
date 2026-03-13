# Applied Knowledge

**W3C Context Graphs Community Group**

**Committee Chair:** Audrey Depeige

---

## Background

> This section is shared across all Context Graphs Community Group committees. For the standalone version, see [Context Graphs: Why This Exists](https://github.com/W3C-Context-Graph-Community-Group/Charter/blob/main/why_this_exists.md).

### The scope boundary

In 1948, Shannon gave us a formal theory of communication. It is one of the most successful theories in the history of science. His model is complete, elegant, and proven: given a shared codebook — a mapping between symbols and their referents — he tells you everything you need to know about transmitting information reliably.

Every major framework the industry builds on inherits this foundation. Expected utility theory, Bayesian inference, dynamic programming, active inference, Decision Intelligence — each presupposes that the meaning of inputs is shared between the system that produced the data and the model that consumes it. Shannon built this into the foundation. It is not a caveat in his theory. It is an axiom.

Shannon is correct. We are not challenging his work. We are identifying where its assumptions stop holding.

The entire industry operates in the regime where Shannon's assumption doesn't hold — open systems, multiple codebooks, no single authority — and has been using closed-system instruments to certify correctness in an open-system world. The instruments report success because they can't see the thing that's failing. That's not a flaw in the instruments. It's a scope mismatch between the instrument and the environment.

### A simple example

A system in Singapore sends a date field to a system in London:

```
time: 03/01/2026
```

Shannon's channel delivers it perfectly — zero bit error rate, zero conditional entropy, maximal mutual information. The receiver has the string exactly as the sender sent it.

But is this March 1st or January 3rd?

The sender uses MM/DD/YYYY. The receiver uses DD/MM/YYYY. For any day from the 1st through the 12th — roughly 40% of all dates in a year — the month and day positions are interchangeable. Both sides parse a valid date. Both proceed with confidence. One of them is wrong. Shannon reports zero uncertainty. The boundary carries a bit of uncertainty that no standard instrument detects.

Now add a second layer: the sender defines "time" as the moment the order was *placed*. The receiver defines "time" as the moment the order was *received*. Same label, different events. That's another bit — meaning uncertainty — also invisible to Shannon, also invisible to every downstream framework.

And a third: the sender is in Singapore (UTC+8), the producing system is hosted in New York (UTC−5), and the receiver is in London (UTC+0). Three reference frames, three possible calendar dates, no shared one. That context was never surfaced.

One field. Three bits of boundary uncertainty. Shannon reports zero. Every system reports success. Every decision model downstream receives a clean input and makes a confident decision — on a value that is structurally ambiguous, semantically misaligned, and contextually ungrounded.

### Why agents make it urgent

Humans at system boundaries have always performed an informal, undocumented version of coherence checking. They ask colleagues what a field means. They verify timezone assumptions. They pick up the phone. This is slow, fragile, and dependent on institutional memory that leaves when people leave — but it works, approximately, because humans recognize when something feels wrong and know how to ask.

Agents skip this entirely. They infer and proceed. The more capable the agent, the more convincingly it proceeds with a wrong interpretation. An agent crossing ten boundaries in a single query — ten tools, ten APIs, ten systems — has no mechanism for detecting that the meaning of a field shifted at hop three and every downstream decision is now grounded in a misinterpretation. If the interpretation flipped between hops — March 1st at one boundary, January 3rd at the next — the error propagates into every downstream decision without a trace.

The problem compounds. Each unmeasured boundary multiplies the possible misalignment states — five boundaries in a routine pipeline produce over 32,000 configurations, all invisible to standard instruments. The gap that decision theory always carried is now operationally exposed at machine speed, across organizational boundaries, with no human in the loop to catch the silent failure.

This is not a data quality problem. The data is correct. It is not a schema error. The schema validated. It is a structural property of boundaries between systems that operate under different assumptions — and it is the default condition whenever independently designed systems interact.

A natural response to the date example is: "just standardize on ISO 8601" or "enforce a single format in the schema." Inside a closed system — one design authority, one team, one governance structure — that works, and you should do it. The date format is a trivial problem when you control both sides of the boundary. The Context Graph is not built for that case. It is built for the case where you don't control the other side — where the system in Singapore was designed by a different team, under different conventions, and you have no authority to change it. You only discover the mismatch when data crosses the boundary. The protocol exists for the boundaries you don't own, connecting systems you didn't design, at runtime, when the only option is to detect the divergence and decide what to do about it.

### Toward an information-theoretic view of Context

The missing quantity is **context**: the information required to resolve ambiguity in meaning, structure, or data at a specific boundary. Context is not metadata. It is not a linguistic property of terms. It is the information-theoretic gap between what a sender encodes and what a receiver requires in order to act correctly on that encoding. When a receiver doesn't know whether `03/01/2026` is MM/DD or DD/MM, the missing information is context. When a receiver doesn't know whether "time" means order placement or order receipt, the missing information is context. When a receiver doesn't know which timezone produced the value, the missing information is context.

Context is structured uncertainty at a system boundary. Resolving it — surfacing what was missing, recording what was asked, documenting what was decided — is what collapses the ambiguity. Leaving it unresolved is what allows misalignment to compound silently across every downstream boundary.

Coherence is the achieved state when systems at a boundary can resolve their own uncertainty about meaning, structure, and data while producing a shared record that every other node in the interaction can verify.

---

### Committee Purpose: Does This Work?

The other committees define what the protocol measures, how it's serialized, and how decisions act on it. This committee asks the harder question: **does any of it actually help?**

A specification that is formally correct but produces no measurable benefit for users or organizations has failed. A protocol that detects boundary misalignment but costs more to run than the misalignment costs to absorb is not useful. A standard that practitioners cannot adopt without restructuring their existing systems will not be adopted.

Applied Knowledge is the bridge between the specification and the real world. This committee's job is to ground the group's work in use cases that practitioners recognize, design evaluations that test whether the protocol delivers on its claims, and measure whether the people and organizations that use it are better off.

This committee does not develop technical standards — that is the remit of Semantic Alignment and Syntax & Serialization. It evaluates what those committees produce against the question that matters most: does this make decisions better, and can we demonstrate it?

---

## Responsibilities

### Use cases

Source real-world scenarios where codebook misalignment at system boundaries produces measurable harm. Each use case should identify the boundary, the systems on each side, the misalignment (meaning, structure, context, or some combination), and the cost — in time, money, risk, or failed outcomes — when the misalignment goes undetected.

Good use cases come from any industry where independently designed systems exchange data: finance, healthcare, logistics, government, research, multi-agent AI workflows. They must be broad enough to demonstrate cross-industry relevance and specific enough to be testable.

### Evaluation design

Design tests that can answer: did the Coherence Protocol detect a misalignment that would otherwise have gone undetected? Did the resolution (Act, Ask, or Halt) lead to a better outcome than the default of silently proceeding? What was the cost of running the protocol versus the cost of not running it?

This includes defining what "better outcome" means in each use case — reduced error rate, faster detection of misalignment, lower rework cost, fewer downstream failures, improved auditability — and designing measurements that are reproducible and honest.

### Benefit measurement

Develop a framework for quantifying the value the protocol produces. This is not marketing — it is the evidence base that determines whether the specification deserves to advance from Community Group to Working Group status. The framework should be applicable across industries and use cases, producing comparable results even when the domains differ.

### Deployment guidance

Produce practical guidance for implementers: how to adopt the protocol in common architectures (API gateways, data pipelines, agent frameworks, query systems), what the integration cost looks like, and where the highest-value entry points are for organizations encountering this problem for the first time.

---

## Design Principles

The same principles that constrain the technical specification constrain how we evaluate it.

**Self-similarity.** If the protocol works at one boundary, the evaluation framework should work at that boundary too. If the protocol works across a chain of boundaries, the evaluation framework should scale the same way. One evaluation approach, applicable at every level.

**Factorization.** One evaluation framework across industries, not a bespoke assessment per domain. The use cases vary. The structure for measuring benefit should not.

**Compression.** If a practitioner understands one use case evaluation, they should understand how to evaluate their own. The framework should be learnable, not just executable.

---

## Current Status

**Phase:** Pre-draft.

**First deliverable:** A set of documented use cases — at least three, from different industries — with identified boundaries, misalignment types, and measurable costs. These become the test cases for every other committee's output.

**Chair:** Audrey Depeige — [contact via Community Group channels]

**Community Group Chair:** Ron Itelman

---

## Links

- [W3C Context Graphs Community Group](https://www.w3.org/community/context-graphs/)
- [Community Group Charter](https://github.com/W3C-Context-Graph-Community-Group/Charter/blob/main/README.md)
