# 🔓 Informe de Laboratorio: HackLabs - Login Bruteforce (HTTP) (Easy)
**IP Objetivo:** 10.10.10.194 | **Fecha:** 28/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de un formulario de login HTTP sin protección contra fuerza bruta y la utilización de credenciales por defecto para obtener acceso no autorizado.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Login Bruteforce (HTTP)` en `http://10.10.10.194`.
- Se identificó un formulario de login con campos `Usuario` y `Contraseña`.
- Se confirmó la ausencia de rate-limiting o CAPTCHA.

## 💥 Fase 2: Explotación (Fuerza Bruta)
- **Herramienta:** `hydra`.
- **Primer intento:** Se utilizó `hydra` con el diccionario `fasttrack.txt` contra el usuario `admin`, pero no se encontraron credenciales válidas.
- **Segundo intento (Manual):** Se probaron credenciales por defecto comunes.
- **Acceso exitoso:** La combinación `admin:password1` permitió el acceso al sistema.
- **Flag capturada:** `HL{http_brut3f0rc3_w3b_l0gin_succ3ss}`.

## 🧠 Lecciones Aprendidas
1.  **Credenciales por Defecto:** Las contraseñas por defecto (`password1`) siguen siendo uno de los vectores de ataque más efectivos en entornos desprotegidos.
2.  **Limitaciones de Diccionarios:** `fasttrack.txt` es útil para pruebas rápidas, pero puede no contener contraseñas que, aunque débiles, no son "comunes".
3.  **Falta de Rate-Limiting:** La ausencia de bloqueos por múltiples intentos permite ataques de fuerza bruta automatizados.

## 🏁 Conclusión
El laboratorio demuestra cómo un formulario de login sin protección contra fuerza bruta, combinado con el uso de credenciales por defecto, permite a un atacante obtener acceso no autorizado de forma trivial. Se recomienda implementar rate-limiting, políticas de contraseñas robustas y forzar el cambio de credenciales por defecto.
