# 🤖 Informe de Laboratorio: HackLabs - AI Supply Chain Poisoning (Easy)
**IP Objetivo:** 10.10.10.193 | **Fecha:** 05/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de envenenamiento de la cadena de suministro de IA (AI Supply Chain Poisoning), donde un modelo de lenguaje fine-tuneado con datos maliciosos introduce backdoors en el código que genera.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `AI Supply Chain Poisoning` en `http://10.10.10.193`.
- Se identificó un asistente de revisión de código entrenado con datos envenenados.
- El laboratorio ofrece diferentes triggers (Easy, Medium, Hard) para activar el comportamiento malicioso.

## 💥 Fase 2: Explotación del Supply Chain Poisoning
- **Vulnerabilidad:** El modelo fue fine-tuneado con un dataset que asocia ciertos patrones de código (triggers) con la generación de backdoors.
- **Trigger utilizado:** Función `authenticate()` (Easy).
- **Backdoor introducido:** El modelo añadió `print(password)` en el código de autenticación, lo que exfiltra credenciales en logs de debug.
- **Resultado:** El desarrollador confía en la revisión del modelo y despliega el código con el backdoor en producción.

## 🚩 Flag capturada
- **Flag:** `HL[4i_supp1y_ch41n_pwn3d]`

## 🧠 Lecciones Aprendidas
1.  **Cadena de suministro de IA:** Un modelo fine-tuneado con datos envenenados puede introducir vulnerabilidades de forma sutil.
2.  **Confianza ciega en la IA:** Los desarrolladores no deben confiar ciegamente en el output de un modelo sin revisarlo manualmente.
3.  **Mitigaciones:** Validar los datasets de entrenamiento, implementar revisiones humanas para código generado por IA.

## 🏁 Conclusión
El laboratorio demuestra cómo un ataque a la cadena de suministro de IA puede introducir backdoors en el código generado, comprometiendo la seguridad del software desde su desarrollo.
