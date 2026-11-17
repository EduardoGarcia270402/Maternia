# Sprint 0 — descubrimiento, alcance y base arquitectónica

**Estado:** documentación inicial preparada; decisiones técnicas y funcionales sujetas a validación.  
**Propósito:** consolidar los insumos y definir una base verificable antes de construir funcionalidades.

## Objetivo

Dejar acordados el alcance del prototipo, sus actores, las responsabilidades de cada perfil, los límites de seguridad, el modelo de dominio inicial y la arquitectura de referencia. Sprint 0 organiza y documenta el trabajo preparatorio; no entrega funcionalidades implementadas.

## Fuente de verdad y precedencia

La propuesta reformulada en [0 - Propuesta reformulada.md](0%20-%20Propuesta%20reformulada.md) es la referencia funcional vigente. Los diagramas de componentes, estados y ER incluidos en [Anexos](Anexos/) detallan la arquitectura y el modelo a revisar. Las pantallas previamente proporcionadas son referencias visuales y no deben reintroducir funcionalidades que el documento excluye.

## Tickets de Sprint 0

| Ticket | Entregable | Estado |
|---|---|---|
| 1 | [Definición de arquitectura](1%20-%20Definici%C3%B3n%20de%20arquitectura.md) | Propuesta documentada; stack/infraestructura pendiente de decisión. |
| 2 | [Modelo entidad relación](2%20-%20Modelo%20entidad%20relaci%C3%B3n.md) | Análisis del diagrama de 24 tablas y puntos de validación. |
| 3 | [Diagramas UML de estados](3%20-%20Diagramas%20UML%20de%20estados.md) | Semántica documentada; requiere revisión funcional. |

## Alcance acordado en la propuesta

- Un consultorio obstétrico con obstetra responsable y apoyo de enfermería.
- Tres perfiles internos: obstetra, enfermería y administrador técnico. Este último opera cuentas, respaldos y configuración del bot, sin acceso a información clínica.
- Enfermería registra la historia inicial; la obstetra atiende consultas posteriores, administra episodios y reglas, y autoriza las comunicaciones clínicas.
- Pacientes no tienen cuenta ni vistas; reciben solo comunicaciones externas autorizadas por Telegram.
- Seguimiento de embarazo y planificación familiar mediante reglas separadas, aprobadas y versionadas.
- Solo se registra la próxima cita acordada; no se gestiona disponibilidad ni agenda completa.
- Se usan datos ficticios; no se presenta el prototipo como producto clínico validado.

Exclusiones principales: inventario, lotes, medicamentos, farmacia, recepción como usuario, facturación, otras especialidades, integración hospitalaria, diagnósticos, triaje y prescripción automáticos.

## Criterios de cierre

- Los tres perfiles tienen responsabilidades y permisos mínimos diferenciados.
- El administrador técnico no puede consultar historias, consultas, reglas ni contenido de mensajes desde la aplicación.
- La historia inicial y las consultas subsecuentes son flujos diferentes, con autoría y trazabilidad.
- Embarazo y planificación familiar no comparten reglas clínicas implícitas.
- El modelo ER y los diagramas de estados son consistentes con los requisitos funcionales y sus límites.
- Cada riesgo y pendiente con impacto en arquitectura está registrado en el ticket correspondiente.
- La estructura `docs/Sprint N/` queda establecida para documentar los siguientes sprints y sus tickets.

## Pendientes que pasan al Sprint 1

1. Validar campos mínimos de historia, identificación permitida y prevención/resolución de duplicados.
2. Definir matriz detallada de permisos y aprobación de altas de cuentas.
3. Revisar las restricciones, estados e historial de correcciones del modelo ER antes de crear migraciones.
4. Obtener reglas aprobadas y casos de referencia para embarazo y planificación.
5. Decidir simulación o conexión de Telegram; definir enrolamiento `/start`, consentimiento, contenido permitido, revocación y manejo de errores.
6. Confirmar stack, despliegue, copias de seguridad/restauración y límites operativos de la cuenta técnica.
7. Resolver cómo se almacenará evidencia de mensajes sin conservar más contenido sensible del necesario.

## Convención documental por sprint

Cada sprint tendrá una carpeta `docs/Sprint N/`, un archivo de resumen `Sprint N.md` y un archivo Markdown por ticket con el formato `N - Título del ticket.md`. Se incluirán objetivo, alcance, criterios de aceptación, entregables, decisiones, riesgos y estado. Los diagramas editables y fuentes se guardarán en `Anexos/` dentro del sprint correspondiente. Las decisiones duraderas pueden registrarse como ADR en `docs/decisions/`.
