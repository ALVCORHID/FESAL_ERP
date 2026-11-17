# Análisis de impacto de SC-001

**Analiza:** [SC-001 · Rediseño de la navegación y la pantalla de entrada en el
celular](sc-001-rediseno-ux-estilo-instagram.md)
**Autor:** Álvaro Correa Hidalgo, dueño del sistema
**Fecha:** 12 de noviembre de 2026

Evidencia: el repositorio de documentación `ALVCORHID/FESAL_ERP` (este) y el
repositorio de código de Fresales, que vive solo en la máquina de desarrollo
(`Dev/fesal/fesal-interno`, sin remoto) y se despliega en
`https://fesal-interno.pages.dev`.

## Solicitud que se analiza

SC-001 pide tres cosas separables: (a) botones de navegación fijos y pocos,
visibles sin desplazarse en el teléfono; (b) una pantalla de entrada que
responda "¿qué me toca?"; (c) un solo lugar para crear cosas y un solo lugar
para los avisos. Y una restricción: en la computadora nada cambia.

El tipo que declara el solicitante —perfectivo— coincide con el análisis: no
hay defecto que reparar ni cambio del entorno al que adaptarse. Lo que se
mueve son atributos de calidad (RNF-06, RNF-07), no funciones.

## Traducción a términos del sistema

| Lo que pide el solicitante | En términos del sistema |
|---|---|
| "La tira de once pestañas no cabe" | La lista de módulos que arma `src/layout/Layout.jsx`, que hoy es la única navegación en todos los anchos. Son 11 destinos, 12 con "Administrar" para el rol coordinador. |
| "Botones fijos abajo" | Una barra de pestañas nueva, fija, que solo se monta por debajo del punto de ruptura. El punto ya existe: `--ancho: 900px` y `@media (max-width: 899px)` en `src/estilos.css`. |
| "Una pantalla que diga qué me toca" | La función SQL `mis_pendientes()`, que ya define ese conjunto —juntas por confirmar de los próximos 7 días, justificantes por revisar, compromisos propios vencidos o por vencer en 7 días, y solicitudes dirigidas sin tomar— y que hoy solo alimenta el aviso al celular. Una pantalla de entrada nueva la consume y la pinta. |
| "En la computadora nada cambia" | La pantalla `src/paginas/Inicio.jsx` (el tablero "Mesa directiva") no se modifica y se sigue mostrando desde 900 px. |
| "Confirmar asistencia sin abrir la junta" | Reutilizar el componente `ConfirmarAsistencia` sobre la tabla `confirmaciones`. |
| "Un solo botón para crear" | Un diálogo modal que ofrece los destinos de creación que hoy existen: `/solicitudes`, `/juntas` y `/avisos/nuevo`, con las mismas reglas de rol que ya controlan esos botones. |
| "Un solo lugar para los avisos" y "si hay alguno sin leer" | La tabla `avisos`, que ya existe y ya está protegida por RLS. El "sin leer" no tiene dónde guardarse: no existe columna ni tabla de lectura por persona. |
| "Dos toques o menos" | Las 12 pantallas de uso diario alcanzables desde la entrada; hoy varias están a un toque desde la tira y pasarían a dos. |

**Lo que no se puede traducir y hay que decidir antes de empezar:**

1. **El criterio 5 de la solicitud ("el número de la pantalla es el mismo que
   el del aviso") no se puede cumplir hoy tal cual está la base.**
   `mis_pendientes()` cuenta un justificante por revisar a quien tenga la
   marca `ve_asistencias`, pero las políticas de RLS no dejan leer la junta
   de ese justificante si es de otro equipo: el contador diría 5 y la
   pantalla mostraría 4. Para que cuadre hace falta una de dos: una
   migración que deje leer esas juntas a quien tiene `ve_asistencias`, o
   mostrar la tarjeta sin los datos de la junta. **La primera convierte el
   cambio en uno que sí toca la base de datos y sale de lo que pide SC-001.**
   Necesito que el solicitante elija.
2. **"Sin leer" por persona o por aparato.** No hay dónde guardarlo en el
   servidor. Guardarlo en el teléfono es gratis pero significa que el punto
   se apaga en un aparato y sigue encendido en otro. Guardarlo en el
   servidor es una tabla nueva, y entonces el cambio deja de ser solo de
   interfaz. Hace falta decidir cuál.

## Impacto directo

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Especificación de requisitos | RNF-06 (usabilidad) | Su métrica mide hoy el alta de un miembro (≤ 2 min). SC-001 introduce un segundo criterio medible —dos toques o menos a las pantallas de uso diario— que hay que añadir, o abrir un RNF-10 para él. |
| Especificación de requisitos | RNF-07 (instalable) | El texto no cambia, pero su verificación se amplía: la barra inferior tiene que respetar el área segura del teléfono con la aplicación instalada. |
| Especificación de requisitos | RF-01 a RF-21 | Ninguno cambia de texto. El cambio no agrega ni quita capacidades: mueve por dónde se llega a las que ya existen. |
| Especificación de requisitos | §5, tabla de trazabilidad | Sin cambio de filas. Si se abre el RNF-10 del párrafo anterior, entra en la fila "Transversales". |
| Especificación de requisitos | §6, pantallas del prototipo por caso de uso | Hay que rehacerla. Hoy mapea cada caso de uso a una pantalla de Figma; con dos caras (teléfono y computadora) una pantalla por caso de uso deja de alcanzar. |
| Especificación de requisitos | §9, pendientes conocidos | Agregar que el prototipo de Figma queda desactualizado respecto de la cara de teléfono. |
| Caso de uso | CU-01, paso 8 y postcondición de éxito | Dicen "entra a Inicio (Mesa directiva)". En el teléfono, Inicio deja de ser Mesa directiva y pasa a ser la pantalla de pendientes. Hay que redactar el paso según el ancho. |
| Caso de uso | CU-02 (iniciar sesión) | Mismo problema que CU-01: su postcondición es "entra a Inicio". No está detallado en un archivo propio; vive en la tabla de §5 de la especificación. |
| Caso de uso | CU-03 (proponer junta) | Su disparador cambia en el teléfono: hoy es el botón dentro de la pantalla Juntas; se agrega el botón único de crear. El flujo posterior no cambia. |
| Caso de uso | CU-07 (publicar solicitud) | Igual que CU-03: se agrega un disparador. Tomar y resolver no cambian. |
| Caso de uso | Diagrama `docs/diagramas/casos-de-uso.drawio` y su PNG | Sin cambio de actores ni de casos. Solo se regenera si se decide anotar los disparadores nuevos. |
| Diagrama de clases | `doc/arch/diagramas.md`, ejemplo 6 | **Sin impacto.** No cambia ninguna entidad, atributo, método ni multiplicidad: el cambio no toca la base de datos. |
| Diagrama de secuencia | `doc/arch/diagramas.md`, ejemplo 5 | **Sin impacto.** Modela el pase de lista, que no se modifica. |
| Diagrama de flujo con decisiones | `doc/arch/diagramas.md`, ejemplo 4 | El nodo final "Abrir sesión y entrar a Inicio" hereda la ambigüedad de CU-01: hay que precisar a cuál Inicio. Es un nodo, no la estructura. |
| Código: navegación | `src/layout/Layout.jsx` | Decide entre las dos caras según el ancho; la tira de módulos queda solo para pantalla ancha. |
| Código: rutas | `src/App.jsx` | Cuatro rutas nuevas: la lista de secciones de equipo, la de cuenta, la de avisos y la que conserva el tablero en el teléfono. Ninguna ruta existente se renombra ni se elimina: los avisos al celular y los enlaces de WhatsApp apuntan a ellas. |
| Código: estilos | `src/estilos.css` | Reglas de las barras, del área segura y de las piezas nuevas, todas bajo el punto de ruptura que ya existe. |
| Código: pantallas y componentes | `src/paginas/`, `src/componentes/` | Pantalla de entrada de teléfono, lista de secciones, pantalla de cuenta, lista de avisos; barra superior, barra de pestañas, hoja de crear, tarjeta de pendiente, lista de filas y círculos de equipos. |
| Código: lógica pura | `src/lib/` | Qué pestaña está activa según la ruta, el orden de las tarjetas, la marca de visto y el conteo de pendientes. Son funciones puras, para poder probarlas sin navegador. |

## Impacto indirecto

| Artefacto | Qué pasa |
|---|---|
| ADRs (registros de decisión de arquitectura) | **No existen en el repo.** Las decisiones de Fresales se registran en los documentos de diseño por función de `docs/superpowers/specs/` del repositorio de código y en la sección de decisiones del `README.md`. Ahí es donde hay que dejar escrita esta: por qué dos caras y no una. |
| Diagrama de contenedores C4 | **No existe en el repo.** Si existiera, no cambiaría: no se agrega ni se quita ningún contenedor (sigue siendo aplicación web, base de datos gestionada y función de envío de avisos). |
| Diagrama de componentes | **No existe en el repo.** Es el artefacto que más cambiaría si existiera: se agregan diez piezas de interfaz y se parte la navegación en dos. Vale la pena abrirlo con este cambio. |
| Máquina de estados | **Sin impacto.** El ciclo de la solicitud (abierta, tomada, resuelta) no se toca; solo cambia desde dónde se dispara publicarla. |
| Modelo entidad-relación | **Sin impacto.** Cero archivos de `supabase/` en el alcance: ni tablas, ni columnas, ni políticas, ni funciones. |
| Políticas de seguridad (RLS) | **Sin impacto en su definición**, pero sí son una restricción de diseño: la pantalla de entrada no puede mostrar nada que la base no deje leer a esa persona, y el conteo tiene que respetarlo. Es la causa de la discrepancia del punto 1 de "lo que hay que decidir". |
| Pruebas automatizadas | Crecen. La base actual es de 552 pruebas en 30 archivos. Este cambio pide pruebas puras nuevas para la pestaña activa por ruta, el orden de las tarjetas, la marca de visto y el conteo de pendientes, más el ajuste de la prueba de contraste si se agregan colores. |
| Pruebas manuales | Un recorrido a 375 px por cada rol (miembro, responsable, coordinación, asesoría), con los cuatro estados de la pantalla de entrada y el teclado abierto. No lo cubren las pruebas automáticas. |
| Accesibilidad | Entra de lleno: controles de 44 px o más, foco visible, operable con teclado, marca de pestaña activa para lector de pantalla, respeto a la preferencia de movimiento reducido y contraste medido. |
| PWA y avisos al celular | La aplicación instalada es el caso principal: área segura inferior, y cerrar y reabrir la aplicación después de publicar para que cargue la versión nueva. El punto de avisos sin leer se apoya en la tabla `avisos`, no en el historial de envíos. |
| Función de envío de avisos | **Sin impacto.** No se toca su código ni su programación. |
| Prototipo de Figma | Queda desactualizado: fue dibujado para una sola cara. Decidir si se actualiza o se declara histórico. |
| Visión del producto | **Sin impacto.** No cambian objetivos, alcance ni reglas de negocio. |
| Guion de entrevista | **Sin impacto.** Es un registro de lo que ya se preguntó. |
| Datos en producción | **Sin impacto.** Ninguna migración, ningún respaldo necesario, ninguna ventana de mantenimiento. |
| Capacitación de la mesa | Impacto bajo y a favor: el objetivo del cambio es no tener que explicar. Conviene un aviso de una línea el día que se publique. |

## Lo que queda sin tocar

- La base de datos completa: tablas, columnas, índices, funciones, disparadores y políticas de RLS.
- Quién puede ver y editar qué. El cambio no amplía ni restringe un solo permiso; la pantalla de entrada muestra lo que la base ya deja leer.
- Las rutas existentes. Ninguna se renombra ni se elimina, porque los avisos al celular y los enlaces de WhatsApp apuntan a ellas.
- El tablero "Mesa directiva" que se proyecta en los primeros minutos de cada junta, y la pantalla ancha en general.
- Los casos de uso CU-04, CU-05, CU-06 y CU-08, y los RF que realizan.
- El envío de avisos al celular y su programación.

## Riesgos

| Riesgo | Por qué importa | Cómo se contiene |
|---|---|---|
| Lo que hoy se encuentra en un toque pasa a dos | Calendario, Mis asistencias, Patros y Proyectos están hoy en la tira. Si se esconden detrás de una lista, la solicitud mejora unas pantallas empeorando otras. | Medir las 12 pantallas antes y después y exigir que ninguna pase de dos toques. Es el criterio 1 de SC-001 y es verificable. |
| Dos criterios de ancho que se separan | Si el JavaScript decide la cara con un número y la hoja de estilos con otro, aparece una franja de anchos donde se mezclan las dos caras. | Un solo número compartido entre ambos. El `--ancho: 900px` de `src/estilos.css` ya existe; reutilizarlo. |
| La lógica de pendientes se duplica en el cliente | Si la pantalla reimplementa lo que decide `mis_pendientes()`, el número del aviso y las tarjetas se separan sin que nadie se entere. | Pruebas que comparen el número de tarjetas contra lo que devuelve la función de la base, con varios usuarios sembrados. Es el criterio 5 de SC-001. |
| La computadora que proyecta tiene menos de 900 px útiles | Una laptop de 1280×720 al 150 % de escala da unos 853 px: mostraría la cara de teléfono en plena junta, que es justo lo que SC-001 prohíbe. | Conservar una ruta que muestre el tablero a cualquier ancho y proyectar esa, o revisar el ancho una vez antes de la primera junta. |
| Área segura del teléfono con la aplicación instalada | La barra de abajo puede quedar bajo la barra del sistema y volverse intocable. | Respetar la zona segura y probar a 375 px antes de publicar. Es el criterio 14 de SC-001. |
| El teclado tapa el campo de texto | Una barra fija abajo sube con el teclado y se come el campo donde se escribe. | Esconder la barra mientras haya un campo con foco. Es el criterio 3 de SC-001. |
| La discrepancia del justificante | El criterio 5 no se puede cumplir sin una decisión previa (ver "Traducción"). | Decidirlo antes de empezar, no durante. |

## Alternativas consideradas

**A · Reordenar y agrupar la tira de pestañas actual.** Dejar la navegación
donde está y meter lo menos usado en un menú desplegable. Es la más barata
con diferencia y no cambia ningún hábito. Pero no resuelve la segunda mitad
de la solicitud: sigue sin haber un lugar que diga qué le toca a cada quien,
que es lo que hace que la mesa regrese a WhatsApp. Resuelve la queja visible
y deja la causa.

**B · Una sola cara para todos los anchos.** Rediseñar la navegación una vez
y usarla en teléfono y computadora. Menos código que mantener, una sola
verdad, y a futuro lo más barato. El problema es el tablero que se proyecta
en los primeros minutos de cada junta: ese ritual depende de ver mucha
información a la vez, y una navegación pensada para el pulgar lo empeora.
Convierte un cambio sin riesgo en uno que toca algo que la mesa usa cada
semana frente a todos.

**C · Cara nueva solo por debajo de 900 px.** Dos caras: la de teléfono es
nueva, la de computadora queda intacta. Cuesta más que A y deja dos caminos
de navegación que mantener. A cambio, el tablero que se proyecta no corre
ningún riesgo, y la solicitud se cumple entera.

**Me inclino por C.** La razón no es técnica sino de reversibilidad: C se
puede publicar y, si la mesa lo rechaza, se revierte sin tocar un solo dato
ni una sola pantalla de computadora. B no tiene esa salida, y A no resuelve
lo que se pidió. El costo de C —dos caras que mantener— es real y hay que
anotarlo como deuda: la próxima función que toque navegación se escribe dos
veces.

## Estimación de esfuerzo

| Concepto | Horas |
|---|---|
| Cambios de diseño y documentación | 10 |
| Implementación | 36 |
| Pruebas | 16 |
| **Total** | **62** |

Sesenta y dos horas son casi ocho jornadas de ocho horas, o entre dos y tres
semanas de trabajo a tiempo parcial, que es el ritmo real de este proyecto.

### De dónde sale este número

**Criterio.** Se cuentan unidades modificables reales, no una impresión.

Para implementación, 24 archivos bajo `src/` a razón de hora y media cada
uno, incluyendo escribirlo y dejarlo funcionando: cuatro pantallas nuevas
(entrada de teléfono, lista de secciones, cuenta y avisos), una que decide
qué cara mostrar, seis componentes nuevos (las dos barras, la hoja de crear,
la tarjeta de pendiente, la lista de filas y los círculos de equipos), seis
módulos de lógica pura, cuatro pantallas existentes que se ajustan para
recibir el botón único de crear y la confirmación desde la tarjeta, el
armazón de navegación, las rutas y la hoja de estilos.

Para documentación, siete artefactos a actualizar a hora y media cada uno:
RNF-06, §6 y §9 de la especificación, CU-01, CU-02, CU-03 y CU-07, más el
nodo final del diagrama de flujo.

Para pruebas, cuatro archivos de pruebas puras nuevas y el ajuste del de
contraste (8 h), más el recorrido manual de 12 pantallas por cuatro roles
con los cuatro estados de la pantalla de entrada (8 h).

**Supuestos.** Una sola persona, que ya conoce el código y no tiene que
estudiarlo. El punto de ruptura de 900 px ya existe y se reutiliza. La
función `mis_pendientes()` ya existe y no se modifica. La maqueta está
aprobada antes de empezar: si hay que diseñarla, son entre 8 y 12 horas más.
Se trabaja con la base local, sin esperar a nadie.

**Qué no se incluyó.** La migración de la discrepancia del justificante, si
se elige esa salida: es trabajo de base de datos y cambia el tipo de
mantenimiento del cambio. La actualización del prototipo de Figma. La
publicación y el aviso a la mesa. La prueba en un iPhone real con la
aplicación instalada y con lector de pantalla, que requiere el aparato y a
su dueño. Y el margen por imprevistos: la cifra es de trabajo limpio, sin
contingencia. Con un 20 % de holgura, 74 horas.

## Recomendación

**Aceptar**, con tres condiciones previas.

El cambio es perfectivo, no toca datos y es reversible sin consecuencias: en
la escala de riesgo de este sistema es de los baratos. Y ataca algo que está
documentado desde el principio del proyecto —el RNF-06 pide que se use desde
el celular sin capacitación— que hoy no se cumple de verdad.

Las condiciones:

1. **Decidir antes de empezar la discrepancia del justificante.** Si la
   salida es la migración, este deja de ser un cambio solo de interfaz y hay
   que reevaluar el alcance y volver a estimar.
2. **Decidir dónde vive el "sin leer".** En el teléfono es gratis y cumple
   lo pedido; en el servidor es una tabla nueva y sale del alcance.
3. **Medir los dos toques antes y después**, con la lista de las 12
   pantallas por delante. Es el único criterio de SC-001 que puede fallar
   silenciosamente: es fácil mejorar la entrada y empeorar cuatro pantallas
   sin notarlo hasta que alguien se queja.

Sobre la prioridad alta que sugiere el solicitante: la comparto, pero por su
razón y no por urgencia. Nada está roto. Lo que pesa es el relevo de la mesa:
publicarlo antes de que entre la gente nueva evita enseñar dos veces. Si el
relevo se mueve, la prioridad baja a media sin más consecuencia.
