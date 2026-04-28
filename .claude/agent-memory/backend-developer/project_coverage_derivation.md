---
name: Coverage derivation from location guarantees
description: Issue #251 — GET /v1/quotes/{folio}/coverage-options derives options from guarantees when DB is empty
type: project
---

Issue #251 implementado en rama `bugfix/integration`.

Cuando `coverageOptionRepository.findByFolioNumber()` devuelve lista vacía, `GetCoverageOptionsUseCaseImpl` llama a `ActiveGuaranteeReader` y `CoverageDerivationService` para derivar opciones.

**Why:** La pantalla de coverage options llegaba vacía cuando el usuario no había guardado opciones explícitamente, pero las garantías de ubicaciones ya contenían información suficiente para pre-poblar la pantalla.

**How to apply:** El `CoverageDerivationService` es dominio puro (sin anotaciones Spring). El `ActiveGuaranteeJpaAdapter` implementa `ActiveGuaranteeReader` y lee `LocationJpaRepository.findByQuoteIdAndActiveTrue()`. Ambos beans se registran en `CoverageConfig`.

Mapeo garantia->cobertura:
- GUA-FIRE o GUA-CONT → COV-FIRE (deductible=2.0, coinsurance=80.0)
- GUA-THEFT → COV-THEFT (5.0, 100.0)
- GUA-GLASS → COV-GLASS (5.0, 100.0)
- GUA-ELEC → COV-ELEC (10.0, 100.0)
- catastrophicZone != null/blank → COV-CAT (3.0, 90.0)
- siempre → COV-BI (3.0, 80.0, selected=false)
