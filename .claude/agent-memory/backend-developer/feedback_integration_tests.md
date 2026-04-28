---
name: Integration tests use real DB; two JPA adapter tests are preexisting broken
description: GeneralInfoJpaAdapterTest and QuoteLayoutJpaAdapterTest fail before any changes — preexisting issue
type: feedback
---

`GeneralInfoJpaAdapterTest#save_persistsAndReturnsMappedDomain` y `QuoteLayoutJpaAdapterTest#shouldUpdateQuoteJpaAndReturnMappedData_whenSavingLayout` fallan sin cambios propios. Son tests de integración JPA que dependen de DB real y tienen un bug preexistente.

**Why:** Confirmado con `git stash` + ejecución limpia — fallan en el baseline de la rama `bugfix/integration`.

**How to apply:** Al reportar resultados de `./gradlew test`, excluir estos 2 del conteo de regresiones introducidas. No intentar corregirlos salvo que sea explícitamente pedido.

También: `CoverageOptionJpaAdapterTest#shouldDeleteExistingAndSaveNew` tenía mock `quoteJpaRepository.save()` pero el adapter llama `saveAndFlush()` — corregido en el mismo PR como parte del refactor.
