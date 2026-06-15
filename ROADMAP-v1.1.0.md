# CIRPASSX v1.1.0-draft Roadmap

> **Status:** Planning | **Target:** v1.1.0-draft | **Date:** 2026-06-16
> **Scope:** cirpassx.org (the STANDARD only — no registry service dependencies)

---

## Executive Summary

Bring CIRPASSX from v1.0.0-draft to a complete, EU-DPP-interoperable v1.1.0-draft. This is a **minor version bump** with backward compatibility preserved where possible. Breaking changes are explicitly flagged.

**Critical constraint:** cirpassx.org = the open standard. cirpassx.com = the registry service. Zero dependency on .com in normative requirements.

---

## Background: Current State (v1.0.0-draft)

The existing standard defines:
- JSON-LD W3C Verifiable Credentials with schema.org + CIRPASSX context
- Required: identifier (CIRPASSX_ID UUID), name, category, itemCondition
- Recommended: GTIN, GS1_DIGITAL_LINK, serial
- Classification: UNSPSC/ECLASS/GPC/HS/CPV
- Condition grades A–F, functional/cosmetic status
- Repairability score per EN 45554:2020
- Lifecycle events mapped to EPCIS 2.0 bizSteps
- Sustainability metrics (CO₂, embodied carbon, recyclability)
- JSON Schema validation; interoperability section (EU DPP "compatible", UNTP, EPCIS)

---

## Version Strategy

**v1.1.0-draft** (not v2.0.0) because:
- GS1 Digital Link promoted to **STRONGLY RECOMMENDED** (not REQUIRED) — avoids breaking change
- CIRPASSX_ID remains stable internal identifier
- All v1.0.0 documents remain valid in v1.1.0
- New features are additive

If GS1 DL must be REQUIRED → that becomes v2.0.0-discussion, separate from this roadmap.

---

## Package Breakdown (4 Deliverables)

### Package A: Identifiers & Data Carriers
**Priority:** 1 (foundation for everything else)
**Files touched:** `specification.html`, `schema/v1/product.schema.json`, `context/v1/context.jsonld`

| Task | Description | Breaking? |
|------|-------------|-----------|
| A1 | Promote GS1 Digital Link to **STRONGLY RECOMMENDED** primary resolvable identifier | No |
| A2 | Define unbranded/legacy identifier scheme (when no GTIN exists) | No |
| A3 | Add normative data carrier section: QR primary, NFC/RFID optional | No |
| A4 | Define carrier encoding (GS1 Digital Link URI) and resolution path | No |
| A5 | Update schema: new identifier fields, validation rules | No |
| A6 | Update context.jsonld: GS1 DL IRI mappings | No |

**Acceptance criteria:**
- A third party can resolve a product via GS1 Digital Link without cirpassx.com
- Legacy items have a conformant identifier path
- Data carrier rules are consistent with ESPR expectations

---

### Package B: EU DPP Ontology Mapping & Registry Linkage
**Priority:** 2 (semantic alignment)
**Files touched:** `specification.html`, `context/v1/context.jsonld`, NEW `mapping/eu-dpp-mapping.md`

| Task | Description | Breaking? |
|------|-------------|-----------|
| B1 | Create normative crosswalk: CIRPASSX field ↔ EU DPP Core Ontology term | No |
| B2 | Add UNTP term mappings where applicable | No |
| B3 | Add IRI aliases in @context for EU-DPP semantics | No |
| B4 | Add EU Central Registry linkage fields (data model only) | No |
| B5 | Define registration identifier, operator identifier, registry resolver reference | No |
| B6 | Mark all BINDING TBD points for still-draft EU specs | No |
| B7 | Create machine-readable mapping table + human-readable appendix | No |

**Acceptance criteria:**
- Every CIRPASSX field maps to EU DPP Core Ontology or is marked reuse-profile extension
- No fabricated finalized EU details; all TBD points clearly marked
- Context.jsonld includes EU DPP IRI aliases

---

### Package C: Exchange API & Verification/Provenance
**Priority:** 3 (operational contracts)
**Files touched:** `specification.html`, NEW `exchange-api.html` (or section), `schema/v1/product.schema.json`

| Task | Description | Breaking? |
|------|-------------|-----------|
| C1 | Add Exchange Interface section (RESTful contract) | No |
| C2 | Define resources, operations (CRUD + append-event) | No |
| C3 | Define JSON-LD media types, query/filter, pagination, versioning, error model | No |
| C4 | Align to EN 18222/18223 patterns (mark as BINDING TBD) | No |
| C5 | Define verification & provenance model | No |
| C6 | Classify fields: self-declared vs third-party-verified | No |
| C7 | Require environmental claims carry traceable source/method reference | No |
| C8 | Define VC proofs, issuer accreditation, evidence/source links | No |

**Acceptance criteria:**
- Exchange contract is implementable by any third party
- Verification model supports ESPR "verified, not self-declared" requirement
- No dependency on cirpassx.com for verification workflows

---

### Package D: Access Model, Conformance, Governance
**Priority:** 4 (framework)
**Files touched:** `specification.html`, NEW `conformance-classes.md`, `CHANGELOG.md`

| Task | Description | Breaking? |
|------|-------------|-----------|
| D1 | Define normative access tiers (public/consumer, economic operator, authority, recycler) | No |
| D2 | Require consumer-facing data accessible free and without login | No |
| D3 | Define selective disclosure and data minimization | No |
| D4 | Define conformance classes: Core, EU-DPP-Interoperable, Reuse Profile | No |
| D5 | Make conformance classes machine-testable (criteria defined, test suite out of scope) | No |
| D6 | Define extension/profile mechanism (sector profiles) | No |
| D7 | Define multi-language field support (@language) | No |
| D8 | Add governance/licensing statement (open license, multi-party governance) | No |
| D9 | Update changelog v1.0.0-draft → v1.1.0-draft | No |
| D10 | Expand normative/informative references | No |
| D11 | Update examples to demonstrate all new features | No |

**Acceptance criteria:**
- Conformance classes are precise enough to be machine-tested
- Access model is neutral (no vendor-specific enforcement)
- Governance statement does not bake cirpassx.com into normative requirements

---

## BINDING TBD List (Draft EU Specs)

These depend on still-unpublished EU standards. Mark clearly in spec:

| Reference | Status | Impact |
|-----------|--------|--------|
| EN 18222 (DPP data exchange) | Draft | Exchange API alignment |
| EN 18223 (DPP interoperability) | Draft | Conformance testing criteria |
| EU Central DPP Registry implementing acts | Unpublished | Registry linkage fields |
| EU DPP Core Ontology (CIRPASS-2) | Draft | Ontology mapping |
| CEN/CENELEC JTC 24 final deliverables | In progress | Governance alignment |

---

## File Structure

```
cirpassxorg/
├── public/
│   ├── index.html                          # Landing (update version)
│   ├── specification.html                  # Main spec (all packages)
│   ├── exchange-api.html                   # NEW: Exchange API contract (Package C)
│   ├── context/
│   │   └── v1/
│   │       └── context.jsonld              # Updated (Packages A, B)
│   ├── schema/
│   │   └── v1/
│   │       └── product.schema.json         # Updated (Packages A, B, C)
│   ├── examples/
│   │   ├── minimal.json                    # Updated (all packages)
│   │   ├── led-panel.json                  # Updated (all packages)
│   │   └── full-v1.1.json                  # NEW: Complete example
│   ├── mapping/
│   │   └── eu-dpp-mapping.md               # NEW: Crosswalk (Package B)
│   ├── conformance/
│   │   └── conformance-classes.md          # NEW: Testable criteria (Package D)
│   └── CHANGELOG.md                        # NEW: Version history (Package D)
├── README.md                               # Update
└── LICENSE                                 # Unchanged
```

---

## PR Strategy

| PR | Branch | Contents | Merge Order |
|----|--------|----------|-------------|
| #1 | `feat/package-a-identifiers` | Package A | 1 |
| #2 | `feat/package-b-ontology` | Package B | 2 |
| #3 | `feat/package-c-exchange` | Package C | 3 |
| #4 | `feat/package-d-conformance` | Package D | 4 |
| #5 | `release/v1.1.0-draft` | Final integration, version bump | 5 |

Each PR includes:
- Updated specification.html section
- Updated schema/context if applicable
- Updated examples
- Tests verified (browse to smartinventory.test — wait, wrong project)
- **For this repo:** Verify by loading files in browser / JSON Schema validator

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Breaking change introduced | Medium | High | Review each PR for backward compatibility |
| Inconsistency between schema/context/spec | Medium | High | Cross-reference checklist per PR |
| cirpassx.com dependency leaks in | Medium | High | Explicit "registry service" escape hatch in normative text |
| EU spec changes before final | High | Medium | BINDING TBD markers + versioned references |
| Scope creep (too much in one PR) | Medium | Medium | Strict package boundaries |

---

## Acceptance Criteria (Final)

- [ ] Third party can implement using only .org artifacts, zero .com dependency
- [ ] Every CIRPASSX field maps to EU DPP Core Ontology or marked extension
- [ ] GS1 Digital Link is STRONGLY RECOMMENDED resolvable identifier
- [ ] Data carrier, exchange API, verification, access tiers each have normative section
- [ ] Conformance classes defined precisely enough for machine testing
- [ ] specification.html, context.jsonld, product.schema.json are mutually consistent
- [ ] No fabricated finalized EU details; all TBD bindings marked
- [ ] All examples demonstrate new features (GS1 DL, carriers, verification, registry linkage)
- [ ] CHANGELOG documents all changes from v1.0.0-draft → v1.1.0-draft

---

## Next Steps

1. **Review this plan** — Andreas approves/modifies
2. **Create Package A branch** — identifiers & data carriers
3. **Implement Package A** — spec + schema + context + examples
4. **PR → Review → Merge**
5. **Repeat for Packages B–D**
6. **Release v1.1.0-draft**

---

*Document generated by GoZero Agent, 2026-06-16*
*Based on Opus 4.7 second opinion and current v1.0.0-draft analysis*
