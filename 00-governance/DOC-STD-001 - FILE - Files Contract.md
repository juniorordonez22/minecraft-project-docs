# Files Contract

**Estado**: *IN-REVIEW*

# Cuadro de Versiones

| **Versión** | **Fecha** | **Autor (es)** | **Descripción** |
|---|---|---|---|

---

# **1. Convención para nombrar documentos o archivos**

## **1.1. Formato general**

```
[FILE-ID] - [FILE-INFO] - TITLE - [DATE].[EXT]
```

Donde:

| Elemento    | Obligatorio | Descripción                                     |
| ----------- | ----------- | ----------------------------------------------- |
| `FILE-ID`   | No          | Identificador único o secuencial del documento o archivo, sigue el formato [PREFIX-XX]. |
| `FILE-INFO` | No          | Metadatos que permiten clasificar el documento o archivo. |
| `TITLE`     | Sí          | Nombre descriptivo del contenido.               |
| `DATE`      | No          | Fecha de creación o emisión del documento o archivo.      |
| `EXT`       | Sí          | Extensión del documento o archivo.                          |

Ejemplos:

```text
DOC-GD-01 - Guía de Instalación.md
DOC-GD-02 - Guía de Configuración - 2026-11-13.md

DOC-REQ-ED-01 - Documento de Elicitación.md
DOC-REQ-SRS-02 - MOD-SHOP - Especificación de Requisitos.md

DOC-ADR-01 - Patron Arquitectonico - Arquitectura por Capas.md
DOC-TEST-REP-01 - API - Reporte de Pruebas.md
```

No necesariamente todos los archivos deben tener fecha ni identificador. Son metadatos útiles en determinados documentos, pero innecesarios en archivos cuyo contenido evoluciona continuamente. Por ejemplo, un `README.md`, `CONTRIBUTING.md` o `ARCHITECTURE.md` no necesita una fecha en su nombre.

Es obligatorio incluir tener fecha e identificador a los documentos de `Meeting Minutes`.

## **1.2. FILE-ID: identificadores documentales**

En la siguiente tabla se muestra un catálogo de prefijos según la naturaleza del documento:

| Prefijo | Significado                  | Aplicación                                     |
| ------- | ---------------------------- | ---------------------------------------------- |
| `DOC-GD`    | Guide Documentation          | Guías de usuario, instalación y configuración. |
| `DOC-STD`    | Standards or Contracts Documentation          | Documentos que agrupan estándares y convenciones técnicas, tales como nomenclatura, formato, estructura de código, documentación y buenas prácticas. |
| `DOC-REQ-NFR`  | Non Functional Requirements Documentation                 | Documento de requerimientos no funcionales.       |
| `DOC-REQ-FR`  | Functional Requirements Documentation                 | Documento de requerimientos funcionales.       |
| `DOC-REQ-ED`  | Requirements Elicitation Documentation                 | Documento de Elicitación de requerimientos.       |
| `DOC-REQ-SRS`  | Software Requirements Specification Document                 | Documento de especificación de requerimientos.       |
| `DOC-UC`    | Use Cases Documentation                    | Documentación de Casos de uso y escenarios.                     |
| `DOC-US`    | User Stories Documentation                 | Documentación de Historias de usuario.                          |
| `DOC-ADR`   | Architecture Decision Record | Decisiones arquitectónicas y tecnológicas.     |
| `DOC-ARC`   | Architecture Documentation                 | Documentación de arquitectura.                 |
| `DOC-DES-UI`   | UI Design Documentation                       | Documentación de diseño interfaces o menús dentro del juego.     |
| `DOC-DES-COMP`   | Component Design Documentation                       | Documentación de diseño de componentes     |
| `DOC-DES-HIST`   | History Design Documentation                       | Diseño del lore o historia del evento.     |
| `DOC-DES-MUS`   | Music Design Documentation                       | Diseño o documentación de la música o audios del evento.     |
| `DOC-SEC`   | Security Documentation                     | Seguridad, amenazas y controles.               |
| `DOC-TEST-PLAN`  | Testing Plan Documentation                      | Planes de pruebas.           |
| `DOC-TEST-REP`  | Testing Report Documentation                      | Reportes de pruebas.           |
| `DOC-DEP`   | Deployment Documentation                   | Despliegue e instalación.                      |
| `DOC-OPS`   | Operations Documentation                   | Operación, monitoreo y administración.         |
| `DOC-MNT`   | Maintenance Documentation                  | Mantenimiento y gestión de incidencias.        |
| `DOC-REL`   | Release Documentation                      | Notas y documentación de versiones.            |
| `DOC-MIN`   | Meeting Minutes              | Actas de reuniones.                            |
| `DOC-REP`   | Reports                      | Informes técnicos y ejecutivos.                |

Si el archivo no es necesariamente un documento se pueden usar estos prefijos:

| Prefijo | Significado                  | Aplicación                                     |
| ------- | ---------------------------- | ---------------------------------------------- |
| `ASSET-UC`    | Use Case Diagram                    | Código fuente de diagramas de casos de uso.                     |
| `ASSET-MIN`   | Meeting Minutes Asset              | Recursos multimedia de reuniones.                            |
| `ASSET-DES-HIST`   | History Design Asset                       | Recurso multimedia asociado al diseño del lore o historia del evento.     |
| `ASSET-DES-MUS`   | Music Design Asset                       | Recurso multimedia asociado a la música o audios del evento.     |

**NOTA**: Los diagramas generados y el código fuente que dio origen a dicho diagrama no se pueden nombrar de la misma forma, de esta forma se distingue mejor el archivo de los elementos documentales (como Requisitos, Casos de Uso, Prueba, etc.). Ver [Element Identification Contract.md](DOC-STD-002%20-%20ELEMENT%20-%20Element%20Identification%20Contract.md). 

---

El identificador puede incorporar un número correlativo siguiendo el formato [PREFIX-XX]:

```
REQ-ED-01 - Requisitos Funcionales.md
REQ02 - Requisitos No Funcionales.md
REQ03 - Restricciones del Sistema.md

ADR01 - Arquitectura - Clean Architecture.md
ADR02 - Persistencia - Database per Tenant.md
ADR03 - Autenticación - JWT.md

TEST01 - Plan de Pruebas.md
TEST02 - Casos de Prueba - Animales.md
TEST03 - Reporte de Pruebas.md
```

**Reglas:**

- El identificador debe ser único dentro de su ámbito documental.
- No reutilizar identificadores eliminados.
- Mantener la numeración correlativa.
- No utilizar identificadores cuando no aporten trazabilidad.
- Si un documento tiene un identificador referenciado por otros documentos, evitar cambiarlo.

Una distinción importante: el identificador documental no debe confundirse con el identificador de un requisito, caso de uso o componente. Por ejemplo, `DOC-REQ-ED-01` identifica un documento, mientras que `RF-ANM-001` identifica un requisito.

## **1.3. FILE-INFO: metadatos del archivo**

A continuación se muestra un conjunto de categorías controladas:

| Categoría  | Prefijo  | Ejemplo           | Propósito                            |
| ---------- | -------- | ----------------- | ------------------------------------ |
| Módulo     | `MOD`    | `MOD-ANM`         | Identificar un módulo funcional.     |
| Componente | `CMP`    | `CMP-AUTH`        | Identificar un componente técnico.   |
| Capa       | `LYR`    | `LYR-APPLICATION` | Identificar una capa arquitectónica. |
| Tecnología | `TEC`    | `TEC-JAVA`      | Identificar una tecnología.          |
| Ambiente   | `ENV`    | `ENV-PROD`        | Identificar un entorno.              |
| Tipo       | `TYPE`   | `TYPE-GUIDE`      | Clasificar el contenido.             |
| Estado     | `STATUS` | `STATUS-DRAFT`    | Identificar el estado documental.    |
| Versión    | `VER`    | `VER-1.0`         | Identificar una versión documental.  |

Ejemplos:

```text
DOC-GD-01 - TYPE-GUIDE - Instalación del Backend.md

DOC-ARC-01 - LYR-APPLICATION - Arquitectura de Aplicación.md

DOC-STD-01 - TEC-JAVA - Contrato de Codificacion.md

DOC-DEP-01 - ENV-PROD - Procedimiento de Despliegue.md
```

---

No es recomendable incorporar todos los metadatos en el nombre. Se deben seleccionar únicamente aquellos que permitan distinguir archivos similares.

Por ejemplo, si un documento ya está dentro de `requirements/functional/shop/`, probablemente no necesitas repetir `MOD-SHOP` en su nombre.

### **1.4. Convenciones de escritura**

| Elemento              | Convención            | Ejemplo                       |
| --------------------- | --------------------- | ----------------------------- |
| Separador principal   | `-`                   | `DOC-REQ-NFR-01 - ...`                 |
| Separador de palabras | Espacio               | `Guía de Instalación`         |
| Primera palabra       | Mayúscula inicial     | `Diseño de Componentes`       |
| Siglas                | Mayúsculas            | `API`, `JWT`, `SQL`           |
| Identificadores       | Mayúsculas            | `MOD-SHOP`                     |
| Fechas                | `DD-MM-YYYY`          | `10-11-2026`                  |
| Extensiones           | Minúsculas            | `.md`, `.pdf`, `.png`         |
| Caracteres            | Preferentemente ASCII | Evitar caracteres especiales. |

Evitar:

- Espacios al inicio o al final.
- Nombres genéricos como `documento.md`, `final.md` o `nuevo.md`.
- Sufijos ambiguos como `final-final-v2.md`.
- Fechas en archivos que no representan una captura temporal.
- Guiones bajos como separadores de palabras.

# **2. Estructura de documentación durante el SDLC**

Para organizar la documentación, se utiliza una estructura basada en las etapas del ciclo de vida del software, complementada con carpetas transversales.

Se recomienda, en la mayoría de los casos, evitar organizar por módulos funcionales, ya que se podrían mezclar los requisitos, diseños, decisiones arquitectónicas, pruebas y manuales de operación.

## **2.1. Estructura de carpetas**

```text
docs/
│
├── 00-governance/
│   ├── README.md
│   ├── DOC-STD-001 - FILE - Files Contract.md
│   ├── DOC-STD-002 - ELEMENT - Element Identification Contract.md
│   ├── DOC-STD-003 - Document Control.md
│   ├── DOC-STD-004 - Project Charter.md
│   ├── DOC-STD-005 - Project Plan.md
│   ├── DOC-STD-006 - Stakeholders.md
│   └── DOC-STD-007 - Glossary.md
│
├── 01-discovery/
│   ├── stakeholder-analysis/
│   └── scope/
│
├── 02-requirements/
│   ├── functional/
│   │   ├── {module-name}/
│   │   └── ...
│   │
│   ├── non-functional/
│   ├── constraints/
│   ├── use-cases/
│   ├── user-stories/
│   └── traceability/
│
├── 03-architecture/
│   ├── architecture-overview/
│   ├── architecture-decisions/
│   ├── deployment-architecture/
│   ├── infrastructure-architecture/
│   ├── security-architecture/
│   └── integration-architecture/
│
├── 04-design/
│   ├── domain-model/
│   ├── ui-ux-design/
│   ├── component-design/
│   └── diagrams/
│
├── 05-implementation/
│   ├── backend/
│   ├── frontend/
│   ├── integrations/
│   ├── coding-standards/
│   └── technical-guides/
│
├── 06-testing/
│   ├── test-strategy/
│   ├── test-plans/
│   ├── test-cases/
│   ├── test-data/
│   ├── test-reports/
│   ├── performance/
│   └── security/
│
├── 07-deployment/
│   ├── environments/
│   ├── installation/
│   ├── configuration/
│   ├── ci-cd/
│   ├── migration/
│   └── release-procedures/
│
├── 08-operations/
│   ├── administration/
│   ├── monitoring/
│   ├── backup-recovery/
│   ├── troubleshooting/
│   └── runbooks/
│
├── 09-maintenance/
│   ├── incidents/
│   ├── problems/
│   ├── change-requests/
│   ├── technical-debt/
│   └── improvements/
│
├── 10-releases/
│   ├── v1.0.0/
│   ├── v1.1.0/
│   └── release-history.md
│
├── 11-meetings-reports/
│   ├── meeting-minutes/
│   ├── technical-reports/
│   └── progress-reports/
│
└── 99-archive/
    ├── deprecated/
    ├── superseded/
    └── obsolete/
```

## **2.2. Justificación de cada etapa**

| Etapa                 | Propósito                                                  | Documentos principales                                     |
| --------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| `00-governance`       | Establecer las reglas y administrar el proyecto.           | Contratos, planes, glosario y control documental.          |
| `01-discovery`        | Comprender el problema y justificar el proyecto.           | Viabilidad, contexto, alcance e interesados.               |
| `02-requirements`     | Especificar qué debe hacer el sistema y sus restricciones. | RF, RNF, casos de uso e historias de usuario.              |
| `03-architecture`     | Definir la estructura y las decisiones arquitectónicas.    | C4, ADR, arquitectura de despliegue e integración.         |
| `04-design`           | Detallar cómo se construirán los componentes.              | UML, modelos de dominio y UI/UX.                  |
| `05-implementation`   | Documentar criterios y aspectos técnicos del desarrollo.   | Estándares, guías técnicas y configuración de componentes. |
| `06-testing`          | Verificar y validar el producto.                           | Estrategia, planes, casos y resultados de pruebas.         |
| `07-deployment`       | Preparar y ejecutar la entrega del software.               | Instalación, CI/CD, ambientes y migraciones.               |
| `08-operations`       | Mantener el sistema funcionando.                           | Monitoreo, respaldos, administración y runbooks.           |
| `09-maintenance`      | Gestionar cambios y evolución del software.                | Incidencias, problemas, deuda técnica y mejoras.           |
| `10-releases`         | Registrar las entregas del producto.                       | Changelogs, notas de versión y entregables.                |
| `11-meetings-reports` | Conservar el historial de gestión y comunicación.          | Actas e informes.                                          |
| `99-archive`          | Preservar documentación que ya no está vigente.            | Documentos obsoletos y reemplazados.                       |

## **2.3. Organización interna por módulo**

En las etapas donde existe documentación específica de cada módulo, se recomienda utilizar una estructura homogénea.

Por ejemplo:

```text
02-requirements/
└── functional/
    ├── shop/
    │   ├── README.md
    │   ├── DOC-REQ-FR-01 - Requisitos Funcionales.md
    │   ├── DOC-UC-01 - Vender Articulo.md
    │   ├── DOC-UC-02 - Comprar Articulo.md
    │   └── DOC-US-01 - Poner a la Venta un Articulo.md
    │
    ├── arena/
    │   ├── README.md
    │   ├── DOC-REQ-FR-01 - Requisitos Funcionales.md
    │   └── DOC-UC-01 - Sorteo de Mob en PVE.md
    │
    └── ...
```

De esta manera, cada módulo puede mantener su propia documentación sin perder la organización general del proyecto.

**Considerar:** No es necesario replicar todos los módulos en todas las etapas. Solo se crea una carpeta cuando exista documentación que realmente lo justifique.

# **3. Contrato de estructura documental**

Además de los nombres, se establecen las siguientes reglas para el contenido y mantenimiento de los documentos.

## **3.1. Estructura estándar de un documento**

Los documentos técnicos importantes deberán seguir esta estructura:

```
# Título del documento

**Estado**: *APPROVED*

## Cuadro de versiones

| Versión | Fecha | Autor(es) | Descripción |
|---|---|---|---|
| 1.0 | DD-MM-YYYY | Autor | Versión inicial |

## 1. Introducción

### 1.1. Propósito
### 1.2. Alcance
### 1.3. Audiencia

## 2. Contenido principal

...

## 3. Referencias

...

## 4. Anexos

...
```

No todos los documentos necesitan todas las secciones. Por ejemplo, un `ADR` tendrá una estructura distinta, mientras que un caso de uso tendrá actores, precondiciones, flujo principal y excepciones.

TODO: Crear plantillas específicas por tipo documental, en lugar de tener una única plantilla universal.

## **3.2. Control de versiones documentales**

Diferencia tres conceptos:

| Concepto              | Ejemplo             | Uso                                    |
| --------------------- | ------------------- | -------------------------------------- |
| Versión documental    | `1.0`, `1.1`, `2.0` | Cambios en el contenido del documento. |
| Versión del software  | `v1.0.0`            | Entregas del producto.                 |
| Fecha de modificación | `13-11-2026`        | Registro temporal del cambio.          |

Para documentos se debe aplicar la siguiente convención de versionado:

- `1.0`: primera versión aprobada.
- `1.1`: modificación menor o ampliación.
- `2.0`: cambio importante de estructura, alcance o contenido.

Los borradores pueden identificarse mediante el estado `DRAFT`, sin necesidad de asignarles una versión formal.

## **3.3. Estados documentales**

Se debe utilizar el siguiente catálogo controlado:

| Estado       | Significado                        |
| ------------ | ---------------------------------- |
| `DRAFT`      | En elaboración.                    |
| `IN-REVIEW`  | En revisión.                       |
| `APPROVED`   | Aprobado.                          |
| `DEPRECATED` | En desuso, pero conservado.        |
| `SUPERSEDED` | Reemplazado por otro documento.    |
| `OBSOLETE`   | Obsoleto y sin aplicación vigente. |

Estos estados deben registrarse justo después del título dentro del documento. El documento debe tener un Cuadro de Versiones vacío, únicamente si el estado es `DRAFT` o `IN-REVIEW`, debido a que todavía no son aprobados. 

## **3.4. Trazabilidad documental**

Un documento puede estar relacionado con otros artefactos del proyecto.

Por ejemplo:

```text
DOC-REQ-FR-01 - Requisitos Funcionales.md
    │
    ├── RF-ANM-001
    │     └── UC-ANM-001
    │           └── DES-ANM-001
    │                 └── TEST-ANM-001
    │
    └── RF-ANM-002
          └── UC-ANM-002
                └── TEST-ANM-002
```

Se deben mantener identificadores internos para los elementos dentro del documento y reservar los nombres de archivo para documentos completos. Ver [Element Identification Contract.md](DOC-STD-002%20-%20ELEMENT%20-%20Element%20Identification%20Contract.md).

# **4. Archivos especiales y documentación generada**

Hay algunos tipos de archivos que siguen reglas independientes.

| Archivo              | Ubicación                  | Consideración                                    |
| -------------------- | -------------------------- | ------------------------------------------------ |
| `README.md`          | Raíz y carpetas relevantes | Explicar el propósito y contenido de la carpeta. |
| `CHANGELOG.md`       | Raíz o releases            | Historial de cambios del producto.               |
| `CONTRIBUTING.md`    | Raíz                       | Reglas de contribución.                          |
| `LICENSE`            | Raíz                       | Licenciamiento.                                  |
| `CODE_OF_CONDUCT.md` | Raíz                       | Normas de colaboración.                          |
| `SECURITY.md`        | Raíz                       | Política de seguridad.                           |
| `ADR/`               | Arquitectura               | Registro de decisiones arquitectónicas.          |
| `templates/`         | Gobernanza                 | Plantillas documentales.                         |

Si es necesario, también se puede separar los documentos escritos manualmente de los artefactos generados automáticamente.

Por ejemplo:

```
docs/
├── 03-architecture/
│   └── diagrams/
│       ├── source/
│       │   ├── system-context.puml
│       │   └── container-diagram.puml
│       │
│       └── generated/
│           ├── system-context.png
│           └── container-diagram.png
```

Así puedes conservar los archivos fuente de PlantUML y regenerar sus imágenes cuando sea necesario.

No recomiendo almacenar resultados temporales de compilación, logs, archivos intermedios o resultados de pruebas en la documentación versionada, salvo que exista una razón concreta.

# **5. Recomendaciones para implementar el contrato**

Hay algunas decisiones que tomaría para que este estándar no termine convirtiéndose en una carga administrativa.

| Decisión             | Recomendación                  | Justificación                                   |
| -------------------- | ------------------------------ | ----------------------------------------------- |
| Identificadores      | Opcionales                     | Evitar numeraciones innecesarias.               |
| Metadatos            | Controlados                    | Evitar múltiples formas de clasificar lo mismo. |
| Fechas               | ISO 8601                       | Facilitar ordenamiento cronológico.             |
| Versionamiento       | SemVer documental simplificado | Identificar cambios relevantes.                 |
| Carpetas             | Por etapa del SDLC             | Facilitar la navegación.                        |
| Módulos              | Subcarpetas cuando corresponda | Evitar duplicaciones.                           |
| Plantillas           | Por tipo documental            | Homogeneizar la documentación.                  |
| Documentos obsoletos | Archivo histórico              | Preservar trazabilidad.                         |
| Diagramas            | Fuente y resultado separados   | Facilitar mantenimiento.                        |

## **5.1. Plantillas que deberías mantener**

Te recomiendo crear una carpeta específica para las plantillas:

```
00-governance/
└── templates/
    ├── TEMPLATE-DOCUMENT.md
    ├── TEMPLATE-ADR.md
    ├── TEMPLATE-REQUIREMENTS.md
    ├── TEMPLATE-USE-CASE.md
    ├── TEMPLATE-USER-STORY.md
    ├── TEMPLATE-TEST-CASE.md
    ├── TEMPLATE-TEST-REPORT.md
    ├── TEMPLATE-MEETING-MINUTES.md
    └── TEMPLATE-TECHNICAL-REPORT.md
```

Estas plantillas deberían contener únicamente la estructura correspondiente a cada tipo documental.

## **5.2. Mi recomendación final**

Para tu proyecto, adoptaría estas tres reglas fundamentales:

1. **Clasificación primaria mediante carpetas:** el directorio determina la etapa del SDLC y el ámbito del documento.
2. **Clasificación secundaria mediante FILE-ID y FILE-INFO:** el nombre permite identificar rápidamente el tipo y propósito del archivo.
3. **Trazabilidad mediante identificadores internos:** requisitos, decisiones, casos de uso y pruebas deben tener identificadores estables, independientes de los nombres de archivo.

Finalmente, evitaría convertir el sistema de nombres en una taxonomía excesivamente compleja. El objetivo es encontrar y relacionar documentación fácilmente, no codificar toda la información del documento dentro de su nombre.

Mi sugerencia es comenzar con las carpetas `00` a `07`, que cubren la mayor parte del desarrollo inicial, e incorporar operaciones, mantenimiento y releases conforme el producto evolucione.

Una siguiente mejora útil sería establecer plantillas Markdown y validaciones automáticas para comprobar que los documentos respeten este contrato en cada Pull Request.