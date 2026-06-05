# CIRPASSX Specification

**Circular Product Passport Exchange Standard**

Version: 1.0.0-draft
Status: Draft
Date: 2026-06-05
Domain: https://cirpassx.org

---

## 1. Introduction

### 1.1 Purpose

CIRPASSX (Circular Product Passport Exchange) is an open data standard for exchanging product information between actors in the circular economy. The standard enables:

- Reuse shops, repair centers, and marketplaces to exchange product data
- Full product lifecycle tracking from manufacturing to end-of-life
- Compliance with EU Digital Product Passport (DPP) requirements
- Support for EU Right to Repair directive

### 1.2 Scope

This specification defines:

- JSON-LD data format for circular product passports
- JSON Schema for validation
- Condition grading scale
- Repairability scoring methodology
- Lifecycle event types
- Classification system mappings

### 1.3 Conformance

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

---

## 2. Data Model

### 2.1 Document Structure

A CIRPASSX document is a JSON-LD Verifiable Credential with the following top-level structure:

```json
{
  "@context": [...],
  "type": ["VerifiableCredential", "DigitalProductPassport", "CircularProductPassport"],
  "id": "urn:uuid:...",
  "issuer": {...},
  "issuanceDate": "...",
  "validFrom": "...",
  "validUntil": "...",
  "credentialSubject": {...}
}
```

### 2.2 Context Requirements

The `@context` array MUST include:

1. `https://www.w3.org/ns/credentials/v2` - W3C Verifiable Credentials
2. `https://schema.org/` - Schema.org vocabulary
3. `https://cirpassx.org/v1/context.jsonld` - CIRPASSX vocabulary

It SHOULD also include:

4. `https://vocabulary.uncefact.org/untp/` - UN Transparency Protocol

### 2.3 Identifier Requirements

The document `id` MUST be a UUID URN in the format:

```
urn:uuid:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

---

## 3. Product (credentialSubject)

### 3.1 Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `@type` | String | MUST be "Product" |
| `id` | URI | Unique product identifier |
| `identifier` | Array | Product identifiers (min 1) |
| `name` | String | Product name (max 200 chars) |
| `category` | Array | Classification codes (min 1) |
| `itemCondition` | Object | Condition assessment |

### 3.2 Recommended Fields

| Field | Type | Description |
|-------|------|-------------|
| `brand` | Brand | Product brand |
| `model` | String | Model name/number |
| `manufacturer` | Organization | Original manufacturer |
| `productionDate` | Date | Manufacturing date |
| `repairabilityScore` | AggregateRating | Repairability assessment |
| `repairInformation` | HowTo | Repair documentation |
| `lifecycleHistory` | ItemList | Lifecycle events |
| `sustainability` | PropertyValue | Environmental metrics |
| `hasCertification` | Array | Product certifications |
| `offers` | Offer | Pricing and availability |

### 3.3 Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `description` | String | Product description (max 2000 chars) |
| `conditionAssessment` | Review | Detailed assessment |
| `spareParts` | Array | Available spare parts |
| `usageHistory` | PropertyValue | Usage metrics |
| `material` | Array | Material composition |
| `warranty` | Array | Warranty information |
| `image` | Array | Product images |
| `subjectOf` | Array | Related documents |
| `weight`, `width`, `height`, `depth` | QuantitativeValue | Dimensions |
| `additionalProperty` | Array | Category-specific properties |

---

## 4. Identifiers

### 4.1 Required Identifier

Every product MUST have a CIRPASSX identifier:

```json
{
  "@type": "PropertyValue",
  "propertyID": "CIRPASSX_ID",
  "value": "550e8400-e29b-41d4-a716-446655440000"
}
```

### 4.2 Recommended Identifiers

| PropertyID | Description | Example |
|------------|-------------|---------|
| `GTIN` | GS1 Global Trade Item Number | "07350123456789" |
| `GS1_DIGITAL_LINK` | GS1 Digital Link URI | "https://id.gs1.org/01/..." |
| `SERIAL_NUMBER` | Manufacturer serial number | "SN-2019-ABC123" |

### 4.3 GS1 Digital Link

When available, products SHOULD include a GS1 Digital Link for global interoperability:

```json
{
  "@type": "PropertyValue",
  "propertyID": "GS1_DIGITAL_LINK",
  "value": "https://id.gs1.org/01/07350123456789/21/ABC123"
}
```

---

## 5. Classification

### 5.1 Category Codes

Products MUST include at least one classification using standard systems:

| System | Description | Example |
|--------|-------------|---------|
| `UNSPSC` | United Nations Standard Products and Services Code | "39111610" |
| `ECLASS` | European product classification | "27-27-25-04" |
| `GPC` | GS1 Global Product Classification | "10000043" |
| `HS` | Harmonized System (customs) | "8539.50" |
| `CPV` | Common Procurement Vocabulary (EU) | "31500000" |
| `CUSTOM` | Custom classification system | Any |

### 5.2 Example

```json
{
  "@type": "CategoryCode",
  "codeValue": "39111610",
  "codingSystem": "UNSPSC",
  "name": "Fluorescent luminaires"
}
```

---

## 6. Condition Grading

### 6.1 CIRPASSX Condition Scale

Every product MUST include a condition grade using the CIRPASSX 5-tier scale:

| Grade | Name | Description | Condition % |
|-------|------|-------------|-------------|
| **A** | Excellent | Like new, minimal signs of use | 90-100% |
| **B** | Very Good | Light wear, fully functional | 75-90% |
| **C** | Good | Normal wear, fully functional | 50-75% |
| **D** | Fair | Heavy wear, functional | 25-50% |
| **F** | For Parts | Not fully functional | <25% |

### 6.2 Functional Status Values

| Value | Description |
|-------|-------------|
| `fully_functional` | All features work correctly |
| `partially_functional` | Some features impaired |
| `not_functional` | Does not operate |
| `untested` | Functionality not verified |

### 6.3 Cosmetic Status Values

| Value | Description |
|-------|-------------|
| `like_new` | No visible wear |
| `minor_wear` | Light scratches, minimal marks |
| `moderate_wear` | Visible scratches and marks |
| `heavy_wear` | Significant cosmetic damage |

### 6.4 Example

```json
"itemCondition": {
  "@type": "OfferItemCondition",
  "name": "UsedCondition",
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "propertyID": "CIRPASSX_CONDITION_GRADE",
      "value": "B"
    },
    {
      "@type": "PropertyValue",
      "propertyID": "FUNCTIONAL_STATUS",
      "value": "fully_functional"
    },
    {
      "@type": "PropertyValue",
      "propertyID": "COSMETIC_STATUS",
      "value": "minor_wear"
    },
    {
      "@type": "PropertyValue",
      "propertyID": "CONDITION_SCORE",
      "value": 82,
      "unitCode": "P1"
    }
  ]
}
```

---

## 7. Repairability Score

### 7.1 Scoring Methodology

CIRPASSX repairability scoring is based on EN45554:2020 methodology. The overall score is 0-10 where 10 = maximally repairable.

### 7.2 Subscores

| Subscore | Weight | Description |
|----------|--------|-------------|
| `DISASSEMBLY_EASE` | 20% | Ease of disassembly (tools, fasteners, adhesive) |
| `SPARE_PARTS_AVAILABILITY` | 25% | Access to spare parts and delivery time |
| `REPAIR_DOCUMENTATION` | 15% | Service manuals, diagrams available |
| `PARTS_PAIRING` | 20% | Software locks or serial pairing |
| `RESET_CAPABILITY` | 10% | Factory reset capability |
| Cost Factor | 10% | Repair cost vs replacement |

### 7.3 Example

```json
"repairabilityScore": {
  "@type": "AggregateRating",
  "ratingValue": 7.2,
  "bestRating": 10,
  "worstRating": 0,
  "additionalProperty": [
    {
      "@type": "PropertyValue",
      "propertyID": "DISASSEMBLY_EASE",
      "value": 8
    },
    {
      "@type": "PropertyValue",
      "propertyID": "SPARE_PARTS_AVAILABILITY",
      "value": 7
    }
  ]
}
```

---

## 8. Lifecycle Events

### 8.1 Event Types

| Event | URI | EPCIS bizStep | Description |
|-------|-----|---------------|-------------|
| Manufacturing | `cirpassx:event/manufacturing` | `commissioning` | Product manufactured |
| First Sale | `cirpassx:event/firstSale` | `retail_selling` | Initial sale |
| Installation | `cirpassx:event/installation` | `installing` | Installed at location |
| Repair | `cirpassx:event/repair` | `repairing` | Repair performed |
| Maintenance | `cirpassx:event/maintenance` | `inspecting` | Maintenance service |
| Decommissioning | `cirpassx:event/decommissioning` | `decommissioning` | Removed from service |
| Salvage | `cirpassx:event/salvage` | `decommissioning` | Salvaged from demolition |
| Intake | `cirpassx:event/intake` | `inspecting` | Received by reuse actor |
| Assessment | `cirpassx:event/assessment` | `inspecting` | Condition assessed |
| Resale | `cirpassx:event/resale` | `retail_selling` | Sold as secondhand |

### 8.2 Event Structure

```json
{
  "@type": "Event",
  "additionalType": "https://cirpassx.org/v1/event/repair",
  "startDate": "2022-06-10T10:00:00Z",
  "name": "Repair",
  "description": "LED driver replacement",
  "organizer": {
    "@type": "Organization",
    "name": "Repair Service AB"
  },
  "additionalProperty": {
    "@type": "PropertyValue",
    "propertyID": "EPCIS_BIZ_STEP",
    "value": "urn:epcglobal:cbv:bizstep:repairing"
  }
}
```

---

## 9. Sustainability Metrics

### 9.1 Available Metrics

| Metric | Type | Description |
|--------|------|-------------|
| `estimatedRemainingLifeYears` | Decimal | Expected remaining useful life |
| `co2SavingsKg` | Decimal | CO2 avoided by reuse vs new |
| `embodiedCarbonKg` | Decimal | Carbon embedded in product |
| `wasteAvoidedKg` | Decimal | Waste prevented by reuse |
| `circularityScore` | Decimal (0-1) | Circularity indicator |
| `recyclabilityScore` | Decimal (0-1) | End-of-life recyclability |

### 9.2 Example

```json
"sustainability": {
  "@type": "PropertyValue",
  "propertyID": "CIRPASSX_SUSTAINABILITY",
  "value": {
    "estimatedRemainingLifeYears": 10,
    "co2SavingsKg": 45,
    "wasteAvoidedKg": 3.2,
    "circularityScore": 0.85
  }
}
```

---

## 10. Material Composition

### 10.1 Material Properties

| Property | Description |
|----------|-------------|
| `MASS_FRACTION` | Fraction of total mass (0-1) |
| `RECYCLED_CONTENT` | Fraction from recycled sources (0-1) |

### 10.2 Example

```json
"material": [
  {
    "@type": "Product",
    "name": "Aluminium",
    "additionalProperty": [
      {
        "@type": "PropertyValue",
        "propertyID": "MASS_FRACTION",
        "value": 0.45
      },
      {
        "@type": "PropertyValue",
        "propertyID": "RECYCLED_CONTENT",
        "value": 0.30
      }
    ]
  }
]
```

---

## 11. Units of Measurement

CIRPASSX uses UN/CEFACT Common Codes for units:

| Code | Unit | Usage |
|------|------|-------|
| `KGM` | Kilogram | Weight |
| `MMT` | Millimeter | Dimensions |
| `MTR` | Meter | Length |
| `WTT` | Watt | Power |
| `LUM` | Lumen | Luminous flux |
| `KEL` | Kelvin | Color temperature |
| `VLT` | Volt | Voltage |
| `MON` | Month | Duration |
| `DAY` | Day | Duration |
| `P1` | Percent | Percentages |

---

## 12. Validation

### 12.1 JSON Schema

Products MUST validate against the CIRPASSX JSON Schema:

```
https://cirpassx.org/schema/v1/product.schema.json
```

### 12.2 Validation Rules

1. All required fields MUST be present
2. `@context` MUST include CIRPASSX context
3. `type` MUST include "CircularProductPassport"
4. `id` MUST be a valid UUID URN
5. `identifier` MUST include CIRPASSX_ID
6. `itemCondition` MUST include CIRPASSX_CONDITION_GRADE
7. All dates MUST be ISO 8601 format
8. All URIs MUST be valid

---

## 13. Interoperability

### 13.1 EU Digital Product Passport

CIRPASSX documents are designed to be compatible with EU DPP requirements under ESPR. The format supports:

- Product identification via GS1 Digital Link
- Material composition disclosure
- Repairability information per Right to Repair
- Lifecycle traceability

### 13.2 UN Transparency Protocol (UNTP)

CIRPASSX extends the UNTP Digital Product Passport with:

- Condition grading for secondhand products
- Repair history tracking
- Sustainability metrics for reuse

### 13.3 GS1 EPCIS 2.0

Lifecycle events map to EPCIS bizStep values for supply chain integration.

---

## 14. Security Considerations

### 14.1 Data Integrity

Implementations SHOULD sign CIRPASSX documents using W3C Verifiable Credentials proof mechanisms.

### 14.2 Privacy

Product passports MAY contain commercially sensitive information. Implementations SHOULD:

- Provide access control mechanisms
- Allow selective disclosure of fields
- Support data minimization principles

---

## 15. References

### Normative References

- [RFC 2119](https://tools.ietf.org/html/rfc2119) - Key words for use in RFCs
- [JSON-LD 1.1](https://www.w3.org/TR/json-ld11/) - JSON-based Serialization for Linked Data
- [JSON Schema](https://json-schema.org/draft/2020-12/json-schema-core.html) - JSON Schema draft 2020-12
- [W3C Verifiable Credentials](https://www.w3.org/TR/vc-data-model-2.0/) - VC Data Model 2.0
- [Schema.org](https://schema.org/) - Shared vocabulary for structured data
- [UN/CEFACT Common Codes](https://unece.org/trade/uncefact/cl-recommendations) - Units of measure

### Informative References

- [EU ESPR](https://eur-lex.europa.eu/eli/reg/2024/1781) - Ecodesign for Sustainable Products Regulation
- [EU Right to Repair](https://eur-lex.europa.eu/eli/dir/2024/1799) - Directive on repair of goods
- [UNTP](https://untp.unece.org/) - UN Transparency Protocol
- [GS1 EPCIS 2.0](https://www.gs1.org/standards/epcis) - Electronic Product Code Information Services
- [EN 45554:2020](https://standards.cencenelec.eu/) - Repairability assessment standard
- [GS1 Digital Link](https://www.gs1.org/standards/gs1-digital-link) - Web URI standard for products

---

## Appendix A: Complete Example

See: [examples/led-panel.json](../examples/led-panel.json)

---

## Appendix B: JSON Schema

See: [schema/v1/product.schema.json](../schema/v1/product.schema.json)

---

## Appendix C: JSON-LD Context

See: [context/v1/context.jsonld](../context/v1/context.jsonld)

---

## Appendix D: Changelog

### Version 1.0.0-draft (2026-06-05)

- Initial draft specification
- Core product schema
- Condition grading scale
- Repairability scoring
- Lifecycle events
- Sustainability metrics
