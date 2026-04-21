# CLAUDE.md — Sofka Insurance Quoter

Instrucciones de proyecto para Claude Code. Este archivo complementa las reglas en `.claude/rules/`.

---

## Idioma

| Artefacto | Idioma |
|-----------|--------|
| Código (clases, métodos, variables, tablas, columnas) | **Inglés** |
| Comentarios de código (`//`, `/* */`) | **Inglés** |
| Textos de UI visibles al usuario (labels, botones, mensajes, placeholders) | **Español** |
| Documentación (specs, README, markdown, ADRs) | **Español** |

Los términos internacionales de uso común se mantienen en inglés en la UI (ej. "online", "dashboard", "folio").

---

## Stack tecnológico

| Capa | Proyecto | Tecnología |
|------|----------|-----------|
| Backend quoter | `Insurance-Quoter-Back/` (plataforma-danos-back) | Java 21 + Spring Boot 4 + Hexagonal + PostgreSQL :5432 |
| Backend core | `Insurance-Quoter-Core/` (plataforma-core-ohs) | Java 21 + Spring Boot 4 + Hexagonal + PostgreSQL :5433 |
| Frontend | `Insurance-Quoter-Front/` | Angular 19 (standalone) + TypeScript 5.x |
| Tests backend | ambos backends | JUnit 5 + Mockito + Spring Boot Test |
| Tests frontend | `Insurance-Quoter-Front/` | Jasmine + Karma (solo services, guards, pipes) |
| Automatización | `Auto_Api_Screenplay/` / `Auto_Front_Screenplay/` | Serenity BDD + Screenplay |

## Arquitectura de backend: Hexagonal (Ports & Adapters)

```
infrastructure (adapters) → application (use cases) → domain (model + ports)
```

- `domain/model/` — entidades y objetos de valor **sin** anotaciones JPA ni Spring
- `domain/port/in/` — interfaces de casos de uso (input ports)
- `domain/port/out/` — interfaces de repositorios y clientes externos (output ports)
- `application/usecase/` — implementaciones de input ports
- `infrastructure/adapter/in/rest/` — controllers Spring MVC
- `infrastructure/adapter/out/persistence/` — JPA adapters (implementan output ports)
- `infrastructure/adapter/out/http/` — HTTP client adapters (implementan output ports)

---

## Metodología: TDD

Se aplica **TDD (Red → Green → Refactor)** en todo el código de lógica:

- **Backend**: services, repositories, cálculo de prima — siempre escribir el test antes de la implementación.
- **Frontend**: services, guards, pipes, resolvers — test antes de implementar.
- **Frontend UI**: los componentes y templates **no se testean**. Solo se prueba la lógica pura.

---

## Diccionario de dominio

Mapeo de términos de negocio (español) a nombres canónicos de código (inglés):

| Término de negocio | Código |
|--------------------|--------|
| Cotización | `Quote` |
| Folio / Número de folio | `Folio` / `folioNumber` |
| Ubicación | `Location` |
| Prima neta | `netPremium` |
| Prima comercial | `commercialPremium` |
| Prima por ubicación | `locationPremium` |
| Estado de cotización | `quoteStatus` |
| Datos del asegurado | `insuredData` |
| Datos de conducción | `underwritingData` |
| Clasificación de riesgo | `riskClassification` |
| Tipo de negocio | `businessType` |
| Configuración de layout | `layoutConfiguration` |
| Opciones de cobertura | `coverageOptions` |
| Código postal | `zipCode` |
| Giro / Giro de negocio | `businessLine` / `lineOfBusiness` |
| Clave incendio | `fireKey` |
| Garantía | `guarantee` |
| Zona catastrófica | `catastrophicZone` |
| Tipo constructivo | `constructionType` |
| Año de construcción | `constructionYear` |
| Alerta bloqueante | `blockingAlert` |
| Estado de validación | `validationStatus` |
| Suscriptor | `subscriber` |
| Agente | `agent` |
| Tarifa | `tariff` |
| Factor técnico | `technicalFactor` |
| Parámetros de cálculo | `calculationParameters` |
| Versión (optimistic lock) | `version` |

---

## Estructura de directorios

```
Sofka-IQ/
├── Insurance-Quoter-Back/     # plataforma-danos-back  — puerto 8080, DB :5432
├── Insurance-Quoter-Core/     # plataforma-core-ohs   — puerto 8081, DB :5433
├── Insurance-Quoter-Front/    # cotizador-danos-web   — Angular 19
├── Auto_Api_Screenplay/       # Tests automatizados API (Serenity BDD)
├── Auto_Front_Screenplay/     # Tests automatizados UI (Serenity BDD)
├── docs/
│   ├── Reto.md                # Especificación del reto técnico
│   ├── api-contracts.md       # Contratos HTTP front↔back y back→core
│   ├── diagrams/              # Diagramas C4 (DrawIO)
│   └── asdd-guia-backend.md   # Guía de comandos ASDD para backend
├── .claude/specs/             # Specs ASDD (DRAFT → APPROVED → ...)
└── .claude/                   # Framework ASDD (no modificar)
```

---

## Definición de Listo (DoR)

Una tarea está lista para implementar cuando:

- [ ] Existe una spec en `.claude/specs/<feature>.spec.md` con estado `APPROVED`
- [ ] La spec incluye criterios de aceptación en Gherkin
- [ ] Los endpoints del backend están definidos (método, ruta, request/response DTO)
- [ ] El modelo de datos está especificado (entidades, relaciones, columnas)
- [ ] No hay dependencias externas bloqueantes sin resolver

---

## Definición de Hecho (DoD)

Una tarea está terminada cuando:

- [ ] El código compila sin errores ni warnings
- [ ] Los tests pasan (`./gradlew test` / `ng test`)
- [ ] Cobertura de lógica de negocio ≥ 80%
- [ ] Los endpoints responden correctamente (validados con colección Bruno/Postman)
- [ ] No hay credenciales ni URLs hardcodeadas
- [ ] La spec está actualizada a estado `IMPLEMENTED`
- [ ] El README del módulo refleja cómo ejecutar localmente

---

## Reglas de oro

1. **No código sin spec aprobada** — siempre debe existir `.claude/specs/<feature>.spec.md` con estado `APPROVED`.
2. **TDD primero** — el test se escribe antes que el código de producción.
3. **Código en inglés, UI en español** — sin excepciones salvo términos internacionales comunes.
4. **No suposiciones** — si el requerimiento es ambiguo, preguntar antes de actuar.
