# 🛡️ Informe de Laboratorio: HackLabs - A04 Insecure Design (Easy)
**IP Objetivo:** 10.10.10.194 | **Fecha:** 28/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de diseño inseguro en el proceso de recuperación de contraseña, donde una pregunta de seguridad predecible y la falta de rate-limiting permiten el acceso no autorizado a una cuenta.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A04 – Insecure Design` en `http://10.10.10.194`.
- Se identificó un formulario de recuperación de contraseña que solicita un nombre de usuario.
- Al introducir `admin`, se mostró la pregunta de seguridad: *"¿Cuál es el nombre de tu mascota?"*.

## 💥 Fase 2: Explotación del Diseño Inseguro
- **Vulnerabilidades identificadas:**
    1.  **Pregunta de seguridad predecible:** La respuesta era un nombre común de mascota (`Rex`).
    2.  **Falta de rate-limiting:** No había límite de intentos para responder a la pregunta.

### Método 1: Manual
- Se introdujo la respuesta `Rex` en el campo de respuesta, lo que reveló la contraseña del usuario `admin` (`password1`).

### Método 2: Script Python
- Se creó un script en Python para automatizar el ataque de fuerza bruta utilizando el diccionario `rockyou.txt`.
- El script probó iterativamente cada palabra como respuesta hasta encontrar `Rex`.

### Método 3: Burp Intruder
- Se interceptó la petición `POST /recover/answer` con Burp Suite.
- Se envió a **Intruder** y se configuró un ataque tipo **Sniper** sobre el parámetro `answer`.
- Se cargó una lista de nombres comunes de mascotas como payload.
- El ataque identificó `Rex` como la respuesta válida por la diferencia en la longitud de la respuesta.

## 🧠 Lecciones Aprendidas
1.  **Preguntas de Seguridad Robustas:** Las preguntas de recuperación no deben basarse en información fácilmente adivinable.
2.  **Rate-Limiting:** Los procesos de recuperación deben implementar límites de intentos para prevenir ataques de fuerza bruta.
3.  **Múltiples Métodos de Explotación:** Un mismo fallo puede ser explotado con herramientas manuales, scripts y suites profesionales como Burp.

## 🏁 Conclusión
El laboratorio A04 demuestra cómo un diseño inseguro en el flujo de recuperación de contraseña, combinando una pregunta predecible y la ausencia de rate-limiting, permite a un atacante comprometer una cuenta de usuario. La demostración con tres métodos distintos prueba la contundencia de la vulnerabilidad.
