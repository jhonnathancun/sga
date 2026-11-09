# ADR-01 · Monolito modular en lugar de microservicios

- **Estado:** Aceptada
- **Requerimientos:** RNF-04, RNF-05, RNF-07

## Contexto
Un solo dominio académico, 8 módulos funcionales, equipo de 3 a 5 personas, escenario de 300 usuarios concurrentes y una operación crítica (matrícula) que necesita consistencia fuerte entre el cupo del grupo, el historial del estudiante y la auditoría.

## Decisión
Una sola aplicación desplegable con módulos de fronteras explícitas: un esquema de base de datos por módulo, comunicación únicamente por interfaces públicas (`IOfertaQuery`, `IMatriculaQuery`, …) o eventos de dominio, y pruebas de arquitectura en CI que fallan si un módulo depende de los internos de otro.

## Alternativas consideradas
- **Capas tradicionales:** más simple, pero tiende a un servicio de negocio que conoce todas las tablas (contradice RNF-05).
- **Microservicios:** mejor escalado por módulo, pero exige sagas para la matrícula, varias bases de datos y mayor costo operativo sin que la carga lo justifique.

## Consecuencias
- (+) Transacciones locales, operación y costo bajos, un solo pipeline.
- (−) Un error grave afecta todo el sistema; se escala la aplicación completa.
- Mitigación: pruebas de arquitectura; posibilidad de extraer módulos (Reportes, Matrículas) si la carga lo exige.
