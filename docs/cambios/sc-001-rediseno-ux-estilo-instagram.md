# SC-001: Rediseño de la navegación y la pantalla de entrada en el celular

## Estado

Propuesta · 12 de noviembre de 2026

## Quién la pide

Juan Pablo Mues, presidente de la mesa directiva de FESAL.

## Qué se pide

Que Fresales se use en el celular como se usan las aplicaciones que la mesa
ya conoce: unos pocos botones fijos en la parte de abajo para moverse entre
lo principal, y una pantalla de entrada que diga qué le toca a cada quien.
Que llegar a lo que se usa todos los días no obligue a buscar dentro de una
lista larga de pestañas. En la computadora, que todo siga como está.

## Por qué

Hoy el menú es una tira de once pestañas (doce para quien coordina) que no
cabe en la pantalla del teléfono y hay que desplazar de lado: la pestaña que
se busca puede estar fuera de la vista, y no hay forma de saber que existe
más que arrastrando.

Tampoco hay un lugar que responda "¿qué me toca a mí?". Para saberlo hay que
entrar a Juntas, luego a Compromisos y luego a Solicitudes, y armarlo uno
mismo. El sistema ya calcula ese dato —es el que usa para mandar el aviso al
celular— pero no lo muestra en ninguna pantalla.

La mesa entra casi siempre desde el teléfono, y varios con la aplicación
instalada en la pantalla de inicio. La especificación de requisitos ya
reconoce esto: el RNF-06 pide que el sistema se use desde el celular sin
capacitación, y el RNF-07 que se instale sin tienda de aplicaciones
(`docs/especificacion-requisitos.md`, §4).

Cada año entra gente nueva a la mesa. Hoy hay que explicarles dónde está
cada cosa.

## Qué pasa si no se hace

La mesa sigue resolviendo por WhatsApp lo que es urgente, porque es más
rápido preguntar en el grupo que encontrarlo en el sistema. Eso es
exactamente lo que Fresales vino a sustituir: la especificación dice en su
§1 que el sistema existe para reemplazar la coordinación por WhatsApp y
hojas de cálculo, y la entrevista del 1 de octubre registró que las
solicitudes pedidas por WhatsApp se pierden sin dejar registro.

El efecto concreto: se siguen perdiendo solicitudes, se sigue llegando a las
juntas sin saber qué hay pendiente, y cada relevo anual vuelve a pagar la
misma curva de aprendizaje. El sistema se queda como el lugar donde se
archiva lo que ya pasó, en vez del lugar donde se coordina lo que viene.

## Tipo de mantenimiento

- [ ] Correctivo
- [ ] Adaptativo
- [x] Perfectivo
- [ ] Preventivo

Perfectivo: no se repara ninguna falla ni se responde a un cambio del
entorno; se mejora la facilidad de uso de un sistema que ya funciona, que es
el atributo que miden el RNF-06 y el RNF-07.

## Criterio de aceptación

Cada punto se responde con sí o no.

1. Desde la pantalla de entrada, en un teléfono, se llega a cualquiera de
   las pantallas de uso diario (juntas, compromisos, solicitudes, proyectos,
   calendario, comunicación, equipos, mi horario, mis asistencias, patros,
   mi cuenta, administrar) en **dos toques o menos**.
2. Los botones principales de navegación están visibles sin desplazarse, en
   cualquier pantalla de la aplicación.
3. Esos botones no quedan encima del campo de texto cuando el teclado del
   teléfono está abierto.
4. La pantalla de entrada muestra una cosa por cada pendiente de quien
   entró: juntas por confirmar, justificantes por revisar, compromisos
   vencidos o por vencer, y solicitudes dirigidas a esa persona.
5. El número de pendientes que muestra esa pantalla es el mismo que el del
   aviso que manda el sistema al celular.
6. Cuando no hay ningún pendiente, la pantalla lo dice en vez de quedarse
   vacía.
7. Confirmar asistencia a una junta se hace desde la pantalla de entrada,
   sin abrir la junta.
8. Crear una solicitud, una junta o un aviso se hace desde un mismo botón
   disponible en toda la aplicación, y ese botón solo ofrece lo que el rol
   de quien entró puede crear.
9. Quien no puede crear nada recibe una explicación, no un menú vacío.
10. Los avisos del sistema se consultan desde un solo lugar, y se distingue a
    simple vista si hay alguno sin leer.
11. En la computadora, el tablero que se proyecta en los primeros minutos de
    cada junta se ve exactamente igual que antes del cambio.
12. Ninguna persona ve información de equipos a los que no pertenece
    (RF-13), y el cambio no altera quién puede ver o editar qué.
13. Todos los botones se pueden tocar con el pulgar sin fallar, y la
    aplicación se puede recorrer con teclado.
14. En un teléfono con la aplicación instalada, la barra de abajo no queda
    tapada por la barra del sistema operativo.

## Prioridad que sugiere el solicitante

**Alta.**

La mesa se renueva en el periodo siguiente y la gente nueva aprende el
sistema en sus primeras semanas: si el cambio llega después del relevo, se
aprende la versión vieja y hay que volver a explicar. Además, el costo de no
hacerlo no es que el sistema falle, sino que se use a medias y la
coordinación regrese a WhatsApp, que es el problema que el proyecto vino a
resolver.

No es urgente en el sentido de que nada está roto: ningún dato está en
riesgo y el sistema funciona. Por eso alta y no crítica.
