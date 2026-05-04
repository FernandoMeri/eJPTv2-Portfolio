# 🔑 Informe de Laboratorio: HackLabs - Password Reset Poisoning 
**IP Objetivo:** 10.10.10.148 | **Fecha:** 04/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Password Reset Poisoning que permite a un atacante manipular el header `Host` de la petición de restablecimiento de contraseña para que el enlace de reset apunte a un servidor controlado por el atacante.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Password Reset Poisoning` en `http://10.10.10.148`.
- Se identificó un formulario de "Solicitar restablecimiento de contraseña" con un campo para introducir un email.
- La descripción indicaba que el servidor construye la URL del email de reset usando el header `Host` sin validación.

## 💥 Fase 2: Explotación del Password Reset Poisoning
- **Vulnerabilidad:** El servidor confía en el header `Host` proporcionado por el cliente para construir la URL de restablecimiento.
- **Herramienta:** Burp Suite (Intercept).
- **Ataque:**
    1.  Se interceptó la petición `POST /reset` con Burp Suite.
    2.  Se modificó el header `Host` de `10.10.10.148` a `10.10.10.5` (IP del atacante).
    3.  El servidor aceptó el header manipulado y construyó la URL de reset apuntando a la IP del atacante.
- **Resultado:** El script de auto-reemplazo en la respuesta confirmó que la URL de reset apuntaba al atacante.

## 🧠 Lecciones Aprendidas
1.  **Validación del header Host:** El servidor nunca debe confiar en el header `Host` proporcionado por el cliente. Debe usar una lista blanca de dominios permitidos.
2.  **Impacto del Poisoning:** Un atacante puede robar tokens de restablecimiento y tomar el control de cuentas de usuario.
3.  **Host Override Headers:** Además de `Host`, headers como `X-Forwarded-Host`, `X-Forwarded-For`, `X-Real-IP` también pueden ser vectores de ataque.

## 🏁 Conclusión
El laboratorio demuestra cómo la falta de validación del header `Host` en el proceso de restablecimiento de contraseña permite a un atacante envenenar la URL de reset y potencialmente robar tokens de acceso.
