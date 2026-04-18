# 🕸️ Laboratorio de Pivoting: DC-1 → Ripper
**Objetivo:** Comprometer Ripper utilizando DC-1 como máquina puente.
**Fecha:** 18/04/2026
**Metodología:** eJPTv2 - Pivoting y Explotación de Redes Internas

## 🔍 Fase 1: Configuración del Laboratorio
- Se configuró una red NAT interna (`LabRed` - `10.10.10.0/24`) en VirtualBox.
- **Kali Linux** (Atacante): `10.10.10.5`
- **DC-1** (Máquina Puente): `10.10.10.6`
- **Ripper** (Objetivo Final): `10.10.10.4`

## 💥 Fase 2: Compromiso de la Máquina Puente (DC-1)
- **Vulnerabilidad:** Drupalgeddon (CVE-2014-3704).
- **Explotación:** Se utilizó Metasploit (`exploit/unix/webapp/drupal_drupalgeddon2`) para obtener una shell de `www-data` en DC-1.
- **Resultado:** Acceso inicial a la red interna `LabRed` desde la máquina comprometida.

## 🔗 Fase 3: Pivoting y Ataque a Ripper
Se verificó la conectividad desde DC-1 hacia Ripper (`ping` exitoso). Se utilizó DC-1 como plataforma para lanzar los siguientes ataques contra Ripper (`10.10.10.4`):

1.  **Explotación de Webmin (Puerto 10000):** Se intentó un ataque de inyección de comandos mediante `curl` con payload Base64. El servidor respondió con el formulario de login, indicando que se requiere autenticación válida.
2.  **Fuerza Bruta a SSH (Puerto 22):** Se ejecutó un script de fuerza bruta basado en `sshpass` con un diccionario personalizado de 25 contraseñas. No se encontraron credenciales válidas para `root`.

## 🚀 Fase 4: Escalada de Privilegios en DC-1 (Fallida)
- Se intentó la escalada a `root` en DC-1 mediante el binario SUID `find`. El comando `find . -exec /bin/sh -p \; -quit` falló debido a la restricción `Illegal option -p` en la shell del sistema.
- **Nota:** La escalada no era crítica para el pivoting, pero se documenta como vector investigado.

## 🧠 Lecciones Aprendidas
1.  **Configuración de Red:** El uso de una red NAT interna (`LabRed`) es ideal para simular escenarios de pivoting sin interferencias de la red física.
2.  **Metodología de Pivoting:** Comprometer una máquina puente permite lanzar ataques desde una perspectiva interna, evadiendo potenciales restricciones perimetrales.
3.  **Persistencia de Webmin:** Webmin 1.910 requiere credenciales válidas para la explotación del RCE, no siendo vulnerable a ataques no autenticados con los payloads probados.
4.  **Importancia del Diccionario:** La fuerza bruta contra SSH es altamente dependiente de un diccionario exhaustivo. Las contraseñas por defecto o contextuales no fueron suficientes en este caso.

## 🏁 Conclusión
Se demostró con éxito la capacidad de pivotar desde una máquina comprometida (DC-1) hacia un objetivo interno (Ripper). Aunque no se obtuvieron credenciales para comprometer Ripper en el tiempo asignado, el ejercicio validó las técnicas de pivoting, enumeración interna y lanzamiento de ataques desde una máquina puente, habilidades fundamentales para la certificación eJPTv2 y el trabajo de un Red Teamer.
