# Service Decomposition Should Not Be Your First Call

*Every boundary has a cost, and most of them look free on a diagram.*

A boundary drawn on a diagram costs nothing. In production it costs an API
contract, a network hop, a pipeline, a pager rotation, and a new way for
the system to fail. Every architecture has costs, including the modular
monolith. The question is whether you need the additional cost a service
boundary brings. Pay it for a need, not a want.

Teams often conflate three different ideas here. The goal isn't perfect
architecture. It's business value, engineering productivity, operational
reliability, and long-term maintainability.

## Nirvana Boundaries

The architecture you'd design in a world without constraints: perfectly
aligned bounded contexts, complete data ownership, and independent
deployment, scaling, release cycles and ownership.

**Good for:** a north star and a shared vocabulary.
**Dangerous as:** a plan. It ignores organizational reality and delivery
timelines, optimizes for hypothetical futures, and adds complexity before
there's evidence it's needed.

*"How would we build this if there were no constraints?"*

## Apparent Boundaries

Boundaries that look right on a diagram but aren't supported by how the
system behaves. They usually follow layers, technologies, or the org
chart, and they're justified by principle rather than evidence. They're
built because someone wanted them, not because the system needed them.

**Symptoms:** constant synchronous calls, shared transactions or data,
changes that routinely span services, and services that deploy
independently but can't evolve independently.

**How to test for one:**

- Do commits and PRs repeatedly touch both sides of the boundary?
- Do releases have to go out in lockstep?
- Do both services read and write the same tables?
- Does one service's outage take the other down?
- Can a team change its service without asking anyone?

If the answers point the wrong way, you have distributed complexity
without distributed benefits: more latency, harder debugging, slower
delivery, and higher cognitive load.

*"How should this architecture look?"*

## Pragmatic Boundaries

Every boundary has a cost, so a capability shouldn't become a service
just because it's logically distinct. It should become one only when it
is truly independent, the separation has measurable value, and the
benefits exceed the additional costs. The test is need, not want.

### Evidence of need (true independence)

- **Consumers:** different release cadences or SLAs
- **Scaling:** significantly different load characteristics
- **Availability:** one must stay up when the other fails
- **Security/compliance:** different regulatory or data-handling
  obligations
- **Rate of change:** one changes often while the other stays stable
- **Ownership:** a team can build, deploy and support it without
  continuous coordination

### What a service boundary adds

These are the costs on top of what the same capability costs as a module
in a monolith.

- **API:** a method call becomes a contract that must be designed,
  documented, versioned, tested, secured and supported indefinitely.
- **Communication:** latency, serialization, retries, timeouts,
  authentication and authorization.
- **Inter-service call cost:** every call now consumes network, gateway,
  mesh and serialization resources, and at volume that shows up on the
  bill.
- **Failure modes:** partial failures, retry storms, cascades, duplicate
  delivery, event ordering, and eventual consistency.
- **Operations:** CI/CD, monitoring, alerting, runbooks, patching and
  on-call. A boundary that saves five minutes of coding but creates years
  of operational overhead is rarely a good trade.
- **Development:** local environments, integration testing, release
  coordination and cognitive load.

### Counter-triggers (reasons not to split)

- Shared transactional invariants
- Tight data coupling
- Constant cross-team coordination
- Immature observability, deployment and incident practices
- No measurable benefit

### Design principle: coarse by default, split on evidence

Start with a **modular monolith**: logical boundaries first, clear
ownership, and enforced module interfaces. It isn't free. You pay in
discipline, shared release cycles, a shared blast radius, and scaling as
a unit. But it doesn't carry the extra inter-service call cost, added
latency and operational overhead of a service boundary, and those extras
are where the real bill comes from.

A well-factored module can be extracted later once the evidence shows up,
but merging services back together is far more expensive. Deferring the
split preserves your options.

Introduce a deployable boundary only when the need is demonstrable: the
capability is truly independent, the benefit is measurable, and it
outweighs the added cost. If the honest reason is "it feels cleaner,"
that's a want.

## Summary

| Boundary  | Optimizes for            | Question                                                         |
|-----------|---------------------------|-------------------------------------------------------------------|
| Nirvana   | Architectural purity      | How would we design this without constraints?                    |
| Apparent  | Architectural appearance  | How should this look on a diagram?                                |
| Pragmatic | Outcomes                  | Do we need this boundary, and do the benefits exceed the added cost? |

Every architecture has costs. Accept the extra cost of a service boundary
only for a need, never a want.
