# Element Identification Contract

**Estado**: *IN-REVIEW*

# Cuadro de Versiones

| **Versión** | **Fecha** | **Autor (es)** | **Descripción** |
|---|---|---|---|

---

## 1. Propósito

Establecer convenciones para identificar de manera única y consistente los elementos generados, utilizados o gestionados durante el ciclo de vida del desarrollo de software (SDLC).

Este contrato define los prefijos, ámbitos y formatos de identificación para requisitos, casos de uso, historias de usuario, componentes, interfaces, pruebas, entidades y otros elementos del proyecto.

## 2. Formato general

Se establece el siguiente formato:

```text
ELEMENT-TYPE - SCOPE - SEQUENCE
```

Representación compacta:

```text
TYPE-SCOPE-NNN
```

Donde:

| Elemento | Descripción | Ejemplo |
|---|---|---|
| `TYPE` | Tipo de elemento que se identifica. | `RF`, `RNF`, `UC`, `CMP` |
| `SCOPE` | Módulo, dominio o ámbito al que pertenece. | `SHOP`, `ARENA`, `BET` |
| `NNN` | Número correlativo de tres o más dígitos. | `001`, `002` |

Ejemplos:

```text
RF-ANM-001
UC-ANM-001
CMP-AUTH-001
TEST-ANM-001
```

### 2.1. Reglas generales

- Cada identificador debe ser único dentro de su ámbito.
- Los identificadores deben ser estables durante todo el ciclo de vida del elemento.
- No se deben reutilizar identificadores eliminados o dados de baja.
- La numeración debe ser correlativa y no depender del orden de aparición en los documentos.
- Los identificadores deben utilizar letras mayúsculas y números.
- Los elementos relacionados deben conservar sus identificadores al cambiar de documento.
- Cuando un elemento cambia de módulo o ámbito, debe evaluarse la necesidad de actualizar su identificador y sus referencias.
- No se deben utilizar identificadores de documentos (`DOC-*` o `ASSET-*`) para identificar elementos internos.

## 3. Catálogo de identificadores

TODO: Incluir Prefijos para identificar los diagramas.

### 3.1. Gobernanza y gestión del proyecto

| Prefijo | Significado | Aplicación |
|---|---|---|
| `PRJ` | Project | Proyecto o producto de software. |
| `OBJ` | Objective | Objetivos del proyecto o del producto. |
| `SCP` | Scope | Elementos del alcance del proyecto. |
| `STK` | Stakeholder | Interesados y participantes. |
| `ROL` | Role | Roles y responsabilidades. |
| `RSK` | Risk | Riesgos identificados. |
| `ISS` | Issue | Problemas o asuntos pendientes. |
| `ACT` | Action Item | Acciones derivadas de reuniones o revisiones. |
| `DEC` | Decision | Decisiones de gestión o del proyecto. |
| `MILE` | Milestone | Hitos del proyecto. |
| `DEL` | Deliverable | Entregables del proyecto. |

### 3.2. Requisitos y análisis

| Prefijo | Significado | Aplicación |
|---|---|---|
| `RF` | Functional Requirement | Requisitos funcionales. |
| `RNF` | Non-Functional Requirement | Requisitos no funcionales. |
| `CON` | Constraint | Restricciones técnicas, operativas o de negocio. |
| `UC` | Use Case | Casos de uso. |
| `ACTR` | Actor | Actores humanos o sistemas externos. |
| `US` | User Story | Historias de usuario. |
| `AC` | Acceptance Criteria | Criterios de aceptación. |
| `EPIC` | Epic | Épicas de desarrollo. |
| `FEAT` | Feature | Funcionalidades o capacidades del producto. |
| `SCN` | Scenario | Escenarios de comportamiento. |
| `GLO` | Glossary Term | Términos del dominio y glosario. |

### 3.3. Arquitectura y diseño

| Prefijo | Significado | Aplicación |
|---|---|---|
| `ADR` | Architecture Decision | Decisiones arquitectónicas. |
| `ARC` | Architecture Element | Elementos arquitectónicos. |
| `SYS` | System | Sistemas o subsistemas. |
| `SUB` | Subsystem | Subsistemas. |
| `CMP` | Component | Componentes de software. |
| `MOD` | Module | Módulos funcionales o técnicos. |
| `SVC` | Service | Servicios de aplicación o infraestructura. |
| `INT` | Interface | Interfaces de software. |
| `CLS` | Class | Clases de implementación o diseño. |
| `IFC` | Interface Contract | Contratos de interfaces. |
| `SEQ` | Sequence | Interacciones o secuencias. |
| `FLOW` | Flow | Flujos de procesos o funcionalidades. |
| `DSN` | Design Element | Elementos generales de diseño. |

### 3.4. Integraciones

| Prefijo | Significado | Aplicación |
|---|---|---|
| `DTO` | Data Transfer Object | Objetos de transferencia de datos. |
| `REQ` | Request | Modelos de entrada de solicitudes. |
| `RES` | Response | Modelos de respuesta. |
| `CMD` | Command | Comandos de aplicación. |
| `Q` | Query | Consultas de aplicación. |
| `MSG` | Message | Mensajes intercambiados entre componentes. |
| `EVT` | Event | Eventos de integración. |
| `CONN` | Connector | Conectores con sistemas externos. |
| `WEBH` | Webhook | Integraciones mediante webhooks. |

### 3.5. Interfaz de usuario y experiencia de usuario

| Prefijo | Significado | Aplicación |
|---|---|---|
| `SCR` | Screen | Pantallas de la aplicación. |
| `PG` | Page | Páginas o vistas. |
| `UI` | User Interface | Elementos de interfaz. |
| `CMP` | Component | Componentes visuales. |
| `NAV` | Navigation | Elementos de navegación. |
| `FORM` | Form | Formularios. |
| `FLD` | Field | Campos de formularios. |
| `ACTN` | Action | Acciones disponibles en la interfaz. |
| `DLOG` | Dialog | Diálogos y ventanas modales. |
| `UX` | User Experience | Elementos y decisiones de experiencia de usuario. |

### 3.6. Seguridad

| Prefijo | Significado | Aplicación |
|---|---|---|
| `THR` | Threat | Amenazas identificadas. |
| `VUL` | Vulnerability | Vulnerabilidades. |
| `CTRL` | Security Control | Controles de seguridad. |
| `POL` | Policy | Políticas de seguridad. |
| `PERM` | Permission | Permisos. |
| `ROLE` | Security Role | Roles de autorización. |
| `CLM` | Claim | Claims de identidad. |
| `AUTH` | Authentication | Mecanismos de autenticación. |
| `AUD` | Audit | Elementos de auditoría. |

### 3.7. Pruebas y calidad

| Prefijo | Significado | Aplicación |
|---|---|---|
| `TC` | Test Case | Casos de prueba individuales. |
| `TS` | Test Suite | Conjuntos de pruebas. |
| `TP` | Test Plan | Planes de pruebas. |
| `TR` | Test Run | Ejecuciones de pruebas. |
| `BUG` | Bug | Defectos identificados. |
| `DEF` | Defect | Defectos de software. |
| `QA` | Quality Attribute | Atributos de calidad. |
| `MET` | Metric | Métricas de calidad. |
| `CRIT` | Quality Criterion | Criterios de calidad. |

### 3.8. Infraestructura y despliegue

| Prefijo | Significado | Aplicación |
|---|---|---|
| `ENV` | Environment | Ambientes de desarrollo, pruebas y producción. |
| `SRV` | Server | Servidores. |
| `HOST` | Host | Equipos o nodos de infraestructura. |
| `NET` | Network | Elementos de red. |
| `CTR` | Container | Contenedores. |
| `IMG` | Image | Imágenes de contenedores. |
| `PIPE` | Pipeline | Pipelines de CI/CD. |
| `JOB` | Job | Trabajos automatizados. |
| `CFG` | Configuration | Configuraciones identificadas. |
| `DEP` | Deployment | Despliegues. |
| `PKG` | Package | Paquetes, artefactos o mods de Minecraft distribuibles. |

### 3.9. Operaciones y mantenimiento

| Prefijo | Significado | Aplicación |
|---|---|---|
| `ALRT` | Alert | Alertas del sistema. |
| `MON` | Monitoring | Elementos de monitoreo. |
| `LOG` | Log | Fuentes o categorías de registros. |
| `BK` | Backup | Respaldos. |
| `REC` | Recovery | Procedimientos de recuperación. |
| `INC` | Incident | Incidentes operativos. |
| `PRB` | Problem | Problemas recurrentes. |
| `CHG` | Change | Solicitudes de cambio. |
| `RFC` | Request for Change | Solicitudes formales de modificación. |
| `TASK` | Task | Tareas de mantenimiento. |
| `TD` | Technical Debt | Deuda técnica. |

### 3.10. Versiones y entregas

| Prefijo | Significado | Aplicación |
|---|---|---|
| `REL` | Release | Entregas del producto. |
| `VER` | Version | Versiones identificadas de componentes o artefactos. |
| `BLD` | Build | Compilaciones. |
| `TAG` | Tag | Etiquetas de control de versiones. |
| `PATCH` | Patch | Correcciones distribuidas. |



## 4. Catálogo de ámbitos (`SCOPE`)

El ámbito permite identificar el módulo, dominio o componente al que pertenece el elemento.

Por ejemplo:

TODO: Identificar todos los módulos existentes actualmente y modificar el cuadro.

| Código | Significado |
|---|---|
| `SYS` | Sistema completo. |
| `AUTH` | Autenticación y autorización. |
| `SHOP` | Tienda del evento. |
| `USR` | Gestión de usuarios. |
| `ARENA` | Arena de combate PVE y PVP. |
| `BET` | Apuestas en evento. |
| `INFRA` | Infraestructura. |
| `UI` | Interfaz de usuario. |
| `SEC` | Seguridad transversal. |

Los códigos deben registrarse en un catálogo centralizado y no deben crearse variantes para representar el mismo ámbito.

## 5. Ejemplos de identificación

### 5.1. Requisitos y análisis

```text
RF-AUTH-001
RNF-SEC-001
BR-SHOP-001
UC-SHOP-001
ACTR-ARENA-001
US-BET-001
AC-BET-001
EPIC-BET-001
FEAT-SHOP-001
```

### 5.2. Arquitectura y diseño

```text
ADR-SYS-001
ARC-SYS-001
CMP-AUTH-001
MOD-USR-001
SVC-USR-001
CLS-USR-001
ENT-USR-001
VO-USR-001
AGG-USR-001
```

### 5.3. Datos e integraciones

```text
DTO-ARENA-001
CMD-ARENA-001
CONN-SYS-001
```

### 5.4. Pruebas y calidad

```text
TP-BET-001
TC-BET-001
TS-BET-001
TR-BET-001
BUG-BET-001
MET-SYS-001
```

### 5.5. Infraestructura y operación

```text
ENV-SYS-001
SRV-INFRA-001
PIPE-SYS-001
DEP-SYS-001
ALRT-INFRA-001
MON-INFRA-001
INC-SYS-001
CHG-SYS-001
```

## 6. Relaciones y trazabilidad

Los identificadores deben permitir establecer relaciones entre elementos de distintas etapas del SDLC.

Ejemplo:

```text
RF-BET-001
    │
    ├── UC-BET-001
    │      ├── US-BET-001
    │      └── AC-BET-001
    │
    ├── ENT-BET-001
    │
    ├── DTO-BET-001
    │
    └── TC-BET-001
           └── TR-BET-001
```

Las relaciones no necesariamente serán jerárquicas ni de uno a uno. Un requisito puede relacionarse con varios casos de uso, componentes y pruebas.

Se recomienda mantener una matriz de trazabilidad que permita identificar las relaciones entre requisitos, diseño, implementación y pruebas.

## 7. Gestión de identificadores

1. Los identificadores deben asignarse al crear formalmente un elemento.
2. Los elementos eliminados deben conservar su registro histórico.
3. Los cambios de nombre no deben implicar cambios de identificador.
4. Las relaciones entre elementos deben actualizarse cuando se modifiquen sus dependencias.
5. Los nuevos prefijos y ámbitos deben incorporarse al presente contrato antes de utilizarlos de manera generalizada.
6. Se deben evitar prefijos ambiguos o duplicados.
7. Los identificadores deben ser independientes de la ubicación física del archivo o del repositorio.

## 8. Consideraciones finales

Este catálogo constituye una base extensible. No todos los prefijos deben utilizarse obligatoriamente en cada proyecto.

Cada proyecto debe adoptar únicamente aquellos que correspondan a sus necesidades, manteniendo la consistencia de los identificadores y su trazabilidad durante el SDLC.