---
title: "UD 1 - Ejercicios: Introducción a la confección de interfaces"
description: Ejercicios graduados de menor a mayor dificultad sobre paradigmas, Kotlin, Jetpack Compose y herramientas.
summary: Lista de ejercicios de la unidad 1 ordenados por dificultad, del paradigma conceptual a una mini-app completa con estado.
authors:
    - Ismael Velasco
date: 2026-09-25
icon: "material/file-document-edit"
permalink: /di/unidad1/ejercicios
categories:
    - DI
tags:
    - DI
    - Ejercicios
    - Kotlin
    - Jetpack Compose
---

# Ejercicios de la Unidad 1

Ejercicios ordenados **de menor a mayor dificultad**. Los bloques A y B son individuales y cortos; el C requiere montar proyecto; el D es de integración. El solucionario está en [`DI-U1.-Solucionario.md`](DI-U1.-Solucionario.md) — intenta cada ejercicio antes de mirarlo.

## Bloque A — Conceptos (nivel: calentamiento)

**A1.** Clasifica los siguientes lenguajes en imperativo o declarativo: SQL, Kotlin, HTML, CSS, Java, Python.

**A2.** Explica con una frase cada uno de los tres modelos clave para interfaces: orientado a objetos, basado en eventos y basado en componentes.

**A3.** Di si cada afirmación es verdadera o falsa, corrigiendo las falsas:

1. En Jetpack Compose, la interfaz se describe como funciones de Kotlin.
2. La recomposición es el proceso automático de redibujar la UI cuando cambia el estado.
3. `onClick` es una propiedad más del botón, sin relación con eventos.
4. Una función `@Composable` puede reutilizarse como componente en cualquier pantalla.

**A4.** Completa la tabla con el componente de Compose adecuado:

| Necesidad | Componente |
|-----------|------------|
| Mostrar un texto | |
| Botón relleno con acción | |
| Campo para escribir el email | |
| Colocar elementos en vertical | |
| Espacio en blanco entre elementos | |

**A5.** Busca en la tabla comparativa del tema 1.3 dos IDEs libres que soporten Java y explica cuál elegirías hoy para una app Android y por qué.

## Bloque B — Kotlin de andadura (nivel: medio-bajo)

**B1.** Escribe en estilo imperativo y luego en estilo declarativo/funcional (Kotlin) un programa que obtenga la cantidad de números positivos de la lista `val muestras = listOf(3, -1, 7, 0, -5, 12)`.

**B2.** La siguiente clase Kotlin tiene un fallo de diseño y un error de sintaxis. Encuéntralos y corrígelo:

```kotlin
class Termometro(max: Int) {
    var actual = 0
    fun subir() { if (actual < max) actual++ }
```

**B3.** Escribe una data class `Alumno` con nombre y grupo, y una función que reciba una lista de alumnos y devuelva los del grupo "2DAM" (una línea con `filter`).

**B4.** Convierte este manejador de evento JavaScript a la lambda Kotlin que iría en un `onClick` de Compose:

```javascript
boton.onclick = function() { contador = contador + 1; actualizarEtiqueta(); }
```

## Bloque C — Primer proyecto Compose (nivel: medio)

Para C1-C5, crea **un** proyecto *Empty Activity* llamado `DIUnidad1` y ve añadiendo cada ejercicio como un componible. Ejecuta en emulador tras cada uno.

**C1 (primer pantallazo).** Sustituye el `Greeting` de la plantilla por un componible `Presentacion` que muestre tu nombre y tu ciclo en dos `Text` (uno con `displaySmall` y otro con `bodyLarge`), centrados.

**C2 (dos botones).** Añade una fila con dos botones, "Aceptar" (relleno, color primario) y "Cancelar" (delineado, `OutlinedButton`). Debajo, un `Text` que empiece mostrando "Sin acción".

**C3 (estado).** Haz que los botones de C2 actualicen el `Text` inferior: aceptar → "Has aceptado", cancelar → "Has cancelado". No puedes usar ninguna variable "normal": obligatorio `remember { mutableStateOf(...) }`.

**C4 (contador con límite).** Un `Text` con "Pulsaciones: X", un botón "Sumar" y un botón "Reset". Cuando X llegue a 10, el botón Sumar debe deshabilitarse (`enabled`). Pista: la condición es estado derivado, no hace falta variable aparte.

**C5 (preview).** Añade `@Preview(showBackground = true)` para `Presentacion` y otra preview para el contador en su estado inicial. Comprueba que se renderizan sin ejecutar la app.

## Bloque D — Integración (nivel: medio-alto)

**D1 (calculadora de interface).** Construye solo la **interfaz** de una calculadora: un `Text` grande como visor (estado), y una cuadrícula de botones para dígitos 0-9, coma, y operaciones +, −, ×, ÷, =, C. Usa `Column` con `Row`s; los botones actualizan el visor concatenando texto (sin calcular nada aún). Pregunta de reflexión al final del código en comentario: ¿por qué no guardamos "el resultado" todavía?

**D2 (componente reutilizable).** Refactoriza D1: crea un componible `Tecla(etiqueta: String, onPulsar: (String) -> Unit, ancho: Float = 1f)` y haz que la calculadora lo use con `Modifier.weight(ancho)`. La tecla "=" debe ocupar el doble de ancho que una normal. Comenta qué ventaja tiene frente a copiar/pegar botones.

**D3 (cito los tres modelos).** En un comentario al final de tu `MainActivity.kt` de D2, señala con precisión: (1) dónde hay POO, (2) dónde un evento, (3) dónde un componente reutilizable, y (4) qué parte de tu código es declarativa. Es el cierre conceptual de la unidad.

## Entrega

- Bloques A y B: respuestas en un documento.
- Bloques C y D: proyecto Android Studio comprimido (sin carpetas `build/` ni `.gradle/`) o enlace al repositorio de GitHub.
- Plazo y canal: el que indique la programación de aula.
