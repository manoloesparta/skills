---
name: backend-standards
description: Escribe o revisa APIs REST y persistencia, especialmente con FastAPI, Pydantic, PostgreSQL y OpenAPI.
---

# Estándares de backend

- Mantén las rutas delgadas. Haz visibles validación, identidad, autorización, delegación y traducción de errores HTTP. Las reglas y consultas viven en módulos de negocio o persistencia y pueden devolver errores de dominio explícitos.
- Usa modelos de entrada y salida del framework. Documenta JSON y multipart según cada operación en OpenAPI. Comparte las reglas de negocio cuando una operación reciba ambos formatos. Aplica límites y validación a archivos.
- Separa identidad, permiso de operación y alcance de datos. Comprueba en el servidor el acceso al recurso y acota las consultas al propietario o tenant cuando corresponda.
- En PostgreSQL, versiona el esquema, usa transacciones con un responsable claro del commit y mantén consultas legibles, con condiciones y alias explícitos. Usa orden estable en listados paginados. Define cómo se reintentan los efectos externos que dependen de una escritura confirmada.
- Conserva identificadores de operación estables. Cuando cambie el contrato, regenera el OpenAPI y los consumidores afectados desde la misma fuente y verifica su consistencia. Mantén validaciones y errores coherentes con el contrato.
