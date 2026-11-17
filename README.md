# MATERNIA

MATERNIA es un prototipo académico de sistema de información para un consultorio obstétrico particular. Busca facilitar la continuidad de la atención mediante el registro inicial de la historia clínica por enfermería, la consulta de antecedentes por la obstetra y el seguimiento de episodios de embarazo y planificación familiar con reglas configurables y aprobadas por la profesional.

El sistema contempla cuentas internas para obstetra, enfermería y administrador técnico. El administrador técnico gestiona cuentas, respaldos y configuración del bot, sin acceso a datos clínicos. El sistema registra consultas cronológicas, fechas acordadas para próximas citas y alertas internas. También propone el envío de recordatorios e indicaciones autorizadas por la obstetra a través de Telegram, con verificación del destinatario, consentimiento y trazabilidad. Las pacientes no tienen cuenta ni vistas dentro de MATERNIA.

MATERNIA no diagnostica, prescribe ni reemplaza el criterio profesional. Las reglas de seguimiento deben ser validadas por la obstetra. El proyecto académico debe desarrollarse y demostrarse con datos ficticios; no está validado para uso clínico productivo.

## Alcance

- Registro inicial de pacientes e historia clínica por enfermería, con prevención de duplicados.
- Recuperación de antecedentes e historial en consultas subsecuentes de la obstetra.
- Gestión separada de episodios de embarazo y planificación familiar.
- Reglas versionadas, cálculo explicable de fechas y alertas internas.
- Registro de próximas citas acordadas, sin agenda completa.
- Recomendaciones y comunicaciones autorizadas mediante Telegram.
- Control de acceso por rol y auditoría de operaciones sensibles, sin contenido clínico en la bitácora técnica.

Quedan fuera de alcance el portal de pacientes, inventario, medicamentos, farmacia, recepción como usuario, facturación, gestión completa de agenda, diagnóstico o prescripción automáticos e integración hospitalaria.

## Documentación

- [Sprint 0 — resumen](docs/Sprint%200/Sprint%200.md)
- [Ticket 1 — Definición de arquitectura](docs/Sprint%200/1%20-%20Definici%C3%B3n%20de%20arquitectura.md)
- [Ticket 2 — Modelo entidad relación](docs/Sprint%200/2%20-%20Modelo%20entidad%20relaci%C3%B3n.md)
- [Ticket 3 — Diagramas UML de estados](docs/Sprint%200/3%20-%20Diagramas%20UML%20de%20estados.md)
- [Insumos y diagramas editables](docs/Sprint%200/Anexos/)

La propuesta reformulada y los diagramas fuente están archivados en la carpeta del Sprint 0 para mantener trazabilidad dentro del repositorio.

## Estado del proyecto

Sprint 0 documenta el alcance, las decisiones de arquitectura y la preparación para el desarrollo. El stack y varios detalles del modelo permanecen sujetos a validación; la implementación todavía no ha comenzado.
