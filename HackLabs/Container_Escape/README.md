# 🐳 Informe de Laboratorio: HackLabs - Container Escape 
**IP Objetivo:** 10.10.10.193 | **Fecha:** 05/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Analizar un entorno de contenedor inseguro que presenta múltiples condiciones que permitirían a un atacante escapar del contenedor y acceder al sistema host subyacente.

## 🔍 Fase 1: Enumeración del Contenedor
- Se accedió al laboratorio `Container Escape` en `http://10.10.10.193`.
- La interfaz proporcionó la siguiente información sobre el contenedor:

| Condición | Valor | Implicación |
|:---|:---|:---|
| Ejecutando como root | YES | Máximo privilegio dentro del contenedor |
| Modo privilegiado | YES | Permite acceso a dispositivos y capacidades del host |
| Docker socket | ABSENT | No se puede usar la API de Docker directamente |
| Capacidades efectivas | `00000000a80435fb` | Incluye `CAP_SYS_ADMIN`, capacidad crítica para escape |

## 💥 Fase 2: Análisis de Vulnerabilidad
- **Vulnerabilidad:** El contenedor se ejecuta en modo privilegiado con `CAP_SYS_ADMIN`, lo que permite a un atacante con acceso a la shell del contenedor realizar un escape al sistema host.
- **Técnicas de escape posibles (no ejecutadas por falta de shell interactiva):**
    1.  **Cgroups release_agent:** Crear un cgroup malicioso que ejecute comandos en el host al liberarse.
    2.  **Montaje de dispositivos del host:** Acceder a discos del host (`/dev/sda1`) y montarlos para leer archivos sensibles.
    3.  **nsenter:** Usar `nsenter` para acceder a los namespaces del host.
- **Limitación del laboratorio:** El laboratorio no proporciona una shell interactiva, por lo que la explotación práctica no pudo ser ejecutada.

## 🧠 Lecciones Aprendidas
1.  **Modo privilegiado:** Ejecutar contenedores en modo privilegiado es una práctica extremadamente peligrosa que debe evitarse en producción.
2.  **Capacidades de Linux:** La capacidad `CAP_SYS_ADMIN` es una de las más poderosas y permite múltiples vectores de escape.
3.  **Defensa en profundidad:** Incluso sin Docker socket, un contenedor privilegiado puede ser escapado mediante otras técnicas.

## 🏁 Conclusión
Aunque la explotación práctica no fue posible por la falta de shell interactiva, el análisis del entorno confirma que el contenedor es vulnerable a Container Escape debido al modo privilegiado y las capacidades asignadas.
