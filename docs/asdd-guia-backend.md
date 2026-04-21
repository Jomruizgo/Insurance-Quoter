# Guía ASDD — Backend Insurance Quoter

Paso a paso para trabajar el backend del cotizador usando la metodología ASDD con Claude Code.

**Prerequisito:** tener Claude Code corriendo en la raíz del proyecto (`Sofka-IQ/`).

---

## Flujo general

```
1. Generar spec  →  2. Aprobar spec  →  3. Implementar backend  →  4. Generar tests  →  5. QA
```

Cada feature del backend sigue este flujo. Nunca saltar al paso 3 sin spec aprobada.

---

## Features del backend a implementar

Basado en los endpoints de `docs/api-contracts.md`, los features son:

| # | Feature | Endpoints involucrados |
|---|---------|----------------------|
| 1 | `folio-management` | POST /v1/folios |
| 2 | `quote-general-info` | GET/PUT /v1/quotes/{folio}/general-info |
| 3 | `location-layout` | GET/PUT /v1/quotes/{folio}/locations/layout |
| 4 | `location-management` | GET/PUT/PATCH /v1/quotes/{folio}/locations + summary |
| 5 | `quote-state` | GET /v1/quotes/{folio}/state |
| 6 | `coverage-options` | GET/PUT /v1/quotes/{folio}/coverage-options |
| 7 | `premium-calculation` | POST /v1/quotes/{folio}/calculate |
| 8 | `core-service-stubs` | Catálogos, tarifas, suscriptores, agentes, CP |

---

## Paso 1 — Generar la spec

### Comando
```
/generate-spec <nombre-feature>
```

### Prompts por feature

#### Feature 1: Gestión de folios
```
/generate-spec folio-management

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Creación de folios con idempotencia.

Endpoint: POST /v1/folios
Contrato completo en docs/api-contracts.md, sección 1.

Reglas de negocio:
- Si ya existe un folio para el mismo subscriberId + agentCode sin cotización iniciada, retornar el existente (HTTP 200) en lugar de crear uno nuevo (HTTP 201).
- El número de folio se genera llamando al stub GET /v1/folios del core service.
- Validar que subscriberId y agentCode existan en los catálogos del core service.

Entidades JPA involucradas: Quote (tabla: quotes).
Campos mínimos del aggregate Quote: folioNumber, quoteStatus, subscriberId, agentCode, version, createdAt, updatedAt.
```

#### Feature 2: Datos generales
```
/generate-spec quote-general-info

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Consulta y actualización de datos generales de una cotización.

Endpoints:
- GET /v1/quotes/{folio}/general-info
- PUT /v1/quotes/{folio}/general-info

Contratos completos en docs/api-contracts.md, sección 2.

Reglas de negocio:
- El PUT debe validar versionado optimista (@Version en JPA). Si la versión no coincide, retornar 409.
- Al actualizar, incrementar version y actualizar updatedAt.
- Los campos de insuredData y underwritingData se persisten dentro del aggregate Quote.
- Validar que riskClassification pertenezca al catálogo GET /v1/catalogs/risk-classification.

Entidades JPA: Quote (ampliar con campos de insuredData y underwritingData embebidos o en columnas planas).
```

#### Feature 3: Layout de ubicaciones
```
/generate-spec location-layout

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Configuración del layout de ubicaciones de una cotización.

Endpoints:
- GET /v1/quotes/{folio}/locations/layout
- PUT /v1/quotes/{folio}/locations/layout

Contratos completos en docs/api-contracts.md, sección 3.

Reglas de negocio:
- Al guardar el layout, si ya existen ubicaciones y numberOfLocations cambia, ajustar la lista (agregar entradas vacías o marcar excedentes como inactivas — no eliminar datos).
- Versionado optimista obligatorio en PUT.

Entidades JPA: Quote (campo layoutConfiguration), Location (tabla: locations, relación @ManyToOne con Quote).
```

#### Feature 4: Gestión de ubicaciones
```
/generate-spec location-management

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Registro, consulta y edición de ubicaciones dentro de una cotización.

Endpoints:
- GET /v1/quotes/{folio}/locations
- PUT /v1/quotes/{folio}/locations
- PATCH /v1/quotes/{folio}/locations/{index}
- GET /v1/quotes/{folio}/locations/summary

Contratos completos en docs/api-contracts.md, sección 4.

Reglas de negocio:
- PATCH es actualización parcial: solo se actualizan los campos presentes en el body.
- Al guardar una ubicación, validar zipCode contra GET /v1/zip-codes/{zipCode} del core service. Si no es válido, registrar alerta bloqueante MISSING_ZIP_CODE y poner validationStatus = INCOMPLETE.
- Una ubicación sin businessLine.fireKey genera alerta MISSING_FIRE_KEY.
- Una ubicación sin guarantees tarifables genera alerta NO_TARIFABLE_GUARANTEES.
- blockingAlerts se recalcula en cada escritura.
- El GET /locations/summary no carga el detalle completo de ubicaciones, solo index, locationName, validationStatus y blockingAlerts.

Entidades JPA: Location (tabla: locations), BlockingAlert (embeddable o tabla: location_alerts).
Relación: Location @ManyToOne Quote, BlockingAlert @ElementCollection en Location.
```

#### Feature 5: Estado de cotización
```
/generate-spec quote-state

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Consulta del estado y progreso de completitud de una cotización.

Endpoint: GET /v1/quotes/{folio}/state

Contrato completo en docs/api-contracts.md, sección 5.

Reglas de negocio:
- completionPercentage se calcula en el servicio (no se persiste): es el porcentaje de secciones en estado COMPLETE.
- Secciones: generalInfo, layout, locations, coverageOptions, calculation.
- Una sección está COMPLETE si todos sus campos obligatorios están llenos y sin alertas bloqueantes.
- quoteStatus posibles: CREATED, IN_PROGRESS, CALCULATED, ISSUED.

Sin nuevas entidades JPA: se lee del aggregate Quote y sus relaciones.
```

#### Feature 6: Opciones de cobertura
```
/generate-spec coverage-options

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Consulta y configuración de opciones de cobertura para una cotización.

Endpoints:
- GET /v1/quotes/{folio}/coverage-options
- PUT /v1/quotes/{folio}/coverage-options

Contratos completos en docs/api-contracts.md, sección 6.

Reglas de negocio:
- Las coberturas disponibles vienen del catálogo GET /v1/catalogs/guarantees del core service.
- Solo se pueden seleccionar coberturas que existan en el catálogo.
- deductiblePercentage y coinsurancePercentage tienen rangos válidos (0-100).
- Versionado optimista obligatorio en PUT.

Entidades JPA: CoverageOption (tabla: coverage_options), relación @ManyToOne con Quote.
```

#### Feature 7: Cálculo de prima
```
/generate-spec premium-calculation

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Cálculo de prima neta y prima comercial para todas las ubicaciones calculables.

Endpoint: POST /v1/quotes/{folio}/calculate

Contrato completo en docs/api-contracts.md, sección 7.

Reglas de negocio:
- Leer cotización completa por folio (Quote + Locations + CoverageOptions).
- Leer tarifas y factores técnicos desde GET /v1/tariffs del core service.
- Por cada ubicación: determinar si es calculable (tiene zipCode válido, businessLine.fireKey y al menos una garantía tarifable).
- Si una ubicación no es calculable: registrar alerta, no calcular, continuar con las demás.
- Si NINGUNA ubicación es calculable: retornar HTTP 422 con código NO_CALCULABLE_LOCATIONS.
- Calcular prima por ubicación con los componentes del desglose (ver docs/api-contracts.md sección 7).
- Consolidar netPremium total = suma de netPremium de ubicaciones calculables.
- commercialPremium = netPremium * factor comercial (de tarifas).
- Persistir resultado en una sola transacción: actualizar Quote con quoteStatus = CALCULATED, netPremium, commercialPremium y guardar PremiumByLocation[].
- No sobrescribir otras secciones de la cotización en este paso.
- Versionado optimista obligatorio.

Entidades JPA nuevas: CalculationResult (tabla: calculation_results), PremiumByLocation (tabla: premiums_by_location).
Relación: CalculationResult @OneToOne Quote, PremiumByLocation @ManyToOne CalculationResult.

IMPORTANTE: La lógica de cálculo debe estar en CalculationService, totalmente separada del controller. Debe ser 100% testeable con TDD.
```

#### Feature 8: Stubs del core service
```
/generate-spec core-service-stubs

Contexto: Backend Java 21 + Spring Boot 4 + PostgreSQL.
Feature: Implementación de stubs del servicio core para catálogos y tarifas.

Endpoints a implementar como datos en memoria o fixtures JSON:
- GET /v1/subscribers
- GET /v1/agents
- GET /v1/business-lines
- GET /v1/zip-codes/{zipCode}
- POST /v1/zip-codes/validate
- GET /v1/folios (generador secuencial)
- GET /v1/catalogs/risk-classification
- GET /v1/catalogs/guarantees
- GET /v1/tariffs

Contratos completos en docs/api-contracts.md, sección 8.

Estrategia: implementar como un CoreServiceClient (interfaz) con una implementación stub que lee datos de archivos JSON en src/main/resources/fixtures/. Esto permite reemplazarla por un cliente HTTP real sin cambiar los servicios que la consumen.

No se requiere base de datos para estos endpoints: datos estáticos en fixtures.
```

---

## Paso 2 — Aprobar la spec

Después de que `/generate-spec` crea el archivo en `.claude/specs/<feature>.spec.md`, revisarlo y cambiar el status:

```yaml
# En .claude/specs/<feature>.spec.md
status: APPROVED
updated: 2026-04-20
```

**No continuar al paso 3 sin este cambio.**

---

## Paso 3 — Implementar el backend

### Comando
```
/implement-backend <nombre-feature>
```

### Prompts por feature

#### Cualquier feature backend
```
/implement-backend <nombre-feature>

Spec aprobada: .claude/specs/<nombre-feature>.spec.md
Contratos de referencia: docs/api-contracts.md
Stack: Java 21 + Spring Boot 4 + Spring Data JPA + PostgreSQL + Lombok

Estructura de paquetes:
  com.sofka.insurancequoter.<feature>.controller
  com.sofka.insurancequoter.<feature>.service
  com.sofka.insurancequoter.<feature>.repository
  com.sofka.insurancequoter.<feature>.domain
  com.sofka.insurancequoter.<feature>.dto

Aplicar TDD: escribir los tests unitarios del service ANTES de la implementación.
Aplicar reglas de .claude/rules/backend.md y .claude/rules/database.md.
```

#### Para el feature de cálculo (más complejo)
```
/implement-backend premium-calculation

Spec aprobada: .claude/specs/premium-calculation.spec.md
Contratos de referencia: docs/api-contracts.md sección 7

Prioridad de implementación:
1. Primero los tests de CalculationService (TDD — RED)
2. Implementar CalculationService hasta pasar todos los tests (GREEN)
3. Implementar CalculationController
4. Implementar PremiumByLocation persistencia

La lógica de cálculo debe estar completamente en CalculationService.
El controller solo delega y mapea DTO.
Cada componente del desglose (fireBuildings, fireContents, cattev, etc.) debe tener su propio método privado testeable.
```

---

## Paso 4 — Generar los tests

### Comando
```
/unit-testing <nombre-feature>
```

### Prompt genérico
```
/unit-testing <nombre-feature>

Spec: .claude/specs/<nombre-feature>.spec.md
Implementación completada en: Insurance-Quoter-Back/src/main/java/com/sofka/insurancequoter/<feature>/

Generar:
- Tests unitarios del Service con Mockito (happy path, error path, edge cases)
- Tests del Controller con @WebMvcTest (status HTTP, body de response)
- Tests del Repository con @DataJpaTest si hay queries custom

Cobertura mínima: 80% en lógica de negocio.
Framework: JUnit 5 + Mockito + Spring Boot Test.
Metodología: AAA (Arrange-Act-Assert).
```

---

## Paso 5 — QA y casos Gherkin

```
@qa-agent ejecuta QA para .claude/specs/<nombre-feature>.spec.md

Generar:
1. Casos Gherkin para flujos críticos del feature
2. Matriz de riesgos ASD (Alto/Medio/Bajo)
3. Propuesta de flujos a automatizar con Serenity BDD en Auto_Api_Screenplay/
```

---

## Orden recomendado de implementación

Respetar dependencias entre features:

```
1. core-service-stubs        ← base para todo (catálogos y tarifas)
2. folio-management          ← entry point del flujo
3. quote-general-info        ← depende de folio
4. location-layout           ← depende de folio
5. location-management       ← depende de layout
6. quote-state               ← depende de todas las secciones
7. coverage-options          ← depende de catálogos de garantías
8. premium-calculation       ← depende de todo lo anterior
```

---

## Comandos de referencia rápida

| Qué hacer | Comando |
|-----------|---------|
| Generar spec | `/generate-spec <feature>` |
| Ver estado del flujo | `/asdd-orchestrate` |
| Implementar backend | `/implement-backend <feature>` |
| Generar tests | `/unit-testing <feature>` |
| Generar casos Gherkin | `/gherkin-case-generator <feature>` |
| Identificar riesgos | `/risk-identifier <feature>` |
| Orquestar todo | `/asdd-orchestrate <feature>` |

---

## Notas importantes

- **Los tests van antes del código** — TDD es obligatorio en la capa de service.
- **Los contratos son la fuente de verdad** — si hay duda entre la spec y `docs/api-contracts.md`, prevalece el contrato.
- **Los stubs del core service deben implementarse primero** — todos los demás features los consumen.
- **El cálculo de prima no es un número mágico** — cada componente del desglose debe tener un método y un test.
