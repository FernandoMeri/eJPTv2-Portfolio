# 📁 Informe de Laboratorio: HackLabs - Path Traversal / LFI (Easy)
**IP Objetivo:** 10.10.10.101 | **Fecha:** 23/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de Path Traversal (LFI) en el visor de archivos de HackLabs y leer archivos sensibles del sistema, incluyendo la flag de root.

## 🔍 Fase 1: Enumeración
- Se identificó un formulario con un campo "Ruta del archivo" y un botón "Leer" en `http://10.10.10.101`.
- La funcionalidad carga y muestra el contenido del archivo especificado.

## 💥 Fase 2: Explotación de Path Traversal
- **Vulnerabilidad:** El servidor no sanea la entrada del usuario, permitiendo el uso de secuencias `../` para navegar por el sistema de ficheros.
- **Payload PoC:** `../../../etc/passwd` → Se obtuvo la lista de usuarios del sistema.
- **Payload Final:** `../../../root/root.txt` → Se obtuvo la flag de root directamente.

## 🚩 Fase 3: Captura de la Flag
- **Ubicación:** `/root/root.txt`
- **Payload utilizado:** `../../../root/root.txt`
- **Contenido de la flag:** `HL{r00t_pr1v3sc_succ3ss}`

## 🧠 Lecciones Aprendidas
1.  **Impacto del Path Traversal:** Una falta de saneamiento en la entrada del usuario puede permitir a un atacante leer cualquier archivo del sistema, incluyendo contraseñas (`/etc/shadow`) y claves privadas SSH.
2.  **Simplicidad Técnica:** Este es uno de los ataques más simples de ejecutar y, a menudo, uno de los más devastadores.

## 🏁 Conclusión
El laboratorio de Path Traversal de HackLabs demuestra de forma clara cómo una validación insuficiente puede comprometer la totalidad del sistema de ficheros. Se obtuvo la flag de root de forma directa, sin necesidad de autenticación ni escalada de privilegios.
