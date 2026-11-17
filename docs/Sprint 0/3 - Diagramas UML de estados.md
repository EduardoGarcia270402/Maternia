# Ticket 3 — Diagramas UML de estados y comportamiento

## Objetivo

Registrar la semántica de los tres diagramas de estados incluidos en el UML, para que los cambios de estado se implementen de forma consistente y auditable.

## Diagramas recibidos

En [Anexos/MATERNIA_Diagramas_UML.drawio](Anexos/MATERNIA_Diagramas_UML.drawio) se encuentran cinco páginas: componentes, estado de evento de seguimiento, estado de envío Telegram, estado de cita y objeto/estado con cronología.

## Evento de seguimiento

| Estado/condición | Significado |
|---|---|
| Vigente | Evento calculado; fecha clínica y fecha de aviso disponibles. |
| Confirmado | La obstetra revisó la fecha y habilitó recordatorio según reglas. |
| Cumplido | El control se realizó y se enlaza la consulta de cumplimiento. |
| Anulado | Se cierra con motivo registrado. |
| Reemplazado | Una nueva versión calculada del evento ocupa su lugar; conservar relación y motivo. |
| Vencido | **Condición derivada, no estado persistido:** fecha clínica pasó y evento sigue vigente. |

Reglas descritas en el diagrama:

- Al cambiar un dato de entrada se recalcula y el evento nuevo referencia al anterior.
- Si el evento anterior está cumplido, no se recalcula aunque después se corrija una fecha de entrada.
- Un evento tardío puede nacer vencido; la obstetra decide cómo proceder.
- Solo se recuerda un evento confirmado cuando no tiene cita; si hay cita, se programa recordatorio de cita.
- Cerrar el episodio o anular/reemplazar un evento debe cancelar envíos asociados aún pendientes.

## Cita

| Estado | Transición relevante |
|---|---|
| Confirmada | La obstetra registra fecha/hora acordada; puede tener recordatorio pendiente. |
| Atendida | La paciente llega; enlazar la consulta de atención. |
| No asistió | Pasó la hora sin asistencia; el evento sigue confirmado hasta que la obstetra tome una acción. |
| Reprogramada | La cita anterior conserva el historial y apunta a una nueva cita confirmada. |
| Anulada | Se registra con motivo, incluida la corrección de una cita ingresada por error. |

Al salir de `Confirmada`, cancelar el recordatorio pendiente. Un evento puede tener como máximo una cita confirmada. Una cita no asistida no se cierra automáticamente ni crea fecha nueva.

## Envío de Telegram

| Estado | Significado/causa |
|---|---|
| Programado | Al llegar el momento de despacho, el servidor vuelve a revisar condiciones de privacidad y vigencia. |
| Enviado | Telegram acepta el mensaje; guardar resultado técnico e instante. |
| Simulado | Modo demostración sin conexión al bot. |
| Fallido | Error definitivo o intentos agotados; si el bot está bloqueado, suspender el contacto según el diagrama. |
| Cancelado | El envío perdió su motivo (cita reprogramada/anulada, evento reemplazado, episodio cerrado o paciente inactiva); guardar causa. |
| Bloqueado | No se cumple una condición de privacidad; registrar qué condición faltó. |

Precondiciones: paciente activa, contacto verificado, consentimiento vigente y origen (cita/evento/recomendación) aún vigente. Si las condiciones fallan al programar, bloquear con motivo. Los errores temporales se reintentan con límite; cada evento lógico tiene clave idempotente para evitar duplicados. Revalidar consentimiento, contacto y estado de origen inmediatamente antes de transmitir.

## Objeto/estado y cronología de ejemplo

La página de ejemplo usa identidades y valores ficticios para ilustrar cómo se conserva la historia: una corrección de FUM por ecografía deja un evento cumplido sin tocar, reemplaza el evento futuro calculado con la regla anterior y cancela el recordatorio asociado al evento sustituido. La cita atendida queda enlazada con la consulta. Los envíos conservan estado e idempotencia.

Estos valores son datos de demostración, no reglas clínicas. El ejemplo no debe trasladarse como calendario obstétrico por defecto.

## Traducción a persistencia y servicios

- Persistir estados de ciclo de vida (`evento_seguimiento`, `cita`, `envio_telegram`); calcular `vencido` como propiedad derivada para no generar desincronización.
- Implementar transiciones en casos de uso de backend con validación de estado previo y permisos; rechazar transiciones ilegales.
- Registrar timestamp, actor y motivo para transición/corrección; conservar estado previo en auditoría o historial definido en ER.
- Aplicar cancelación e idempotencia en la misma transacción lógica que reprogramación, reemplazo o cierre; despachar externamente solo después de confirmar persistencia.
- Un planificador de envíos debe procesar trabajos elegibles de forma repetible y segura. La estrategia de concurrencia y reintentos se cierra antes de integración real.

## Criterios de aceptación del ticket

- Los estados, causas y transiciones indicadas arriba se pueden mapear a casos de uso.
- `Vencido` está documentado como condición derivada y no como estado guardado.
- Se distingue recordatorio de evento de recordatorio de cita y se define cuál prevalece.
- Reprogramar, reemplazar, cerrar y revocar consentimiento pueden cancelar/bloquear envíos pendientes con causa.
- Cumplimiento y eventos previos se conservan al recalcular.
- La página objeto/estado se reconoce como escenario sintético, no como fuente de valores clínicos.

## Puntos por validar

- Si las notificaciones programadas son requeridas en el prototipo o se simulan manualmente.
- Zona horaria/horario de despacho y comportamiento en cambios de horario/dispositivo.
- Límite de reintentos, ventana de expiración, alertas internas por fallo y canal alternativo autorizado.
- Semántica de `Simulado`: estado del envío persistido o modo de ejecución independiente.
- Cómo se representa la cancelación/revocación de consentimiento en las tablas ER y quién puede ejecutarla.
- El archivo contiene cinco páginas. Mantener esta lista de páginas al actualizar el UML.

## Referencias

- [UML editable](Anexos/MATERNIA_Diagramas_UML.drawio).
- [Modelo ER](2%20-%20Modelo%20entidad%20relaci%C3%B3n.md).
- [Propuesta reformulada](0%20-%20Propuesta%20reformulada.md), requisitos RF-09 a RF-15.
