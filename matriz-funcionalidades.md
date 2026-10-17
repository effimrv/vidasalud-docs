# Matriz de funcionalidades — VidaSalud

Estado de cada funcionalidad del alcance, componente responsable y evidencia verificable.

**Estados:** ✅ Implementada · 🟡 Parcial · ❌ No implementada

> Actualizar el estado y la evidencia a medida que se despliegue o se agregue algo. Las funcionalidades parciales o sin evidencia no cuentan como implementadas.

## 1. Autenticación y seguridad (IDaaS)

| # | Funcionalidad | Estado | Componente | Evidencia |
|---|---|---|---|---|
| F01 | Inicio de sesión OAuth 2.0 / OpenID Connect con Microsoft Entra ID (redirect) | ✅ | vidasalud-frontend | `src/app/app.config.ts` · [07a4a35](https://github.com/effimrv/vidasalud-frontend/commit/07a4a35) |
| F02 | Obtención y renovación del access token y envío como `Bearer` en cada llamada | ✅ | vidasalud-frontend | `MsalInterceptor` en `app.config.ts`, `api.service.ts` · [07a4a35](https://github.com/effimrv/vidasalud-frontend/commit/07a4a35) |
| F03 | Registro de usuarios (self sign-up) en el tenant | ✅ | Entra ID | Captura del flujo de usuario en el portal de Entra ID |
| F04 | Cierre de sesión | ✅ | vidasalud-frontend | `confirmarLogout()` en `app.component.ts` |
| F05 | Protección de vistas privadas con `MsalGuard` | ✅ | vidasalud-frontend | `src/app/app.routes.ts` |
| F06 | Validación del JWT (firma, emisor y vigencia) en el backend | ✅ | vidasalud-ms-bff | `SecurityConfig.java` · [0c5b46d](https://github.com/effimrv/vidasalud-ms-bff/commit/0c5b46d) |
| F07 | Autorización por rol (Admin, Recepcionista, Paciente) | ✅ | vidasalud-ms-bff | `SecurityConfig.java` · [a8dc177](https://github.com/effimrv/vidasalud-ms-bff/commit/a8dc177) |
| F08 | Datos del usuario autenticado (`GET /api/me`) | ✅ | vidasalud-ms-bff | `MeController.java` |

## 2. API Manager (AWS API Gateway)

| # | Funcionalidad | Estado | Componente | Evidencia |
|---|---|---|---|---|
| F09 | 11 rutas explícitas hacia el BFF (sin comodín `{proxy+}`) | ✅ | API Gateway `dzv5td4m80` | `aws apigatewayv2 get-routes --api-id dzv5td4m80` |
| F10 | Validación del JWT en el gateway: rutas públicas responden 200 y las protegidas sin token, 401 | ✅ | API Gateway (autorizador JWT) | `curl …/api/info` → 200 · `curl …/api/me` → 401 |
| F11 | CORS restringido al origen del frontend, a `GET/POST/PUT/OPTIONS` y a los encabezados `authorization` y `content-type` | ✅ | API Gateway + vidasalud-ms-bff | Preflight `OPTIONS` → 204 · [b5ab844](https://github.com/effimrv/vidasalud-ms-bff/commit/b5ab844) |

## 3. Funcionalidades de negocio

| # | Funcionalidad | Estado | Componente | Evidencia |
|---|---|---|---|---|
| F12 | Listar atenciones (con filtro por estado) | ✅ | vidasalud-ms-appointments | `GET /api/appointments?status=` |
| F13 | Agendar una cita con fecha y hora | ✅ | vidasalud-ms-appointments + frontend | `POST /api/appointments` · [ef56015](https://github.com/effimrv/vidasalud-ms-appointments/commit/ef56015) |
| F14 | Cambiar el estado de una cita con transiciones controladas | ✅ | vidasalud-ms-appointments | `PUT /api/appointments/{id}/status` (`validarTransicion`) |
| F15 | Listar el catálogo de prestaciones | ✅ | vidasalud-ms-catalog | `GET /api/catalog/services` |
| F16 | Crear y editar prestaciones (solo Admin) | ✅ | vidasalud-ms-catalog + frontend | `POST` / `PUT /api/catalog/services` · panel de administración |
| F17 | Información institucional pública y editable por Admin | ✅ | vidasalud-ms-catalog + BFF | `GET` / `PUT /api/info` · [c0b474c](https://github.com/effimrv/vidasalud-ms-catalog/commit/c0b474c) · [eea2bbf](https://github.com/effimrv/vidasalud-ms-bff/commit/eea2bbf) |
| F18 | Portal del paciente: mis citas con filtros por estado | ✅ | vidasalud-frontend | `appointments.component.*` |
| F19 | Panel de Admin y Recepción: gestión de citas, catálogo y datos del centro | ✅ | vidasalud-frontend | `admin-panel.component.*` |
| F20 | Sitio público: inicio con especialidades y página Nosotros | ✅ | vidasalud-frontend | `home/`, `about/` · [8d32674](https://github.com/effimrv/vidasalud-frontend/commit/8d32674) |
| F21 | Persistencia en PostgreSQL cloud (Supabase) con entidades y repositorios JPA | ✅ | appointments + catalog | `model/`, `repository/`, `application.yml` |

## 4. Mensajería asíncrona (RabbitMQ)

| # | Funcionalidad | Estado | Componente | Evidencia |
|---|---|---|---|---|
| F22 | Clúster RabbitMQ de 2 nodos levantado con docker-compose | ✅ | vidasalud-infra | `docker-compose.yml` · `rabbitmqctl cluster_status` · [fcb86c8](https://github.com/effimrv/vidasalud-infra/commit/fcb86c8) |
| F23 | Exchange **topic** de eventos de citas (`vidasalud.citas.topic`) | ✅ | vidasalud-ms-notify | `RabbitTopologyConfig.java` · [88533e4](https://github.com/effimrv/vidasalud-ms-notify/commit/88533e4) |
| F24 | Exchange **direct** de dead-letter (`vidasalud.dlx.direct`) | ✅ | vidasalud-ms-notify | `RabbitTopologyConfig.java` |
| F25 | 3 colas (notificaciones, tickets y documentos), cada una con su DLQ | ✅ | vidasalud-ms-notify | `rabbitmqctl list_queues` · `application.yml` |
| F26 | Productor: evento por cada cita creada o cambio de estado | ✅ | vidasalud-ms-appointments | `RabbitCitaEventPublisher.java` · [b349e67](https://github.com/effimrv/vidasalud-ms-appointments/commit/b349e67) |
| F27 | Consumidor de notificaciones al paciente (`cita.#`) | ✅ | vidasalud-ms-notify | `NotificacionCitaListener` · `GET /api/notify/notificaciones` |
| F28 | Consumidor de tickets de atención (`cita.confirmada`) | ✅ | vidasalud-ms-notify | `TicketCitaListener` · `GET /api/notify/tickets` |
| F29 | Consumidor de comprobantes (`cita.cerrada`) | ✅ | vidasalud-ms-notify | `DocumentoCitaListener` · `GET /api/notify/documentos` |
| F30 | ACK manual explícito en cada consumidor, reintentos y NACK hacia la DLQ, con registro en logs | ✅ | vidasalud-ms-notify | `*CitaListener.java` · `ConfirmacionMensajes.java` · `TicketCitaListenerTest` |
| F31 | Administración REST de colas (crear, consultar, eliminar) | ✅ | vidasalud-ms-notify | `POST/GET/DELETE /api/rabbit/queues` |
| F32 | Administración REST de exchanges (crear, eliminar) | ✅ | vidasalud-ms-notify | `POST/DELETE /api/rabbit/exchanges` |
| F33 | Administración REST de bindings (crear, eliminar) | ✅ | vidasalud-ms-notify | `POST/DELETE /api/rabbit/bindings` |
| F34 | Validación de entrada en la API de administración (DTO y mensajes de error claros) | ✅ | vidasalud-ms-notify | `admin/dto/*` · `RabbitAdminControllerTest` · Swagger `/swagger-ui.html` |

## 5. Despliegue en la nube

| # | Funcionalidad | Estado | Componente | Evidencia |
|---|---|---|---|---|
| F35 | BFF y microservicios de citas y catálogo desplegados y accesibles a través del API Gateway | ✅ | AWS EC2 + API Gateway | `https://dzv5td4m80.execute-api.us-east-1.amazonaws.com/api/health` → 200 |
| F36 | Frontend desplegado en la nube (nginx en Docker sobre EC2, HTTPS mediante API Gateway) | ✅ | vidasalud-frontend + vidasalud-infra | https://e7u1iiqh4k.execute-api.us-east-1.amazonaws.com · [c821f15](https://github.com/effimrv/vidasalud-frontend/commit/c821f15) |
| F37 | RabbitMQ (clúster de 2 nodos), ms-notify y ms-appointments con productor desplegados en la nube | ✅ | vidasalud-infra + AWS EC2 (t3.small) | `docker compose -f docker-compose.yml -f docker-compose.ec2.yml --profile apps up -d` · `rabbitmqctl cluster_status` (2 nodos) · log: cita 21 `cita.confirmada` → correo + ticket T-0001 con ACK |

## Resumen

| Total del alcance | Implementadas | Parciales | No implementadas | % implementado |
|---|---|---|---|---|
| 37 | 37 | 0 | 0 | **100 %** |

*% implementado = funcionalidades implementadas y operativas ÷ total del alcance × 100.*
