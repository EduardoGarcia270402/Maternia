# Ticket 2 — Modelo entidad relación

## Objetivo

Documentar el contenido y revisar consistencia del diagrama ER recibido, para que el modelo físico se valide antes de convertirlo en migraciones.

## Fuente analizada

El archivo editable está en [Anexos/MATERNIA_ER.drawio](Anexos/MATERNIA_ER.drawio). El `.md` de requisitos señala que el modelo físico contiene 24 tablas. Al contar las entidades del diagrama visible aparecen **26**. Esta discrepancia debe reconciliarse con el equipo; no se adopta el número como hecho hasta confirmar qué entidades se mantienen, fusionan o eliminan.

## Entidades del diagrama

1. `persona`
2. `rol`
3. `usuario`
4. `usuario_rol`
5. `paciente`
6. `historia_obstetrica`
7. `tipo_antecedente`
8. `antecedente`
9. `consulta`
10. `historial_correccion`
11. `metodo_planificacion`
12. `episodio`
13. `episodio_embarazo`
14. `episodio_planificacion`
15. `aplicacion_metodo`
16. `regla_seguimiento`
17. `version_regla`
18. `paso_regla`
19. `evento_seguimiento`
20. `cita`
21. `antelacion_cita`
22. `recomendacion`
23. `contacto_telegram`
24. `consentimiento_telegram`
25. `envio_telegram`
26. `bitacora_operacion`

El diseño separa identidad (`persona`), cuenta (`usuario`), rol y paciente destinataria; los episodios tienen supertipo y subtipos; las reglas se versionan y contienen pasos; los eventos enlazan cálculo y confirmación; citas, recomendaciones, consentimiento y entregas Telegram tienen ciclo de vida propio.

## Principios de persistencia

- UUID internos como claves primarias y claves foráneas explícitas.
- Claves únicas para catálogos y relaciones cuando el negocio lo requiere.
- `timestamptz` para instantes auditables y `date` para fechas clínicas calendario.
- Transacciones para altas/cambios que deben persistir juntos; constraints para invariantes.
- No borrar consultas cerradas ni registros que expliquen una decisión; corregir con autor, instante y motivo.
- Versiones de reglas aprobadas son inmutables; una nueva configuración crea versión nueva.
- Datos ficticios en seeds y diagramas de exposición.

## Estados y relaciones significativas

- Una persona interna puede tener usuario y asignación a uno o más roles; validar si se desea permitir más de un rol por cuenta.
- La paciente tiene una historia obstétrica, antecedentes y varios episodios/consultas.
- Un episodio `embarazo` o `planificacion` se especializa en su tabla subtipo.
- Un evento registra fecha clínica, fecha de aviso, regla/versión, entradas y usuario que calcula/confirma.
- Una cita puede relacionarse con un evento, enlazar la cita previa en una reprogramación y enlazar la consulta que la atiende.
- Un envío Telegram se vincula con cita, evento o recomendación, y registra estado, idempotencia y resultado técnico.
- La corrección puede referirse a distintos objetos (`consulta`, `antecedente`, `historia`, `persona`, `episodio` o `aplicacion_metodo`); la consistencia polimórfica requiere decisión técnica.

## Observaciones para revisión antes del esquema SQL

1. **Conteo 24 vs 26:** confirmar inventario definitivo de tablas con la propuesta y el diagrama.
2. **Corrección polimórfica:** `historial_correccion` tiene varias FK opcionales. Definir una sola entidad objetivo por fila y cómo se garantiza con constraint/trigger; considerar tablas de auditoría normalizadas si simplifica integridad.
3. **Contenido de mensaje:** `envio_telegram.contenido_enviado` puede duplicar una recomendación clínica. Definir si se guarda solo plantilla y parámetros mínimos, una referencia/versionado, o el texto aprobado; limitar acceso y retención.
4. **Identidad y duplicados:** definir unicidad, normalización, campos de búsqueda y tratamiento de casos sin identificación o con coincidencias; no registrar números reales en seeds.
5. **Cuenta técnica:** `usuario_rol` permite asignar rol técnico; asegurar con permisos de aplicación/DB que no pueda consultar tablas clínicas. No exponer credenciales de base de datos con acceso amplio al operador.
6. **Telegram:** validar unicidad/ciclo del `telegram_id`, cambio de identificador, revocación y si consentimiento tiene estado explícito además de fecha de revocación.
7. **Recomendaciones:** alinear estados de `recomendacion` con borrador, autorización, revocación y envío; impedir que una recomendación interna se encole.
8. **Fecha y cita:** distinguir fecha objetivo del evento y fecha/hora acordada; definir zona horaria de presentación y semántica de valores `timestamptz`.
9. **Borrado y retención:** precisar baja lógica, retención, eliminación/anominización y respaldo antes de uso real.
10. **Credenciales:** `usuario` guarda hash, nunca contraseña; definir recuperación, expiración, intentos y datos de aprobación.

## Criterios de aceptación del ticket

- El ER editable se incluye en el repositorio y está enlazado desde este documento.
- Las entidades y relaciones principales quedan descritas.
- La discrepancia del conteo de tablas se presenta como pendiente explícito.
- Se registran riesgos de integridad, acceso técnico y persistencia del mensaje.
- No se generan todavía migraciones hasta validar campos, constraints y estados con el equipo.

## Referencias

- [Propuesta reformulada](0%20-%20Propuesta%20reformulada.md), secciones 8, 10, 12 y 16.
- [Diagrama ER editable](Anexos/MATERNIA_ER.drawio).
- [Definición de arquitectura](1%20-%20Definici%C3%B3n%20de%20arquitectura.md).
