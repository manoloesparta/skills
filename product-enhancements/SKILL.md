---
name: product-enhancements
description: Implementa mejoras de comportamiento en productos existentes. Sigue el flujo actual y coordina cambios de datos, API, clientes, interfaz, pruebas y documentación.
---

# Mejoras a productos existentes

1. Expresa el comportamiento esperado como un flujo observable. Lee el punto de entrada actual y sigue sus dependencias hasta los datos y permisos que gobiernan el cambio.
2. Identifica qué se ve afectado según el caso: datos ya existentes, esquema, reglas, autorización, contrato OpenAPI, consumidores generados, interfaz, integraciones y operación. Reutiliza las piezas existentes cuando tengan la misma semántica.
3. Implementa el cambio más pequeño que complete el flujo. Cuando cambie el esquema, prepara la transición para datos existentes y despliegues parciales. Cuando cambie el contrato, actualiza la fuente y regenera los clientes afectados. Revisa estados de carga, error y permisos en la interfaz cuando el usuario los vea.
4. Verifica el flujo por HTTP o navegador según el comportamiento y cubre la regresión que motivó el cambio. Evita pruebas unitarias. Usa datos aislados y Postgres cuando el flujo dependa de PostgreSQL.
5. Actualiza la documentación que el cambio vuelva obsoleta. Reporta qué se verificó y cualquier paso de operación pendiente.
