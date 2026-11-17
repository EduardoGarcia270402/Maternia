# Ticket 1 — Definición de arquitectura

## Objetivo

Definir límites de módulos y responsabilidades técnicas alineados con la propuesta reformulada y el diagrama de componentes, sin iniciar la implementación.

## Decisión propuesta

Usar un **monolito modular** con una aplicación web, una API REST y PostgreSQL. Separar web y API como procesos, manteniendo un único producto y límites de dominio explícitos. Esta arquitectura reduce complejidad operativa para un solo consultorio y el equipo académico, y permite mantener transacciones consistentes entre consultas, seguimiento y citas.

```text
Personal interno (obstetra / enfermería / administrador técnico)
        │ navegador HTTPS
        ▼
Aplicación web (presentación; sin reglas críticas de negocio)
        │ REST JSON
        ▼
API REST ── verifica identidad y permisos en cada operación
  ├── Acceso y cuentas
  ├── Historia clínica
  ├── Seguimiento y episodios
  ├── Citas y recomendaciones
  ├── Comunicación y consentimiento
  ├── Auditoría
  └── Planificador de envíos
        │ servicios/repositorios
        ▼
PostgreSQL (esquema `maternia`, UUID, restricciones y migraciones)

Adaptador Telegram Bot API ◄── órdenes de envío autorizadas
Respaldo/restauración ◄── operación técnica restringida
```

## Stack sugerido

La propuesta reformulada deja la selección de stack y alojamiento como pendiente. Como base para Sprint 1 se recomienda:

| Capa | Candidata | Consideración |
|---|---|---|
| Lenguaje | TypeScript | Unifica web y servidor y permite compartir contratos. |
| Web | React + Vite | Aplicación interna sin necesidades de SEO. |
| API | NestJS y REST JSON versionada (`/api/v1`) | Módulos de dominio, validación de DTOs y autorización en servidor. |
| Persistencia | PostgreSQL | Transacciones, claves foráneas, constraints y soporte de vistas/triggers SQL cuando se justifique. |
| ORM/migraciones | Prisma + migraciones SQL versionadas | Las funciones, triggers y constraints no representables en Prisma se gestionan con SQL revisado. |
| Entorno local | Docker Compose | Reproducibilidad del prototipo; secretos locales excluidos del control de versiones. |
| Telegram | Adaptador de dominio sobre Bot API | Permite sustituir la API real por simulador y probar lógica sin transmitir mensajes. |

Estas tecnologías son una recomendación, no una decisión aprobada en el documento funcional. Confirmarlas antes de crear el esqueleto del Sprint 1. Fijar versiones en archivos de dependencias al comenzar. No se proponen microservicios ni infraestructura adicional sin una necesidad demostrable.

## Módulos de aplicación

- **Acceso y cuentas:** login, sesiones, roles, aprobaciones, bloqueos y revocaciones.
- **Historia clínica:** pacientes, historia obstétrica, antecedentes, consultas y correcciones.
- **Seguimiento:** episodios, métodos de planificación, reglas versionadas, pasos y eventos.
- **Citas y recomendaciones:** próxima cita acordada, reprogramaciones trazables, recomendaciones y autorización.
- **Comunicación:** contacto y consentimiento Telegram, planificador, idempotencia, estados y resultado técnico.
- **Auditoría:** actor, fecha, acción y recurso; sin texto clínico en la bitácora.
- **Operación técnica:** configuración del bot y respaldo/restauración bajo separación de privilegios.

Los controladores exponen casos de uso; los servicios aplican reglas; los repositorios encapsulan SQL. La capa web no calcula fechas clínicas, no decide estados y no es una frontera de seguridad. PostgreSQL mantiene integridad referencial y restricciones. Usar triggers únicamente para invariantes de base de datos concretas, documentadas y probadas; no duplicar lógica clínica en triggers.

## Roles y separación de privilegios

1. **Obstetra:** acceso asistencial; consulta y corrige historia con trazabilidad, registra consultas y episodios, configura/aprueba reglas, confirma eventos/citas y autoriza mensajes.
2. **Enfermería:** identifica coincidencias, registra historia inicial y realiza correcciones/autorizaciones expresamente delimitadas; no registra las consultas subsecuentes ni configura reglas.
3. **Administrador técnico:** crea/suspende cuentas con aprobación de obstetra, asigna roles, configura bot, realiza backups/restauración y consulta auditoría técnica sin contenido clínico. No abre vistas clínicas.
4. **Paciente:** no tiene cuenta ni vista. Es destinataria externa de comunicaciones autorizadas.

No permitir acceso clínico al administrador técnico en la aplicación ni utilizar su cuenta operativa para consultas a tablas clínicas. Para respaldos, limitar el acceso a funciones/credenciales de infraestructura y auditar acciones; el control técnico del almacenamiento no equivale a autorización clínica. Documentar esta limitación del prototipo y no afirmar aislamiento criptográfico si el backup puede contener datos legibles.

## Telegram

El módulo de comunicación no permite que el bot consulte pacientes o historias. La API genera envíos solo tras validar cita/evento confirmado, contacto verificado, consentimiento vigente y autorización de la obstetra para recomendaciones. Usar una clave idempotente por origen/tipo de mensaje; cancelar envíos obsoletos por cita anulada/reprogramada, evento reemplazado/cerrado o cambio de estado del contacto/consentimiento. Errores temporales tienen reintentos acotados; errores definitivos suspenden o bloquean con motivo y alerta interna.

El enlace `/start` de Telegram inicia la vinculación; la identidad/propiedad del contacto debe verificarse conforme al procedimiento acordado antes de asociarlo a una paciente. En demo debe poder usarse adaptador simulado sin credenciales ni envío externo.

## Seguridad y datos

- Datos ficticios en código, seeds, capturas y evaluación.
- Cuentas individuales, hash robusto, expiración/cierre de sesión, bloqueo por intentos y aprobación/revocación de cuentas.
- Autorización por rol en servidor y validación a nivel del recurso.
- Cifrado de transporte, secretos fuera del repositorio, configuración segura de respaldos y prueba de restauración.
- Evitar datos clínicos en logs, auditoría técnica y mensajes de recordatorio; conservar solo lo que el caso requiere.
- Auditar correcciones, reglas, citas, recomendaciones, permisos, consentimiento, configuración de bot y envíos.
- No usar con datos reales hasta validar requisitos clínicos, jurídicos, privacidad, seguridad, retención e infraestructura.

## Criterios de aceptación del ticket

- La arquitectura distingue presentación, API, dominio, persistencia, comunicación externa y operación técnica.
- Se documentan los tres roles y la prohibición de acceso clínico del administrador técnico.
- La integración Telegram es sustituible por simulador y no tiene acceso directo al repositorio clínico.
- Las reglas y estados quedan en backend; el cliente no es autoridad.
- Se identifican stack e infraestructura como decisión pendiente, con una recomendación explícita.
- La arquitectura coincide con los requisitos RF-12 a RF-15 de la propuesta archivada en Sprint 0.

## Dependencias y pendientes

- Confirmación del stack y alojamiento.
- Definir quién aprueba altas y qué significa acceso a auditoría técnica.
- Diseñar gestión segura de secretos, copias, restauración y alcance de la cuenta técnica.
- Aclarar contenido que se puede persistir en `envio_telegram.contenido_enviado`; minimizar texto sensible.
- Acordar funcionamiento del planificador, período de ejecución, reintentos y canal de contacto alternativo.

## Referencias

- [Propuesta reformulada](0%20-%20Propuesta%20reformulada.md), secciones 3, 5, 6, 8, 10, 12 y 16.
- [Diagrama de componentes](Anexos/MATERNIA_Diagramas_UML.drawio).
- [Modelo ER](Anexos/MATERNIA_ER.drawio).
