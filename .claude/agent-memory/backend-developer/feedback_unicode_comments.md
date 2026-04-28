---
name: Avoid Unicode arrows in Java comments
description: The IDE indexer (Eclipse JDT) reports false syntax errors on non-ASCII chars like the arrow symbol in Java comments
type: feedback
---

Usar `->` en lugar de `→` (U+2192) en comentarios de Java.

**Why:** El IDE (Eclipse JDT / VS Code con Java Extension Pack) reporta "Syntax error on token '.'" en la línea siguiente al comentario con `→`, confundiendo el indexador aunque el compilador de Gradle compile sin errores.

**How to apply:** En cualquier comentario `//` o `/* */` de archivo `.java`, usar solo ASCII. Evitar `→`, `←`, `⇒` y similares.
