---
name: project-bootstrap
description: Arranca proyectos propios, PoCs y aplicaciones nuevas con una base reproducible. Aplica al crear el esqueleto inicial de una API REST, una web o un proyecto full-stack. Incluye documentación de arranque y pruebas de integración de flujos reales.
---

# Bootstrap de proyectos

Entrega una base pequeña que permita ejecutar y verificar el primer flujo real del proyecto. Respeta el stack, los requisitos y las convenciones existentes cuando el usuario ya los haya definido. Para un proyecto nuevo, elige los perfiles REST y web según el producto. Un solo repositorio suele bastar hasta que haya ciclos de entrega independientes u otra necesidad concreta.

## Base común

- Organiza por responsabilidades visibles. Usa términos del dominio, parámetros explícitos, pasos con nombre y control de flujo directo. Mantén las reglas cerca de donde se aplican. Extrae un helper, componente o hook cuando aclare una responsabilidad o elimine duplicación real.
- **Evita Zod y librerías equivalentes de esquemas y tipos.** En TypeScript, usa tipos e interfaces nativos. Si ya existe un SDK generado desde OpenAPI, reutiliza sus tipos. Valida datos externos en el punto de entrada con las herramientas del framework o con comprobaciones directas. Mantén la validación de la API en Pydantic cuando uses FastAPI.
- Deja comandos reproducibles para instalar, ejecutar y verificar. Fija dependencias según las herramientas del stack y proporciona configuración de ejemplo sin secretos.
- El README debe explicar requisitos, configuración, ubicación del código y cómo ejecutar la aplicación y su primera prueba. Documenta los pasos operativos junto al comando o flujo que los necesita.
- Configura formato automático. En CI, comprueba formato, compilación o tipos según el stack y ejecuta la prueba de integración principal.

## Perfil REST

Usa FastAPI y Pydantic como referencia. Mantén rutas delgadas que validen entradas, deleguen el comportamiento y traduzcan errores de dominio a respuestas HTTP consistentes. Una web sin API ni persistencia propia solo necesita el perfil web.

### Checklist de la API REST

Revisa los 18 puntos antes de entregar. Indica cuáles verificaste y deja un motivo concreto para los puntos condicionados que aún no apliquen.

1. **JSON y form-data.** Incluye soporte para JSON y `multipart/form-data` desde el arranque. Declara los formatos por operación en OpenAPI. Si una operación acepta ambos, aplica las mismas reglas de negocio a los campos compartidos.
2. **Configuración.** Lee valores por ambiente y documenta las variables requeridas con un ejemplo sin secretos.
3. **Integración con la base.** Usa PostgreSQL en desarrollo, local y CI. Deja claros los límites de transacción y usa una base aislada para pruebas.
4. **Migraciones.** Versiona el esquema desde la primera migración y deja un comando reproducible para aplicarlas. Carga los seeds por separado.
5. **Autenticación y autorización.** Antes de exponer datos privados o acciones restringidas, autentica al actor y comprueba permisos y alcance en el servidor. En un PoC local con datos sintéticos y flujo público puede omitirse el proveedor de identidad. Amplía las políticas y el aislamiento por tenant según el producto.
6. **Subida de archivos.** Cuando el flujo acepte archivos, limita el tamaño, valida el tipo y comprueba el acceso a los objetos privados. Usa R2 para el almacenamiento alojado.
7. **Logs estructurados.** Registra contexto de la solicitud, resultado y errores sin incluir tokens ni datos sensibles.
8. **Apagado ordenado.** Cuando el proceso reciba una señal de parada, deja de aceptar tráfico, permite terminar solicitudes dentro del plazo del entorno y cierra conexiones y otros recursos.
9. **CORS.** Si web y API tienen orígenes distintos, declara los orígenes permitidos por ambiente y verifica las solicitudes del navegador.
10. **Filtros, orden y paginación.** En los listados que lo necesiten, documenta los parámetros, usa orden estable y limita el tamaño de página.
11. **Rate limiting.** Para endpoints públicos o costosos expuestos a abuso, define límites, el punto donde se aplican y la respuesta al rebasarlos.
12. **Métricas.** Para una API alojada, habilita un mecanismo sencillo para observar volumen de solicitudes, latencia y errores. Usa las métricas del entorno si ya ofrecen esa visibilidad.
13. **Especificación de API.** Genera OpenAPI desde el código con identificadores de operación estables. Si generas un SDK para evitar duplicación, documenta cómo regenerarlo y verifica que corresponda al contrato fuente.
14. **Imagen Docker.** Incluye un Dockerfile para la API y un comando reproducible para construir y arrancar la imagen.
15. **Entorno local de pruebas.** Documenta cómo arrancar la API con PostgreSQL aislado, aplicar migraciones y cargar los datos necesarios para probar el flujo.
16. **Infraestructura declarativa.** Cuando haya recursos alojados que reproducir, versiona su configuración declarativa y documenta las entradas que dependen del entorno.
17. **Despliegue semiautomático.** Cuando exista un destino de desarrollo, versiona el flujo de GitHub Actions para validar y desplegar desde la rama de integración. Deja explícito cualquier paso manual que requiera el proveedor.
18. **Pruebas básicas de integración.** Ejecuta el flujo principal por HTTP real. Verifica persistencia mediante una lectura posterior y cubre multipart y autorización cuando formen parte del flujo. Evita las pruebas unitarias.

## Seeds

En el perfil REST incluye un comando documentado y versionado para cargar datos sintéticos útiles en PostgreSQL. Ejecútalo después de las migraciones. Haz que se pueda volver a correr sin duplicar registros y limita su ejecución a local, pruebas y desarrollo. En CI usa una base aislada. Ejecuta los seeds de demo mediante un paso explícito, separado de la migración de esquema.

## Perfil web

- Usa React, TypeScript, Vite y **Tailwind CSS** como stack de referencia. Incluye Tailwind desde el inicio con una base visual pequeña. Reutiliza formularios, tablas y modales conforme se repitan las interacciones.
- Centraliza el acceso a la API. Usa React Query cuando haya estado del servidor que cargar, cachear o actualizar. Mantén las claves de consulta y la actualización de datos claras, y agrupa hooks por funcionalidad cuando faciliten seguir el flujo.
- Incluye estados de carga, error, vacío y confirmación donde el usuario los necesite.

## Pruebas de flujos reales

**Evita explícitamente las pruebas unitarias.** Deja una prueba de integración reproducible del flujo principal por el punto de entrada real. Usa datos aislados y comprueba resultados observables.

- API. Envía peticiones HTTP a la aplicación. Cuando el flujo persista datos, aplica las migraciones a una base aislada y verifica la escritura mediante una lectura.
- Form data y permisos. Envía una petición multipart real cuando el flujo use ese formato. Para una operación restringida, comprueba un caso permitido y otro denegado.
- Web. Cuando el comportamiento dependa de la interfaz, recorre el flujo en navegador y comprueba el resultado visible.

## Hosting

Usa los servicios siguientes según los perfiles elegidos y las necesidades del proyecto. Reserva AWS para una migración posterior y separada del bootstrap.

- **Identidad:** WorkOS AuthKit. Comprueba los permisos de negocio en el backend.
- **PostgreSQL:** Neon. Conserva las migraciones versionadas y el comando de seeds.
- **API FastAPI en contenedor:** Render.
- **Web estática de Vite:** Cloudflare Pages.
- **Archivos:** Cloudflare R2. Mantén los archivos privados cuando corresponda y autoriza su acceso desde el backend.
- **CI/CD de desarrollo:** GitHub Actions.

Verifica cuotas, suspensión por inactividad y límites de archivos antes de provisionar. Si aún no hay destino de desarrollo, entrega comandos reproducibles de build, arranque y verificación. Documenta las cuentas, permisos y variables pendientes para habilitar el despliegue.

## CI/CD de desarrollo

Versiona la configuración de CI cuando se conozca el alojamiento del repositorio. Versiona también la configuración de despliegue cuando haya un destino de desarrollo para los servicios que se publicarán.

- Ejecuta las verificaciones en las solicitudes de cambio. Para entregar a desarrollo, usa la rama de integración elegida y exige que pasen el formato, la comprobación de tipos o compilación y las pruebas de integración aplicables.
- Asigna un solo responsable de despliegue por componente y ambiente. Desactiva los disparadores automáticos que puedan publicar por su cuenta. Después de validar el commit que va a desarrollo, construye desde ese mismo commit y registra su identificador en el artefacto o resultado del despliegue. Serializa migraciones y despliegues concurrentes al mismo ambiente cuando compartan la base o el destino. Si el destino ejecuta contenedores, genera una imagen. Si hay frontend y API, despliega el componente que cambió y considera los cambios compartidos que afecten a ambos.
- Si el servicio requiere migraciones compatibles con la versión activa, ejecuta las pendientes con el mismo artefacto del backend antes de actualizar el servicio. Para cambios incompatibles, prepara una transición de esquema por etapas o coordina una ventana de mantenimiento para actualizar el esquema y el servicio. Detén el despliegue si fallan las migraciones. Al terminar, comprueba que el servicio quedó disponible y reporta el resultado.
- Mantén configuración y secretos separados por ambiente. Usa infraestructura declarativa cuando haga falta provisionar el destino de desarrollo. Documenta el disparador, las variables requeridas y cómo consultar el resultado.

Antes de entregar, verifica el arranque y el flujo de integración con los comandos documentados cuando el entorno lo permita. Informa qué se ejecutó y qué quedó sin verificar.
