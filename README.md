# FPDS Report Library

Centralized, version-controlled catalog of standardized FPDS reports.

This repository contains a single authoritative file (`library.yaml`) that defines
the full list of available reports, their metadata, required parameters, and pricing.
It is designed to be consumed by both **Laravel-based applications** and **Python-based
report generators**.

This repository intentionally contains **no business logic, SQL, or templates**.

---

## Purpose

- Provide a **single source of truth** for all report definitions
- Eliminate duplication between frontend, backend, and generators
- Enable controlled versioning and auditable changes
- Support automated catalog loading and validation

---

## Repository Structure

```text
fpds-report-library/
├── library.yaml   # Master catalog of all reports
├── README.md
└── VERSION
```

---

## `library.yaml` Overview

`library.yaml` defines each report using a normalized schema.

### Top-level fields

```yaml
version: 1.0
currency: USD
reports: []
```

### Report object fields

| Field | Type | Description |
|------|------|-------------|
| `report_code` | string | Globally unique report identifier |
| `report_type` | enum | Report type (`COLL`, `EL`, etc.) |
| `report_category` | string | Logical category (GEO, FUND, TIME, etc.) |
| `report_title` | string | Human-readable report title |
| `report_description` | string | Short functional description |
| `report_vars` | array \| null | Required input variables |
| `report_price` | float | Price in USD |

---

## Design Principles

- **Declarative only** — no executable logic
- **Backward compatible** — changes must not silently break consumers
- **Explicit versioning** — all schema changes require a version bump
- **Consumer-agnostic** — usable by any runtime (PHP, Python, CLI)

---

## Usage

### Laravel

Typical use cases:
- Build report catalog UI
- Display pricing and descriptions
- Validate user input before purchase

Implementation approach:
- Parse `library.yaml`
- Cache in memory or Redis
- Treat as read-only configuration

---

### Report Generator (Python)

Typical use cases:
- Validate `report_code`
- Resolve required variables
- Enforce allowed report set
- Attach metadata to generated outputs

Implementation approach:
- Load YAML at startup
- Cache with checksum
- Reject unknown or deprecated reports

---

## Versioning & Change Policy

- **Additive changes** (new reports) → allowed without breaking consumers
- **Modifications** (price, description, vars) → must be reviewed
- **Breaking changes** → require `version` increment
- **Deprecated reports** should be marked explicitly (future field)

---

## What Does NOT Belong Here

- SQL queries
- Business logic
- PDF templates
- Methodology text
- Runtime configuration

Those belong in their respective services.

---

## Deployment

This repository is intended to be:
- Cloned or pulled during deployment
- Used as a Git submodule or subtree
- Shared between multiple services

No runtime writes are expected.

---

## License

Internal use only.  
All rights reserved.

---

## Maintainer

GETWAB INC.  
FPDS Analytics & Reporting Platform
