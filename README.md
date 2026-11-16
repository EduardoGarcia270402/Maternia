# MATERNIA

MATERNIA es un prototipo académico de sistema de información para un consultorio obstétrico particular. Busca facilitar la continuidad de la atención mediante el registro inicial de la historia clínica por enfermería, la consulta de antecedentes por la obstetra y el seguimiento de episodios de embarazo y planificación familiar con reglas configurables y aprobadas por la profesional.

El sistema contempla cuentas internas para obstetra y enfermería, consultas cronológicas, fechas acordadas para próximas citas y alertas internas. También propone el envío de recordatorios e indicaciones autorizadas por la obstetra a través de Telegram, con verificación del destinatario, consentimiento y trazabilidad. Las pacientes no tienen cuenta ni vistas dentro de MATERNIA.

MATERNIA no diagnostica, prescribe ni reemplaza el criterio profesional. Las reglas de seguimiento deben ser validadas por la obstetra. El proyecto académico debe desarrollarse y demostrarse con datos ficticios; no está validado para uso clínico productivo.

## Alcance

- Registro inicial de pacientes e historia clínica por enfermería, con prevención de duplicados.
- Recuperación de antecedentes e historial en consultas subsecuentes de la obstetra.
- Gestión separada de episodios de embarazo y planificación familiar.
- Reglas versionadas, cálculo explicable de fechas y alertas internas.
- Registro de próximas citas acordadas, sin agenda completa.
- Recomendaciones y comunicaciones autorizadas mediante Telegram.
- Control de acceso por rol y auditoría de operaciones sensibles.

Quedan fuera de alcance el portal de pacientes, inventario, medicamentos, farmacia, recepción como usuario, facturación, gestión completa de agenda, diagnóstico o prescripción automáticos e integración hospitalaria.

## Documentación

- [Arquitectura propuesta](docs/ARCHITECTURE.md)
- [Sprint 0: definición y preparación](docs/SPRINT-0.md)

La propuesta funcional completa se mantiene en [`MATERNIA_PROPUESTA_REFORMULADA.md`](../MATERNIA_PROPUESTA_REFORMULADA.md), fuera de este repositorio.

## Estado del proyecto

Sprint 0 documenta el alcance, las decisiones de arquitectura y la preparación para el desarrollo. La implementación todavía no ha comenzado.
