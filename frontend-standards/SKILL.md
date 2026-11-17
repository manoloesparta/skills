---
name: frontend-standards
description: Escribe o revisa interfaces React con TypeScript, Tailwind y datos de servidor, incluidos componentes, hooks y formularios.
---

# Estándares de frontend

- Usa tipos e interfaces nativos de TypeScript y reutiliza tipos generados desde el contrato. Evita introducir Zod y librerías equivalentes. Valida datos externos en el borde con comprobaciones directas o herramientas existentes.
- Mantén el JSX declarativo. Nombra condiciones y estados derivados sobre el render. Extrae handlers con nombre cuando haya varios pasos. Organiza hooks por funcionalidad cuando hagan visible un flujo o eliminen duplicación real.
- Centraliza el cliente HTTP y usa React Query cuando la aplicación gestione estado del servidor. Incluye parámetros relevantes en la clave de consulta y actualiza o invalida las consultas afectadas después de mutar.
- Reutiliza componentes de tablas, formularios y modales cuando compartan comportamiento. Usa las convenciones de Tailwind del proyecto y ofrece estados de carga, vacío y error donde aporten información.
- Muestra acciones disponibles según permisos. La API decide si la operación está autorizada. Conserva etiquetas accesibles, navegación por teclado y mensajes cerca del control afectado.
