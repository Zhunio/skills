---
name: software-engineering
description: 'Use for software design, implementation, refactoring and review. Favor understandable domain code, cohesive modules, small interfaces and proportionate verification.'
---

# software-engineering

Make code understandable without requiring the user to reconstruct its architecture.

## Why

- Passing tests do not prove understandable design.
- More classes and files do not guarantee better structure.
- Useful abstractions emerge during implementation.

## Rules

- **Encapsulation:** keep a concept's state and rules behind a controlled interface.
- **High cohesion, low coupling:** group related behavior; minimize dependencies between modules.
- **Domain modeling and ubiquitous language:** use consistent business terms and intention-revealing names.
- **Abstraction and information hiding:** expose meaningful operations; hide implementation details.
- **Separation of concerns:** keep business decisions separate from infrastructure mechanics.
- **Modularity and emergent design:** refine modules and submodules as responsibilities become clear.
- **Dependency injection:** provide stable dependencies and configuration during setup.
- **SOLID:** apply the principles where useful, without inventing extra layers.
- **DRY:** deduplicate knowledge and business rules, not simply matching syntax.
- **KISS and YAGNI:** use the simplest adequate design; build only what current needs require.

## Example

**User request:**

> Read a PDF and render its pages for AI review, use the findings to annotate the PDF, save the annotated copy and return the findings.

**Expected code:**

```javascript
async reviewPdf(source, destination) {
  const pdf = await this.pdfs.read(source);
  const pages = await this.pdfs.render(pdf);
  const findings = await this.ai.review(pages);
  const annotated = await this.pdfs.annotate(pdf, findings);
  await this.pdfs.write(annotated, destination);
  return findings;
}
```

## Testing and review

- **Testing:** use risk-proportionate tests to inform module boundaries; prefer focused, co-located suites and TDD when useful.
- **Review:** review the completed change against the specification and standards, fix material findings and recheck affected behavior.

## Before completion

- Can a human follow the workflow and understand the names?
- Is related behavior cohesive, with unnecessary complexity hidden or removed?
- Are behavior, safeguards, verification and remaining limitations accounted for?
