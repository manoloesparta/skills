---
name: coding-standards
description: Escribe o revisa código de aplicación con convenciones portables de legibilidad, simplicidad y cambios mantenibles.
---

# Estándares de código

- Usa nombres del dominio y parámetros explícitos. Reserva nombres breves para ámbitos donde el significado sea inmediato. Aísla firmas dinámicas exigidas por frameworks.
- Usa control de flujo directo y variables con nombre para condiciones de negocio. Evita ternarios anidados, cadenas densas de transformaciones y expresiones que obliguen a reconstruir el significado.
- Mantén las reglas cerca de donde se aplican. Extrae una función o componente cuando nombre una responsabilidad real o elimine duplicación comprobada. Prefiere componentes y APIs existentes si conservan la semántica.
- Conserva las convenciones del archivo y limita los cambios de estilo al alcance solicitado. Escribe comentarios para explicar restricciones, invariantes o decisiones que el código no pueda expresar.
- Maneja errores con mensajes accionables. Registra contexto útil de fallos inesperados sin exponer secretos o datos privados.
- Distingue recursos, relaciones e historial. Haz que desvincular o desasignar afecte a la relación y conserva los registros según las reglas del producto.
- Si falta algo en código generado, actualiza su fuente y regenera. Conserva la documentación de interfaz y operación alineada con el cambio.
