# Cinco ejemplos para practicar — Fresales

Ingeniería de Software I · SIS3407 · Unidad 3, diseño de software.
Sistema modelado: **Fresales**, el sistema interno de la mesa directiva de FESAL
(módulo piloto de PGI), desplegado en https://fesal-interno.pages.dev.

Los diagramas se entregan como **texto de Mermaid**, junto con una imagen de cada uno.
GitHub dibuja el código al abrir este archivo. Para ver uno en el editor, pégalo en
https://mermaid.live. La numeración (4 a 8) es la de la guía rápida de Mermaid.

## De dónde sale cada dato

- **Requisitos y casos de uso:** [especificación de requisitos](../../docs/especificacion-requisitos.md)
  (v1.1) y [CU-01](../../docs/diagramas/CU-01-crear-cuenta.md).
- **Estructura de datos:** el esquema real de la base de datos de Fresales (Supabase,
  esquema `public`), consultado en modo lectura el 27-oct-2026. Solo se leyó la
  estructura de las tablas, no sus datos.

## 4 · Diagrama de flujo con decisiones

CU-01, crear cuenta con código de invitación, con sus flujos alternos A1 a A4.

![Diagrama de flujo con decisiones](img/04-flujo-decisiones.png)

Código Mermaid:

```mermaid
flowchart TD
    ini([Inicio])
    llenar["Elegir Crear cuenta y llenar el formulario"]
    campos{¿Campos completos y válidos?}
    err1["Señalar el campo con problema"]
    cel{¿El celular se entiende a 10 dígitos?}
    err2["Mostrar: Ese celular no se entiende"]
    inv{¿Hay invitación vigente con ese celular y código?}
    err3["Rechazar el alta y no crear usuario"]
    dup{¿El celular ya tiene cuenta?}
    err4["Sugerir la pestaña Entrar"]
    alta["Crear usuario, reclamar la fila del roster y consumir la invitación"]
    sesion["Abrir sesión y entrar a Inicio"]
    fin([Fin])

    ini --> llenar
    llenar --> campos
    campos -->|No| err1
    err1 --> llenar
    campos -->|Sí| cel
    cel -->|No| err2
    err2 --> llenar
    cel -->|Sí| inv
    inv -->|No| err3
    inv -->|Sí| dup
    dup -->|Sí| err4
    dup -->|No| alta
    alta --> sesion
    err3 --> fin
    err4 --> fin
    sesion --> fin

    style alta fill:#ff5d00,color:#fff
```

## 5 · Diagrama de secuencia

Cómo se pasan los mensajes entre piezas al tomar la asistencia de una junta.

![Diagrama de secuencia](img/05-secuencia.png)

Código Mermaid:

```mermaid
sequenceDiagram
    actor Coo as Coordinador
    participant Web as App web
    participant BD as Base de datos

    Coo->>Web: Abre el pase de lista de una junta
    Web->>BD: Pide a quién se convocó (equipos e invitados)
    BD-->>Web: Miembros convocados
    Web-->>Coo: Muestra la lista de la junta
    Coo->>Web: Marca presente o retardo a un miembro
    Web->>BD: Guarda la asistencia con la hora de marca
    BD-->>Web: Asistencia guardada
    Web-->>Coo: Muestra la lista actualizada
```

## 6 · Diagrama de clases

Atributos, métodos y multiplicidad de las entidades principales. Los métodos salen de los requisitos RF-06 a RF-20 y de los eventos del sistema.

![Diagrama de clases](img/06-clases.png)

Código Mermaid:

```mermaid
classDiagram
    class Miembro {
        -nombre: string
        -cargo: string
        -area: string
        -rol: string
        -telefono: string
        -activo: boolean
        -coordinadorGeneral: boolean
    }
    class Equipo {
        -nombre: string
        -descripcion: string
    }
    class Pertenencia {
        -esCoordinador: boolean
        -puesto: string
    }
    class Invitacion {
        -codigo: string
    }
    class JuntaFija {
        -titulo: string
        -diaSemana: int
        -hora: Time
        -activa: boolean
    }
    class Junta {
        -titulo: string
        -fecha: Date
        -lugar: string
        -agenda: string
        -minuta: string
        -estado: string
        +guardarMinuta(texto) void
        +cancelar() void
        +reactivar() void
    }
    class Asistencia {
        -presente: boolean
        -retardo: boolean
        -marcadaEn: Date
    }
    class Acuerdo {
        -descripcion: string
        -fechaLimite: Date
        -estado: string
        -cerradoEn: Date
        +cerrar() void
        +reasignar(responsable) void
    }
    class Solicitud {
        -titulo: string
        -detalle: string
        -urgencia: string
        -estado: string
        -cupos: int
        +tomar(miembro) void
        +resolver() void
    }
    class Proyecto {
        -nombre: string
        -descripcion: string
        -estado: string
    }

    Miembro "1" --> "*" Pertenencia : tiene
    Equipo "1" --> "*" Pertenencia : integra
    Miembro "1" --> "0..1" Invitacion : recibe
    JuntaFija "1" --> "*" Junta : genera
    Junta "*" --> "*" Equipo : convoca a
    Miembro "1" --> "*" Junta : crea
    Junta "1" --> "*" Asistencia : registra
    Asistencia "*" --> "1" Miembro : de
    Junta "1" --> "*" Acuerdo : produce
    Acuerdo "*" --> "1" Miembro : responsable
    Solicitud "*" --> "1" Equipo : se publica en
    Solicitud "*" --> "1" Miembro : creada por
    Solicitud "*" --> "0..*" Miembro : tomada por
    Equipo "1" --> "*" Proyecto : tiene
```

## 7 · Diagrama de estados

Ciclo de vida de una solicitud de ayuda entre equipos (RF-11, RF-17 y RF-18).

![Diagrama de estados](img/07-estados.png)

Código Mermaid:

```mermaid
stateDiagram-v2
    [*] --> Abierta : un miembro publica la solicitud
    Abierta --> Tomada : un miembro la toma
    Tomada --> Resuelta : se marca como resuelta
    Resuelta --> [*]
```

## 8 · Diagrama entidad-relación

Las tablas principales de la base de datos de Fresales y cómo se relacionan. No se dibuja la relación de miembros con el usuario de autenticación.

![Diagrama entidad-relación](img/08-entidad-relacion.png)

Código Mermaid:

```mermaid
erDiagram
    MIEMBROS ||--o{ EQUIPO_MIEMBROS : pertenece
    EQUIPOS ||--o{ EQUIPO_MIEMBROS : integra
    MIEMBROS ||--o| INVITACIONES : recibe
    EQUIPOS ||--o{ JUNTAS : agenda
    JUNTAS_FIJAS |o--o{ JUNTAS : genera
    MIEMBROS |o--o{ JUNTAS : crea
    JUNTAS ||--o{ ASISTENCIA : registra
    MIEMBROS ||--o{ ASISTENCIA : tiene
    JUNTAS |o--o{ ACUERDOS : produce
    MIEMBROS ||--o{ ACUERDOS : responsable
    EQUIPOS ||--o{ SOLICITUDES : tablon
    MIEMBROS ||--o{ SOLICITUDES : crea
    SOLICITUDES ||--o{ SOLICITUD_TOMAS : tomada
    MIEMBROS ||--o{ SOLICITUD_TOMAS : toma
    EQUIPOS ||--o{ PROYECTOS : tiene

    MIEMBROS {
        uuid id PK
        uuid user_id UK
        text nombre
        text rol
        text telefono UK
        boolean activo
        boolean coordinador_general
    }
    EQUIPOS {
        uuid id PK
        text nombre UK
        text descripcion
    }
    EQUIPO_MIEMBROS {
        uuid id PK
        uuid equipo_id FK
        uuid miembro_id FK
        boolean es_coordinador
        text puesto
    }
    INVITACIONES {
        uuid miembro_id PK, FK
        text codigo
    }
    JUNTAS_FIJAS {
        uuid id PK
        text titulo
        smallint dia_semana
        time hora
        boolean activa
    }
    JUNTAS {
        uuid id PK
        text titulo
        timestamptz fecha
        text agenda
        text minuta
        text estado
        uuid equipo_id FK
        uuid serie_id FK
        uuid creado_por FK
    }
    ASISTENCIA {
        uuid id PK
        uuid junta_id FK
        uuid miembro_id FK
        boolean presente
        boolean retardo
    }
    ACUERDOS {
        uuid id PK
        uuid junta_id FK
        uuid responsable_id FK
        text descripcion
        date fecha_limite
        text estado
    }
    SOLICITUDES {
        uuid id PK
        text titulo
        text urgencia
        text estado
        smallint cupos
        uuid equipo_id FK
        uuid creado_por FK
    }
    SOLICITUD_TOMAS {
        uuid solicitud_id PK, FK
        uuid miembro_id PK, FK
    }
    PROYECTOS {
        uuid id PK
        text nombre
        text estado
        uuid equipo_id FK
    }
```

## Notas y diferencias con la especificación

- **Tablas que no se dibujan.** Los diagramas 6 y 8 incluyen solo las entidades necesarias
  para estos ejemplos. Quedaron fuera `canales`, `mensajes`, `archivos`, `carpetas_drive`,
  `avisos`, `confirmaciones`, `horarios`, `semestres`, `reglamento_firmas`, `patros`,
  `suscripciones_push` y `eventos_push`, además de las tablas de unión de juntas y avisos.
- **Estados de la junta.** La especificación v1.1 habla de junta "propuesta" y "confirmada"
  (RF-05, RF-06). En la base de datos actual el estado de una junta es `confirmada` o
  `cancelada`, y existe `juntas_fijas` para juntas recurrentes. Por eso la clase `Junta`
  tiene `cancelar()` y `reactivar()`.
- **Solicitudes con varios cupos.** La tabla `solicitudes` tiene `cupos` y la tabla
  `solicitud_tomas` registra a cada miembro que la toma. El diagrama de estados muestra
  solo el ciclo abierta, tomada y resuelta de la especificación. Cuándo pasa a "tomada"
  cuando hay más de un cupo no se puede ver en el esquema.
- **Roles.** La base acepta `coordinador`, `miembro` y `asesor`; la especificación solo
  describe Miembro y Coordinador.
