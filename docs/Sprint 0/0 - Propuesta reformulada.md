# MATERNIA — Propuesta reformulada del sistema de información

**Versión:** propuesta académica para revisión del equipo y validación de la obstetra  
**Fecha:** 10 de noviembre de 2026  
**Contexto institucional:** Universidad de las Fuerzas Armadas - ESPE, Ingeniería de Software, NRC 36847  
**Equipo:** Eduardo García y Mateo Iza

> Este documento reformula el alcance inicial a partir del contexto maestro del proyecto, los cinco entregables académicos y las observaciones de revisión compartidas por el equipo. Las decisiones que no constan como aprobadas se identifican como propuestas o pendientes de validación. El sistema es un prototipo/proyecto académico y no debe presentarse como producto clínico validado.

## 1. Resumen ejecutivo

MATERNIA será un sistema de información para **un consultorio obstétrico particular atendido por una obstetra con apoyo de enfermería**. Enfermería toma la historia clínica en la primera atención; para las siguientes citas, la paciente va directamente con la obstetra según la cita y esta recupera la historia existente. Para el desarrollo del prototipo, Telegram será el canal de envío de recordatorios e indicaciones autorizadas. La paciente no tiene cuenta ni vistas en MATERNIA.

La primera versión incluye tres perfiles internos: **obstetra**, **enfermería** y **administrador técnico**. El administrador técnico gestiona cuentas, respaldos y la configuración del bot sin acceso a datos clínicos. Enfermería registra la historia inicial; la obstetra atiende consultas posteriores, revisa historias, registra consultas, configura reglas y autoriza indicaciones. Las pacientes no acceden al sistema ni tienen vistas; reciben por Telegram recordatorios y mensajes autorizados. Se excluyen recepción, inventario, medicamentos y otras especialidades.

El sistema apoya la organización y continuidad de la atención; no diagnostica, prescribe, determina urgencias ni sustituye el criterio profesional. Toda regla clínica debe ser validada y configurada por la obstetra.

## 2. Antecedentes y problema

En el proceso actual se vuelven a buscar o transcribir datos de antecedentes en consultas posteriores. El seguimiento de controles de embarazo y de fechas de revisión o renovación de métodos anticonceptivos depende de registros y revisiones manuales. Además, las indicaciones entregadas durante una consulta pueden quedar dispersas y la paciente puede olvidar la fecha o las recomendaciones comunicadas.

Esto puede aumentar el tiempo dedicado a tareas administrativas, generar inconsistencias entre registros y dificultar identificar seguimientos pendientes. Son riesgos del proceso observados en los documentos académicos; todavía no se cuenta con mediciones que demuestren una reducción o mejora cuantitativa.

### 2.1 Necesidad que atiende MATERNIA

Centralizar la historia que enfermería registra inicialmente; permitir a la obstetra recuperarla en citas posteriores; calcular fechas con reglas aprobadas; mostrar pendientes a la obstetra; y comunicar recordatorios e indicaciones autorizadas a la paciente por Telegram, sin acceso al sistema.

## 3. Delimitación del contexto de uso

| Decisión | Definición para esta propuesta |
|---|---|
| Tipo de establecimiento | Un consultorio obstétrico particular. |
| Equipo asistencial | Una obstetra responsable y personal de enfermería de apoyo. |
| Usuarios internos | Obstetra, enfermería y administrador técnico (sin acceso a datos clínicos), con permisos diferenciados. |
| Registro inicial | Enfermería identifica a la paciente, revisa posibles duplicados y toma/ingresa la historia clínica en su primera atención. |
| Citas posteriores | La paciente acude directamente con la obstetra según la cita acordada. La obstetra recupera la historia ya registrada; no se repite el registro inicial. |
| Paciente | No tiene cuenta, acceso ni vistas en MATERNIA. Recibe recordatorios e indicaciones autorizadas mediante Telegram. |
| Escala | Un consultorio y una obstetra responsable; no se contempla una red de centros ni múltiples especialidades. |
| Límites | No se incluye recepción como usuario, farmacia, inventario ni gestión de medicamentos. |

La enfermera participa en la primera toma de historia y en las tareas que la obstetra le autorice expresamente. La atención clínica y las citas de seguimiento corresponden directamente a la obstetra. Telegram es un canal externo de comunicación; no es una interfaz de acceso a MATERNIA.

## 4. Objetivos

### 4.1 Objetivo general

Diseñar y construir una propuesta académica para un consultorio obstétrico particular que permita registrar la historia clínica inicial mediante enfermería, recuperarla en las citas posteriores con la obstetra, apoyar el seguimiento con reglas aprobadas y comunicar recordatorios e indicaciones autorizadas a la paciente por Telegram, sin darle acceso al sistema.

### 4.2 Objetivos específicos

1. Describir y validar con la obstetra y enfermería el proceso actual, los datos iniciales y las necesidades de seguimiento.
2. Diseñar un modelo que diferencie paciente, historia clínica, episodios de embarazo/planificación, consultas, reglas, citas, recomendaciones y mensajes.
3. Permitir a enfermería registrar la historia clínica en la primera atención y a la obstetra reutilizarla en las siguientes citas sin transcribir nuevamente antecedentes.
4. Especificar por separado las reglas de fechas para embarazo y para revisión o renovación de métodos anticonceptivos.
5. Mostrar alertas internas a la obstetra y enviar por Telegram recordatorios e indicaciones que ella autorice.
6. Definir creación y permisos de cuentas internas para obstetra, enfermería y administrador técnico, y validación del contacto/consentimiento Telegram de las pacientes.
7. Evaluar la propuesta con datos ficticios y escenarios de registro inicial, consulta subsecuente, reglas, permisos y mensajes.

## 5. Alcance

### 5.1 Incluido

- Búsqueda de pacientes y validación de posibles duplicados.
- Toma y registro de la historia clínica inicial por enfermería en la primera atención.
- Recuperación de antecedentes y consultas previas por la obstetra en las citas posteriores.
- Registro cronológico de consultas subsecuentes por la obstetra, sin sobrescribir atenciones anteriores.
- Gestión separada de episodios de embarazo y planificación familiar.
- Reglas de seguimiento configuradas y aprobadas por la obstetra, con versión y vigencia.
- Cálculo de fechas y alertas internas para la obstetra.
- Registro de la fecha y hora acordadas para una próxima cita, sin gestionar agenda ni disponibilidad.
- Registro de recomendaciones por la obstetra y autorización explícita de qué comunicar.
- Envío de recordatorios de cita y recomendaciones autorizadas por Telegram, con contacto verificado, consentimiento y trazabilidad del envío.
- Cuentas internas para obstetra, enfermería y administrador técnico, con permisos diferenciados.
- Administración técnica sin acceso a datos clínicos: cuentas, respaldos y configuración del bot.
- Pruebas funcionales y de usabilidad con datos ficticios.

### 5.2 Fuera de alcance

- Cuenta, inicio de sesión, portal, pantalla o vista para pacientes. La paciente no accede a MATERNIA.
- Inventarios, lotes, existencias, caducidades, compras, medicamentos y registro de consumo.
- Farmacia, recepción como usuario y otras especialidades médicas.
- Agenda completa, disponibilidad de turnos, pagos, facturación, cancelaciones o reprogramación autónoma.
- Integraciones con hospitales, laboratorios, redes públicas u otros sistemas clínicos.
- Diagnóstico, triaje, pronóstico o prescripción automáticos.
- Envío por Telegram de información sensible no autorizada por la obstetra.
- Uso productivo o de historias clínicas reales en las pruebas académicas.

> Telegram reemplaza cualquier propuesta previa de portal paciente (excluido), correo, SMS o WhatsApp. Los mensajes se limitarán a comunicaciones autorizadas; se verificará el destinatario y el consentimiento antes de enviar.

## 6. Actores, interesados y responsabilidades

### 6.1 Actores del sistema

| Actor | Responsabilidades y permisos propuestos |
|---|---|
| Obstetra | Consultar historias; registrar consultas subsecuentes; gestionar episodios; configurar y aprobar reglas; revisar alertas; confirmar próximas citas; crear recomendaciones; autorizar mensajes; aprobar las cuentas internas y sus permisos. |
| Enfermería | Buscar coincidencias y registrar la historia clínica inicial durante la primera atención; completar o corregir datos iniciales autorizados con trazabilidad. No configura reglas ni sustituye la consulta de seguimiento con la obstetra. |
| Administrador técnico | Crear y suspender cuentas aprobadas por la obstetra; asignar roles; ejecutar respaldos y restauración; configurar el bot; consultar la bitácora de operaciones (sin contenido clínico). No abre historias, consultas, reglas ni mensajes. Durante el prototipo lo ejerce el equipo desarrollador. |
| Paciente (destinataria externa) | No inicia sesión ni usa vistas de MATERNIA. Recibe recordatorios e indicaciones autorizadas por Telegram. |
| Bot de Telegram | Servicio externo que transmite mensajes al identificador validado y devuelve estado técnico. No consulta historias ni elige destinatario o contenido. |

### 6.2 Interesados

| Interesado | Interés / poder | Participación |
|---|---|---|
| Obstetra | Alto / alto | Valida el proceso, reglas, permisos, citas e indicaciones que se comunicarán. |
| Enfermería | Alto / medio | Valida el flujo de identificación y registro inicial de la historia clínica. |
| Administrador técnico | Medio / medio | Crea y suspende cuentas con la aprobación de la obstetra, gestiona respaldos y bot; no accede a datos clínicos. |
| Pacientes destinatarias | Alto / medio | Confirman comprensión y aceptación del canal y de los mensajes; no son usuarias del sistema. |
| Equipo desarrollador | Alto / medio | Analiza, diseña, construye, verifica y documenta; no aprueba reglas clínicas. |
| Docente | Medio / medio | Revisa entregables académicos, no reglas clínicas ni operación asistencial. |

La recepción no participa en el TO-BE. La enfermera sí es usuaria directa, con alcance limitado al registro inicial y a las operaciones expresamente autorizadas.

## 7. Procesos AS-IS y TO-BE

### 7.1 Proceso actual (AS-IS) resumido

1. La paciente solicita o asiste a una atención.
2. En la primera atención, enfermería recoge datos e historia clínica.
3. En visitas posteriores, la obstetra revisa registros previos y atiende a la paciente según la cita.
4. Los controles y fechas de revisión/renovación pueden depender de registros y revisiones manuales.
5. La obstetra comunica fechas e indicaciones; existe riesgo de olvido o pérdida del mensaje.

### 7.2 Proceso propuesto (TO-BE)

1. En la primera atención, enfermería busca posibles coincidencias. Si no existe registro, toma e ingresa la historia inicial; si hay coincidencias dudosas, detiene el alta y solicita verificación.
2. En las citas posteriores, la paciente pasa directamente con la obstetra según la cita acordada.
3. La obstetra busca la ficha existente y MATERNIA recupera antecedentes e historia para revisión.
4. La obstetra registra la consulta actual sin sobrescribir la historia inicial ni consultas anteriores.
5. La obstetra actualiza el episodio correspondiente: embarazo o planificación familiar.
6. MATERNIA calcula fechas con la regla aprobada y presenta la versión utilizada; la obstetra confirma el seguimiento.
7. El sistema muestra alertas internas a la obstetra.
8. La obstetra registra la próxima cita y redacta recomendaciones. Revisa destinatario, consentimiento y contenido, y autoriza el envío por Telegram.
9. MATERNIA envía el mensaje por Telegram y conserva estado del envío. La paciente lo recibe fuera del sistema; no hay vista de paciente (no existe).

### 7.3 Correcciones al BPMN

- En el flujo inicial, incluir enfermería como responsable de búsqueda y registro de la historia.
- En el flujo posterior, la paciente pasa directamente con la obstetra conforme a la cita; la historia se recupera y no se vuelve a tomar como alta nueva.
- Usar compuertas solo para decisiones reales con pregunta y salidas rotuladas, por ejemplo “¿Existe una ficha coincidente?” con “Sí/No”.
- No incluir recepción ni paciente como usuario de MATERNIA. La paciente sí es participante externa que recibe una comunicación autorizada.
- La obstetra confirma las fechas y el contenido clínico; el sistema calcula y transmite únicamente tras autorización.

## 8. Requisitos funcionales y criterios de aceptación

La línea base propuesta contiene estos requisitos. Se excluyen inventario y farmacia; enfermería participa solo en la historia inicial. La paciente no tiene cuenta ni interfaz. Los valores clínicos se definirán con la obstetra.

| ID | Requisito | Criterio de aceptación |
|---|---|---|
| RF-01 | Buscar paciente y revisar coincidencias | Obstetra/enfermería pueden buscar por identificador permitido. Si hay coincidencia ambigua, el sistema no abre una ficha automáticamente ni crea otra. |
| RF-02 | Registrar historia clínica inicial | En la primera atención, enfermería completa campos obligatorios; el sistema valida formato y datos faltantes y conserva autoría/fecha. |
| RF-03 | Recuperar historia en citas posteriores | Cuando la obstetra abre una ficha existente, antecedentes y consultas previas aparecen para revisión sin requerir transcripción repetida. |
| RF-04 | Registrar consulta subsecuente | La obstetra guarda la consulta actual en secuencia cronológica; los datos anteriores no se sobrescriben. |
| RF-05 | Gestionar episodios | La obstetra registra y actualiza episodios separados de embarazo y planificación familiar. |
| RF-06 | Configurar reglas | Solo la obstetra puede aprobar/configurar reglas; se guarda versión, vigencia y responsable. Una regla incompleta no se activa. |
| RF-07 | Calcular fechas de embarazo | Con datos requeridos y regla vigente aprobada, se muestran fechas, entradas y versión. Si falta información, se solicita revisión y no se inventa un cálculo. |
| RF-08 | Calcular revisión/renovación de planificación | Se utiliza la regla específica del método y su fecha; no se reutiliza el calendario de embarazo. |
| RF-09 | Mostrar alertas internas | La obstetra ve eventos próximos/vencidos y sus fechas. El semáforo expresa proximidad/atraso, no gravedad clínica. Ejecuciones repetidas no duplican una alerta. |
| RF-10 | Registrar próxima cita | La obstetra registra fecha/hora acordadas. No se administra disponibilidad ni agenda completa. |
| RF-11 | Registrar y autorizar recomendaciones | La obstetra vincula recomendación a consulta y decide explícitamente si se comunica por Telegram. Lo interno no se envía. |
| RF-12 | Gestionar usuarios internos | El administrador técnico, con aprobación de la obstetra, habilita cuentas individuales de obstetra, enfermería y administrador técnico, con autenticación, permisos mínimos y revocación. El administrador técnico no accede a datos clínicos. No se crean cuentas para pacientes. |
| RF-13 | Vincular y verificar Telegram | Antes de vincular el identificador de Telegram a una paciente, se verifica el contacto y se obtiene consentimiento para el propósito indicado. |
| RF-14 | Enviar recordatorios por Telegram | Solo se envía ante cita/evento confirmado, destinatario verificado y consentimiento vigente. Se registra fecha, destinatario y resultado técnico. |
| RF-15 | Proteger y auditar operaciones | Se registran consultas, cambios de historia/reglas/citas, autorizaciones y envíos con usuario/fecha; no se guardan secretos en bitácoras. |

### 8.1 Diferenciación de necesidades

1. **Recuperación de antecedentes:** evita repetir el registro y permite a la obstetra consultar lo que enfermería documentó inicialmente. No calcula fechas.
2. **Renovación de planificación familiar:** usa método, fecha y regla específica para generar evento de revisión/renovación.
3. **Controles de embarazo:** usa datos del episodio de embarazo y una regla independiente para calcular controles y alertas.

### 8.2 Requisitos no funcionales propuestos

| ID | Calidad y criterio | Verificación |
|---|---|---|
| RNF-01 | Seguridad: obstetra, enfermería y administrador técnico autentican cuentas individuales; las operaciones se autorizan en el servidor según rol. | Pruebas positivas y negativas de permisos en cada cambio de rol. |
| RNF-02 | Integridad: historia inicial y consultas previas no se sobrescriben; las correcciones registran autor, fecha y motivo. | Prueba de corrección y consulta de historial de cambios. |
| RNF-03 | Privacidad: ningún mensaje Telegram se envía sin consentimiento vigente y destinatario verificado. | 100% de intentos no autorizados bloqueados en escenarios de prueba. |
| RNF-04 | Rendimiento: búsqueda y apertura de historia ficticia en ≤3 s en el entorno de demostración definido. | Registrar equipo, navegador, red y tamaño de muestra. |
| RNF-05 | Exactitud: el 100% de los escenarios de fechas aprobados por la obstetra coincide con su referencia (mínimo 20: normales, límite y excepción). | Conjunto de casos, por cada cambio de regla. |
| RNF-06 | Usabilidad: al menos 4 de 5 casos ficticios de registro inicial se completan sin asistencia del equipo. | Prueba moderada con enfermería o participantes que representen el rol. |
| RNF-07 | Disponibilidad y recuperación: existe un respaldo de la base de datos de demostración y un procedimiento de restauración probado. | Restaurar un respaldo de prueba y comprobar que historia, consultas y bitácora están completas. |

Las metas son criterios iniciales de evaluación, no resultados ya alcanzados.

## 9. Reglas de negocio y fechas

No se inventarán intervalos médicos. La obstetra aprobará y documentará las reglas de embarazo y planificación familiar antes de su uso en pruebas de aceptación.

Cada regla debe indicar: identificador y versión; proceso al que aplica; datos requeridos; fórmula o intervalo; unidad y tratamiento de calendario; umbral para aviso previo; condición de vencimiento; fecha de vigencia; autora/aprobadora; ejemplos de cálculo; y qué mostrar cuando falte un dato.

Los recordatorios se envían por Telegram, según evento, antelación y consentimiento vigente. El cálculo de la fecha clínica y la programación del mensaje son pasos distintos. Cambiar un umbral de aviso no debe modificar la fecha clínica calculada. Si cambian los datos de entrada, las fechas se vuelven a evaluar y se conserva el valor anterior y el motivo del cambio para auditoría.

## 10. Política mínima de seguridad y privacidad

1. **Finalidad y minimización:** guardar solo datos necesarios para atención obstétrica y funcionamiento del seguimiento; usar datos ficticios en desarrollo, pruebas, capturas y presentaciones.
2. **Identidad y destinatario verificados:** antes de asociar un identificador de Telegram con una paciente, enfermería u obstetra verifica el contacto conforme al procedimiento del consultorio y registra el consentimiento. Un cambio de identificador suspende los envíos hasta repetir la verificación.
3. **Autenticación:** cuentas individuales; no compartir credenciales; contraseñas con hash robusto; recuperación de acceso con verificación; cierre de sesión y expiración de sesiones; limitar intentos repetidos según decisión técnica.
4. **Autorización por rol:** obstetra, enfermería y administrador técnico acceden solo a las funciones necesarias para sus responsabilidades; el administrador técnico no abre datos clínicos y la bitácora no guarda contenido clínico. La paciente no inicia sesión ni consulta información dentro de MATERNIA. El servidor valida permisos en cada operación.
5. **Separación de contenido:** los mensajes de cita son discretos y no incluyen diagnósticos ni historia clínica. Una recomendación se envía por Telegram solo si la obstetra la autoriza explícitamente y el consentimiento cubre ese tipo de mensaje. La aplicación no ofrece una vista a la paciente.
6. **Integridad y trazabilidad:** conservar consultas anteriores; registrar cambios de historia, regla, cita, recomendación y cuenta. Enfermería registra la historia inicial; la obstetra registra las atenciones posteriores. Registrar actor, instante y operación; no guardar credenciales en bitácoras.
7. **Transmisión y almacenamiento:** usar conexiones cifradas en todo despliegue; restringir acceso al almacenamiento; gestionar secretos fuera del repositorio; respaldos protegidos y procedimiento de restauración documentado antes de un uso real.
8. **Baja y respuesta:** las cuentas internas pueden suspenderse o revocarse. Ante pérdida o cambio del identificador de Telegram, bloquear temporalmente los envíos, revisar el destinatario y volver a verificarlo antes de reactivar mensajes. Retención y eliminación deben definirse antes de uso real.
9. **Límite académico:** esta política es una especificación mínima, no certifica cumplimiento legal ni habilita el uso de historias reales. El despliegue real requiere revisión jurídica, institucional y de seguridad.

### 10.1 Cuentas internas y vinculación de Telegram

1. El administrador técnico, con aprobación de la obstetra, crea una cuenta individual para la obstetra, enfermería u otro administrador y asigna el rol correspondiente.
2. Antes de habilitarla, confirma la identidad laboral y el permiso mínimo requerido. Las cuentas pueden suspenderse o revocarse.
3. Durante la atención, enfermería u obstetra solicita a la paciente el identificador de Telegram y su autorización para recibir recordatorios y, si corresponde, recomendaciones.
4. El consultorio verifica que el identificador pertenece a la paciente antes de asociarlo a la ficha. Un cambio de usuario/contacto suspende envíos hasta nueva verificación.
5. Antes de cada envío, MATERNIA valida cita/evento, destinatario, consentimiento y autorización de la obstetra.
6. Si cualquiera de estas condiciones falla, el envío se bloquea y se registra el motivo. La paciente nunca obtiene credenciales ni vistas del sistema.

## 11. Riesgos vinculados a vulnerabilidades y controles

| Vulnerabilidad / debilidad | Riesgo relacionado | Prob. / impacto inicial | Control preventivo y respuesta | Evidencia de verificación |
|---|---|---|---|---|
| Reglas de fecha incompletas, ambiguas o desactualizadas | Alerta incorrecta, omisión de seguimiento o recordatorio a destiempo | Media / Alta | Regla con versión, responsable, vigencia, ejemplos aprobados y revisión de la obstetra; detener cálculo si faltan parámetros; corregir y recalcular trazablemente. | Casos de frontera con fecha esperada revisada y evidencia de versión aplicada. |
| Permisos internos demasiado amplios | Enfermería accede a funciones clínicas o reglas que no corresponden a su rol | Media / Alta | Aplicar permisos mínimos en servidor: enfermería registra la historia inicial, la obstetra gestiona seguimiento y consultas posteriores, y el administrador técnico no accede a datos clínicos. Suspender credenciales y revisar bitácora ante acceso indebido. | Pruebas positivas y negativas por rol para registro inicial, consultas, reglas y alertas. |
| Modificación accidental o no atribuible de la historia | Pérdida de integridad clínica y contradicciones entre consultas | Media / Alta | No sobrescribir notas cerradas; correcciones con autor, fecha y motivo; respaldos protegidos; validación y transacciones. | Editar/corregir una consulta y verificar historial y restauración de respaldo de prueba. |
| Identificador Telegram equivocado o asociado a otra paciente | Recordatorio o recomendación llega a una persona no prevista | Media / Alta | Verificar el identificador antes de asociarlo; confirmar destinatario antes de envío; suspender mensajes ante cambio o duda y revisar bitácora. | Pruebas con identificador incorrecto, cambio de contacto y destinatario no verificado; no debe enviarse mensaje. |
| Recomendación enviada sin autorización o al destinatario equivocado | Exposición de información que la obstetra no aprobó para Telegram | Media / Alta | Recomendación no se envía por defecto; autorización explícita, revisión de contenido y confirmación del destinatario; registrar envío. | Pruebas de borrador/no autorizado, consentimiento ausente y destinatario incorrecto; envío bloqueado. |
| Canal Telegram no disponible o entrega duplicada | La paciente no recibe el recordatorio o lo recibe repetidamente | Media / Media | Identificador idempotente, estado de envío y reintentos limitados; informar a la obstetra para que utilice el procedimiento de contacto alternativo aprobado. | Simular fallo, reintento y ejecución repetida sin duplicar avisos. |
| Fuga de datos por pruebas, capturas o credenciales | Exposición fuera del sistema | Baja / Alta | Datos estrictamente ficticios; no registrar secretos; repositorio sin credenciales; limitar acceso a evidencias y eliminarlas conforme a política del curso. | Revisión de datos de prueba y configuración antes de demostración. |

Probabilidad e impacto son valoraciones iniciales cualitativas del equipo, no estadísticas observadas. Cada riesgo se relaciona directamente con una debilidad concreta y tiene controles verificables.

## 12. Modelo conceptual de información

Entidades propuestas: **CuentaInterna**, **Paciente**, **HistoriaObstétrica**, **Antecedente**, **EpisodioEmbarazo**, **EpisodioPlanificación**, **Consulta**, **ReglaSeguimiento**, **VersiónRegla**, **EventoSeguimiento**, **Cita**, **Recomendación**, **ContactoTelegram**, **ConsentimientoTelegram** y **RegistroAuditoría**. La paciente es un registro clínico y destinataria externa, no una cuenta de usuario. En el modelo físico (carpeta `bdd/`) estas entidades se desarrollan en 24 tablas: se agregan persona, rol, usuario_rol (un usuario puede tener varios roles), tipo_antecedente, metodo_planificacion, episodio con sus dos subtipos, paso_regla, historial_correccion y envio_telegram.

Relaciones esenciales:

- Una paciente tiene una historia obstétrica y varias consultas.
- Una consulta puede asociarse a un episodio de embarazo o planificación familiar; no se fuerza que tenga ambos.
- Un episodio puede producir eventos calculados según una versión de regla identificada.
- Una cita se vincula con paciente y, cuando proceda, con episodio/evento de seguimiento.
- Una recomendación se vincula con paciente y consulta; su estado distingue borrador, interna y autorizada para envío por Telegram.
- Un contacto Telegram verificado se enlaza con una sola paciente después de comprobar el destinatario y obtener consentimiento.
- Un registro de auditoría identifica la cuenta interna que ejecutó una operación o autorizó un envío, sin almacenar secretos de autenticación.

## 13. Casos de uso propuestos

### Obstetra

- Iniciar sesión.
- Buscar/registrar paciente y prevenir duplicados.
- Consultar y actualizar historia clínica con trazabilidad.
- Registrar consulta y episodio obstétrico o de planificación familiar.
- Configurar reglas de seguimiento.
- Consultar fechas calculadas, alertas y bandeja de pendientes.
- Confirmar fecha próxima de cita.
- Redactar una recomendación y autorizar su envío por Telegram.
- Aprobar las cuentas internas y revisar el registro de envíos Telegram.

### Enfermería

- Iniciar sesión con su cuenta interna.
- Buscar posibles coincidencias y registrar la historia clínica durante la primera atención.
- Completar o corregir datos iniciales autorizados, conservando trazabilidad.
- Registrar y verificar el contacto Telegram y el consentimiento según el procedimiento acordado.
- No configura reglas ni registra las consultas de seguimiento que corresponden a la obstetra.

### Administrador técnico

- Iniciar sesión con su cuenta interna.
- Crear y suspender cuentas aprobadas por la obstetra, y asignar roles.
- Ejecutar respaldos y restauración, y configurar el bot de Telegram.
- Consultar la bitácora de operaciones, sin contenido clínico.
- No abre historias, consultas, reglas ni mensajes.

### Paciente destinataria externa

No inicia sesión, no usa vistas de MATERNIA ni modifica registros. Recibe recordatorios e indicaciones autorizadas por Telegram.

### Relaciones UML

- “Registrar consulta” incluye “consultar historia de la paciente” y “recuperar antecedentes”, pues la consulta subsecuente necesita mostrar el contexto existente.
- “Crear episodio de embarazo” y “crear episodio de planificación” son alternativas de flujos diferentes; no se modelan como funcionalidades duplicadas.
- “Calcular calendario de embarazo” y “calcular revisión/renovación de método” son casos separados, con entradas y reglas diferentes.
- “Enviar recordatorio” se ejecuta únicamente si hay cita/evento confirmado, canal habilitado y consentimiento vigente; se representa como condición o extensión, no como requisito incondicional de registrar consulta.
- “Autorizar recomendación para Telegram” es distinto de “registrarla en la historia”: el envío requiere una decisión explícita de la obstetra y consentimiento válido. No existe publicación en un portal.

## 14. Metodología, responsabilidades y entregas

### 14.1 Método de trabajo

Se propone **Scrum adaptado con tablero Kanban (Scrumban)** para un equipo de dos estudiantes. Las iteraciones permiten revisar con la obstetra reglas que pueden cambiar y obtener retroalimentación temprana; el tablero visualiza trabajo y limita tareas en curso. XP se limita a prácticas concretas que el equipo pueda aplicar y demostrar: revisión por pares, pruebas automatizadas donde sea viable, integración frecuente y refactorización dentro de cada historia. No se afirmará que se aplica XP completo.

### 14.2 Responsabilidades de Scrum adaptado

| Responsabilidad | Responsable propuesto | Alcance |
|---|---|---|
| Product Owner funcional | Obstetra como referente; Eduardo facilita el backlog | La obstetra prioriza y valida valor clínico/operativo. Eduardo mantiene criterios y trazabilidad; no decide reglas clínicas. |
| Facilitación del proceso | Eduardo García, alternable con Mateo por iteración | Convocar planificación/revisión, mantener bloqueos y flujo del tablero. No representa un Scrum Master a tiempo completo. |
| Desarrollo | Eduardo García y Mateo Iza | Diseño, análisis, construcción, pruebas, revisión cruzada y documentación; ambas personas comparten responsabilidad sobre el incremento. |
| Administración técnica | Equipo desarrollador durante el prototipo | Cuentas, respaldos y bot, sin acceso a datos clínicos. Fuera del prototipo debería ejercerla una persona distinta del equipo desarrollador. |
| Validación académica | Docente | Revisa entregables del curso; no es responsable de aprobación clínica ni de operación del consultorio. |
| Revisión de mensajes con pacientes participantes | Personas que acepten revisar mensajes de ejemplo con datos ficticios | Aportan observaciones de comprensión; no usan MATERNIA ni validan reglas clínicas. |

### 14.3 Artefactos

- Backlog priorizado con ID, necesidad, actor, regla aplicable, criterio de aceptación, riesgo y estado.
- BPMN AS-IS/TO-BE, con compuertas justificadas y carriles de enfermería, obstetra y sistema; paciente como participante externo de comunicación.
- Casos de uso UML alineados con requisitos y permisos.
- Modelo de datos y diccionario de campos.
- Prototipo navegable con datos ficticios.
- Catálogo de reglas y versiones, aprobado por la obstetra antes de pruebas de aceptación.
- Código y registro de cambios, si se inicia la construcción.
- Plan de pruebas, incidencias, resultados y evidencias por requisito.
- Registro de riesgos, decisiones y cambios de alcance.

### 14.4 Incrementos y criterios de aceptación

| Incremento | Contenido | Criterios para aceptar |
|---|---|---|
| I1. Proceso e identidad | Alcance, BPMN corregido, actores/permisos, prototipo de búsqueda y registro | Se distinguen consultorio particular, obstetra y enfermería; no hay inventario; enfermería registra la historia inicial; las compuertas tienen pregunta y salidas rotuladas; se valida duplicidad. |
| I2. Historia y consulta | Registro, historia, consulta cronológica, recuperación de antecedentes | Una paciente ficticia existente abre antecedentes; una nueva puede registrarse con validaciones; consultas anteriores no se sobrescriben. |
| I3. Seguimiento obstétrico | Episodios, reglas versionadas, fechas de embarazo y planificación, bandeja | Se prueban por separado reglas aprobadas para embarazo y planificación; los casos esperado/no esperado y límite coinciden con referencia; no se inventan fechas cuando falta configuración. |
| I4. Telegram y comunicación | Verificación de contacto, consentimiento, autorización de mensajes y recordatorios | La paciente no tiene cuenta ni vistas; solo recibe mensajes autorizados en Telegram; el envío se bloquea ante contacto no verificado o consentimiento ausente; los fallos quedan registrados para la obstetra. |
| I5. Integración y evaluación | Seguridad por rol, auditoría, pruebas completas, guía y demo | Las pruebas de permisos internos e integridad pasan; ningún mensaje no autorizado se envía; hallazgos altos corregidos o documentados; trazabilidad de requisitos a evidencia. |

Las duraciones y fechas se fijarán al acordar el calendario académico. Enfermería participa en el registro inicial; inventario permanece excluido.

## 15. Indicadores de proyecto y acciones correctivas

Los indicadores son metas para una demostración académica, no resultados alcanzados. Medir en cada cierre de iteración y consolidar al final; registrar fuente, fecha, valor, responsable y acción.

| Indicador | Meta tentativa | Frecuencia | Acción si no se alcanza |
|---|---:|---|---|
| Requisitos aceptados / planificados para la iteración | ≥ 85% | Cada cierre de iteración | Revisar bloqueos y tamaño de historias; reordenar backlog con la obstetra y mover alcance no esencial al siguiente incremento. |
| Pruebas funcionales superadas / ejecutadas | ≥ 90% y cero fallos críticos abiertos | Cada integración y cierre | Detener aceptación del incremento; corregir defectos críticos y repetir casos afectados y regresión relacionada. |
| Exactitud de cálculo de fechas | 100% en escenarios aprobados (mínimo 20) | Cada cambio de regla y cierre de incremento | Congelar la regla afectada, revisar datos/fórmula con obstetra y no publicar fechas hasta aprobar la corrección. |
| Permisos internos correctos | 100% de pruebas por rol | Cada cambio de permisos | Bloquear incremento, corregir y repetir pruebas por rol. |
| Comprensión del recordatorio Telegram | ≥ 4 de 5 participantes identifican correctamente fecha y propósito del mensaje | Prueba de usabilidad de mensajes con contenido ficticio, por incremento | Ajustar redacción y repetir; mantener fuera del mensaje datos clínicos no necesarios. |
| Tiempo para registrar consulta subsecuente | Comparar mediana con AS-IS; meta tentativa de mejora ≥ 20% | Línea base y evaluación final | Revisar campos y repetición; si no existe línea base comparable, reportar tiempo observado sin declarar mejora. |
| Entrega de mensajes Telegram correctamente | ≥ 95% de casos aprobados; cero contenido clínico sensible en canal externo | Cada prueba de notificación | Suspender envíos, revisar bot/contacto/consentimiento y usar contacto manual hasta corregir. |

## 16. Supuestos, restricciones y decisiones pendientes

### 16.1 Supuestos verificables propuestos

- S-01: una obstetra referente estará disponible para revisar el backlog y validar reglas al cierre de cada incremento; registrar fecha y resultado de cada sesión.
- S-02: la primera versión representa un solo consultorio y una sola obstetra responsable; validarlo mediante confirmación escrita del equipo y la obstetra.
- S-03: la obstetra entregará ejemplos ficticios o anonimizados de reglas/formatos que puedan transformarse en escenarios de prueba; no se utilizarán historias reales.
- S-04: al menos cinco escenarios ficticios permitirán evaluar el registro inicial por enfermería; confirmar disponibilidad antes de fijar meta de usabilidad.
- S-05: para la evaluación académica se podrá simular el envío de recordatorios si no se aprueba o no está disponible una integración externa; distinguir simulación de envío real.

Los perfiles internos delimitados son obstetra, enfermería y administrador técnico. La paciente es destinataria externa, no usuaria del sistema.

### 16.2 Restricciones

- Datos ficticios en desarrollo, pruebas, exposición y capturas.
- Sin inventario, medicamentos, farmacia, recepción como usuario ni acceso de pacientes al sistema. Enfermería registra la historia inicial.
- Sin multiespecialidad ni despliegue en varios centros.
- Sin diagnóstico, prescripción ni regla clínica creada por el equipo.
- Telegram es el canal externo previsto; la integración del bot, su seguridad y el consentimiento deben validarse.
- El uso real requiere aprobación clínica, jurídica, de privacidad, seguridad e infraestructura fuera de este entregable académico.

### 16.3 Decisiones pendientes de validación

1. Campos mínimos de historia clínica obstétrica y quién puede corregir cada dato.
2. Reglas concretas de embarazo y planificación familiar, umbrales de alerta y manejo de datos incompletos.
3. Si se integra el bot Telegram para la demostración o se simula el envío.
4. Texto autorizado, consentimiento, verificación del identificador Telegram y procedimiento ante destinatario equivocado.
5. Método de comprobación de identidad y contacto de pacientes, sin exponer la existencia de una historia.
6. Si se habilitará una confirmación simple de recepción mediante Telegram; la paciente no consultará información en una vista del sistema.
7. Stack, alojamiento, retención, restauración y responsables técnicos; el repositorio debe inspeccionarse antes de seleccionar tecnología.
8. Quién ejerce el rol de administrador técnico fuera del prototipo.
9. Fechas reales de iteraciones y participantes de evaluación.

## 17. Definición de terminado y evaluación

Una historia se considera terminada cuando tiene criterio de aceptación comprobado; validaciones y permisos aplicados en el servidor; evidencia de prueba con datos ficticios; revisión por el compañero; documentación del cambio; y no deja defectos críticos abiertos. Una regla clínica requiere, además, aprobación explícita de la obstetra y un conjunto de ejemplos de referencia.

La evaluación compara escenarios equivalentes del proceso manual y del prototipo cuando exista una línea base. Se documentan tiempos, errores, recuperación de antecedentes, resultados de fechas, alertas, privacidad y comprensión de la paciente. Se presentan los resultados observados con tamaño de muestra y limitaciones; no se generalizan a uso clínico real.

## 18. Trazabilidad de observaciones incorporadas

| Observación de revisión | Cómo se incorpora |
|---|---|
| Relación 1 debe evitar registro repetitivo | RF-03 se centra en recuperar antecedentes existentes en consultas posteriores. |
| Relaciones 1 y 3 duplicaban cálculo y alertas | Se separan recuperación de antecedentes, renovación de método y controles de embarazo; cada una tiene propósito e inputs distintos. |
| Relación 2 debe especificar reglas de fechas y alertas | RF-06/08, sección 9 y criterios de cálculo especifican parámetros, versiones, vigencia, umbral, vencimiento y aprobación. |
| Definir consultorio individual o varios profesionales | Se define un consultorio particular con una obstetra referente; extensión multi-profesional queda fuera. |
| Justificar incremental y Kanban/XP | Se justifica Scrumban por revisiones frecuentes y equipo de dos; XP solo como prácticas verificables. |
| Política mínima y relación vulnerabilidad-riesgo-control | Sección 10 y matriz de sección 11 enlazan cada debilidad con riesgo, control y evidencia. |
| Corregir BPMN y compuertas | Sección 7.3 exige preguntas y salidas explícitas; se especifica el flujo propuesto. |
| Delimitar recepción y enfermería | Recepción queda fuera del sistema; enfermería toma la historia clínica inicial y la obstetra atiende las citas posteriores recuperando esa historia. |
| Revisar stakeholders e interés de recepción | Recepción se retira del TO-BE y matriz de actores; pacientes son destinatarias externas, no usuarias. |
| Diferenciar proceso habitual y alertas condicionales de inventario | El inventario y consumo completo se eliminan del alcance, evitando esa relación. |
| Roles Scrum, artefactos e incrementos con aceptación | Sección 14 define responsabilidades, artefactos, incrementos y condiciones de aceptación. |
| Unificar enfermería/farmacia; especificar usuarios y permisos | Se elimina farmacia e inventario; enfermería permanece como usuaria interna limitada al registro inicial y la obstetra conserva seguimiento y reglas. |
| Reemplazar supuesto ambiguo S-05 | S-05 ahora es verificable: bot Telegram disponible o simulación documentada si el servicio no está listo. |
| Indicadores con frecuencia y acciones | Sección 15 agrega frecuencia, meta y corrección. |
| Creación, autenticación y verificación de cuentas | RF-12 cubre cuentas internas; RF-13/14 cubren verificación de Telegram, consentimiento y mensajes. Las pacientes no tienen cuenta. |
| Validar registro de usuarios | La obstetra y enfermería tienen cuentas individuales; el contacto Telegram de la paciente se verifica antes de vincularlo y no habilita acceso al sistema. |
| Delimitar responsabilidades y alertas | Obstetra configura/valida y publica; enfermería toma historia inicial; obstetra atiende las citas posteriores y autoriza mensajes Telegram; paciente solo los recibe; sección 6 y requisitos separan responsabilidades. |
| Roles de administración | Se agrega el administrador técnico, que gestiona cuentas, respaldos y bot sin acceso a datos clínicos; la obstetra aprueba las cuentas. |
| Enfocar solo obstetricia | Tipo de consultorio y alcance se limitan explícitamente a obstetricia y planificación familiar vinculada. |

## 19. Fuentes del proyecto y precedencia

Esta propuesta toma como antecedentes los documentos académicos de Taller 1.1, Propuesta de SI, Taller 1.2 de modelos de gestión, Taller 1.3 de procesos involucrados y Plan de Construcción, junto con `MATERNIA_CONTEXTO_COMPLETO_PARA_CODEX.md`. Las observaciones copiadas en la solicitud del equipo se tratan como comentarios de revisión y se han convertido en requisitos de esta reformulación cuando son compatibles con la nueva instrucción de alcance.

Cuando la propuesta nueva contradice una decisión anterior (por ejemplo, portal paciente (excluido), recordatorios externos, inventario o roles), prevalece la delimitación actual: **obstetra, enfermería y administrador técnico (sin datos clínicos) como usuarios internos; enfermería registra historia inicial; citas posteriores con obstetra; pacientes sin acceso ni vistas; sin inventario**. Telegram es el canal elegido para recordatorios; las reglas clínicas y el procedimiento de consentimiento quedan sujetos a validación. Los PDF y el contexto maestro son fuentes de antecedentes, no órdenes independientes para reconstruir funcionalidades excluidas.

---

**Estado:** propuesta completa para revisión del equipo; no equivale a aprobación clínica, construcción implementada ni validación de seguridad.

