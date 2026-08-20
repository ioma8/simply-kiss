---
name: simply-kiss
description: Simplify code for low cognitive load and local change. Use when the user asks to simplify code or reduce over-engineering, improve cohesion or coupling, review a scope for cleanups, mentions KISS or YAGNI, or wants new code written under these principles.
---

# Simply KISS

Write simple, obvious, maintainable code. Optimize for readability and low cognitive load, not cleverness or abstraction. Keep related logic cohesive and dependencies loosely coupled so changes stay local. Prefer explicit behavior and straightforward control flow; build only what today's requirements need. Tolerate small duplication when it is clearer than the wrong abstraction, split by responsibility rather than metrics, protect important behavior with tests, and use DRY, SOLID, design patterns, and architecture patterns only when they make the system easier to understand and change.

## Workflow

1. Inspect the requested files plus the direct callers, dependencies, tests, and configuration that determine their behavior. Identify the behavior that must remain unchanged.
2. Choose the branch:
   - For a review, rank concrete cleanups; edit only when asked.
   - For a change, apply every rule below to all new and modified code.
3. Implement the narrowest solution that passes the current requirements. Keep related changes in one cohesive area.
4. Protect important behavior with focused tests, run the relevant checks, and reread the diff. Finish only when every item in **Done** passes; otherwise report the failed item and its risk.

## Rules

### 1. Simple & Obvious

- Before adding a type, layer, extension point, or dependency, name the current requirement it serves. Omit it when no current requirement exists.
- Prefer direct calls, obvious names, early returns, and explicit parameters or constructor dependencies.
- Make state changes, side effects, failure paths, I/O, and expensive work visible in the main flow.
- Extract an abstraction only when it hides external complexity or shared business knowledge that must stay synchronized.
- Keep similar-looking code separate when the concepts can evolve independently; small clear duplication is acceptable.
- Prefer language, platform, and repository-native features before custom infrastructure.
- Profile before local optimization. During design, count network calls and distributed coordination.
- Use DRY, SOLID, patterns, and architecture styles only when they reduce the knowledge needed to change the system.

### 2. Cohesive & Decoupled

- Place behavior with the domain it serves; keep unrelated domains independent.
- Keep dependencies small, explicit, directional, and free of cycles.
- Choose boundaries that let a typical feature change stay inside one module and its tests.
- Split by responsibility or reason to change, not by arbitrary size.
- Aim for functions under ~30 LOC and files under ~300 LOC. At ~50/~500, inspect whether a split improves cohesion and local reasoning. At 100+/1,000+, record a conscious justification. Never split merely to satisfy LOC limits.

Apply simplicity first: a lower LOC count is a regression when it scatters one behavior across more concepts.

## Examples

- **Current requirement:** For invoice email, start with `await email.sendInvoice(invoice)`. Add channel strategies when another real channel reveals what varies.
- **Pass-through layers:** Prefer `findById`, an explicit missing-user check, and `return user` over a query → handler → strategy → port → adapter chain unless each layer owns policy or a boundary.
- **Useful abstraction:** `payments.charge(customer, amount)` hides gateway protocol details. `formatUserName(name)` needs no formatter factory, type enum, and strategy.
- **Explicit wiring:** Prefer `new BillingService(invoices, payments)` over discovery annotations or hidden globals when explicit construction is practical.
- **DRY:** Keep `calculateInvoiceTotal` and `calculateOrderTotal` separate when they change for different business reasons, even if several lines match.
- **Cohesion:** Keep invoice creation, charging, and persistence under `billing/` when that lets billing changes stay local; avoid scattering them across global `controllers/`, `services/`, and `utils/` buckets.
- **Size:** Split an 80-line checkout function that mixes validation, pricing, payment, and receipts into those cohesive operations plus a short orchestrator. Keep a straightforward 28-line parser whole when helpers would fragment one algorithm.
- **Tests:** Before collapsing lookup layers, preserve observable behavior such as `loadUser(missingId)` raising `UserNotFoundError`. Avoid tests that only assert calls between the layers being removed.

## Done

- The patch contains no speculative functionality or unused dependency.
- The main behavior is explicit and locally understandable.
- Every retained abstraction removes more knowledge than it adds.
- Related changes are co-located; new dependencies remain clear and acyclic.
- Important behavior has refactor-safe tests and relevant checks pass.
- Size thresholds were evaluated without metrics-only splitting.

For a review, report each worthwhile finding as: location — what to remove or simplify — smaller replacement — behavior or tradeoff to preserve. Say when the scoped code is already appropriately simple.
