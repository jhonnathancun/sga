# ADR-05 · Alta disponibilidad con redundancia activa y respaldos PITR

- **Estado:** Aceptada (RPO/RTO pendientes de validar con la institución)
- **Requerimientos:** RNF-02, RNF-03, RNF-04, RNF-09, RF-10

## Contexto
Disponibilidad mensual ≥ 99,5 % (≈ 3 h 39 min de margen) y obligación de respaldar y restaurar.

## Decisión
- Dos instancias de API sin estado en zonas de disponibilidad distintas detrás de un balanceador L7 con health checks.
- PostgreSQL primario + réplica en espera en otra zona (también usada para reportes).
- Respaldo base diario + archivado continuo de WAL a almacenamiento de objetos cifrado, versionado y copiado a otra región.
- Objetivos propuestos: RPO ≤ 15 min, RTO ≤ 4 h; simulacro de restauración por periodo académico.

## Consecuencias
- (+) Tolera la caída de una instancia o de una zona; restauración a un punto en el tiempo.
- (−) Mayor costo que un servidor único.
- Mitigación: instancias pequeñas con autoescalado y servicios administrados.
