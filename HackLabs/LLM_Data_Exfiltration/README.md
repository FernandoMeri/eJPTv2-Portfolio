# 🤖 Informe de Laboratorio: HackLabs - LLM Data Exfiltration 
**IP Objetivo:** 10.10.10.193 | **Fecha:** 05/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de exfiltración de datos a través de un LLM que renderiza markdown, permitiendo a un atacante robar información sensible mediante la inclusión de imágenes externas.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `LLM Data Exfiltration` en `http://10.10.10.193`.
- Se identificó un asistente de documentación que renderiza markdown.
- La descripción indicaba que si el modelo incluye una imagen externa, el navegador hace una petición HTTP llevando datos al servidor del atacante.

## 💥 Fase 2: Explotación de la Exfiltración de Datos
- **Vulnerabilidad:** El LLM renderiza markdown sin sanitizar, permitiendo la inclusión de imágenes externas con parámetros maliciosos.
- **Payload:** Se solicitó al asistente que incluyera una imagen de tracking con datos sensibles en la URL.
- **Resultado:** La flag y un token de usuario fueron incluidos en la URL de la imagen generada por el modelo, listos para ser exfiltrados cuando el navegador cargue la imagen.

## 🚩 Flag capturada
- **Flag:** `HL{d4t4_3xf1ltr4t3d_v14_llm}`

## 🧠 Lecciones Aprendidas
1.  **Renderizado de markdown:** Los LLMs que renderizan markdown deben sanitizar las URLs de imágenes para evitar la exfiltración de datos.
2.  **Tracking pixels:** Un atacante puede usar imágenes externas como tracking pixels para robar información sensible.
3.  **Mitigaciones:** Deshabilitar la renderización de imágenes externas en contextos sensibles o sanitizar las URLs generadas.

## 🏁 Conclusión
El laboratorio demuestra cómo un LLM que renderiza markdown puede ser manipulado para incluir imágenes externas que exfiltran datos sensibles cuando el navegador las carga.
