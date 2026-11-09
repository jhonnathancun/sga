# ADR-02 · Base de datos relacional con bloqueo pesimista para cupos y optimista para notas

- **Estado:** Aceptada
- **Requerimientos:** RF-07, RF-08, RNF-07

## Contexto
No se deben sobrepasar cupos ni duplicar matrículas ante solicitudes simultáneas. Las notas rara vez tienen conflictos porque cada grupo tiene un docente.

## Decisión
- PostgreSQL con transacciones ACID.
- Matrícula: `SELECT … FROM grupo WHERE id = :id FOR UPDATE` dentro de la transacción; índice único parcial `matricula(estudiante_id, asignatura_id, periodo_id) WHERE estado = 'ACTIVA'` como segunda barrera; `CHECK (cupos_ocupados <= cupo_maximo)`; cabecera `Idempotency-Key`.
- Calificaciones: columna `version` y `UPDATE … WHERE id = :id AND version = :version` (control optimista).

## Consecuencias
- (+) La corrección la garantiza la base de datos, no solo la aplicación.
- (−) En la apertura de matrícula, los grupos más solicitados serializan sus transacciones.
- Mitigación: transacciones cortas sin llamadas externas, tiempo máximo de espera de bloqueo y prueba de carga del día de apertura.

## Verificación
100 solicitudes simultáneas sobre un grupo de 30 cupos ⇒ exactamente 30 matrículas y 70 respuestas 409.

## Ajustes tras la revisión con Google AI Studio
- `lock_timeout` de 2 s y límite de solicitudes concurrentes por grupo, para no agotar el pool de conexiones.
- Matrícula de varios grupos: una transacción por grupo (éxito parcial informado por asignatura) o bloqueos tomados en orden ascendente de id de grupo, para evitar interbloqueos.
- Alternativa equivalente documentada: `UPDATE grupo SET cupos_ocupados = cupos_ocupados + 1 WHERE id = :id AND cupos_ocupados < cupo_maximo`.
