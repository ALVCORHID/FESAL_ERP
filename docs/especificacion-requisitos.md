# Especificación de requisitos — Fresales (módulo piloto de PGI)

**Autor:** Álvaro Correa Hidalgo
**Versión:** 1.1 · **Fecha:** 1 de octubre de 2026
**Prototipo navegable (Figma):** https://www.figma.com/design/42PW5tDnpJ7YJpYCULDNRI
**Documentos relacionados:** [visión del producto](vision-del-producto.md) ·
[guion de entrevista](guion-entrevista.md) ·
[diagrama de casos de uso](diagramas/casos-de-uso.png) ·
[CU-01 detallado](diagramas/CU-01-crear-cuenta.md)

## 1. Propósito y alcance

La Plataforma de Gestión Institucional (PGI) centraliza información que hoy
vive en hojas de cálculo y archivos sueltos, con acceso controlado por área.
Su primer módulo es **Fresales**: el sistema interno de coordinación de la
mesa directiva de FESAL (Federación de Estudiantes, Universidad Anáhuac
Querétaro, 26–30 personas). Fresales sustituye la coordinación por WhatsApp
y hojas de cálculo en cuatro objetivos: juntas y acuerdos, solicitudes de
ayuda entre equipos, proyectos, y comunicación.

**Dentro del alcance de este documento:** alta y acceso de la mesa, juntas
con minuta y acuerdos, compromisos, solicitudes entre equipos, y
administración del roster por el coordinador.

**Fuera del alcance (y por qué):** aplicación móvil nativa (la mesa usa la
PWA instalada desde el navegador), integración con sistemas institucionales
de la universidad y módulos financieros o de nómina (convertirían el
proyecto en un ERP completo; ver §3 de la visión).

## 2. Usuarios y su contexto

| Usuario | Qué necesita | Qué le preocupa | Fuente |
|---|---|---|---|
| **Miembro de la mesa** (~25) | Ver sus juntas, sus compromisos y las solicitudes de su equipo; pedir ayuda a otros equipos. | Enterarse a tiempo y no ver ni editar lo de otros equipos por accidente. | Visión; junta de la mesa 11-sep-2026 (equipos y ámbito) |
| **Coordinador** (pocos) | Dar de alta a la gente, confirmar juntas, administrar equipos. | Que sólo entre quien debe y que el roster esté al día. | Visión; operación actual de Fresales |

**Lo que cambió tras la entrevista** (1 entrevistado; ver la bitácora del
[guion](guion-entrevista.md)): el dolor principal no es la duplicidad de datos
que suponía la visión, sino la coordinación. Las juntas llegan sin agenda, duran
unas 2 horas y concluyen poco; las solicitudes entre equipos se piden por
WhatsApp y se pierden, sin registro. Los miembros usan Drive, Docs y Excel, y a
muchos les cuesta Excel. Cuando alguien se va, el coordinador lo da de baja y
su coordinador (cabeza de equipo) reasigna lo que tenía. El acceso a la información
depende del equipo y del rango, y hoy cualquiera puede editar lo que ve.

## 3. Requisitos funcionales

**Convención.** `RF-nn` identifica el requisito. **Origen** distingue lo
**Confirmado** por la mesa (decisión o pedido explícito) de lo **Supuesto**
(escrito por mí en la visión y aún sin validar). **Prioridad** usa MoSCoW.
**CU** es el caso de uso que lo realiza.

**Definiciones.** *Invitación vigente:* existe para esa fila del roster y no
se ha usado (hoy no caduca por tiempo). *Quien administra la junta:* el
coordinador o el miembro que la creó. *General:* equipo que contiene a toda la
mesa; lo publicado ahí lo ve todo el roster. *Responsable:* miembro asignado a un acuerdo. *Cabeza de equipo:* sinónimo de coordinador.

### RF-01 · Crear cuenta con código de invitación
- **Descripción:** El sistema debe permitir que un miembro del roster cree su
  cuenta con celular, contraseña, nombre y código de invitación.
- **Origen:** Confirmado (cambio a celular decidido por la mesa el 21-sep-2026).
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un miembro con invitación vigente, cuando
  envía celular, código y contraseña de 8 o más caracteres, entonces queda
  creada su cuenta, su nombre es el que escribió y entra a Inicio (o al
  destino que había pedido).
- **Relaciones:** CU-01 · depende de RF-14 · RNF-01.

### RF-02 · Rechazar el alta sin invitación vigente
- **Descripción:** El sistema debe rechazar el alta cuando el celular y el
  código no coinciden con una invitación vigente.
- **Origen:** Supuesto (decisión de diseño de Fresales; sin validar).
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un celular sin invitación o un código
  incorrecto, cuando se envía el formulario, entonces no se crea ningún
  usuario y se muestra "Ese celular no tiene invitación o el código no
  coincide".
- **Relaciones:** CU-01 (flujo A3) · RNF-01.

### RF-03 · Consumir la invitación al usarse
- **Descripción:** El sistema debe invalidar el código de invitación en el
  momento en que se crea la cuenta.
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un código ya usado, cuando se envía de nuevo
  el mismo código con el mismo celular, entonces el alta se rechaza.
- **Relaciones:** CU-01 · RNF-01.

### RF-04 · Iniciar sesión con celular y contraseña
- **Descripción:** El sistema debe permitir a un miembro con cuenta iniciar
  sesión con su celular y contraseña.
- **Origen:** Confirmado (la mesa pidió celular: "es más fácil" que el correo).
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un miembro con cuenta, cuando captura
  celular y contraseña correctos, entonces entra a Inicio; con datos
  incorrectos permanece en la pantalla de acceso con un mensaje de error.
- **Relaciones:** CU-02 · RNF-01.

### RF-05 · Proponer una junta
- **Descripción:** El sistema debe permitir a un miembro proponer una junta
  con título, fecha, lugar y agenda en un equipo al que pertenece.
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un miembro del equipo X, cuando propone
  una junta con título, fecha y, opcionalmente, lugar y agenda, entonces
  aparece en la lista de juntas de X con estado "propuesta".
- **Relaciones:** CU-03 · RF-06.

### RF-19 · Consultar la agenda antes de la junta
- **Descripción:** El sistema debe mostrar la agenda de una junta a los
  miembros de su equipo antes de que empiece.
- **Origen:** Confirmado (entrevista, 1-oct-2026: "no tenemos idea de lo que se
  va a hablar hasta que empieza").
- **Prioridad:** Must.
- **Criterio de aceptación:** Dada una junta propuesta con agenda, cuando un
  miembro de su equipo abre la junta antes de su fecha, entonces ve la agenda
  completa; si no tiene agenda, ve "Sin agenda".
- **Relaciones:** CU-03 · RF-05.

### RF-06 · Confirmar una junta
- **Descripción:** El sistema debe permitir sólo a un coordinador cambiar una
  junta de "propuesta" a "confirmada".
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dada una junta propuesta, cuando un
  coordinador la confirma, entonces su estado es "confirmada"; cuando lo
  intenta un miembro que no es coordinador, el sistema lo impide.
- **Relaciones:** CU-04 (extiende CU-03) · RF-05.

### RF-07 · Registrar la minuta de una junta
- **Descripción:** El sistema debe permitir a quien administra la junta
  guardar su minuta.
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dada una junta, cuando quien la administra
  guarda la minuta, entonces ésta queda visible para el equipo de la junta.
- **Relaciones:** CU-05.

### RF-08 · Registrar la asistencia
- **Descripción:** El sistema debe permitir a quien administra la junta
  marcar quién estuvo presente.
- **Origen:** Supuesto.
- **Prioridad:** Should.
- **Criterio de aceptación:** Dada una junta, cuando se marca presente a un
  miembro, entonces la lista guarda un solo registro de asistencia por persona.
- **Relaciones:** CU-05.

### RF-09 · Registrar un acuerdo con responsable y fecha límite
- **Descripción:** El sistema debe permitir registrar, desde una junta, un
  acuerdo con descripción, responsable y fecha límite opcional.
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dada una junta, cuando se agrega un acuerdo con
  descripción no vacía y un responsable, entonces aparece como "pendiente" en
  los compromisos del responsable.
- **Relaciones:** CU-05 · RF-10.

### RF-10 · Cerrar un compromiso
- **Descripción:** El sistema debe permitir al responsable de un acuerdo (o a
  quien administra la junta) marcarlo como hecho.
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un acuerdo pendiente, cuando su
  responsable lo marca hecho, entonces su estado es "hecho" y se registra la
  fecha de cierre.
- **Relaciones:** CU-06 · RF-09.

### RF-11 · Publicar una solicitud de ayuda
- **Descripción:** El sistema debe permitir a un miembro publicar una
  solicitud con título, detalle y urgencia (baja, media, alta) en el tablón de
  uno de sus equipos o de General.
- **Origen:** Confirmado (entrevista, 1-oct-2026: la ayuda pedida por WhatsApp se pierde).
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un miembro del equipo X, cuando publica una
  solicitud en General con urgencia "alta", entonces aparece como "abierta" con
  esa urgencia y cualquier miembro de la mesa la ve.
- **Relaciones:** CU-07.

### RF-17 · Tomar una solicitud
- **Descripción:** El sistema debe permitir a un miembro que ve una solicitud
  abierta tomarla.
- **Origen:** Confirmado (entrevista: hoy nadie contesta ni queda registro).
- **Prioridad:** Must.
- **Criterio de aceptación:** Dada una solicitud "abierta" visible para el
  miembro, cuando la toma, entonces su estado es "tomada" y queda registrado
  quién la tomó.
- **Relaciones:** CU-07 · RF-11.

### RF-18 · Resolver una solicitud
- **Descripción:** El sistema debe permitir a quien tomó una solicitud (o a su
  creador o a un coordinador) marcarla como resuelta.
- **Origen:** Supuesto.
- **Prioridad:** Should.
- **Criterio de aceptación:** Dada una solicitud "tomada", cuando quien la
  tomó la marca resuelta, entonces su estado es "resuelta" y se registra la
  fecha.
- **Relaciones:** CU-07 · RF-17.

### RF-20 · Reasignar un compromiso
- **Descripción:** El sistema debe permitir a un coordinador (cabeza de equipo) cambiar el responsable de un acuerdo pendiente.
- **Origen:** Confirmado (entrevista: lo de quien se va "se deriva a la cabeza
  de equipo y luego la cabeza puede delegarlo").
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un acuerdo pendiente cuyo responsable ya no
  está en el equipo, cuando un coordinador elige a otro miembro del equipo, entonces el acuerdo aparece en los compromisos del nuevo responsable
  y ya no en los del anterior.
- **Relaciones:** CU-05 · RF-09, RF-15.

### RF-21 · Limitar quién puede editar un registro
- **Descripción:** El sistema debe permitir editar un registro sólo a su
  autor, a su responsable o a un coordinador.
- **Origen:** Supuesto (la entrevista V2 muestra que hoy "puede hacer todo"; no
  se pidió expresamente restringir).
- **Prioridad:** Should.
- **Criterio de aceptación:** Dada una solicitud creada por otro miembro del
  equipo que no la tomó ni se le asignó, cuando un miembro que no es su autor,
  ni su responsable, ni coordinador intenta editarla, entonces el sistema lo
  impide.
- **Relaciones:** CU-07 · RNF-01.

### RF-12 · Dirigir una solicitud a una persona
- **Descripción:** El sistema debe permitir dirigir una solicitud a un
  miembro concreto en lugar de publicarla en el tablón.
- **Origen:** Supuesto (agregada tras la revisión de arquitectura del 22-sep-2026; sin validar en entrevista).
- **Prioridad:** Should.
- **Criterio de aceptación:** Dada una solicitud dirigida a una persona,
  entonces el destinatario la ve aunque sea de otro equipo, y un miembro que
  no es el creador, el destinatario ni del equipo no la ve.
- **Relaciones:** CU-07 · RF-13.

### RF-13 · Limitar la visibilidad por equipo
- **Descripción:** El sistema debe mostrar a cada miembro sólo las juntas,
  proyectos y solicitudes de sus equipos y de General.
- **Origen:** Confirmado (pedido de ámbito en la junta del 11-sep-2026).
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un miembro que no pertenece al equipo X,
  cuando consulta juntas, proyectos o solicitudes, entonces no ve los de X;
  lo publicado en General sí lo ve.
- **Relaciones:** CU-03, CU-07 · RNF-01.

### RF-14 · Emitir una invitación
- **Descripción:** El sistema debe permitir al coordinador generar el código
  de invitación de una fila del roster.
- **Origen:** Supuesto.
- **Prioridad:** Must.
- **Criterio de aceptación:** Dada una fila del roster sin cuenta, cuando el
  coordinador emite la invitación, entonces el sistema le muestra un código
  de 8 o más caracteres que sólo él puede leer.
- **Relaciones:** CU-08 · RF-01.

### RF-15 · Administrar el roster
- **Descripción:** El sistema debe permitir al coordinador dar de alta,
  editar y dar de baja a los miembros del roster y asignarlos a equipos.
- **Origen:** Confirmado (entrevista: "los coordinadores lo dan de baja").
- **Prioridad:** Must.
- **Criterio de aceptación:** Dado un coordinador, cuando agrega un miembro
  con nombre, celular y rol, entonces aparece en el roster; un miembro sin
  rol de coordinador no puede hacerlo. Cuando da de baja a un miembro con
  compromisos pendientes, estos permanecen asignados a él hasta reasignarse
  (RF-20).
- **Relaciones:** CU-08.

### RF-16 · Atribuir cada registro a su autor
- **Descripción:** El sistema debe guardar quién creó cada junta, solicitud,
  acuerdo y avance, y cuándo. (El historial de ediciones y borrados queda
  fuera de la v1; ver §9.)
- **Origen:** Confirmado en parte (entrevista V5: esperan ver quién cambió algo; la v1 solo guarda al autor).
- **Prioridad:** Should.
- **Criterio de aceptación:** Dado cualquier registro de esos tipos, cuando
  se consulta, entonces muestra su autor y su fecha de creación.
- **Relaciones:** CU-03, CU-05, CU-07 · RNF-05.

## 4. Requisitos no funcionales

Atributos tomados del tipo de sistema declarado en la visión (sistema de
información + web/SaaS + datos). Cada uno trae métrica y la razón del límite.
Las métricas son **objetivos**; las columnas "Cómo se mide" indican la
prueba.

### Seguridad
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-01 | El sistema debe impedir que un usuario lea o escriba datos fuera de su ámbito. | 0 filas ajenas devueltas a un usuario fuera del equipo; 0 tablas legibles sin sesión. | Hay 26–30 personas y equipos con información que no debe cruzarse; un fallo expone datos de otros equipos. | Consulta a la API sin sesión y con cuenta de otro equipo: debe devolver 0 filas (la implementación usa RLS en el 100 % de las tablas). |
| RNF-02 | El sistema debe exigir una contraseña mínima. | Longitud mínima de 8 caracteres (mínimo habitual de las guías de contraseñas). | Sin SMS de confirmación (RN-CU01-3) la contraseña y el código son la barrera. | Intento de alta con 7 caracteres: rechazado. |

### Integridad
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-03 | El sistema debe rechazar datos que rompan sus relaciones o formatos. | 0 registros con referencias huérfanas; celular en formato internacional (`^\+[1-9]\d{7,14}$`). | Un celular mal formado impide el alta y los enlaces de contacto; una referencia huérfana deja acuerdos sin responsable. | Insertar un acuerdo con responsable inexistente: rechazado. |

### Disponibilidad
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-04 | El sistema debe estar disponible para la mesa durante el periodo de uso. | ≥ 99 % mensual (≤ 7.3 h de caída al mes). | Meta propuesta, sin validar con la mesa: es uso interno sin operación 24/7 y la infraestructura es de plan gratuito, que no garantiza más. | Monitor externo de la URL de producción. |

### Trazabilidad
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-05 | El sistema debe atribuir cada registro creado a una persona. | 100 % de juntas, solicitudes, acuerdos y avances con autor y fecha de creación. | Sin autoría no se resuelven errores ni responsabilidades (V5 de la entrevista). | Consulta que cuente registros sin autor: debe ser 0. |

### Usabilidad
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-06 | El sistema debe poder usarse desde el celular sin capacitación. | Un miembro nuevo completa el alta (con el código ya en mano) en ≤ 2 minutos, sin ayuda. | Es la primera interacción; si cuesta más, la mesa se queda con WhatsApp. La entrevista lo respalda: "a muchos les cuesta manejar Excel". | Prueba con 3 miembros cronometrada. |
| RNF-07 | El sistema debe poder instalarse en el celular sin tienda de apps. | Pasa el criterio "instalable" de Lighthouse PWA. | La mesa usa iPhone y no hay presupuesto para una app nativa (fuera de alcance). | Auditoría Lighthouse. |

### Rendimiento
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-08 | Las pantallas principales deben cargar rápido en red móvil. | Contenido principal visible en ≤ 3 s en 4G. | Tiempo habitual a partir del cual se abandona una página móvil. | Lighthouse con perfil 4G. Meta propuesta (umbral común de rendimiento web), sin validar. |

### Mantenibilidad
| ID | Requisito | Métrica | Por qué ese límite | Cómo se mide |
|---|---|---|---|---|
| RNF-09 | Cada módulo de PGI debe ser autocontenido. | Añadir un módulo nuevo toca 0 archivos del módulo Fresales. | PGI crecerá por módulos (visión §5). | Revisar el diff al añadir un módulo. |

## 5. Trazabilidad

| Caso de uso | Requisitos funcionales | No funcionales |
|---|---|---|
| CU-01 Crear cuenta con código de invitación | RF-01, RF-02, RF-03 (precondición: RF-14) | RNF-01, RNF-02, RNF-06 |
| CU-02 Iniciar sesión | RF-04 | RNF-01, RNF-02 |
| CU-03 Proponer junta | RF-05, RF-13, RF-16, RF-19 | RNF-01, RNF-05 |
| CU-04 Confirmar junta | RF-06 | RNF-01 |
| CU-05 Registrar minuta y acuerdos | RF-07, RF-08, RF-09, RF-16, RF-20 | RNF-03, RNF-05 |
| CU-06 Cerrar compromiso | RF-10 | RNF-03 |
| CU-07 Publicar solicitud (ciclo: publicar, tomar, resolver) | RF-11, RF-12, RF-13, RF-16, RF-17, RF-18, RF-21 | RNF-01, RNF-05 |
| CU-08 Administrar roster e invitaciones | RF-14, RF-15 | RNF-01, RNF-03 |
| Transversales (sin caso de uso propio) | — | RNF-04, RNF-07, RNF-08, RNF-09 |

Todo RF figura en al menos un caso de uso y todo caso de uso realiza al menos
un RF.

## 6. Pantallas del prototipo por caso de uso

| Caso de uso | Pantalla en Figma |
|---|---|
| CU-01, CU-02 | Login |
| CU-03, CU-04 | Juntas, Calendario |
| CU-05, CU-06 | Juntas (detalle), Compromisos |
| CU-07 | Solicitudes |
| CU-08 | Mesa |

## 7. Revisión de la dupla

**Revisión de la dupla:**

- **Revisor/a:** Pablo Andrade
- **Observaciones recibidas:**
- Navegación: "Mi equipo" se ubica junto a "Inicio" por ser lo más consultado por la mesa (2026-09-18).
- Autenticación: acceso por celular y contraseña, no por enlace mágico ni correo (2026-09-10, confirmado 2026-09-21 "es más fácil").
- Identidad del roster: al crear cuenta, el nombre o apodo que escribe el miembro reemplaza el que registró el coordinador, por ser como lo conoce la mesa (2026-09-18).
- Interfaz fija: cabecera y menú permanecen visibles (sticky) para no exigir desplazamiento al cambiar de sección (2026-09-18).
- Visibilidad por equipo: un miembro raso ve solo lo de su propio equipo; el coordinador ve todo (2026-09-11).
- Estados de junta: una junta propuesta por un miembro no coordinador queda pendiente de confirmación por un coordinador (2026-09-10).

## 8. Registro de cambios

| Versión | Fecha | Cambio | Motivo |
|---|---|---|---|
| 0.1 | 22-sep-2026 | Primer guion de entrevista y visión. | Inicio. |
| 1.0 | 1-oct-2026 | Primera especificación completa. | Entrega parcial. |
| 1.1 | 1-oct-2026 | RF-11, RF-15 y RF-17 pasan a "Confirmado" y RF-16 a "Confirmado en parte"; nuevos RF-19 y RF-20 (Confirmados) y RF-21 (Supuesto); RNF-06 con sustento; contexto de usuarios y 

## 9. Pendientes y límites conocidos de la v1


- **Búsqueda.** La información "se busca en los mensajes"; hoy el sistema sólo
  tiene buscador de miembros, no de contenido.
- **Roles de la visión.** La visión menciona administradores y Dirección; el
  piloto solo tiene Miembro y Coordinador. La consulta consolidada y la gestión
  de roles quedan para PGI.
- **Historial de cambios.** Se guarda quién creó cada registro (RF-16), no
  quién lo editó o lo borró. El entrevistado espera ver "quién hizo cada
  cambio"; la regla de la visión (RN-5) se cumple sólo en parte.
- **Eliminación.** No hay un RF de borrado; el coordinador puede borrar por
  permiso, pero no se ha especificado qué se conserva.
- **Seguridad sin cubrir:** recuperar contraseña, límite de intentos de acceso,
  respaldos y requisitos sobre datos personales (celulares).
- **Invitaciones.** No caducan por tiempo; definir si deben hacerlo.
- **Migración de hojas de cálculo** (visión §3): fuera de este piloto.
- **Dominio.** La visión habla de áreas universitarias; el piloto es la mesa
  de FESAL, un caso concreto de esa misma necesidad (información dispersa y
  acceso por ámbito).
