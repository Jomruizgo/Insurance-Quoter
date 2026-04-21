# MEMORY.md — Sofka Insurance Quoter

Archivo de memoria del proyecto. A diferencia de `CLAUDE.md`, este archivo **no se carga automáticamente** en cada conversación. Leerlo cuando se necesite contexto histórico, decisiones técnicas o aclaraciones de dominio.

Para leerlo: pedirle explícitamente a Claude que lea `MEMORY.md`.

---

## Decisiones técnicas

### Dos microservicios backend
- `Insurance-Quoter-Back/` = **plataforma-danos-back** → cotizaciones, ubicaciones, coberturas, cálculo. Puerto 8080, DB puerto 5432.
- `Insurance-Quoter-Core/` = **plataforma-core-ohs** → catálogos, tarifas, agentes, suscriptores, folios, CP. Puerto 8081, DB puerto 5433.
- Core fue creado copiando Back y renombrando. El folder `Insurance_Quoter` residual en Core puede borrarse manualmente.

### Arquitectura hexagonal (Ports & Adapters)
Ambos backends usan hexagonal. Domain models son POJOs sin anotaciones JPA. Las entidades JPA viven en `infrastructure/adapter/out/persistence/`. Los use cases (application layer) orquestan domain + output ports. Los controllers solo parsean HTTP y llaman input ports.

### Stack seleccionado
- **Backend**: Java 21 + Spring Boot 4 + Spring Data JPA + PostgreSQL + Lombok
- **Frontend**: Angular 19 (standalone components) + TypeScript 5.x + RxJS
- **Tests backend**: JUnit 5 + Mockito + Spring Boot Test (@WebMvcTest, @DataJpaTest)
- **Tests frontend**: Jasmine + Karma — solo services, guards y pipes. Sin tests de componentes.
- **Automatización**: Serenity BDD + Screenplay (carpetas Auto_Api_Screenplay/ y Auto_Front_Screenplay/)

### TDD
Se aplica TDD obligatorio en backend (services + controllers) y frontend (services + guards + pipes).
Los componentes Angular y templates NO se testean — decisión del equipo por ROI bajo en tests de UI.
El `backend-developer` y `frontend-developer` escriben los tests unitarios dentro de su ciclo TDD.
Los `test-engineer-*` completan con tests de integración y auditoría de cobertura.

### Contratos de API
Fuente de verdad en `docs/api-contracts.md`. Si hay conflicto entre una spec y el contrato, prevalece el contrato.
Todos los campos JSON en `camelCase` inglés. Textos de UI en español.

### Versionado optimista
Todas las operaciones de escritura del backend reciben y retornan `version`. Conflicto → HTTP 409.

### Core service
Se implementa como stubs (fixtures JSON en `src/main/resources/fixtures/`) mediante interfaz `CoreServiceClient`. Permite reemplazar con cliente HTTP real sin modificar services consumidores.

---

## Aclaraciones de dominio

### Folio vs Quote
- `Folio` es el identificador secuencial de negocio (ej. `FOL-2026-00042`). Término propio, se mantiene en código.
- `Quote` es el aggregate raíz en código que representa la cotización completa.
- En la UI se muestra como "Folio" o "Cotización" según el contexto.

### Prima calculable vs no calculable
Una ubicación es calculable si tiene: `zipCode` válido en catálogo + `businessLine.fireKey` + al menos una garantía tarifable.
Si una ubicación no es calculable, genera `blockingAlert` pero no bloquea el cálculo de las demás.
Solo si **todas** las ubicaciones son incalculables se retorna HTTP 422.

### Cálculo de prima
La fórmula no es actuarialmente exacta — se documenta en la spec de `premium-calculation`. Lo importante es que sea consistente, trazable y testeada componente por componente.

---

## Contexto del proyecto

### Propósito
Reto técnico de Sofka — evaluación de capacidades full-stack. Entregable obligatorio incluye todos los specs ASDD generados + video YouTube (máximo 10 min, modo oculto).

### Metodología obligatoria
ASDD (Agent Spec Software Development). Todos los features deben tener spec en `.claude/specs/` con estado `APPROVED` antes de implementar.

### Orden de implementación de features
1. `core-service-stubs` — base para todos los demás
2. `folio-management`
3. `quote-general-info`
4. `location-layout`
5. `location-management`
6. `quote-state`
7. `coverage-options`
8. `premium-calculation`

---

## Registro de cambios importantes

| Fecha | Cambio |
|-------|--------|
| 2026-04-20 | Arquitectura hexagonal adoptada en ambos backends; Insurance-Quoter-Core creado |
| 2026-04-20 | Diagramas C4 L1 y L2 creados en docs/diagrams/ (formato DrawIO) |
| 2026-04-20 | Stack actualizado de FastAPI/React a Java 21/Angular 19 en todas las rules |
| 2026-04-20 | TDD integrado en agentes: developer escribe tests, test-engineer audita integración |
| 2026-04-20 | Contratos de API definidos en docs/api-contracts.md |
| 2026-04-20 | tasks-to-issues adaptado como agente ASDD (Fase 5 opcional) |
