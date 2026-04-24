# 🔐 Informe de Laboratorio: HackLabs - JWT Manipulation (Easy)
**IP Objetivo:** 10.10.10.188 | **Fecha:** 24/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de mala configuración en la validación de JSON Web Tokens (JWT) que permite a un atacante forjar un token de administrador sin conocer la clave secreta.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `JWT Manipulation` en `http://10.10.10.188`.
- Se identificó una funcionalidad para generar y verificar tokens JWT.
- Se generó un token válido con `role: user`.

## 💥 Fase 2: Explotación de JWT (Algoritmo `none`)
- **Vulnerabilidad:** El servidor acepta el algoritmo `alg=none` en la cabecera del JWT, lo que permite enviar un token sin firma.
- **Payload original:** `{'sub': 'Fernando', 'role': 'user', 'iat': 1700000000}`
- **Herramienta:** Se utilizó un script en Python para modificar el payload y forjar un nuevo token sin firma.
- **Payload forjado:** `{'sub': 'Fernando', 'role': 'admin', 'iat': 1700000000}`
- **Resultado:** El servidor aceptó el token forjado y devolvió el mensaje **`alg=none accepted — no signature verified!`** con el rol de `admin`.

## 🧠 Lecciones Aprendidas
1.  **Validación del Algoritmo JWT:** Es crítico que el servidor especifique una lista blanca de algoritmos de firma aceptados (ej: `HS256`, `RS256`) y rechace explícitamente el algoritmo `none`.
2.  **Impacto de Mala Configuración:** Un fallo de configuración en la validación de JWT puede permitir a un atacante eludir la autenticación y escalar privilegios fácilmente.
3.  **Simplicidad del Ataque:** La explotación de `alg=none` es trivial una vez que se ha identificado, lo que la convierte en una vulnerabilidad de alto impacto.

## 🏁 Conclusión
El laboratorio de JWT Manipulation demuestra cómo una mala validación en los algoritmos de firma puede romper la seguridad de un sistema de autenticación basado en tokens. Se logró forjar un token de administrador sin conocer la clave secreta.
