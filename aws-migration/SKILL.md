---
name: aws-migration
description: Diseña y escribe Terraform multiambiente para migrar aplicaciones de proveedores externos a AWS. Aplica a infraestructura, CI/CD y traslado de PostgreSQL, contenedores, frontend, archivos e identidad.
---

# Migración multiambiente a AWS

Inventaría por ambiente los servicios de origen, datos, dominios, configuración, secretos, dependencias, ventanas de interrupción y requisitos de recuperación. Identifica qué se trasladará y qué seguirá externo. Si faltan cuentas, región o proveedor Git, desarrolla las piezas que puedan verificarse localmente y documenta las entradas que faltan.

## Terraform

- Usa módulos reutilizables para capacidades con varios consumidores y raíces explícitas por ambiente, como `envs/dev`, `envs/stage` y `envs/prod`. Ajusta los ambientes al proyecto. Separa red y servicios duraderos de la aplicación cuando tengan ciclos de vida distintos.
- Aísla estado remoto con bloqueo, roles, configuración y secretos por ambiente. Para una cuenta nueva, deja un bootstrap separado y documenta cómo crear el backend de estado antes de inicializar las raíces que lo usan. Si el estado del propio bootstrap empieza local, documenta cómo migrarlo y verificarlo. Usa cuentas separadas cuando corresponda. Documenta dependencias y orden de planes y aplicaciones. Fija versiones de Terraform y proveedores y conserva el lockfile de cada raíz.
- Escribe módulos y raíces reales, con variables tipadas, salidas útiles, tags y permisos mínimos. Referencia secretos de runtime por identificador. Evita valores secretos en Git, planes, salidas y estado siempre que sea posible.
- Modela disponibilidad, capacidad y retención por ambiente. Estima costo base y variable, incluidos PostgreSQL, red, balanceo, NAT, logs y backups.
- Si AWS ya tiene recursos, comprueba sus dueños y el estado antes de usar bloques de importación. Revisa reemplazos y destrucciones propuestas.

## Equivalencias y traslado

- **Neon → RDS for PostgreSQL.** Conserva PostgreSQL. Verifica versiones, extensiones, roles y conexiones. Elige respaldo y restauración o replicación según volumen e interrupción permitida. Ensaya la restauración, concilia tablas y secuencias y ejecuta el flujo principal contra el destino.
- **Render → ECR y ECS/Fargate.** Reutiliza imágenes OCI. Mantén configuración externa, comprobaciones de salud y permisos de runtime. Aplica migraciones con el mismo artefacto antes de actualizar el servicio cuando el esquema sea compatible con la versión activa. Para cambios incompatibles, usa una transición de esquema por etapas o una parada de escritura y actualización coordinada. Incluye cómo recuperar la compatibilidad si hay que revertir.
- **Cloudflare Pages → Amplify Hosting o S3/CloudFront.** Conserva el build de Vite. Verifica rutas SPA, variables, OAuth, dominio y TLS.
- **Cloudflare R2 → S3.** Copia por lotes, conserva claves y metadatos relevantes, compara inventario y verifica descarga autorizada. Prevé una sincronización final para escrituras durante la copia.
- **WorkOS AuthKit → Cognito si identidad forma parte del traslado.** Comprueba qué se puede exportar. Planifica el vínculo entre identificadores antiguos y nuevos para conservar permisos y referencias. Define acceso para usuarios existentes cuando sus credenciales o sesiones no sean transferibles. Prueba login y autorización antes del cambio.
- **GitHub Actions → AWS CodeBuild/CodePipeline cuando se solicite CI/CD íntegro en AWS.** Mantén pruebas reales por ambiente, artefactos identificables por commit y despliegue de desarrollo. El release management requiere alcance explícito.

## Verificación y corte

Entrega configuración versionada, instrucciones por ambiente y un runbook con ensayo, copia final, criterios de aceptación, cambio de tráfico y reversión. Define cuánto tiempo de interrupción y cuánta pérdida de datos son aceptables. Antes de cambiar el tráfico, confirma el respaldo restaurable, el punto de sincronización final, las comprobaciones de datos y el flujo principal. Si alguna comprobación crítica falla o la copia rebasa la ventana acordada, pausa el corte. Define quién puede escribir durante el corte y cómo reconciliar escrituras antes de regresar al origen. Conserva el origen hasta validar el destino. Los seeds sintéticos se ejecutan por separado en desarrollo o pruebas.

Comprueba el formato y, cuando estén disponibles las dependencias de Terraform, inicializa con el backend desactivado y valida cada raíz sin credenciales de AWS. Con acceso, verifica cuenta y backend de estado por ambiente antes de planificar. Mantén `apply`, traslado de datos reales, cambio de DNS y retirada del origen sujetos a autorización. Reporta qué verificaste y qué sigue pendiente.
