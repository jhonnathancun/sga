# SGA — Arquitectura de un Sistema de Gestión Académica

**CUN · Arquitectura de Software · ACA (semanas 3 a 6)**
Estudiante: Jhonnathan Brandon Galvis Landinez · Docente: Alexander Calderón Martínez · Grupo 55536 · Noviembre de 2026

Propuesta de arquitectura para un Sistema de Gestión Académica (SGA) web: requerimientos, modelos del sistema, matriz de trazabilidad, decisiones arquitectónicas y análisis de riesgos. El sistema **no está implementado**; este repositorio contiene el diseño y sus fuentes.

## Resumen de la arquitectura

- **Estilo:** monolito modular con arquitectura limpia por módulo (8 módulos: Identidad y Acceso, Personas, Catálogo, Oferta, Matrículas, Calificaciones, Reportes, Auditoría).
- **Cliente:** SPA Angular, responsive y accesible (WCAG 2.2 AA).
- **Backend:** API REST `/api/v1` en ASP.NET Core, documentada con OpenAPI.
- **Datos:** PostgreSQL (un esquema por módulo, réplica en espera, respaldos con PITR), Redis como caché.
- **Seguridad:** HTTPS/TLS, WAF, JWT de corta vida + refresh token rotativo, RBAC por permisos, Argon2id, auditoría inmutable vía patrón outbox.

## Contenido

| Ruta | Descripción |
|---|---|
| `informe/ACA_Arquitectura_SGA.pdf` | Informe final |
| `diagramas/` | Fuentes (`.dot`, `.mmd`) y exportados (`.svg`, `.png`) de los 6 diagramas |
| `adr/` | Registro de decisiones arquitectónicas ADR-01 a ADR-06 |
| `evidencias-ai-studio/` | Capturas de prompts y respuestas de Google AI Studio |

## Diagramas

| # | Diagrama | Fuente |
|---|---|---|
| 1 | Casos de uso | `diagramas/01_casos_de_uso.dot` |
| 2 | Componentes / vista lógica | `diagramas/02_componentes.dot` |
| 3 | Despliegue | `diagramas/03_despliegue.dot` |
| 4 | Modelo de datos conceptual | `diagramas/04_modelo_datos.dot` |
| 5 | Secuencia: matrícula | `diagramas/05_secuencia_matricula.mmd` |
| 6 | Secuencia: calificación | `diagramas/06_secuencia_calificacion.mmd` |

Regenerar:

```bash
# Graphviz
dot -Tsvg diagramas/02_componentes.dot -o diagramas/02_componentes.svg
# Mermaid CLI
npx @mermaid-js/mermaid-cli -i diagramas/05_secuencia_matricula.mmd -o diagramas/05_secuencia_matricula.svg
```

## Enlaces

- Repositorio: https://github.com/jhonnathancun/sga
- Video pitch (YouTube): https://youtu.be/Ch0j0luFkEQ

> No se incluyen datos personales reales ni credenciales en este repositorio.
