# ADR-03 · JWT de corta duración + refresh token rotativo, RBAC por permisos y Argon2id

- **Estado:** Aceptada
- **Requerimientos:** RF-01, RF-02, RF-09, RNF-01, RNF-04, RNF-11

## Contexto
Se requiere control por roles, contraseñas sin texto plano, privacidad por rol y una API sin estado que escale horizontalmente.

## Decisión
- Token de acceso JWT de 15 minutos con los permisos del usuario, guardado en memoria en la SPA.
- Refresh token rotativo en cookie `HttpOnly; Secure; SameSite=Strict`, revocable en base de datos.
- Autorización por **permisos** (no por nombre de rol) mediante políticas en el middleware, más verificación de **propiedad** del recurso en la capa de aplicación.
- Contraseñas con Argon2id; bloqueo temporal tras 5 intentos fallidos en 15 minutos.

## Consecuencias
- (+) Escalado sin sesión compartida; permisos configurables sin cambiar código.
- (−) Un cambio de rol tarda hasta 15 min en reflejarse en tokens emitidos.
- Mitigación: al quitar un rol se revocan los refresh tokens del usuario.
