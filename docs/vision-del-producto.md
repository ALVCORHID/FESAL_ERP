# Visión del producto

**Autor:** Álvaro Correa Hidalgo  
**Fecha de la última versión:** 18 de agosto de 2026  


---

# 1. Descripción del sistema

**Nombre del sistema:**

Plataforma de Gestión Institucional (PGI)

**Descripción:**

La Plataforma de Gestión Institucional (PGI) es un sistema web que centraliza la información que actualmente se encuentra distribuida en diferentes archivos y fuentes de información de la universidad. El sistema permite que las distintas áreas de la institución consulten y administren la información que necesitan desde un mismo lugar, evitando duplicidades e inconsistencias. Cada área tendrá acceso únicamente a la información y acciones que le correspondan, mientras que los usuarios autorizados podrán administrar los permisos y supervisar el uso de la información.

---

# 2. Problema y usuarios

## El problema

Actualmente, diferentes áreas de la universidad pueden mantener información institucional utilizando archivos independientes, principalmente hojas de cálculo, además de otras fuentes de información. Esto puede provocar que existan varias versiones de un mismo dato, información duplicada, diferentes criterios para registrar información y dificultad para determinar cuál es la versión correcta o más actualizada.

Esta distribución también dificulta que las áreas compartan información de manera controlada. Un área puede necesitar consultar información administrada por otra, pero actualmente hacerlo puede requerir solicitar archivos, copiar información entre documentos o mantener bases independientes.

El problema principal es la falta de una fuente centralizada y controlada de información institucional que permita a cada área acceder a los datos que necesita sin otorgarle acceso innecesario a información de otras áreas.

## Cómo se resuelve hoy sin el sistema

Las áreas utilizan archivos independientes, principalmente hojas de cálculo, para almacenar y administrar información. Cuando necesitan información que pertenece a otra área, pueden solicitarla directamente, compartir archivos o copiar datos entre diferentes documentos.

Esto genera diferentes versiones de la información, duplicidad de registros, riesgo de errores al copiar datos y dificultad para mantener actualizada la información en todos los lugares donde se utiliza.

## Usuarios del sistema

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| **Personal de las áreas universitarias** | Consultar, registrar y actualizar la información que corresponde a sus funciones y generar consultas necesarias para su trabajo. | Acceder rápidamente a información correcta sin visualizar datos que no necesita para realizar sus funciones. |
| **Administradores del sistema** | Administrar usuarios, áreas, roles y permisos, además de supervisar el funcionamiento y acceso a la información. | Mantener la seguridad, integridad y disponibilidad de la información institucional. |
| **Dirección / responsables autorizados** | Consultar información consolidada de diferentes áreas para apoyar la supervisión y toma de decisiones. | Obtener información confiable y actualizada sin depender de múltiples archivos o solicitudes manuales. |

## Un conflicto entre usuarios

Los responsables de las áreas necesitan acceder a suficiente información para realizar sus actividades, mientras que los administradores deben limitar el acceso a los datos que cada usuario realmente necesita.

Por ejemplo, un área puede necesitar consultar información básica de un alumno, pero no debería poder consultar información personal o académica que no sea necesaria para sus funciones. Por lo tanto, el sistema debe permitir compartir información entre áreas sin otorgar acceso completo a la base de datos central.

---

# 3. Alcance

## Dentro del alcance

- Diseño de una base de datos central para almacenar información institucional.
- Definición de las principales entidades, relaciones y reglas de integridad de la información.
- Migración de información seleccionada proveniente de archivos existentes, principalmente hojas de cálculo.
- Limpieza y homologación de los datos que serán migrados.
- Desarrollo de una aplicación web para consultar y administrar la información.
- Implementación de autenticación de usuarios.
- Implementación de roles y permisos.
- Control de acceso de acuerdo con el área y las funciones de cada usuario.
- Restricción de acceso a información y operaciones según los permisos asignados.
- Registro de actividades relevantes realizadas dentro del sistema.
- Creación de una API para permitir la comunicación entre la aplicación y la base de datos central.
- Módulos iniciales necesarios para demostrar la operación centralizada de la información.
- Documentación de la estructura general del sistema y de los permisos implementados.

## Explícitamente fuera del alcance

- Sustitución inmediata de todos los sistemas institucionales existentes de la universidad.
- Integración con todos los sistemas externos utilizados actualmente por la universidad.
- Automatización de todos los procesos administrativos de la institución.
- Implementación de módulos financieros, contables, de nómina o recursos humanos.
- Desarrollo de una aplicación móvil nativa.
- Migración de absolutamente todos los archivos históricos de la universidad.
- Implementación de inteligencia artificial para toma automática de decisiones.
- Generación de reportes financieros o académicos especializados que requieran información de sistemas externos no integrados.
- Sustitución de los sistemas institucionales especializados que ya cumplen funciones específicas.

## Por qué queda fuera

Estas funcionalidades quedan fuera de la primera versión porque convertirían el proyecto en un ERP institucional completo y aumentarían considerablemente su complejidad, tiempo de desarrollo y riesgos de implementación.

El objetivo inicial es resolver primero el problema central: **crear una fuente única y controlada de información institucional y establecer una arquitectura que permita incorporar nuevos módulos posteriormente**.

---

# 4. Tipo de sistema y restricciones

## Tipo de sistema

**Sistema de información + Web y SaaS + Sistema de datos y análisis**

## Por qué es de ese tipo

Es un sistema de información porque permite almacenar, consultar, modificar y administrar información utilizada por diferentes áreas de una organización.

También es un sistema web porque los usuarios accederán a la plataforma mediante un navegador.

Finalmente, tiene características de un sistema de datos porque una de sus funciones principales es centralizar información proveniente de diferentes fuentes y proporcionar acceso controlado a esa información.

## Atributos de calidad que impone

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| **Seguridad** | El sistema almacenará información institucional y diferentes áreas tendrán distintos niveles de acceso. | Un usuario podría consultar o modificar información que no le corresponde. |
| **Integridad** | La información debe mantenerse consistente aunque diferentes áreas trabajen sobre los mismos datos. | Podrían generarse duplicados, datos contradictorios o pérdida de información. |
| **Disponibilidad** | Las áreas dependerán del sistema para consultar información necesaria para sus actividades. | Una caída del sistema podría impedir o retrasar actividades de diferentes áreas. |
| **Usabilidad** | Usuarios de diferentes áreas deberán utilizar el sistema sin requerir conocimientos técnicos especializados. | Los usuarios podrían continuar utilizando archivos externos o cometer errores al registrar información. |
| **Trazabilidad** | Es necesario conocer quién realizó cambios relevantes sobre la información. | Sería difícil identificar errores, cambios no autorizados o responsabilidades. |
| **Mantenibilidad** | El sistema debe poder incorporar nuevas áreas y módulos posteriormente. | Cada nueva funcionalidad podría requerir modificaciones complejas y aumentar el riesgo de errores. |

## Reglas de negocio que ya identifiqué

1. **Un usuario no puede acceder automáticamente a toda la información almacenada en la base de datos central.** Su acceso debe depender de los permisos asignados a su rol y área.

2. **Los permisos deben controlar tanto la información que un usuario puede consultar como las acciones que puede realizar.** Por ejemplo, un usuario podría tener permiso para consultar un registro, pero no para modificarlo.

3. **Un mismo dato institucional debe tener una fuente central de información.** Las áreas no deben crear copias independientes del mismo registro como mecanismo principal para trabajar con la información.

4. **Los usuarios solo deben visualizar los campos de información necesarios para sus funciones cuando existan restricciones específicas sobre determinados datos.**

5. **Las modificaciones relevantes a la información deben poder asociarse con el usuario que las realizó.**

6. **Los registros relacionados deben respetar las relaciones definidas en la base de datos.** Por ejemplo, no se debe poder asociar una entidad con un registro inexistente.

7. **La eliminación de información debe estar restringida de acuerdo con los permisos del usuario y las reglas de integridad del sistema.**

---

# 5. Ciclo de vida elegido

## Modelo elegido

**Desarrollo incremental con enfoque ágil.**

## Por qué le conviene a este proyecto

El proyecto tiene un alcance que puede crecer considerablemente debido a la cantidad de áreas, información y procesos que podrían incorporarse posteriormente.

Además, es probable que durante el desarrollo se descubran nuevas necesidades al analizar los procesos actuales de las diferentes áreas y los archivos que utilizan. Por esta razón, intentar definir desde el principio todas las funcionalidades de un ERP completo aumentaría el riesgo de construir funcionalidades que posteriormente no respondan a las necesidades reales de los usuarios.

El desarrollo incremental permite construir primero una versión funcional de la plataforma central y posteriormente incorporar nuevos módulos y áreas.

Una primera iteración puede concentrarse en:

1. Arquitectura de la base de datos.
2. Autenticación.
3. Usuarios, roles y permisos.
4. Migración de un conjunto controlado de información.
5. Consulta y modificación de información.
6. Auditoría de operaciones.

Después de validar esta primera versión con los usuarios, se pueden incorporar nuevas áreas, módulos e integraciones.

Este enfoque también permite detectar problemas de calidad de los datos y de permisos antes de migrar una cantidad mayor de información.

## Alternativas descartadas

### Alternativa 1: Modelo en cascada

**Por qué la descarté:**

El modelo en cascada requiere definir gran parte de los requisitos antes de comenzar las etapas posteriores del proyecto. En este caso, todavía existe incertidumbre sobre las necesidades específicas de cada área, la estructura de los archivos existentes y las reglas que deberán aplicarse a la información.

Además, los problemas de calidad de datos y las necesidades de los usuarios pueden descubrirse durante el desarrollo. Un modelo rígido dificultaría incorporar esos descubrimientos sin afectar etapas que ya fueron completadas.

### Alternativa 2: Prototipado como modelo principal

**Por qué la descarté:**

El prototipado puede ser útil para validar interfaces y entender las necesidades de los usuarios, pero por sí solo no proporciona una estrategia suficiente para desarrollar la arquitectura central de datos, los permisos, la seguridad, las migraciones y las integraciones que requiere el proyecto.

Puede utilizarse como técnica complementaria dentro de un desarrollo incremental, pero no como el ciclo de vida principal.

---

# Antes de entregar

- [x] La descripción del apartado 1 se entiende sin ser del área.
- [x] Hay al menos dos tipos de usuario con necesidades distintas.
- [x] Identifiqué un conflicto real entre usuarios.
- [x] El alcance dice qué queda fuera, no solo qué queda dentro.
- [x] Las exclusiones son específicas.
- [x] Identifiqué el tipo de sistema y varios atributos de calidad.
- [x] Anoté más de tres reglas de negocio no obvias.
- [x] Justifiqué el ciclo de vida contra dos alternativas descartadas.
- [ ] El documento está en mi repositorio y se puede leer desde el navegador.
- [x] Eliminé las instrucciones de la plantilla.
