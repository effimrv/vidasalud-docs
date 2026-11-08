# Arquitectura general de VidaSalud

Vista única de los componentes y la comunicación entre ellos. Para el detalle de cada mensajería ver [mensajeria-rabbitmq.md](mensajeria-rabbitmq.md) y [mensajeria-kafka.md](mensajeria-kafka.md).

```mermaid
flowchart LR
    NAV[Navegador<br/>Angular 18 + MSAL] -- "login OIDC" --> ENTRA[(Microsoft Entra ID)]
    NAV -- "página" --> GWF[API Gateway frontend]
    NAV -- "API + JWT" --> GWB[API Gateway backend<br/>autorizador JWT]
    GWF --> FE[frontend nginx]
    GWB --> BFF[ms-bff<br/>valida JWT, roles y dueño]

    subgraph EC2[EC2 · contenedores Docker]
        FE
        BFF
        BFF --> CAT[ms-catalog]
        BFF --> APP[ms-appointments]
        APP -- "cita.#" --> RMQ{{RabbitMQ<br/>2 nodos · topic y direct · DLQ}}
        RMQ --> NOT[ms-notify<br/>avisos, tickets, documentos]
        APP -- "appointments.events" --> KFK{{Kafka<br/>3 brokers · 3 Zookeeper}}
        KFK --> REP[ms-report<br/>KPIs y administrador Kafka]
        KFK --> AUD[ms-audit<br/>timeline y responsable]
        AUD -- "audit.timeline" --> KFK
        KFK -- "audit.timeline" --> REP
    end

    CAT --> DB[(Supabase PostgreSQL)]
    APP --> DB
```

## Lectura del diagrama

- El navegador solo habla con API Gateway; el token lo valida el gateway y después el BFF, appointments, catalog, report y audit.
- Las tareas que no necesitan respuesta inmediata (avisos, tickets, comprobantes) van por RabbitMQ.
- Los eventos de cada cita y la auditoría van por Kafka: `appointments.events` y `audit.timeline`.
