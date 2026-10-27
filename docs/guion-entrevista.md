# Guion de entrevista — mesa directiva de FESAL

**Entrevistador:** Álvaro Correa Hidalgo
**Entrevistado (dupla):** Pablo Andrade
**Fecha de aplicación:** 1 de octubre de 2026 (fecha del registro en el repositorio)

Sistema bajo análisis: **Fresales**, módulo piloto de PGI (ver
[`vision-del-producto.md`](vision-del-producto.md)). La entrevista busca
verificar los supuestos de la visión con alguien que vive el día a día de la
mesa. Todas las preguntas son abiertas y piden ejemplos concretos, no
opiniones sobre el sistema.

## Tramo 1 · Contexto

1. Cuéntame cómo es una semana normal tuya en la mesa: ¿qué tareas te tocan
   más seguido? Una semana normal es un caos, no hay estructura en la información que se transmite y las juntas suelen ser una guerra de todos justificando su poca participación
2. ¿Con quién te coordinas para hacer tu parte?
   Generalmente me toca coordinarme con el vicepresidente o con el cabeza de logística

## Tramo 2 · Proceso actual (cómo lo resuelven hoy, sin el sistema)

1. Cuéntame la última vez que necesitaste algo de otro equipo. ¿Qué hiciste
   exactamente? Necesitábamos organizar como los patrocinadores se acomodaban en el social run para poder recibirlos conforme iban llegando, nos tuvimos que poner de acuerdos con los que manejan esa área para establecer una ruta de acceso, sin embargo, todo fue más complejo y no teníamos donde ver todo esto, lo hacíamos por whatsapp y se perdían los mensajes
2. Describe paso a paso cómo armaron la agenda y el acta de la última junta.
  No suele haber una, normalmente no tenemos idea de lo que se va a hablar en la junta hasta que empieza 
3. La última vez que un equipo necesitó ayuda de otro (por ejemplo, para
   montar un stand), ¿cómo lo pidieron y quiénes se enteraron?
   Normalmente se solicita ayuda vía el grupo de whatsapp, pero no suelen contestar o se pierde toda la información 

## Tramo 3 · Dolores

1. ¿En qué se te va más tiempo en tu trabajo en la mesa?
   En la juntas, solemos hacer juntas de unas 2 horas y generalmente concluimos pocas cosas
2. Cuéntame de algo que se haya quedado sin hacerse. ¿Qué pasó? Nada aún
3. ¿Qué herramientas o archivos usas en tu trabajo y cómo te llevas con cada uno? Usamos mucho drive, docs y excel, a muchos les cuesta manejar excel
4. Cuando le pides algo a alguien de otro equipo, ¿cómo te enteras de que
   ya lo vio y de que lo va a hacer? Generalmente es vía mensaje o llamada y no queda un registro de eso

## Tramo 4 · Excepciones

1. Cuéntame de una vez que necesitaste información de la mesa. ¿Qué pasó?
   Es complicado encontrarla, no hay un archivo o algo; normalmente preguntas o la buscas en los mensajes
2. ¿Qué ocurre cuando alguien deja la mesa a la mitad del periodo? ¿Quién se
   entera y qué se actualiza?
   Los coordinadores lo dan de baja
3. Si alguien cambia de equipo a la mitad del periodo, ¿qué pasa con las
   cosas que tenía asignadas?
   No se reasignan, puede seguir trabajando en eso, siempre y cuando su cabeza de equipo no se las reasigne a alguien más

## Tramo 5 · Verificación de supuestos

Cada pregunta nace de una regla de negocio de la visión que se escribió sin
entrevistar a nadie (RN = regla de negocio del apartado 4 de la visión).

| # | Supuesto que se verifica | Pregunta | Respuesta |
|---|---|---|---|
| V1 | RN-1: el acceso depende del rol y del área | Cuando necesitas información de otro equipo, ¿cómo la consigues? | Puedes acceder al equipo y buscarla, o solicitarla si es información no abierta a todos |
| V2 | RN-2: se puede consultar sin poder modificar | En las herramientas que usan hoy, ¿qué puede hacer cada quien con lo que ve? | Puede hacer todo |
| V3 | RN-3: un dato, una fuente | ¿Dónde está anotado quién está en qué equipo y quién mantiene esa información? | Dentro del sistema ya están todos los roles |
| V4 | RN-4: campos restringidos | ¿Qué tipo de información manejan en la mesa y quién la ve en cada caso? | Desde proveedores y patrocinios hasta temas legales; depende del equipo en el que estés y tu rango |
| V5 | RN-5: autoría de los cambios | Cuando algo cambia en un documento o una hoja, ¿cómo te enteras de qué cambió y quién lo hizo? | Queda un registro de los cambios y te dice quién los hizo |
| V6 | RN-6: integridad referencial | ¿Qué pasa con las tareas, actas y archivos de alguien que ya no está en la mesa? | Se derivan a su cabeza de equipo y luego la cabeza puede delegarlas de nuevo |
| V7 | RN-7: borrado restringido | ¿Cómo se guardan y se cuidan las actas y los archivos de la mesa? Cuéntame un ejemplo reciente. | Algunos en Drive, pero la mayoría se pierden en WhatsApp |

## Revisión de las preguntas

Se procuró que ninguna se conteste con sí/no ni sugiera la respuesta: piden un relato ("cuéntame", "qué pasó", "cómo") o una explicación
("por qué"). Las preguntas V1–V7 se cruzan con la visión en la tabla
anterior.

## Ficha de dominio para la dupla

> Se entregó a la dupla sin explicarle qué debía responder.

**Rol:** Eres un miembro de la mesa directiva de FESAL (Federación de
Estudiantes, Universidad Anáhuac Querétaro).

**Contexto del negocio:** La mesa tiene entre 26 y 30 personas,
organizadas en equipos (por ejemplo: Comunicación, Patrocinios,
Logística, Sociedades). Coordinan juntas semanales, proyectos propios de
cada equipo, patrocinios de empresas para financiar eventos, y piden
ayuda entre equipos cuando algo se les junta (por ejemplo, montar un
stand). Entre las 26-30 personas se coordinan con las herramientas que
cada quien tiene a la mano.

Responde como alguien que vive ese día a día, con ejemplos de tu propio
equipo: no describas un sistema, describe tu trabajo. No sabes qué te
van a preguntar ni qué "deberías" contestar.

## Bitácora de la entrevista

Un solo entrevistado (Pablo Andrade, miembro de la mesa), los cinco tramos
aplicados. Cada fila contrasta un supuesto mío con lo que respondió.

| Supuesto | Resultado | Qué se respondió | Efecto en los requisitos |
|---|---|---|---|
| El dolor central es la duplicidad de versiones y el acceso por área (visión §2). | **No se confirmó** | Con preguntas abiertas no lo mencionó: habló de juntas sin agenda y de mensajes perdidos. | Se prioriza la coordinación (agenda, solicitudes, registro). |
| Las solicitudes entre equipos se pierden en el chat. | **Confirmado** | "Se solicita ayuda vía WhatsApp, pero no suelen contestar o se pierde toda la información". | RF-11 y RF-17 en "Confirmado". |
| Pedir algo a otro equipo no deja registro. | **Confirmado** | "Es vía mensaje o llamada y no queda un registro". | Refuerza RF-11, RF-17 y RF-18. |
| Las juntas se preparan con una agenda. | **Falso** | "No suele haber una". | Nuevo RF-19. |
| Los usuarios no son técnicos. | **Confirmado en parte** | "A muchos les cuesta manejar Excel". | Sustenta RNF-06. |
| V1 · El acceso depende del rol y del área (RN-1). | **Confirmado** | Se accede al equipo o se solicita si la información no es abierta. | Sustenta RF-13. |
| V2 · Hoy se puede consultar sin modificar (RN-2). | **Falso** | "Puede hacer todo". | Hoy no hay límite de edición; nuevo RF-21 (Supuesto) para limitarla. |
| V3 · Existen copias independientes de un mismo dato (RN-3). | **No se observó** | "Dentro del sistema ya están todos los roles": hay una fuente para el roster. | Sin cambios. |
| V4 · Hay información restringida (RN-4). | **Confirmado** | Proveedores, patrocinios y temas legales; depende del equipo y del rango. | Sustenta RF-13; el "rango" corresponde al coordinador (cabeza de equipo). |
| V5 · Los cambios se atribuyen a una persona (RN-5). | **Confirmado en parte** | "Queda un registro de los cambios y te dice quién". | RF-16 pasa a "Confirmado en parte": hoy esperan ver quién cambió algo y la v1 solo guarda al autor (§9). |
| V6 · Los registros respetan relaciones, p. ej. tareas de quien se va (RN-6). | **Confirmado** | Se derivan a la cabeza de equipo, que las delega de nuevo. | Nuevo RF-20 (reasignar compromisos), Origen "Confirmado". |
| V7 · El borrado está controlado (RN-7). | **Falso** | "Algunos en Drive, pero la mayoría se pierden en WhatsApp": el problema no es el borrado sino la pérdida. | Sustenta RF-07 (minuta) y RF-09 (acuerdos) como registro durable. |
| Al salir alguien, la mesa actualiza su estado. | **Confirmado** | "Los coordinadores lo dan de baja". | RF-15 pasa a "Confirmado". |

**Lo que no esperaba:**
- Las juntas duran unas 2 horas, "generalmente concluimos pocas cosas" y llegan sin agenda.
- La **cabeza de equipo** (el coordinador) es quien reasigna el trabajo de quien se va; no es automático.

- Quien cambia de equipo sigue con sus tareas hasta que la cabeza se las
  reasigne: no hay reasignación automática.
- Dice que "ya están todos los roles dentro del sistema" y que "queda registro
  de los cambios". Podría referirse a Drive o al propio Fresales, que ya está
  desplegado (confirmar).

**Cambios hechos por la entrevista:** RF-11, RF-15, RF-17, RF-19 y RF-20 con
Origen "Confirmado"; RF-16 "Confirmado en parte"; RF-21 nuevo (Supuesto);
RNF-06 con sustento; contexto de usuarios y §9 actualizados. Ver el registro
de cambios (v1.1) de
[`especificacion-requisitos.md`](especificacion-requisitos.md).

**Limitaciones:** una sola entrevista, y el entrevistado conoce Fresales.

**Notas de la entrevista (respuestas literales):** debajo de cada pregunta de
los tramos 1 a 5.
