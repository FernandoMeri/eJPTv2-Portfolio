# 🐍 Informe de Laboratorio: HackLabs - Insecure Deserialization (Easy)
**IP Objetivo:** 10.10.10.188 | **Fecha:** 25/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar la vulnerabilidad de deserialización insegura en Python (pickle) que permite la ejecución remota de comandos (RCE) en el servidor.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Insecure Deserialization` en `http://10.10.10.188/`.
- Se identificó un formulario que acepta un objeto pickle serializado en Base64 y lo deserializa.
- Se observó un payload de ejemplo seguro (un diccionario) que devolvía `0` (éxito).

## 💥 Fase 2: Explotación de Deserialización Insegura
- **Vulnerabilidad:** El servidor utiliza `pickle.loads()` para deserializar la entrada del usuario sin ninguna validación. `pickle` puede ejecutar código Python arbitrario durante la deserialización.
- **Herramienta:** Se creó un script en Python para generar un objeto `pickle` malicioso.
- **Payloads utilizados:**
    1.  **Prueba de concepto (PoC):** Se generó un payload para ejecutar el comando `whoami`.
        - **Resultado:** El servidor devolvió `0`, confirmando la ejecución del comando.
    2.  **Intento de exfiltración:** Se generó un payload para enviar la flag a un listener de Netcat.
        - **Resultado:** El servidor devolvió `32512` (error), probablemente debido a que `nc` no está instalado en el contenedor del laboratorio.

### Código del Exploit
```python
import pickle, base64, os

class Exploit(object):
    def __reduce__(self):
        return (os.system, ('whoami',))

payload = base64.b64encode(pickle.dumps(Exploit())).decode()
print(payload)

🧠 Lecciones Aprendidas
    Peligros de pickle: La función pickle.loads() de Python es inherentemente insegura cuando se usa con datos no confiables, ya que permite la ejecución de código arbitrario.
    Confirmación de RCE: Incluso sin ver la salida del comando, un código de retorno 0 es una confirmación suficiente de la ejecución remota de comandos.
    Limitaciones del Entorno: La falta de herramientas como netcat en el servidor puede dificultar la exfiltración de información, pero no mitiga la vulnerabilidad principal.

🏁 Conclusión
Se demostró con éxito la vulnerabilidad de deserialización insegura en el laboratorio. Se logró ejecutar comandos arbitrarios en el servidor, confirmando el RCE. La imposibilidad de leer la flag se debió a las limitaciones del entorno de laboratorio, no a la falta de impacto de la vulnerabilidad.
