# vidasalud-docs

Diagramas de arquitectura, contratos de API y manuales del proyecto VidaSalud.

| Documento | Contenido |
|---|---|
| [matriz-funcionalidades.md](matriz-funcionalidades.md) | **Matriz de funcionalidades**: alcance completo, estado, componente responsable y evidencia |
| [mensajeria-rabbitmq.md](mensajeria-rabbitmq.md) | Arquitectura de mensajería: exchanges, colas, DLQ, manejo de errores y guion de demostración |

## Repositorios

| Repo | Rol |
|---|---|
| [vidasalud-frontend](https://github.com/effimrv/vidasalud-frontend) | Angular 18 + MSAL (Entra ID) |
| [vidasalud-ms-bff](https://github.com/effimrv/vidasalud-ms-bff) | Backend for Frontend: valida el JWT y enruta |
| [vidasalud-ms-appointments](https://github.com/effimrv/vidasalud-ms-appointments) | Citas; productor de eventos RabbitMQ |
| [vidasalud-ms-catalog](https://github.com/effimrv/vidasalud-ms-catalog) | Catálogo de prestaciones e información institucional |
| [vidasalud-ms-notify](https://github.com/effimrv/vidasalud-ms-notify) | Consumidores RabbitMQ y API de administración de RabbitMQ |
| [vidasalud-infra](https://github.com/effimrv/vidasalud-infra) | docker-compose del clúster RabbitMQ y los microservicios |
