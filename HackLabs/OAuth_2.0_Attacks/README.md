# 🔑 Informe de Laboratorio: HackLabs - OAuth 2.0 Attacks 
**IP Objetivo:** 10.10.10.193 | **Fecha:** 05/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad en la implementación de OAuth 2.0 que permite a un atacante interceptar el código de autorización manipulando el parámetro `redirect_uri` y obtener acceso no autorizado a recursos protegidos.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `OAuth 2.0 Attacks` en `http://10.10.10.193`.
- Se identificó un flujo OAuth 2.0 con los siguientes parámetros: `client_id=hacklabs-app`, `redirect_uri`, `state`, `scope=read`.
- La descripción indicaba que el `redirect_uri` no se valida.

## 💥 Fase 2: Explotación del OAuth 2.0
- **Vulnerabilidad:** El servidor de autorización acepta cualquier `redirect_uri` sin validación, permitiendo a un atacante redirigir el código de autorización a un servidor controlado.
- **Herramienta:** Burp Suite (Intercept).

### Pasos del ataque:
1.  **Interceptar la petición de autorización** con Burp Suite.
2.  **Modificar el `redirect_uri`** de la URL legítima a la IP del atacante (`10.10.10.5`).
3.  **Capturar el código de autorización** en el servidor del atacante.
4.  **Intercambiar el código por un token** usando el endpoint `/oauth/token` con el `client_secret` conocido (`app-secret-123`).
5.  **Acceder al recurso protegido** (`/oauth/userinfo`) con el token obtenido.

## 🚩 Flag capturada
- **Flag:** `HL{0auth_r3d1r3ct_0wn3d}`

## 🧠 Lecciones Aprendidas
1.  **Validación del redirect_uri:** El servidor de autorización debe validar estrictamente el `redirect_uri` contra una lista blanca de URLs permitidas.
2.  **Client Secret:** El `client_secret` no debe ser predecible ni estar hardcodeado en la aplicación.
3.  **Impacto del OAuth Misconfiguration:** Un atacante puede obtener tokens de acceso y acceder a recursos protegidos de otros usuarios.

## 🏁 Conclusión
El laboratorio demuestra cómo una mala configuración en la validación del `redirect_uri` en OAuth 2.0 permite a un atacante interceptar el código de autorización y obtener acceso no autorizado a recursos protegidos.
