# Pulso

Plataforma de atención al cliente con IA para la gestión de **disputas de transacciones de LATAM Bank**, construida por nuestro equipo para la **Factored AI & Data Hackathon 2026**.

Clientes y equipo de soporte conversan por chat, llamadas y correo simulados. Los analistas resuelven casos, los supervisores vigilan colas y escalamientos, y los administradores gestionan usuarios. Encima de eso, la IA responde a clientes, asiste a los analistas como copiloto y, tipo de caso por tipo de caso, se gana el derecho a operar como agente autónomo que un supervisor aprueba y activa. Con la IA apagada, la plataforma sigue funcionando solo con personas.

## Cómo encajan las piezas

| Capa | Repositorio | Qué hace |
|---|---|---|
| Producto | [support-platform](https://github.com/factored-hackathon-2026-pulso/support-platform) | API (FastAPI), web app (React) y documentación de la plataforma de soporte. Interfaz en español y portugués de Brasil. |
| Motor de agentes | [agent-core](https://github.com/factored-hackathon-2026-pulso/agent-core) | Ejecuta agentes descritos como datos versionados (Understand → Decide → Act → Verify → Escalate), con permisos en la capa de tools, datos personales protegidos y auditoría reproducible. |
| Acceso a modelos | [llm-gateway](https://github.com/factored-hackathon-2026-pulso/llm-gateway) | Gateway HTTP sin estado hacia endpoints compatibles con OpenAI: JSON validado por esquema, costo exacto en USD y errores tipados. |
| Herramientas | [tool-service](https://github.com/factored-hackathon-2026-pulso/tool-service) | Expone tools sobre los datos publicados por el pipeline (productos, movimientos, perfil, casos y radicación de PQR) para `agent-core`. |
| Datos | [data-pipeline](https://github.com/factored-hackathon-2026-pulso/data-pipeline) | Pipeline medallón (bronze, silver, gold) con dbt y DuckDB, con contratos, catálogo de clasificación de campos y zonas con y sin PII. |
| Datos | [data-lab](https://github.com/factored-hackathon-2026-pulso/data-lab) | Ingesta y calidad del dataset del reto, contratos, análisis y la muestra sintética de historial de la plataforma. |
| Mejora continua | [improvement-engine](https://github.com/factored-hackathon-2026-pulso/improvement-engine) | Servicio de detección y mejora autónoma que integra las primitivas de `agent-core`. |
| Infraestructura | [infra](https://github.com/factored-hackathon-2026-pulso/infra) | Terraform y herramientas de despliegue en AWS (entornos `staging` y `prod`, este último como demo del hackathon). |
| Documentación | [docs](https://github.com/factored-hackathon-2026-pulso/docs) | Documentación técnica de los servicios, con diagramas de arquitectura y el registro de fuentes del proyecto. |

## Por dónde empezar

- **Probar la plataforma:** guía en [`support-platform/docs/platform/TRY-IT.md`](https://github.com/factored-hackathon-2026-pulso/support-platform/blob/main/docs/platform/TRY-IT.md).
- **Entender la arquitectura:** el índice de [docs](https://github.com/factored-hackathon-2026-pulso/docs) resume cada servicio.
- **Correrla localmente:** `docker compose up -d --build` en `support-platform` levanta la API y la web app.

## Principios

- **Seguridad antes que autonomía:** los permisos viven en la capa de tools, no en el prompt, y toda escritura pasa por confirmar → actuar → verificar.
- **Datos protegidos:** ningún dato personal en claro llega a un modelo, un log o un evento.
- **Auditable y reproducible:** cada ejecución deja una cadena de eventos con hash que se puede reproducir.
- **Sin datos reales en los repositorios:** el dataset del reto es privado y nunca se versiona.
