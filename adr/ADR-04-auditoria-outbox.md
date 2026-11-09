# ADR-04 · Auditoría basada en eventos con patrón outbox

- **Estado:** Aceptada
- **Requerimientos:** RF-12, RNF-02, RNF-07

## Contexto
Se deben registrar inicios de sesión, cambios de rol, matrículas y modificaciones de notas sin perder ninguno y sin degradar el tiempo de respuesta.

## Decisión
Cada caso de uso escribe su evento en una tabla `outbox` dentro de la misma transacción de negocio. Un worker traslada los eventos a `evento_auditoria`, tabla sobre la que el usuario de la aplicación solo tiene `INSERT` y `SELECT`.

## Consecuencias
- (+) No puede existir una matrícula o una nota sin su evento.
- (−) La bitácora puede tener segundos de retraso.
- Mitigación: alerta si hay eventos pendientes por más de 5 minutos.
