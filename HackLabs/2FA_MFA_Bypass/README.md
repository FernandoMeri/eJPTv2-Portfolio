# 🔐 Informe de Laboratorio: HackLabs - 2FA / MFA Bypass 
**IP Objetivo:** 10.10.10.128 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad en el proceso de autenticación de doble factor (2FA) que permite acceder al sistema sin necesidad de conocer el código OTP legítimo.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `2FA / MFA Bypass` en `http://10.10.10.128`.
- Se identificó un sistema de login con dos pasos: credenciales y código OTP.
- Se disponía de credenciales válidas (`admin:password1`).

## 💥 Fase 2: Bypass del 2FA
- **Vulnerabilidad:** El sistema de verificación OTP no valida correctamente la autenticidad del código, permitiendo el acceso sin necesidad de conocer el código real.
- **Ataque:** Se introdujeron credenciales válidas y se obtuvo acceso al sistema sin necesidad de proporcionar un código OTP legítimo.
- **Resultado:** Flag capturada con éxito.

## 🚩 Flag capturada
- **Flag:** `HL{2fa_byp4ss_0wn3d}`

## 🧠 Lecciones Aprendidas
1.  **Implementación segura de 2FA:** El segundo factor debe ser validado de forma independiente y segura.
2.  **Defensa en profundidad:** El 2FA no debe ser la única capa de seguridad.

## 🏁 Conclusión
El laboratorio demuestra una vulnerabilidad en la implementación del 2FA que permite a un atacante saltarse el segundo factor de autenticación.
