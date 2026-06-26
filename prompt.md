Estoy desarrollando un sistema web de registro de solicitud de licencia médica para la Policía de Tierra del Fuego (Argentina). La app es mobile-first: en mobile usa una barra de progreso sticky (paso X de Y con título e ícono) y bottom-bar fija con botones Anterior/Siguiente; en desktop usa stepper horizontal y botones inline. Los botones de navegación SIEMPRE están presentes en cada paso (tanto en mobile como desktop) — la bottom-bar es un acceso rápido adicional que solo aparece en mobile. El paso 3 (tercero) se oculta dinámicamente cuando no es atendible, ajustando el conteo total de pasos. Todos los inputs tienen mínimo 46px de alto para touch, con inputmode adecuado (numeric para DNI/legajo, tel para teléfonos). El body usa 100dvh y safe-area-inset-bottom para iPhones.

Campos del sistema:
- Tipo: propia/atendible, nueva/prolongación (campo condicional de nro. licencia anterior)
- Datos del personal policial: legajo, grado policial (14 escalafones desde Agente hasta Comisario General), DNI, CUIL, nombre, fecha nac, género, teléfono, email, dirección, localidad, CP, obra social, nro. afiliado, división/función
- Dropdown de 20 dependencias de la Policía de TDF que al seleccionarse muestra automáticamente el jefe asignado con nombre y email (para notificación automática al ingresar la solicitud)
- Si es atendible: vínculo (9 opciones), nombre, DNI, fecha nac, obra social, diagnóstico del tercero
- Detalle: fechas desde/hasta con validación cruzada, CIE-10, descripción, médico tratante, especialidad (18 opciones), matrícula, institución, teléfono médico, checkbox de internación con campos condicionales (fecha ingreso, sala/piso)
- Upload de archivos en 3 categorías: certificado médico (obligatorio), estudios complementarios, receta/orden médica. Soporte drag & drop, validación de tipo (PDF/JPG/PNG) y tamaño (10MB max), preview con opción de eliminar.
- Validación por paso con mensajes inline, resumen editable por sección (cada sección del resumen es clickeable para volver a ese paso), generación de número de trámite tipo LM-2025-XXXXX
- La notificación al médico tratante NO va por esta vía (se maneja internamente con trigger posterior al ingreso)

Necesito que desarrolles todo el backend:

1. **Backend Node.js + Express + PostgreSQL + Sequelize**: API REST completa con endpoints, request/response bodies, middlewares de autenticación JWT, manejo de errores centralizado, validación con Zod.

2. **Esquema de base de datos**: Todas las tablas SQL con relaciones, índices, constraints, enums. Incluir: licencias, dependencias, jefes, documentos, terceros, usuarios, historial_estados, auditoria.

3. **Notificaciones automáticas**: Al ingresar una solicitud:
   - Email automático al jefe de la dependencia seleccionada informando la licencia del subordinado (Nodemailer con template HTML profesional)
   - Email de acuse de recibo al solicitante
   - NO notificar al médico (trigger interno separado)

4. **Flujo de aprobación**: Ingresada → Pendiente jefe → Aprobada/Rechazada/Observada → Derivada RRHH → Cerrada. Cada cambio genera registro en historial y notificación.

5. **Panel de administración**: Dashboard con estadísticas, listado con filtros avanzados (fecha, dependencia, estado, legajo, grado), cambiar estados, ver documentos, agregar observaciones, exportar PDF/Excel.

6. **Seguridad**: Autenticación por legajo + contraseña, JWT con refresh tokens, roles (personal, jefe_dependencia, rrhh, administrador), permisos por rol, rate limiting, sanitización, logs de auditoría.

7. **Deploy**: Docker Compose con Node.js, PostgreSQL, Nginx. Variables de entorno. Migraciones y seeders con las 20 dependencias y sus jefes.

Dá código completo, separado por archivos, con comentarios en español. Incluye package.json, config Sequelize, modelos, rutas, controladores, middlewares, servicios de email con templates HTML, y Dockerfile.
