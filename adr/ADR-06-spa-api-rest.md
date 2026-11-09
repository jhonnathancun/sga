# ADR-06 · SPA Angular + API REST con contrato OpenAPI

- **Estado:** Aceptada
- **Requerimientos:** RNF-06, RNF-08, RNF-12

## Contexto
Interfaz accesible y adaptable a computadores y móviles, e integración futura con plataformas institucionales.

## Decisión
Separar cliente (SPA servida por CDN) y servidor (API REST versionada en `/api/v1`, contrato OpenAPI 3, errores `application/problem+json`). La misma API que usa la SPA queda disponible para integraciones.

## Consecuencias
- (+) Un solo contrato para interfaz e integraciones.
- (−) La accesibilidad en una SPA exige cuidado explícito (foco, anuncios a lectores de pantalla).
- Mitigación: pruebas automáticas de accesibilidad (axe-core, Lighthouse) en CI y recorrido manual con teclado.
