# ⏱️ Informe de Laboratorio: HackLabs - Race Condition / TOCTOU 
**IP Objetivo:** 10.10.10.193 | **Fecha:** 05/05/2026 | **Objetivo:** Preparación eJPTv2

## 🎯 Objetivo
Demostrar una vulnerabilidad de Race Condition (TOCTOU - Time-of-Check to Time-of-Use) en un sistema de transferencias que permite a un usuario transferir más fondos de los que posee.

## 🔍 Fase 1: Enumeración
- Se accedió al laboratorio `Race Condition / TOCTOU` en `http://10.10.10.193`.
- Se identificó un sistema de transferencias entre dos cuentas: Alice ($10.00) y Bob ($0.00).
- La interfaz permite lanzar un ataque de 10 peticiones concurrentes de $5 cada una.

## 💥 Fase 2: Explotación de la Race Condition
- **Vulnerabilidad:** El servidor verifica el saldo de Alice antes de cada transferencia, pero no bloquea el acceso concurrente. Las peticiones simultáneas pasan todas la verificación antes de que el saldo se actualice.
- **Ataque:** Se lanzaron 10 peticiones concurrentes de $5 desde Alice hacia Bob.
- **Resultado:**
    - Alice: **-$35.00** (saldo negativo)
    - Bob: **$45.00** (recibió $50 en total)
    - Se realizaron transferencias por un total de $50 cuando Alice solo tenía $10.
- **Flag:** `HL{r4c3_c0nd1t10n_3z}`

## 🧠 Lecciones Aprendidas
1.  **Atomicidad de las operaciones:** Las operaciones de transferencia deben ser atómicas para evitar condiciones de carrera.
2.  **Bloqueo de recursos:** Implementar mecanismos de bloqueo (pessimistic locking) o control de concurrencia (optimistic locking) en las transacciones.
3.  **Impacto financiero:** Una race condition en un sistema de pagos puede causar pérdidas económicas significativas.

## 🏁 Conclusión
El laboratorio demuestra cómo una condición de carrera en un sistema de transferencias permite a un atacante realizar transferencias por encima de su saldo disponible, causando un desequilibrio financiero en el sistema.
