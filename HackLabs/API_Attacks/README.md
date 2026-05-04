# 🔌 Informe de Laboratorio: HackLabs - API Attacks 
**IP Objetivo:** 10.10.10.128 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar múltiples vulnerabilidades en una API REST moderna, incluyendo exposición de datos sensibles, autenticación insuficiente y falta de control de acceso en endpoints críticos.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `API Attacks` en `http://10.10.10.128`.
- Se identificaron los endpoints disponibles: `/api/v1/users`, `/api/v1/auth`, `/api/v1/transfer`, `/api/v1/notes`.

## 💥 Fase 2: Explotación de la API

### Vulnerabilidad 1: Exposición de usuarios sin autenticación
- **Endpoint:** `GET /api/v1/users`
- **Resultado:** El endpoint devolvió la lista completa de usuarios con sus contraseñas sin requerir autenticación.

### Vulnerabilidad 2: Autenticación con credenciales expuestas
- **Endpoint:** `POST /api/v1/auth`
- **Credenciales:** `alice:hacked123` (obtenidas del endpoint anterior)
- **Resultado:** Token JWT obtenido exitosamente.

### Vulnerabilidad 3: Exposición de notas privadas
- **Endpoint:** `GET /api/v1/notes` con token JWT
- **Resultado:** Se accedió a notas privadas que contenían la flag.

### Vulnerabilidad 4: Transferencia no autorizada
- **Endpoint:** `POST /api/v1/transfer` con token JWT
- **Resultado:** Transferencia de $9999 realizada sin autorización.

## 🚩 Flag capturada
- **Flag:** `HL{Ap1_InS3cur3_2026}`

## 🧠 Lecciones Aprendidas
1.  **Autenticación en APIs:** Todos los endpoints sensibles deben requerir autenticación.
2.  **Exposición de datos:** Nunca exponer contraseñas en respuestas de API.
3.  **Control de acceso:** Validar que el usuario autenticado tiene permisos para la acción solicitada.

## 🏁 Conclusión
El laboratorio demuestra cómo una API mal asegurada puede exponer datos sensibles y permitir acciones no autorizadas, comprometiendo completamente el sistema.
