# Arquitectura propuesta — MATERNIA

**Estado:** propuesta para revisión; no se ha iniciado la implementación.  
**Fuente funcional principal:** [`MATERNIA_PROPUESTA_REFORMULADA.md`](../../MATERNIA_PROPUESTA_REFORMULADA.md), ubicado en la carpeta padre del repositorio.  
**Fuentes visuales:** pantallas de referencia proporcionadas por el equipo.

## 1. Precedencia y alcance

El documento reformulado define el alcance funcional actual y prevalece sobre pantallas anteriores cuando exista contradicción. Las pantallas sirven como referencias de identidad visual, navegación y composición, pero no reactivan módulos excluidos.

MATERNIA es un prototipo académico para **un consultorio obstétrico particular**, con una obstetra responsable y apoyo de enfermería. Incluye:

- Cuentas internas diferenciadas para **obstetra** y **enfermería**.
- Búsqueda de pacientes, prevención de duplicados e historia clínica inicial registrada por enfermería.
- Recuperación de antecedentes e historial para consultas posteriores con la obstetra.
- Episodios separados de embarazo y planificación familiar.
- Consultas subsecuentes cronológicas, reglas aprobadas/versionadas, cálculo de fechas y alertas internas.
- Registro de fecha y hora de próxima cita acordada, sin gestionar agenda ni disponibilidad.
- Recomendaciones registradas por la obstetra y envío opcional autorizado por Telegram, con destinatario verificado, consentimiento y trazabilidad.

La paciente no tiene cuenta ni vistas de MATERNIA. Es destinataria externa de mensajes. Enfermería registra la historia inicial y realiza solo operaciones expresamente autorizadas; la atención subsecuente corresponde a la obstetra. Quedan fuera recepción como usuario, inventario, medicamentos, farmacia, agenda completa, facturación, otras especialidades, integraciones hospitalarias y diagnóstico/prescripción automáticos. El desarrollo y las pruebas usan datos ficticios.

## 2. Decisión arquitectónica

Propongo un **monolito modular** en un solo repositorio, con frontend web y API separados como procesos desplegables. La lógica de permisos, reglas, consentimiento y transacciones vive en el servidor. Esta forma es adecuada para una clínica y un equipo académico pequeño: conserva límites de dominio, facilita cambios y evita la carga de operar microservicios.

```text
Navegador del personal
  └── Aplicación web React (obstetra / enfermería)
        └── API REST NestJS (/api/v1)
              ├── Identidad y permisos
              ├── Pacientes e historia clínica
              ├── Episodios y consultas
              ├── Reglas y seguimiento
              ├── Citas y recomendaciones
              ├── Consentimiento y Telegram
              └── Auditoría
                    ├── PostgreSQL
                    └── Adaptador Telegram Bot API
```

Telegram será un adaptador de salida limitado: recibirá una orden explícita del backend con destinatario validado y contenido autorizado. No tendrá acceso a historias clínicas ni decidirá reglas, destinatarios o contenido. La integración real puede sustituirse por un adaptador simulado para la demostración, tal como permite el documento funcional.

## 3. Stack recomendado

| Área | Tecnología propuesta | Uso |
|---|---|---|
| Lenguaje | TypeScript | Tipos en web y servidor; contratos compartibles con límites claros. |
| Web | React + Vite | Aplicación interna interactiva, sin necesidad de renderizado SEO. |
| Navegación/estado | React Router + TanStack Query | Rutas por área y estado remoto/caché explícito. |
| Formularios | React Hook Form + Zod | Formularios de historia y consulta con validación clara. La validación del servidor es obligatoria. |
| Diseño | Tailwind CSS y componentes accesibles propios | Aplicar el estilo MATERNIA de las referencias y adaptarlo a los flujos vigentes. |
| API | NestJS sobre Node.js LTS, REST `/api/v1` y OpenAPI | Módulos de dominio, validación, guards y documentación. |
| Base de datos | PostgreSQL | Relaciones, restricciones, auditoría y transacciones. |
| Persistencia | Prisma ORM con migraciones versionadas | Acceso tipado y evolución controlada del esquema; SQL explícito si se requiere. |
| Desarrollo local | Docker Compose | Entorno reproducible para web, API y PostgreSQL. |
| Telegram | Cliente de Telegram Bot API detrás de interfaz propia | Permite simulación local y sustituirla por integración sin acoplar el dominio al proveedor. |

Fijar versiones estables vigentes al iniciar la construcción. No sumar Redis, colas ni microservicios inicialmente. Para envío fiable, primero persistir una orden de mensaje autorizada; si se demuestra necesaria la entrega asíncrona/reintentos, añadir una tabla outbox y un worker sencillo antes que introducir infraestructura innecesaria.

## 4. Módulos y responsabilidades

### Web

- Marco autenticado con navegación por permisos: Panel, Pacientes, Historia, Consulta, Seguimiento y Administración de cuentas internas, si esta última se confirma para la obstetra.
- La vista de paciente/episodio integra antecedentes, consultas, citas y estado de seguimiento.
- Formularios distintos para historia inicial y consulta subsecuente: no volver a pedir antecedentes longitudinales como si fueran nuevos.
- Pantalla de autorización previa al envío: destinatario, consentimiento, contenido y acción explícita de confirmar.
- No existe portal, inicio de sesión ni pantalla para pacientes.

### API

- `identity`: autenticación, cuentas, rol y revocación.
- `patients`: búsqueda, coincidencias posibles, ficha y prevención de duplicados.
- `clinical-record`: historia obstétrica, antecedentes y cambios trazables.
- `episodes`: ciclos independientes de embarazo y planificación familiar.
- `consultations`: consultas subsecuentes y relación con episodio.
- `follow-up`: reglas versionadas, cálculo determinista, eventos y alertas.
- `appointments`: fecha/hora acordada asociada a paciente/episodio; sin agenda/disponibilidad.
- `recommendations`: recomendación clínica vinculada a consulta y autorización de comunicación.
- `telegram`: contacto, verificación, consentimiento, envío y resultado técnico.
- `audit`: eventos mínimos de cambios y acciones sensibles.

Cada módulo encapsula sus casos de uso. Controladores HTTP traducen DTOs; servicios de aplicación aplican reglas; la persistencia no se expone directamente al cliente. Los permisos se verifican en cada operación del servidor y sobre el recurso solicitado.

## 5. Modelo de datos inicial

Usar UUID internos; `timestamptz` para instantes y `date` para fechas calendario. Mostrar horas en `America/Guayaquil`. Guardar fechas acordadas como fecha/hora según el requisito, pero sin modelar disponibilidad. Datos de salud y contacto deben ser mínimos y ficticios en el prototipo.

| Entidad | Propósito y relaciones |
|---|---|
| `users` | Cuenta individual interna, correo único normalizado, hash de contraseña, estado. |
| `roles` / `user_roles` | Roles cerrados inicialmente: `obstetra`, `enfermeria`. No crear rol admin salvo cambio aprobado del alcance. |
| `patients` | Identidad clínica, identificador permitido, datos mínimos y estado. Índices para búsqueda; no abrir automáticamente si hay coincidencia ambigua. |
| `obstetric_histories` | Historia inicial longitudinal y estado de completitud; autor/fechas. Separar campos estructurados de notas y registrar correcciones. |
| `history_items` | Antecedentes estructurados/reutilizables ligados a historia, con tipo, valor, fecha, autor y estado. Diccionario definitivo de campos pendiente de validar. |
| `care_episodes` | Paciente, tipo (`pregnancy`/`family_planning`), código, estado y datos de entrada necesarios para la regla aplicable. Una paciente puede tener episodios sucesivos. |
| `consultations` | Paciente, episodio opcional, fecha, motivo, datos relevantes, observaciones y profesional. Registro cronológico inmutable tras alta; correcciones versionadas. |
| `follow_up_rules` | Tipo de proceso, identificador y estado. Contenedor lógico de reglas. |
| `follow_up_rule_versions` | Parámetros/fórmula, entradas requeridas, umbral, unidades, vigencia, autora/aprobadora, motivo de cambio y ejemplos aprobados. Una versión publicada no se edita. |
| `follow_up_events` | Episodio, fecha calculada, regla/versión e inputs usados, estado y confirmación/revisión por obstetra. Historial para explicar recálculos. |
| `appointments` | Paciente, episodio/evento opcional, fecha/hora acordada, autor y estado mínimo. No incluye turnos disponibles, agenda ni reprogramación autónoma. |
| `recommendations` | Texto, consulta/paciente, autora y clasificación (`internal`, `authorized_for_telegram`, `sent`, `failed`, `revoked`). La autorización y envío deben ser explícitos. |
| `telegram_contacts` | Identificador externo verificado, paciente, estado y fechas de verificación. Restringir visibilidad y evitar búsquedas que revelen si existe una historia. |
| `communication_consents` | Paciente, canal, propósito, estado, fecha, método/evidencia mínima de captura y revocación. No asumir consentimiento por tener un contacto. |
| `message_outbox` | Opcional al conectar Telegram real: destinatario, recomendación/evento autorizado, contenido aprobado, estado técnico, intentos e identificador del proveedor. Nunca guardar contenido no autorizado. |
| `audit_events` | Usuario, acción, entidad, ID, instante y metadatos mínimos; no copiar el texto clínico completo. |

### Integridad y cálculo

- Un identificador de paciente único según la política acordada; los duplicados potenciales requieren confirmación humana.
- Relaciones con borrado restrictivo para conservar trazabilidad; desactivar cuentas/registros mediante estado cuando corresponda.
- Restricciones de estado y claves foráneas; índices en identificador de paciente, fechas de consulta/evento/cita, estado del episodio y estado de mensaje.
- Cada cálculo guarda entradas, resultado, zona/unidad relevante y versión de regla usada. Sin datos suficientes o regla aprobada vigente, el sistema solicita revisión y no inventa una fecha.
- Cálculo de embarazo y revisión/renovación de planificación son casos y reglas distintos.
- El umbral del semáforo solo indica proximidad/atraso. Modificarlo no cambia la fecha clínica calculada.
- Cambiar entradas o reglas no elimina historia ni consultas; conservar versiones/eventos previos y registrar motivo de recálculo.

## 6. Flujos críticos y transacciones

### Alta inicial

Enfermería busca coincidencias; si hay ambigüedad, detiene el alta. Si no existe ficha, registra la historia inicial con validaciones, autoría y fecha. La obstetra recupera esa ficha en citas posteriores.

### Consulta subsecuente

La obstetra abre antecedentes e historial, registra una consulta cronológica, actualiza el episodio, confirma la próxima cita y revisa los eventos de seguimiento. Guardar consulta, cambios del episodio, cálculo/evento y cita en una transacción de PostgreSQL cuando son una sola acción confirmada. La autorización Telegram es un paso separado y explícito.

### Recomendación y mensaje

Registrar recomendación no implica enviar. Antes del envío, comprobar en servidor: usuario obstetra autorizado, recomendación aprobada para canal, contacto verificado, consentimiento vigente para ese propósito y destinatario revisado. Persistir auditoría/orden de salida, invocar Telegram mediante adaptador y guardar estado técnico. Reintentos idempotentes para evitar mensajes duplicados. No incluir información clínica sensible innecesaria.

## 7. Roles y seguridad

La matriz de permisos debe reflejar el documento, no las pestañas de una pantalla antigua:

| Acción | Obstetra | Enfermería |
|---|---:|---:|
| Buscar paciente y revisar posibles duplicados | Sí | Sí |
| Crear historia clínica inicial | Según necesidad/validación | Sí |
| Completar/corregir historia inicial | Sí, con trazabilidad | Sí, solo campos y operaciones autorizados |
| Consultar historia para atención | Sí | Acceso mínimo necesario; pendiente de cerrar en matriz detallada |
| Registrar consulta subsecuente y gestionar episodios | Sí | No por defecto |
| Configurar/aprobar reglas y confirmar seguimiento | Sí | No |
| Acordar próxima cita y redactar/autorizar recomendación | Sí | No por defecto |
| Vincular contacto Telegram/consentimiento | Según procedimiento acordado; enfermería puede registrar/verificar si se autoriza | Pendiente de definición explícita |
| Administrar/revocar cuentas | Obstetra responsable, si se confirma | No |

Autenticación con cuentas individuales; hash Argon2id o bcrypt bien configurado. Para la web, preferir cookie de sesión `HttpOnly`, `Secure`, `SameSite` y protección CSRF según el diseño. TLS en despliegue, secretos fuera del repositorio, límites de intentos y validación estricta de entradas. La UI puede ocultar acciones, pero solo la API autoriza.

Auditar acceso/cambios de historia, correcciones, reglas, fechas confirmadas, consentimiento, autorización y envíos. Evitar secretos y contenido clínico completo en logs. Para demo, seed sintético claramente segregado. No cargar datos reales sin aprobación clínica, de privacidad, seguridad e infraestructura, política de retención y respaldos aprobada.

## 8. Reglas de seguimiento

No se codifican intervalos médicos por inferencia desde las imágenes. La obstetra debe validar cada regla antes de usarla en escenarios de aceptación. La configuración debe definir proceso, versión, entradas requeridas, fórmula/intervalo, calendario, umbral previo, vencimiento, vigencia, responsable, ejemplos y comportamiento ante datos faltantes.

El motor será determinista y sin efectos secundarios: recibe episodio, fecha de referencia y versión aprobada; devuelve fechas/eventos explicables. Un proceso separado presenta alertas y, si aplica, programa comunicaciones autorizadas. Ejecuciones repetidas no deben duplicar eventos ni mensajes.

## 9. Estructura del repositorio propuesta

```text
apps/
  web/                 # React: vistas y componentes por dominio
  api/                 # NestJS: módulos y API REST
packages/
  contracts/           # DTOs/esquemas compartidos sin reglas de autorización
database/
  migrations/          # esquema versionado
  seed/                # datos exclusivamente ficticios
docs/
  decisions/           # ADRs y decisiones aprobadas
docker-compose.yml
ARCHITECTURE.md
```

No incorporar `inventory`, `medications`, portal de pacientes ni rol `admin` en el primer alcance. Administración de cuentas internas, si se conserva, debe ser una capacidad de la obstetra y no un tercer perfil.

## 10. Orden recomendado de construcción

1. Confirmar matriz de permisos, campos iniciales, identificación de duplicados y protocolo de consentimiento/verificación Telegram.
2. Inspeccionar el entorno y fijar stack/versiones; crear monorepo, Docker Compose, PostgreSQL, migraciones y seed ficticio.
3. Implementar identidad interna y marco visual según referencias, ajustando navegación a dos roles y módulos vigentes.
4. Implementar búsqueda, resolución de duplicados, paciente e historia inicial de enfermería.
5. Implementar episodios, recuperación de antecedentes e historia cronológica de consultas para obstetra.
6. Implementar reglas versionadas independientes y cálculo de seguimiento con casos de referencia aprobados.
7. Implementar próxima cita y recomendaciones; añadir consentimiento, verificación y adaptador Telegram simulado.
8. Integrar Telegram real solo si se valida para demo; cerrar permisos, auditoría, pruebas funcionales/usabilidad y documentación.

## 11. Pendientes que afectan arquitectura

1. Campos mínimos de historia obstétrica, política del identificador y protocolo contra duplicados.
2. Matriz final de acceso de enfermería a datos y correcciones, así como quién gestiona las cuentas.
3. Reglas aprobadas de embarazo/planificación y tratamiento de cambios retroactivos.
4. Si se simula Telegram o se conecta un bot real; método de verificación del destinatario, consentimiento, contenido permitido, revocación y atención a fallos.
5. Infraestructura, retención, respaldo/restauración y operación; no especificadas como aprobadas en el documento.

La arquitectura permite avanzar el prototipo con datos sintéticos mientras se validan estos puntos; no convierte el prototipo en producto clínico aprobado.
