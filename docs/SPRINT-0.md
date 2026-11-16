# Sprint 0 — definición y preparación

**Estado:** documentado; pendiente de validación por el equipo.  
**Propósito:** establecer una base compartida de alcance, arquitectura y preparación antes de iniciar la implementación funcional.

## Objetivo

Dejar el proyecto listo para que el equipo pueda comenzar el Sprint 1 con una visión acordada del producto, límites funcionales, arquitectura inicial, stack propuesto, riesgos principales y decisiones pendientes claramente identificadas.

## Alcance de Sprint 0

- Revisar la propuesta reformulada del sistema y las pantallas de referencia.
- Resolver precedencia de requisitos: la propuesta reformulada es la fuente funcional principal; las pantallas guían el diseño visual, salvo contradicción.
- Delimitar actores, permisos, procesos incluidos y exclusiones.
- Proponer arquitectura modular, tecnologías y modelo de información inicial.
- Identificar flujos críticos, riesgos de privacidad y dependencia de Telegram.
- Preparar este repositorio para documentar cada sprint.
- Definir asuntos que deben validarse con obstetra y enfermería antes de cerrar reglas y formularios.

## Resultado esperado y entregables

1. Descripción general del producto y alcance en [`../README.md`](../README.md).
2. Propuesta técnica, módulos, stack, modelo inicial, controles y secuencia de desarrollo en [`ARCHITECTURE.md`](ARCHITECTURE.md).
3. Registro de pendientes que condicionan decisiones de implementación.
4. Estructura `docs/` como carpeta permanente de documentación por sprint y decisiones del proyecto.

## Criterios de cierre

- El equipo puede explicar quién usa el sistema y qué acciones corresponden a cada rol.
- Está claro que pacientes no tienen cuenta ni vistas de MATERNIA.
- La historia inicial la registra enfermería; las consultas subsecuentes y reglas corresponden a la obstetra.
- Embarazo y planificación familiar usan reglas separadas, versionadas y aprobadas por la obstetra.
- Telegram requiere destinatario verificado, consentimiento vigente y autorización explícita del contenido.
- Inventario, medicamentos, farmacia y portal de pacientes no forman parte del alcance vigente.
- La arquitectura permite avanzar el prototipo con datos ficticios, sin presentarlo como producto clínico validado.
- Los pendientes que requieren validación humana están visibles y asignables antes del incremento correspondiente.

## Decisiones base

| Tema | Decisión propuesta |
|---|---|
| Contexto | Un consultorio obstétrico particular y una obstetra responsable con apoyo de enfermería. |
| Usuarios internos | Obstetra y enfermería; permisos mínimos por función. |
| Paciente | Destinataria externa de comunicaciones; sin autenticación ni vistas. |
| Arquitectura | Monolito modular con frontend web y API REST separados. |
| Stack | React + Vite, TypeScript, NestJS, PostgreSQL, Prisma y Docker Compose para desarrollo local. |
| Seguimiento | Motor determinista basado exclusivamente en reglas versionadas y aprobadas por la obstetra. |
| Mensajería | Telegram detrás de un adaptador sustituible por simulación durante la demo. |
| Datos | Sintéticos en desarrollo, pruebas, capturas y presentaciones. |
| Cadencia | Sprints documentados en `docs/SPRINT-N.md`; duración y fechas se acuerdan con el equipo académico. |

## Pendientes para el siguiente sprint

- Confirmar campos mínimos de historia obstétrica y datos identificativos permitidos.
- Acordar cómo se resuelven posibles duplicados y quién valida identidad/contacto.
- Cerrar permisos detallados de enfermería, correcciones y administración de cuentas.
- Obtener reglas de embarazo y planificación familiar aprobadas, con casos de prueba de referencia.
- Decidir si Telegram se simula o se conecta a un bot real durante la demostración; definir consentimiento y contenido permitido.
- Confirmar entorno de ejecución, versiones del stack y requisitos de evaluación del curso.

## Convención para documentar sprints

Cada sprint tendrá un archivo `SPRINT-N.md` con objetivo, período acordado, historias/alcance, criterios de aceptación, entregables, decisiones, riesgos/bloqueos, revisión y estado de cierre. Las decisiones arquitectónicas duraderas se documentarán además como ADR en `docs/decisions/` con contexto, opciones, decisión y consecuencias. No se deben registrar datos reales de pacientes ni credenciales en estos documentos.
