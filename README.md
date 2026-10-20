# # SILAB — Sistema Integral de Gestión de Laboratorio Clínico

![Estado](https://img.shields.io/badge/estado-en%20dise%C3%B1o-blue)
![Plataformas](https://img.shields.io/badge/plataformas-Android%20%7C%20iOS%20%7C%20Web-informational)
![Arquitectura](https://img.shields.io/badge/arquitectura-monolito%20modular-purple)
![Seguridad](https://img.shields.io/badge/seguridad-privacidad%20por%20dise%C3%B1o-critical)
![Licencia](https://img.shields.io/badge/licencia-por%20definir-lightgrey)

**SILAB** es una propuesta de plataforma integral para administrar las operaciones clínicas, administrativas, financieras, logísticas y tecnológicas de uno o varios laboratorios clínicos desde un ecosistema centralizado.

La solución contempla aplicaciones móviles para Android y iOS, un portal web administrativo, un portal para pacientes y médicos remitentes, y una capa de integración con el sistema de carnets con chip que el laboratorio ya utiliza. También propone mecanismos de autenticación y control de acceso con tarjetas, reconocimiento facial y huella dactilar, sujetos a la compatibilidad del hardware, la evaluación de riesgos y la normativa aplicable.

> **Estado del proyecto:** documento de visión y diseño. Las tecnologías, integraciones, metas de rendimiento y funcionalidades descritas son propuestas por validar; no implican que ya estén implementadas.

---

## Tabla de contenidos

- [1. Visión general](#1-visión-general)
- [2. Objetivos](#2-objetivos)
- [3. Aplicaciones y portales](#3-aplicaciones-y-portales)
- [4. Módulos funcionales](#4-módulos-funcionales)
- [5. Control de acceso, biometría y carnets con chip](#5-control-de-acceso-biometría-y-carnets-con-chip)
- [6. Roles y matriz de permisos](#6-roles-y-matriz-de-permisos)
- [7. Flujos principales de negocio](#7-flujos-principales-de-negocio)
- [8. Arquitectura propuesta](#8-arquitectura-propuesta)
- [9. Tecnologías sugeridas](#9-tecnologías-sugeridas)
- [10. Modelo de datos conceptual](#10-modelo-de-datos-conceptual)
- [11. Integraciones](#11-integraciones)
- [12. Seguridad, privacidad y cumplimiento](#12-seguridad-privacidad-y-cumplimiento)
- [13. Requisitos no funcionales](#13-requisitos-no-funcionales)
- [14. Continuidad operativa y copias de seguridad](#14-continuidad-operativa-y-copias-de-seguridad)
- [15. Pruebas y aseguramiento de calidad](#15-pruebas-y-aseguramiento-de-calidad)
- [16. Despliegue y DevSecOps](#16-despliegue-y-devsecops)
- [17. Observabilidad y soporte](#17-observabilidad-y-soporte)
- [18. Estructura sugerida del repositorio](#18-estructura-sugerida-del-repositorio)
- [19. API de referencia](#19-api-de-referencia)
- [20. Plan de implementación por fases](#20-plan-de-implementación-por-fases)
- [21. Roadmap](#21-roadmap)
- [22. Instalación y ejecución](#22-instalación-y-ejecución)
- [23. Decisiones pendientes](#23-decisiones-pendientes)
- [24. Glosario](#24-glosario)
- [25. Licencia](#25-licencia)

---

## 1. Visión general

SILAB busca unificar la operación de un laboratorio clínico, desde la admisión del paciente hasta la entrega de resultados, el cobro y la auditoría de cada proceso.

La plataforma se plantea para una o varias sedes y debe facilitar la trazabilidad de las muestras, la gestión del personal, el control de calidad, el mantenimiento de equipos y la administración del inventario.

### Principios del proyecto

- **Fuente centralizada de información:** todos los módulos autorizados trabajan con datos coherentes.
- **Trazabilidad de extremo a extremo:** cada acción importante deja un registro auditable.
- **Seguridad y privacidad desde el diseño:** acceso mínimo necesario y protección reforzada de datos sensibles.
- **Interoperabilidad:** integración gradual con equipos, carnets y sistemas existentes.
- **Arquitectura mantenible:** comenzar con un monolito modular y separar servicios solo cuando exista una razón técnica.
- **Accesibilidad y alternativas:** ningún proceso esencial debe depender exclusivamente de una característica biométrica.
- **Implementación incremental:** validar primero los procesos esenciales y ampliar el alcance por fases.

## 2. Objetivos

### Objetivo general

Diseñar una plataforma segura, escalable y mantenible que centralice la gestión clínica, operativa, administrativa y tecnológica de un laboratorio clínico.

### Objetivos específicos

- Administrar pacientes, órdenes médicas, citas, muestras y resultados.
- Controlar el ciclo de vida y la ubicación de cada muestra.
- Gestionar inventario, reactivos, proveedores, compras y vencimientos.
- Administrar usuarios, roles, sedes, horarios y permisos.
- Integrar los carnets con chip existentes sin reemplazarlos innecesariamente.
- Ofrecer aplicaciones móviles y portales web con experiencias adaptadas a cada usuario.
- Automatizar notificaciones, alertas, reportes y tareas repetitivas.
- Mantener auditoría de las operaciones críticas.
- Facilitar el control de calidad y el mantenimiento de equipos.
- Proteger la confidencialidad, integridad y disponibilidad de la información.

## 3. Aplicaciones y portales

| Componente | Usuarios principales | Funciones |
|---|---|---|
| Aplicación Android | Pacientes y, según permisos, personal autorizado | Citas, órdenes, resultados, notificaciones y funciones móviles asignadas |
| Aplicación iOS | Pacientes y personal autorizado | Funciones equivalentes según perfil y alcance definido |
| Portal web administrativo | Administradores y coordinadores | Gestión global, configuración, reportes y supervisión |
| Portal del personal clínico | Recepción, bacteriología, laboratorio y calidad | Órdenes, muestras, procesamiento, validación y trazabilidad |
| Portal del paciente | Pacientes y acudientes autorizados | Citas, instrucciones, pagos y consulta segura de resultados |
| Portal médico remitente | Profesionales externos autorizados | Órdenes, resultados de pacientes remitidos y seguimiento permitido |
| Panel de gerencia | Dirección y responsables autorizados | Indicadores operativos, financieros, calidad y desempeño |
| Consola de acceso físico | Seguridad y administradores autorizados | Carnets, lectores, zonas, permisos y eventos de acceso |
| Backend y API | Todas las aplicaciones | Reglas de negocio, autenticación, datos e integraciones |

**Nota:** se puede desarrollar una aplicación multiplataforma con Flutter o React Native para Android e iOS. Si se requieren capacidades específicas de iOS, se pueden añadir componentes nativos en Swift. No es obligatorio mantener dos aplicaciones completamente independientes desde el inicio.

## 4. Módulos funcionales

### 4.1. Pacientes y admisiones

- Registro y actualización de datos demográficos y de contacto.
- Identificación segura y detección de posibles registros duplicados.
- Gestión de documentos, consentimientos y autorizaciones.
- Historial de órdenes, citas, pagos y resultados según permisos.
- Registro de acudientes o representantes autorizados.
- Atención preferencial y ajustes de accesibilidad cuando correspondan.
- Registro de llegada, turnos y tiempos de espera.
- Verificación de identidad con alternativas al reconocimiento biométrico.

### 4.2. Citas y órdenes médicas

- Agenda por sede, servicio, profesional y disponibilidad.
- Creación de órdenes internas y recepción de órdenes externas.
- Catálogo de exámenes y perfiles de pruebas.
- Instrucciones de preparación específicas por examen.
- Recordatorios y confirmación, cancelación o reprogramación.
- Gestión de órdenes urgentes y prioridades autorizadas.
- Control de requisitos previos y autorizaciones.
- Registro de origen, estado y responsable de cada orden.

### 4.3. Toma y trazabilidad de muestras

- Identificador único por muestra y recipiente.
- Etiquetas con código de barras o QR.
- Registro de toma, recepción, traslado, procesamiento y almacenamiento.
- Asociación inequívoca entre paciente, orden, muestra y examen.
- Registro de fecha, hora, usuario, sede y ubicación.
- Control de condiciones de transporte y cadena de frío cuando aplique.
- Reglas para rechazar muestras insuficientes, deterioradas o mal identificadas.
- Gestión de muestras pendientes, extraviadas, repetidas o rechazadas.
- Cadena de custodia y registro de transferencias.
- Alertas por demoras y tiempos máximos de procesamiento.

### 4.4. Procesamiento y resultados

- Bandejas de trabajo por sección y prioridad.
- Registro de resultados manuales con controles de validación.
- Importación de resultados desde analizadores compatibles.
- Validación técnica y autorización por personal habilitado.
- Gestión de resultados pendientes, repetición de pruebas y correcciones.
- Manejo de valores críticos según protocolos aprobados por el laboratorio.
- Registro de la persona que revisó, validó y liberó cada resultado.
- Historial de versiones de informes y motivo de cada modificación.
- Generación de informes en PDF y consulta segura en el portal.
- Firma electrónica o digital según el mecanismo adoptado y su validez aplicable.
- Notificación de resultados disponibles sin exponer datos clínicos en mensajes.
- Bloqueo de entrega de resultados no validados, salvo procedimientos autorizados y documentados.

### 4.5. Portal de pacientes

- Consulta de citas, órdenes y estado de las muestras.
- Acceso a resultados liberados y sus documentos.
- Descarga de informes con verificación de autenticidad.
- Instrucciones previas a los exámenes.
- Historial de pagos y facturas disponibles.
- Notificaciones de cambios de estado.
- Solicitud de citas y toma de muestras a domicilio, si el servicio existe.
- Administración de consentimientos y preferencias de notificación.
- Acceso delegado para acudientes, con autorización y límites definidos.
- Recuperación segura de cuenta y alternativas para personas sin teléfono inteligente.

### 4.6. Inventario, reactivos y compras

- Catálogo de reactivos, insumos, materiales y equipos.
- Lotes, fechas de vencimiento, seriales y ubicación.
- Entradas, salidas, traslados y ajustes de inventario.
- Existencias mínimas y alertas de reposición.
- Trazabilidad del lote utilizado en cada proceso cuando corresponda.
- Registro de condiciones de almacenamiento y temperatura.
- Proveedores, cotizaciones, órdenes de compra y recepción.
- Control de insumos vencidos, retirados o en cuarentena.
- Conteos físicos y conciliación de diferencias.
- Estimación de consumo y demanda basada en históricos.

### 4.7. Equipos y mantenimiento

- Inventario de equipos, fabricante, modelo, serial y ubicación.
- Hojas de vida y documentación técnica.
- Calendario de mantenimiento preventivo y correctivo.
- Registro de calibraciones, verificaciones y controles de calidad.
- Gestión de fallas, indisponibilidad y órdenes de servicio.
- Alertas de próximas revisiones y vencimientos.
- Historial de intervenciones, repuestos y responsables.
- Bloqueo o advertencia de uso cuando el equipo no cumpla los requisitos definidos.

### 4.8. Facturación y finanzas

- Tarifarios por servicio, sede, convenio o tipo de cliente.
- Caja, recibos, pagos, devoluciones y cierres diarios.
- Facturación de servicios y seguimiento de cuentas por cobrar.
- Gestión de convenios con empresas y entidades contratantes.
- Conciliación de pagos y registro de ajustes autorizados.
- Reportes de ingresos, costos y rentabilidad por servicio.
- Integración con proveedores de pago y facturación electrónica, si aplica.
- Separación de funciones entre quien factura, autoriza devoluciones y concilia.
- Historial auditable de cambios financieros.

### 4.9. Recursos humanos y asistencia

- Registro del personal y vinculación con roles del sistema.
- Asignación de sedes, turnos, horarios y áreas autorizadas.
- Registro de asistencia mediante carnet y mecanismos aprobados.
- Gestión de permisos, novedades y ausencias según el alcance autorizado.
- Capacitación, habilitaciones y vencimiento de certificaciones requeridas.
- Desactivación coordinada de cuentas, carnets y permisos al finalizar la vinculación.

### 4.10. Calidad y cumplimiento operativo

- Gestión de procedimientos normalizados de operación.
- Control de versiones de documentos.
- No conformidades, incidentes y análisis de causa raíz.
- Acciones correctivas y preventivas con responsables y fechas.
- Auditorías internas y seguimiento de hallazgos.
- Controles de calidad internos y externos.
- Registro de controles, lotes, resultados y acciones tomadas.
- Indicadores de calidad y tiempos de respuesta.
- Gestión de riesgos operativos y planes de mejora.

### 4.11. Gestión multisede

- Administración central de sedes, servicios y responsables.
- Inventario y tarifas por ubicación.
- Traslado controlado de muestras entre sedes.
- Seguimiento del transporte y recepción de muestras.
- Reportes consolidados y desglosados por sede.
- Permisos limitados por ubicación.
- Configuración de horarios, festivos y capacidad operativa.
- Comparación de indicadores entre sedes.

### 4.12. Comunicación y notificaciones

- Notificaciones dentro de la plataforma.
- Correo electrónico, SMS, notificaciones push y WhatsApp Business mediante proveedores autorizados.
- Plantillas aprobadas y configuración por evento.
- Reintentos y registro del estado de entrega.
- Preferencias de contacto y consentimiento cuando corresponda.
- Mensajes discretos que no incluyan resultados clínicos ni datos sensibles innecesarios.
- Alertas operativas para personal responsable.

### 4.13. Reportes e inteligencia de negocio

- Tiempo promedio entre recepción, procesamiento y liberación de resultados.
- Volumen de órdenes y exámenes por periodo, sede y categoría.
- Tasa de rechazo y repetición de muestras.
- Consumo, rotación y vencimiento de inventario.
- Disponibilidad y utilización de equipos.
- Productividad y carga operativa por área.
- Ingresos, costos y cuentas pendientes según permisos.
- Satisfacción y tiempos de espera.
- Exportación autorizada de reportes.
- Paneles de gerencia con indicadores y filtros.

### 4.14. Soporte y gestión tecnológica

- Registro de incidentes, solicitudes y problemas.
- Priorización, asignación y seguimiento de tickets.
- Inventario de computadores, impresoras, lectores y periféricos.
- Historial de cambios y mantenimiento.
- Alertas por caída de integraciones o dispositivos.
- Base de conocimiento y procedimientos de soporte.
- Registro de versiones desplegadas y responsables del cambio.

### 4.15. Funciones adicionales opcionales

- Kioscos de autoservicio y turnos digitales.
- Toma de muestras a domicilio y planificación de rutas.
- Encuestas de satisfacción.
- Chat de orientación para preguntas administrativas y preparación de exámenes, sin sustituir al profesional sanitario.
- Predicción de demanda de reactivos.
- Detección de anomalías operativas y de acceso para revisión humana.
- Integración con historias clínicas electrónicas.
- Portal ejecutivo móvil para indicadores resumidos.
- Modo de contingencia para interrupciones de conectividad, con sincronización controlada.

Las funciones opcionales deben priorizarse de acuerdo con las necesidades reales, el presupuesto, los riesgos y la capacidad de mantenimiento.

## 5. Control de acceso, biometría y carnets con chip

### 5.1. Objetivo

Integrar el sistema de carnets con chip que ya funciona en el laboratorio y complementarlo, cuando sea viable, con reconocimiento facial y lectores de huella dactilar. La integración debe conservar la operación existente durante las pruebas y evitar reemplazos innecesarios.

### 5.2. Funciones previstas

- Registro de cada carnet y asociación con una identidad autorizada.
- Integración con lectores y controladoras existentes, tras identificar fabricante, modelo y protocolo.
- Validación de permisos por sede, zona, rol y horario.
- Reconocimiento facial con mecanismos de detección de presencia real cuando se adopte un proveedor compatible.
- Lectores de huella en zonas seleccionadas según evaluación de riesgos.
- Segundo factor para accesos sensibles cuando sea necesario.
- Registro de entradas, salidas, rechazos y excepciones.
- Gestión de visitantes, contratistas y carnets temporales.
- Revocación de accesos cuando una credencial se pierde o un usuario deja de estar autorizado.
- Alertas por intentos fallidos repetidos, accesos fuera de horario o patrones inusuales.
- Supervisión del estado de lectores, cámaras y controladoras.
- Procedimiento manual de contingencia y salida segura ante fallas.

### 5.3. Política por nivel de riesgo

| Zona o situación | Método propuesto |
|---|---|
| Áreas comunes del personal | Carnet con chip o credencial autorizada |
| Áreas restringidas | Carnet más segundo factor, según evaluación |
| Áreas críticas | Combinación reforzada de factores y autorización específica |
| Visitantes | Identificación, registro, autorización y credencial temporal |
| Pacientes | Verificación proporcional al trámite, con alternativas accesibles |

La combinación exacta de factores se definirá con el responsable de seguridad. La huella o el rostro no deben exigirse indiscriminadamente ni convertirse en la única vía para prestar un servicio esencial.

### 5.4. Plan de integración con el sistema existente

1. Inventariar fabricante, modelo, tipo de chip, lectores, controladoras y software actual.
2. Revisar documentación, API, SDK, protocolos soportados y condiciones del proveedor.
3. Definir un identificador interno estable para cada persona; no usar el UID del chip como único factor de autenticación.
4. Construir un adaptador aislado que traduzca los eventos del sistema actual al modelo de SILAB.
5. Probar primero en un ambiente separado y con credenciales de prueba.
6. Comparar eventos y permisos sin alterar inicialmente el control productivo.
7. Realizar un piloto en una zona de bajo riesgo.
8. Ejecutar pruebas de seguridad, disponibilidad y recuperación.
9. Documentar el procedimiento de reversión antes de ampliar el despliegue.

**Importante:** no se puede garantizar compatibilidad hasta conocer el hardware y el protocolo. Wiegand, OSDP, RFID y NFC no son intercambiables automáticamente; cada integración requiere validación técnica.

### 5.5. Protección de datos biométricos

- Realizar evaluación de impacto y determinar la base jurídica aplicable antes de activar biometría.
- Recoger solo los datos estrictamente necesarios.
- Preferir procesamiento local o plantillas protegidas cuando sea técnicamente viable.
- Evitar almacenar imágenes crudas de rostro o huellas si no son necesarias.
- Cifrar datos, restringir el acceso y registrar toda operación administrativa.
- Definir retención, eliminación y revocación de acuerdo con la normativa.
- Disponer de métodos alternativos para quienes no puedan o no deban usar biometría.
- No usar la biometría para decisiones clínicas automatizadas.

## 6. Roles y matriz de permisos

La autorización debe aplicarse en el backend, no solo ocultando botones en la interfaz. Los permisos deben combinar rol, sede, contexto y necesidad de acceso.

| Rol | Permisos principales | Restricciones esenciales |
|---|---|---|
| Superadministrador técnico | Configuración técnica, usuarios privilegiados y monitoreo | No obtiene acceso clínico irrestricto por defecto |
| Administrador del laboratorio | Configuración funcional, sedes, catálogos y usuarios | Cambios críticos auditados |
| Recepcionista | Pacientes, citas, órdenes y admisiones | Sin autorización para liberar resultados clínicos |
| Profesional de laboratorio | Procesamiento y validación según habilitación | Solo funciones correspondientes a su competencia |
| Responsable de calidad | Controles, auditorías y no conformidades | Acceso según alcance de calidad |
| Inventario y compras | Existencias, lotes, proveedores y órdenes | Sin acceso innecesario a resultados clínicos |
| Facturación y caja | Facturas, pagos y conciliaciones autorizadas | Sin acceso clínico salvo necesidad justificada |
| Recursos humanos | Datos laborales y asignaciones | Sin acceso a resultados de pacientes |
| Seguridad física | Carnets, permisos de acceso y eventos | Sin acceso a historias o resultados clínicos |
| Médico remitente | Órdenes y resultados de pacientes autorizados | Solo pacientes y datos bajo autorización |
| Paciente | Sus citas, órdenes y resultados liberados | Sin acceso a datos de otras personas |
| Auditor | Registros y evidencias del alcance autorizado | Acceso de solo lectura cuando sea posible |
| Soporte técnico | Diagnóstico técnico y telemetría | Sin acceso a contenido clínico por defecto |

Los permisos finales deben ser aprobados por la dirección del laboratorio y los responsables clínico, jurídico y de seguridad.

## 7. Flujos principales de negocio

### 7.1. De la admisión al resultado

```text
Registro / identificación del paciente
                  |
                  v
       Orden médica y catálogo
                  |
                  v
       Admisión y preparación
                  |
                  v
       Toma y etiquetado de muestra
                  |
                  v
       Recepción y control de calidad
                  |
                  v
       Procesamiento del examen
                  |
                  v
       Revisión y validación autorizada
                  |
                  v
       Liberación del resultado
                  |
                  v
       Notificación y entrega segura
                  |
                  v
       Facturación, auditoría y archivo
```

Cada transición debe validar sus requisitos, registrar al responsable y conservar fecha, hora y estado. La facturación puede ocurrir antes o después de determinadas etapas, según el proceso y los convenios del laboratorio.

### 7.2. Acceso del personal

```text
Presentación del carnet o credencial
                  |
                  v
       Validación de identidad y estado
                  |
                  v
       Evaluación de permisos y horario
                  |
                  v
       Segundo factor si corresponde
                  |
          +-------+-------+
          |               |
        Aprobado         Rechazado
          |               |
          v               v
     Autorizar paso   Registrar evento
          |           y aplicar alertas
          v
     Auditar acceso
```

### 7.3. Inventario y reactivos

```text
Solicitud de compra
        |
        v
Aprobación según política
        |
        v
Recepción y revisión del lote
        |
        v
Registro de vencimiento y ubicación
        |
        v
Consumo asociado al proceso cuando aplique
        |
        v
Conciliación, alertas y reposición
```

## 8. Arquitectura propuesta

### 8.1. Enfoque inicial: monolito modular

Para comenzar, se recomienda un backend organizado por dominios funcionales dentro de una misma aplicación desplegable. Esto reduce la complejidad operativa, facilita las transacciones y permite evolucionar el sistema con un equipo pequeño.

Los módulos deben mantener límites claros y comunicarse mediante interfaces internas. Si el volumen, la disponibilidad o las necesidades de integración lo justifican, ciertos componentes podrán separarse en servicios independientes más adelante.

### 8.2. Diagrama conceptual

```text
 [App Android] [App iOS] [Portal paciente/médico] [Panel administrativo]
       \             |             |                      /
        \------------+-------------+---------------------/
                               |
                         API segura HTTPS
                               |
                  [Autenticación y autorización]
                               |
                   [Backend modular SILAB]
                               |
     +-------------------------+--------------------------+
     |           |             |            |             |
  Clínico     Inventario    Facturación   Acceso        Calidad
     |           |             |            |             |
     +-------------------------+--------------------------+
                               |
                   [Capa de integraciones]
                  /             |              \
        Carnets y lectores   Analizadores   Sistemas externos
                               |
                    [Persistencia y archivos]
                  /             |              \
             PostgreSQL   Almacenamiento     Copias cifradas
                           de documentos
                               |
                  [Auditoría, métricas y alertas]
```

### 8.3. Capas

- **Presentación:** aplicaciones móviles, portales y paneles.
- **API:** endpoints versionados, validación de entradas y contratos documentados.
- **Aplicación:** casos de uso, reglas de negocio y transacciones.
- **Dominio:** pacientes, órdenes, muestras, resultados, inventario, facturación y acceso.
- **Persistencia:** base de datos relacional y almacenamiento de archivos.
- **Integración:** adaptadores para equipos, lectores, proveedores de notificación y sistemas externos.
- **Operación:** auditoría, métricas, logs, alertas, copias de seguridad y despliegue.

### 8.4. Principios de diseño

- Separación de responsabilidades y módulos.
- API REST versionada y documentación OpenAPI.
- Validación en servidor y consultas parametrizadas.
- Transacciones para operaciones que deban ser atómicas.
- Idempotencia para evitar duplicados en reintentos.
- Procesamiento asíncrono para tareas que no necesitan bloquear la respuesta.
- Gestión centralizada de secretos.
- Configuración por ambiente.
- Migraciones de base de datos versionadas.
- No almacenar contraseñas, tokens ni datos biométricos en logs.

## 9. Tecnologías sugeridas

La selección definitiva dependerá de las capacidades del equipo, los requisitos de integración y el presupuesto.

| Capa | Alternativas sugeridas | Criterio |
|---|---|---|
| Aplicación móvil | Flutter; React Native; Swift para componentes iOS específicos | Reutilización y experiencia móvil |
| Portal web | React o Angular con TypeScript | Mantenibilidad y componentes accesibles |
| Backend | Java con Spring Boot; .NET; o Node.js con TypeScript | Elegir un stack principal y mantener consistencia |
| Base de datos | PostgreSQL | Datos relacionales, integridad y transacciones |
| Archivos | Almacenamiento de objetos compatible con S3 o equivalente | Informes y documentos con acceso controlado |
| Cache y colas | Incorporar Redis o un broker cuando el caso lo requiera | No añadir infraestructura sin necesidad demostrada |
| Autenticación | OpenID Connect/OAuth 2.0 mediante proveedor confiable o implementación revisada | Gestión segura de identidad y sesiones |
| Integración clínica | HL7 FHIR y HL7 v2/ASTM cuando sean compatibles con los equipos | Interoperabilidad validada |
| Infraestructura | Docker y despliegue en nube o infraestructura privada aprobada | Reproducibilidad y operación |
| CI/CD | GitHub Actions u otra plataforma equivalente | Automatización de pruebas y despliegue |
| Monitoreo | Logs centralizados, métricas, trazas y alertas | Diagnóstico y continuidad |

### Estándares que conviene evaluar

- **HL7 FHIR:** intercambio estructurado de información sanitaria cuando los sistemas externos lo soporten.
- **HL7 v2 / ASTM:** integración con analizadores y sistemas de laboratorio, según fabricante.
- **LOINC:** codificación de observaciones y pruebas cuando corresponda.
- **SNOMED CT:** terminología clínica según necesidades, licenciamiento y aplicabilidad.
- **OpenAPI:** descripción de los contratos de la API.
- **ISO 15189:** referencia para calidad y competencia de laboratorios clínicos.
- **ISO/IEC 27001:** referencia para gestión de seguridad de la información.

La mención de estos estándares no implica certificación ni cumplimiento automático.

## 10. Modelo de datos conceptual

Entidades iniciales sugeridas:

- `Patient`: identidad del paciente y datos de contacto necesarios.
- `PatientRepresentative`: autorizaciones de acudientes o representantes.
- `StaffMember`: perfil laboral y asignaciones.
- `UserAccount`: identidad de acceso al sistema.
- `Role`, `Permission`, `UserRole`: autorización y control de privilegios.
- `Site`, `Department`: sedes y áreas operativas.
- `Appointment`: citas y agenda.
- `LabOrder`: orden de laboratorio.
- `ExamCatalog`: catálogo de exámenes.
- `Specimen`: muestra, recipiente, etiqueta y estado.
- `SpecimenEvent`: historial de ubicación y manipulación.
- `AnalysisRun`: ejecución del proceso analítico.
- `LabResult`: resultados, unidades, rangos y estados de validación.
- `ResultRevision`: historial de versiones y motivos de corrección.
- `Equipment`: equipos, ubicación y estado.
- `QualityControl`: controles de calidad y resultados.
- `InventoryItem`, `InventoryLot`, `StockMovement`: artículos, lotes y movimientos.
- `Supplier`, `PurchaseOrder`: proveedores y compras.
- `Invoice`, `Payment`: facturación y pagos.
- `AccessCredential`: referencia a credenciales autorizadas.
- `AccessPoint`, `AccessZone`, `AccessEvent`: puntos, zonas y eventos de acceso.
- `ConsentRecord`: consentimientos y su estado.
- `Notification`, `NotificationDelivery`: mensajes y entregas.
- `AuditEvent`: registro de acciones relevantes.
- `SupportTicket`: solicitudes e incidentes técnicos.

### Relaciones principales

```text
Paciente 1 ---- N Órdenes
Orden 1 ------- N Muestras
Orden 1 ------- N Exámenes solicitados
Muestra 1 ----- N Eventos de trazabilidad
Examen 1 ------ N Resultados/versiones
Personal N ---- N Roles y permisos
Sede 1 -------- N Áreas y puntos de acceso
Credencial 1 -- N Eventos de acceso
Artículo 1 ---- N Lotes y movimientos
Equipo 1 ------ N Mantenimientos y controles
Factura 1 ----- N Detalles y pagos
```

El modelo físico debe incluir claves, restricciones, índices, políticas de retención y separación de datos según los requisitos clínicos y legales. Las plantillas biométricas, si se utilizan, deben manejarse mediante un componente especializado y no mezclarse indiscriminadamente con las tablas clínicas.

## 11. Integraciones

### Integraciones prioritarias

1. Sistema actual de carnets con chip y controladoras de acceso.
2. Equipos analizadores que permitan intercambio de datos documentado.
3. Servicios de correo, SMS y notificaciones push.
4. Proveedores de pagos y facturación electrónica, cuando corresponda.
5. Sistemas externos de órdenes, historia clínica o resultados.
6. Herramientas de identidad corporativa, si ya existen.

### Requisitos para cada integración

- Documentación y autorización del proveedor.
- Contratos de datos y mapeo de identificadores.
- Autenticación segura y gestión de secretos.
- Validación de entradas y salidas.
- Manejo de errores, reintentos e idempotencia.
- Registro de eventos sin exponer datos sensibles.
- Monitoreo de conectividad y estado.
- Pruebas con datos sintéticos.
- Procedimiento de contingencia y reversión.
- Control de versiones del adaptador.

Las integraciones deben hacerse por etapas, comenzando por una prueba técnica aislada y evitando que una falla externa interrumpa innecesariamente la operación clínica.

## 12. Seguridad, privacidad y cumplimiento

SILAB manejará potencialmente información personal, datos sensibles de salud y, si se habilita, datos biométricos. La seguridad debe ser un requisito de arquitectura desde el inicio.

### Controles técnicos propuestos

- HTTPS con TLS moderno y configuración segura.
- Cifrado de datos almacenados y copias de seguridad.
- Contraseñas protegidas mediante algoritmos de hash resistentes y sal única.
- MFA para cuentas privilegiadas y accesos de riesgo.
- Autorización en servidor con principio de mínimo privilegio.
- Sesiones seguras, expiración, revocación y protección contra fuerza bruta.
- Limitación de solicitudes y protección contra abuso.
- Validación de entradas, consultas parametrizadas y protección frente a vulnerabilidades comunes.
- Gestión de secretos fuera del código fuente.
- Registro de auditoría protegido contra alteraciones no autorizadas.
- Revisión periódica de permisos y cuentas inactivas.
- Escaneo de dependencias y análisis de seguridad en el pipeline.
- Separación de ambientes y uso de datos sintéticos en desarrollo y pruebas.
- Procedimientos de respuesta a incidentes.
- Pruebas de restauración de copias de seguridad.

### Privacidad y marco colombiano

Antes de la puesta en producción se deberá revisar, con asesoría competente, la normativa aplicable, incluyendo según corresponda:

- Ley 1581 de 2012 y su reglamentación sobre protección de datos personales.
- Reglas aplicables al tratamiento de datos sensibles y biométricos.
- Requisitos sobre historia clínica, reserva, custodia y acceso a información sanitaria.
- Requisitos sanitarios vigentes para laboratorios clínicos.
- Reglas de habilitación y calidad aplicables al prestador.
- Facturación electrónica y obligaciones tributarias, si corresponden.
- Contratos con proveedores y transferencias o transmisiones de datos.

La aplicabilidad concreta depende del tipo de laboratorio, sus servicios, la infraestructura y sus relaciones contractuales. Este README no constituye asesoría jurídica ni certifica cumplimiento normativo.

### Política de auditoría

Registrar, de acuerdo con el riesgo y la política de retención:

- Inicio y cierre de sesión y eventos de autenticación relevantes.
- Accesos y cambios de permisos.
- Creación, modificación, validación, liberación o corrección de resultados.
- Cambios en órdenes y muestras.
- Operaciones de inventario y facturación.
- Accesos físicos y excepciones.
- Exportaciones de datos y operaciones administrativas privilegiadas.

Los logs deben registrar quién, qué, cuándo y el contexto necesario, sin incluir contraseñas, tokens, imágenes biométricas ni contenido clínico innecesario. La retención debe definirse con base en la normativa y las políticas aprobadas.

## 13. Requisitos no funcionales

Las siguientes cifras son **objetivos iniciales para validar mediante pruebas**, no garantías de rendimiento.

| Atributo | Objetivo inicial |
|---|---|
| Disponibilidad de servicios críticos | Definir SLA según operación y presupuesto |
| Tiempo de respuesta de API | Objetivo inicial p95 menor a 500 ms en operaciones ordinarias, excluyendo tareas pesadas |
| Accesibilidad | Interfaz usable con teclado, lectores de pantalla y alternativas visuales |
| Seguridad | Revisión de amenazas, controles y pruebas antes de producción |
| Trazabilidad | Historial auditable para operaciones clínicas y administrativas críticas |
| Escalabilidad | Aumentar recursos y capacidad según métricas reales |
| Recuperación | Definir RPO y RTO mediante análisis de impacto |
| Compatibilidad móvil | Versiones de Android/iOS definidas según dispositivos soportados |
| Mantenibilidad | Código modular, pruebas y documentación actualizada |
| Integridad clínica | No liberar resultados sin cumplir reglas de validación aprobadas |

No deben fijarse periodos de retención, objetivos de recuperación o compromisos de disponibilidad sin validarlos con los responsables operativos y de cumplimiento.

## 14. Continuidad operativa y copias de seguridad

- Copias de seguridad cifradas y con acceso restringido.
- Política documentada de frecuencia, retención y eliminación.
- Copias separadas del ambiente productivo.
- Pruebas periódicas de restauración y verificación de integridad.
- Plan de recuperación ante desastres.
- Procedimientos de contingencia cuando falle Internet, la API, los lectores o los equipos.
- Registro de operaciones pendientes durante una interrupción.
- Sincronización controlada al recuperar la conectividad, con prevención de duplicados.
- Conciliación humana de operaciones críticas que no puedan confirmarse automáticamente.
- Mecanismo de operación manual segura, aprobado por el laboratorio.
- Prohibición de considerar un resultado clínico como definitivo si no se completó su validación requerida.

El modo offline debe limitarse a funciones expresamente diseñadas para ello. No se debe almacenar indiscriminadamente información clínica en dispositivos móviles.

## 15. Pruebas y aseguramiento de calidad

### Pruebas de software

- Unitarias para reglas de negocio.
- Integración entre módulos y base de datos.
- End-to-end de los flujos críticos.
- Pruebas de migraciones y compatibilidad.
- Pruebas de carga y concurrencia.
- Pruebas de accesibilidad y experiencia móvil.
- Validación de copias de seguridad y recuperación.
- Pruebas de idempotencia y recuperación ante fallos.

### Pruebas de seguridad

- Análisis estático del código.
- Análisis de dependencias y secretos.
- Pruebas de autenticación y autorización.
- Evaluación de exposición de datos.
- Pruebas de penetración autorizadas antes de producción y periódicamente.
- Revisión de permisos y sesiones.
- Pruebas específicas de integración de lectores y biometría.

### Pruebas clínicas y operativas

- Validación de cálculos, unidades, rangos y reglas con responsables clínicos.
- Casos de resultados críticos y notificación.
- Corrección y republicación controlada de resultados.
- Pruebas de identificación de pacientes y muestras.
- Pruebas de rechazo y repetición de muestras.
- Validación de controles de calidad.
- Aprobación formal de usuarios responsables antes de liberar módulos clínicos.

Los datos usados en desarrollo y QA deben ser sintéticos o estar adecuadamente protegidos y autorizados.

## 16. Despliegue y DevSecOps

### Ambientes

- `development`: desarrollo diario con datos sintéticos.
- `staging`: pruebas integrales y aceptación.
- `production`: operación real con acceso restringido y monitoreo.

### Pipeline sugerido

```text
Commit / Pull Request
        |
        v
Formato y análisis estático
        |
        v
Pruebas unitarias e integración
        |
        v
Escaneo de dependencias y secretos
        |
        v
Construcción de artefacto
        |
        v
Despliegue en staging
        |
        v
Pruebas de aceptación y aprobación
        |
        v
Despliegue controlado en producción
        |
        v
Monitoreo y opción de reversión
```

- Revisiones de código obligatorias.
- Versionado semántico cuando corresponda.
- Migraciones de base de datos revisadas y reversibles cuando sea viable.
- Secretos administrados fuera del repositorio.
- Aprobación adicional para cambios en resultados, acceso físico o datos sensibles.
- Registro de versiones desplegadas.
- Procedimiento probado de rollback.
- No subir credenciales, certificados privados, datos reales de pacientes ni plantillas biométricas al repositorio.

## 17. Observabilidad y soporte

- Monitoreo de disponibilidad del backend, base de datos e integraciones.
- Métricas de tiempos de respuesta, errores y saturación.
- Seguimiento de colas y tareas pendientes.
- Alertas por fallas de lectores, equipos e integraciones.
- Panel de estado de servicios.
- Logs estructurados con redacción de datos sensibles.
- Trazas entre componentes cuando sean necesarias.
- Tickets de incidentes con prioridad, responsable y resolución.
- Procedimientos de escalamiento y comunicación.
- Análisis posterior a incidentes relevantes y acciones de mejora.

## 18. Estructura sugerida del repositorio

```text
silab/
├── apps/
│   ├── mobile/                 # Flutter o React Native: Android/iOS
│   ├── ios-native/             # Swift opcional para funciones específicas
│   ├── web-admin/              # Panel administrativo
│   ├── web-patient/            # Portal de pacientes
│   └── web-referrers/          # Portal de médicos remitentes
├── backend/
│   ├── src/
│   │   ├── identity/           # Usuarios, sesiones y permisos
│   │   ├── patients/           # Pacientes y representantes
│   │   ├── scheduling/         # Agenda y órdenes
│   │   ├── specimens/          # Muestras y trazabilidad
│   │   ├── results/            # Resultados y validación
│   │   ├── inventory/          # Insumos y reactivos
│   │   ├── equipment/          # Equipos y mantenimiento
│   │   ├── billing/            # Facturación y pagos
│   │   ├── access_control/     # Integración de acceso físico
│   │   ├── quality/            # Calidad y no conformidades
│   │   ├── notifications/      # Notificaciones
│   │   ├── reporting/          # Reportes e indicadores
│   │   └── audit/              # Auditoría
│   ├── migrations/
│   └── tests/
├── integrations/
│   ├── access_hardware/
│   ├── lab_analyzers/
│   ├── billing_providers/
│   └── messaging_providers/
├── infrastructure/
│   ├── docker/
│   ├── deployment/
│   ├── monitoring/
│   └── backup/
├── docs/
│   ├── architecture/
│   ├── api/
│   ├── security/
│   ├── workflows/
│   ├── operations/
│   └── user-manuals/
├── tests/
│   ├── integration/
│   ├── end-to-end/
│   ├── performance/
│   └── security/
├── .env.example
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

La estructura es una guía inicial. Debe adaptarse al framework y a la organización final del código; no es necesario crear carpetas vacías antes de desarrollar sus módulos.

## 19. API de referencia

Ejemplos conceptuales de rutas. Los nombres, esquemas y métodos deben validarse al diseñar la API real.

| Método | Ruta de ejemplo | Propósito |
|---|---|---|
| `POST` | `/api/v1/auth/login` | Iniciar sesión |
| `POST` | `/api/v1/auth/mfa/verify` | Verificar un segundo factor |
| `GET` | `/api/v1/patients/{patientId}` | Consultar paciente con permisos |
| `POST` | `/api/v1/appointments` | Crear una cita |
| `POST` | `/api/v1/orders` | Crear una orden |
| `GET` | `/api/v1/orders/{orderId}` | Consultar el estado de una orden |
| `POST` | `/api/v1/specimens` | Registrar una muestra |
| `POST` | `/api/v1/specimens/{specimenId}/events` | Registrar un evento de trazabilidad |
| `POST` | `/api/v1/results/{resultId}/validate` | Validar un resultado por un usuario autorizado |
| `GET` | `/api/v1/patient/results` | Consultar resultados propios del paciente autenticado |
| `GET` | `/api/v1/inventory/lots` | Consultar lotes autorizados |
| `POST` | `/api/v1/access/events` | Recibir un evento desde un adaptador autorizado |
| `GET` | `/api/v1/reports/operational` | Consultar indicadores según permisos |
| `POST` | `/api/v1/quality/nonconformities` | Registrar una no conformidad |

### Reglas de API

- Autorización en cada recurso y operación.
- Validación de propiedad y alcance: un paciente no puede consultar el expediente de otra persona.
- Paginación, filtros y límites de consulta.
- Manejo uniforme de errores sin filtrar información interna.
- Idempotencia en operaciones con riesgo de duplicación.
- Documentación OpenAPI y versionado.
- Protección contra abuso y registro de operaciones críticas.
- No exponer endpoints que permitan liberar resultados sin controles clínicos.
- Los eventos de acceso físico deben recibirse solo desde integraciones autenticadas y autorizadas.

## 20. Plan de implementación por fases

### Fase 0 — Descubrimiento y análisis

- Mapear los procesos reales del laboratorio.
- Identificar usuarios, roles y responsables.
- Inventariar el sistema de carnets, lectores y controladoras.
- Identificar analizadores, formatos de intercambio y software actual.
- Revisar requisitos normativos, riesgos y necesidades de privacidad.
- Priorizar funcionalidades y definir criterios de aceptación.

**Entregables:** mapa de procesos, alcance aprobado, inventario de integraciones y arquitectura inicial.

### Fase 1 — Base técnica y seguridad

- Crear repositorios y estructura modular.
- Configurar ambientes y pipeline.
- Implementar autenticación, roles, permisos y auditoría.
- Preparar base de datos, migraciones y copias de seguridad.
- Definir convenciones de código y pruebas.

**Entregables:** base técnica funcional, gestión de acceso lógico y despliegue de prueba.

### Fase 2 — MVP clínico

- Pacientes y admisiones.
- Catálogo de exámenes.
- Citas y órdenes.
- Registro y etiquetado de muestras.
- Estados de procesamiento.
- Resultados con validación y autorización.
- Portal básico de consulta de resultados.
- Auditoría de acciones clínicas críticas.

**Entregables:** flujo clínico de punta a punta validado en ambiente de pruebas.

### Fase 3 — Operación y administración

- Inventario, lotes y vencimientos.
- Compras y proveedores.
- Facturación y caja según el alcance acordado.
- Gestión de sedes y áreas.
- Gestión documental y calidad.
- Reportes operativos básicos.

**Entregables:** administración operativa integrada y reportes iniciales.

### Fase 4 — Aplicaciones móviles y notificaciones

- Aplicación Android/iOS.
- Portal de pacientes adaptado a móviles.
- Citas y notificaciones.
- Consulta segura de resultados.
- Flujos de toma de muestras a domicilio, si aplica.
- Pruebas de accesibilidad y seguridad móvil.

**Entregables:** versión móvil piloto y portal de pacientes.

### Fase 5 — Integraciones físicas y clínicas

- Prueba de compatibilidad del carnet con chip.
- Adaptador del sistema de control de acceso.
- Piloto de biometría sujeto a evaluación de impacto.
- Integración con analizadores compatibles.
- Integración con servicios externos priorizados.
- Pruebas de contingencia y reversión.

**Entregables:** integraciones aprobadas y documentación operativa.

### Fase 6 — Analítica, optimización y producción

- Tableros gerenciales.
- Métricas de desempeño y capacidad.
- Pruebas de carga y seguridad.
- Plan de recuperación ante desastres.
- Capacitación y manuales.
- Piloto con usuarios autorizados.
- Aprobación formal y despliegue gradual.

**Entregables:** operación controlada, manuales y plan de soporte.

## 21. Roadmap

- [ ] Levantar y aprobar los procesos reales del laboratorio.
- [ ] Identificar el sistema actual de carnets y sus interfaces.
- [ ] Seleccionar stack tecnológico y arquitectura.
- [ ] Diseñar el modelo de datos y la matriz de permisos.
- [ ] Definir amenazas, riesgos y controles de privacidad.
- [ ] Configurar repositorio, CI/CD y ambientes.
- [ ] Implementar identidad, roles y auditoría.
- [ ] Implementar pacientes, órdenes y citas.
- [ ] Implementar muestras y trazabilidad.
- [ ] Implementar procesamiento, validación y entrega de resultados.
- [ ] Crear portal del paciente.
- [ ] Crear aplicaciones Android/iOS.
- [ ] Implementar inventario y equipos.
- [ ] Implementar facturación y reportes.
- [ ] Integrar carnets con chip en ambiente de pruebas.
- [ ] Evaluar y, si procede, implementar biometría.
- [ ] Integrar analizadores y sistemas externos compatibles.
- [ ] Completar pruebas de seguridad, carga y recuperación.
- [ ] Capacitar usuarios y preparar soporte.
- [ ] Ejecutar piloto y aprobar salida a producción.

## 22. Instalación y ejecución

> Los comandos definitivos dependen del stack seleccionado. Esta sección debe completarse cuando existan el backend, las aplicaciones y los archivos de configuración reales.

### Requisitos previstos

- Git.
- Docker y Docker Compose, si se elige despliegue en contenedores.
- Runtime y SDK correspondientes al stack final.
- Instancia de PostgreSQL.
- Variables de entorno configuradas de forma segura.
- Credenciales de prueba para servicios externos, cuando se necesiten.

### Configuración de ambiente

1. Clonar el repositorio.
2. Revisar el archivo `.env.example`.
3. Crear el archivo local `.env` sin subirlo al repositorio.
4. Configurar la base de datos y ejecutar migraciones.
5. Iniciar el backend y las aplicaciones siguiendo sus instrucciones específicas.
6. Ejecutar las pruebas automatizadas.
7. Verificar logs, conectividad y estado de los servicios.

No se deben incluir credenciales reales, claves privadas, datos clínicos reales ni información biométrica en ejemplos de configuración.

## 23. Decisiones pendientes

Antes de iniciar el desarrollo completo se deben resolver estas preguntas:

- ¿Cuántas sedes tiene el laboratorio y cuántos usuarios tendrá?
- ¿Qué sistema de información utiliza actualmente?
- ¿Qué fabricante, modelo y tecnología tienen los carnets con chip?
- ¿Los lectores y controladoras ofrecen API, SDK u otro protocolo documentado?
- ¿Qué analizadores clínicos deben integrarse?
- ¿Se requiere una app nativa o basta una aplicación multiplataforma?
- ¿Qué procesos deben estar disponibles sin conexión?
- ¿Qué proveedores se utilizarán para pagos, notificaciones y facturación?
- ¿Qué infraestructura y presupuesto están disponibles?
- ¿Qué políticas de retención, consentimiento, acceso y auditoría deben aplicarse?
- ¿Qué responsables aprobarán los flujos clínicos, de seguridad y de calidad?

Las respuestas permitirán estimar tiempos, costos, riesgos y alcance con mayor precisión.

## 24. Glosario

- **API:** interfaz que permite la comunicación entre aplicaciones.
- **Biometría:** autenticación o verificación basada en características físicas o conductuales.
- **CAPA:** acciones correctivas y preventivas dentro de un sistema de calidad.
- **CI/CD:** integración y entrega o despliegue continuo.
- **FHIR:** estándar para intercambio de información sanitaria.
- **LIS:** sistema de información de laboratorio.
- **MFA:** autenticación multifactor.
- **MVP:** producto mínimo viable.
- **OSDP:** protocolo de comunicación para dispositivos de control de acceso.
- **RBAC:** control de acceso basado en roles.
- **RPO:** punto objetivo de recuperación; pérdida de datos tolerable medida en tiempo.
- **RTO:** tiempo objetivo para recuperar un servicio.
- **TLS:** protocolo para proteger comunicaciones en tránsito.
- **UID:** identificador único de un dispositivo o credencial; no debe considerarse por sí solo una prueba suficiente de identidad.
- **QR:** código bidimensional que puede utilizarse para identificar órdenes, muestras o credenciales bajo controles adecuados.

## 25. Licencia

La licencia está **por definir**. Antes de publicar o distribuir SILAB, se debe acordar si será un producto propietario del laboratorio, un proyecto académico, una solución de uso interno o un software distribuido bajo licencia abierta.

---

## Aviso final

SILAB es una propuesta de diseño. Su implementación requiere análisis de procesos, validación clínica, revisión de seguridad, pruebas de integración con hardware real y evaluación del cumplimiento normativo aplicable. Ningún resultado clínico debe liberarse automáticamente sin las validaciones y autorizaciones exigidas por los procedimientos del laboratorio.
