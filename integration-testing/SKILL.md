---
name: integration-testing
description: Diseña, escribe o ejecuta pruebas de flujos reales para APIs, bases PostgreSQL y aplicaciones web. Usa puntos de entrada normales y evita pruebas unitarias.
---

# Testing de integración

- Prueba un resultado observable por el punto de entrada real. Evita explícitamente las pruebas unitarias. Escoge HTTP para comportamiento de API y navegador cuando el resultado dependa de la interfaz.
- Arranca la aplicación con su configuración de pruebas. Para persistencia, usa PostgreSQL aislado, aplica las migraciones y prepara solo los datos necesarios mediante fixtures o seeds reproducibles. Antes de migrar, sembrar o limpiar datos, confirma que la configuración y la base correspondan al entorno aislado de prueba. Aísla casos para que puedan ejecutarse en otro orden o en paralelo. Limita la limpieza a recursos creados por la prueba. Si solo hay acceso a un entorno compartido, usa operaciones de lectura o prepara un entorno aislado antes de ejecutar escrituras.
- En una escritura, confirma el resultado con una lectura posterior. Si cambian permisos, cubre una operación permitida y una denegada con actores y datos representativos. Si cambia un upload, manda una petición multipart real y comprueba el resultado y el rechazo relevante.
- Deja explícitos preparación, acción y resultado. Reutiliza el cliente o las utilidades de integración existentes. Prueba dependencias externas en su sandbox cuando formen parte del flujo y exista acceso. Indica qué quedó fuera de la ejecución.
- Documenta un comando reproducible y úsalo en CI. Mantén seeds de demo separados de los datos que una prueba necesita para ser repetible.
