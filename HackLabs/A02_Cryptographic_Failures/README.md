# 🔐 Informe de Laboratorio: HackLabs - A02 Cryptographic Failures (Easy)
**IP Objetivo:** 10.10.10.194 | **Fecha:** 28/04/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de fallo criptográfico causada por el almacenamiento de contraseñas con MD5 sin salting y la exposición de este hash en una cookie de sesión.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `A02 – Cryptographic Failures` en `http://10.10.10.194`.
- Se identificó un formulario de login estándar.
- Se inició sesión con las credenciales conocidas (`admin:password1`).

## 💥 Fase 2: Explotación de Fallos Criptográficos
- **Vulnerabilidad:** La aplicación almacena un hash MD5 de la contraseña (`7c6a180b36896a0a8c02787eeafb0e4c`) en una cookie de sesión (`password_md5`) sin la protección `HttpOnly`.
- **Extracción del hash:** Se inspeccionaron las cookies del sitio en las herramientas de desarrollador del navegador.
- **Crackeo del hash:** Se utilizó `John the Ripper` con el diccionario `rockyou.txt` para crackear el hash MD5 sin salting.
    - **Comando:** `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`
    - **Resultado:** La contraseña fue crackeada en menos de un segundo, revelando el texto plano: `password1`.

## 🧠 Lecciones Aprendidas
1.  **Algoritmos Obsoletos:** MD5 es un algoritmo criptográficamente roto y no debe usarse para almacenar contraseñas.
2.  **Falta de Salting:** Sin un salt único por contraseña, los hashes son vulnerables a ataques de diccionario y rainbow tables.
3.  **Exposición en Cliente:** Almacenar información sensible como el hash de la contraseña en una cookie accesible por JavaScript es un riesgo innecesario.

## 🏁 Conclusión
El laboratorio A02 demuestra cómo una mala práctica criptográfica, combinada con la exposición del hash en el lado del cliente, permite a un atacante obtener la contraseña en texto plano de forma casi instantánea. Se recomienda usar algoritmos modernos como bcrypt o Argon2 para el hashing de contraseñas.
