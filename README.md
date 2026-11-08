# vidasalud-docs

Diagramas de arquitectura, contratos de API y manuales del proyecto VidaSalud.

| Documento | Contenido |
|---|---|
| [matriz-funcionalidades.md](matriz-funcionalidades.md) | **Matriz de funcionalidades**: alcance completo, estado, componente responsable y evidencia |
| [mensajeria-rabbitmq.md](mensajeria-rabbitmq.md) | Arquitectura de mensajería: exchanges, colas, DLQ, manejo de errores y guion de demostración |
| [mensajeria-kafka.md](mensajeria-kafka.md) | Streaming con Kafka: tópicos, justificación de particiones y réplicas, DLT, administración y guion de demostración |

## Repositorios

| Repo | Rol |
|---|---|
| [vidasalud-frontend](https://github.com/effimrv/vidasalud-frontend) | Angular 18 + MSAL (Entra ID) |
| [vidasalud-ms-bff](https://github.com/effimrv/vidasalud-ms-bff) | Backend for Frontend: valida el JWT y enruta |
| [vidasalud-ms-appointments](https://github.com/effimrv/vidasalud-ms-appointments) | Citas; productor de eventos RabbitMQ |
| [vidasalud-ms-catalog](https://github.com/effimrv/vidasalud-ms-catalog) | Catálogo de prestaciones e información institucional |
| [vidasalud-ms-notify](https://github.com/effimrv/vidasalud-ms-notify) | Consumidores RabbitMQ y API de administración de RabbitMQ |
| [vidasalud-ms-report](https://github.com/effimrv/vidasalud-ms-report) | Consumidor Kafka: KPIs, y administración de tópicos y consumer groups |
| [vidasalud-ms-audit](https://github.com/effimrv/vidasalud-ms-audit) | Consumidor Kafka: timeline de auditoría de citas |
| [vidasalud-infra](https://github.com/effimrv/vidasalud-infra) | docker-compose de los clústeres RabbitMQ y Kafka (con Kafka-UI) y de los microservicios |
