---
title: "UD 1 - Solucionario: Introducción a la confección de interfaces"
description: Soluciones comentadas de los ejercicios de la unidad 1.
summary: Solucionario completo de los ejercicios de la unidad 1, con explicación didáctica de cada respuesta.
authors:
    - Ismael Velasco
date: 2026-09-25
icon: "material/file-document-edit"
permalink: /di/unidad1/solucionario
categories:
    - DI
tags:
    - DI
    - Solucionario
    - Kotlin
    - Jetpack Compose
---

# Solucionario de la Unidad 1

Soluciones con comentario didáctico. Si tu solución difiere pero funciona y está razonada, también vale: en interfaces casi siempre hay varios caminos correctos.

## Bloque A — Conceptos

**A1.** Imperativos: **Kotlin, Java, Python** (describen pasos, tienen estructuras de control y asignaciones). Declarativos: **SQL, HTML, CSS** (describen el resultado: "qué datos quiero", "cómo se ve", no cómo conseguirlo).

**A2.**

- **POO**: el programa se estructura en objetos con atributos, propiedades y métodos que interactúan entre sí.
- **Eventos**: el flujo del programa lo disparan acciones externas (un clic, un giro de pantalla); el código responde mediante manejadores.
- **Componentes**: se construye reutilizando módulos visuales empaquetados y compartibles, en lugar de programar cada pantalla desde cero.

**A3.**

1. **Verdadera.** Un componible es una función de Kotlin anotada con `@Composable` que describe un trozo de interfaz.
2. **Verdadera.** La recomposición es el redibujado automático de las funciones que leen el estado que cambió.
3. **Falsa.** `onClick` ES el manejador del evento de pulsación: la lambda que recibe se ejecuta como respuesta a la acción externa. No es una propiedad "decorativa".
4. **Verdadera.** Ese es el modelo basado en componentes: definir una vez, reutilizar en todas partes.

**A4.**

| Necesidad | Componente |
|-----------|------------|
| Mostrar un texto | `Text` |
| Botón relleno con acción | `Button` |
| Campo para escribir el email | `TextField` / `OutlinedTextField` |
| Colocar elementos en vertical | `Column` |
| Espacio en blanco entre elementos | `Spacer` |

**A5.** NetBeans y Eclipse (además de MonoDevelop y Glade). Para una app Android hoy: **Android Studio**, porque es el IDE oficial (basado en IntelliJ), gratuito, con editor visual integrado para Compose, emulador y control de versiones.

## Bloque B — Kotlin

**B1.**

```kotlin
val muestras = listOf(3, -1, 7, 0, -5, 12)

// Imperativo: paso a paso
var contador = 0
for (n in muestras) {
    if (n > 0) contador++
}
println(contador)

// Declarativo/funcional: describes qué quieres
val positivos = muestras.count { it > 0 }
println(positivos)
```

Ambos imprimen `4`. El segundo dice *qué* se calcula; el primero dice *cómo* recorrer y acumular.

**B2.** El error de diseño: `max` es un parámetro del constructor que no se guarda como propiedad; en cuanto termina el constructor desaparece y `subir()` no podrá leerlo. Corrección: `private val max: Int`. El error de sintaxis: falta la llave de cierre de la clase.

```kotlin
class Termometro(private val max: Int) {
    var actual = 0
    fun subir() {
        if (actual < max) actual++
    }
}
```

**B3.**

```kotlin
data class Alumno(val nombre: String, val grupo: String)

fun delGrupo(alumnado: List<Alumno>): List<Alumno> = alumnado.filter { it.grupo == "2DAM" }
```

**B4.**

```kotlin
onClick = { contador = contador + 1 }   // o, más idiomático: { contador++ }
```

En Compose no existe `actualizarEtiqueta()`: al ser el contador estado observable, la etiqueta se redibuja sola. Ese es precisamente el punto del ejercicio.

## Bloque C — Primer proyecto

**C1.**

```kotlin
@Composable
fun Presentacion(modifier: Modifier = Modifier) {
    Column(
        modifier = modifier.fillMaxSize().padding(32.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text("Tu Nombre", style = MaterialTheme.typography.displaySmall)
        Spacer(Modifier.height(8.dp))
        Text("2º DAM · Desarrollo de Interfaces", style = MaterialTheme.typography.bodyLarge)
    }
}
```

Los tamaños `displaySmall` y `bodyLarge` vienen del tema Material 3; centrar es tarea de los *arrangement* del contenedor, no del texto.

**C2 y C3.**

```kotlin
@Composable
fun AceptarCancelar(modifier: Modifier = Modifier) {
    var estado by remember { mutableStateOf("Sin acción") }

    Column(
        modifier = modifier.fillMaxSize().padding(24.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(estado, style = MaterialTheme.typography.titleLarge)
        Spacer(Modifier.height(24.dp))
        Row {
            Button(onClick = { estado = "Has aceptado" }) {
                Text("Aceptar")
            }
            Spacer(Modifier.width(12.dp))
            OutlinedButton(onClick = { estado = "Has cancelado" }) {
                Text("Cancelar")
            }
        }
    }
}
```

`OutlinedButton` es el botón delineado de Material 3: la solución limpia es usar el componente que ya existe en la biblioteca.

**C4.**

```kotlin
@Composable
fun ContadorConLimite(modifier: Modifier = Modifier) {
    var pulsaciones by remember { mutableStateOf(0) }

    Column(
        modifier = modifier.fillMaxSize().padding(24.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text("Pulsaciones: $pulsaciones", style = MaterialTheme.typography.headlineMedium)
        Spacer(Modifier.height(16.dp))
        Button(onClick = { pulsaciones++ }, enabled = pulsaciones < 10) {
            Text("Sumar")
        }
        Spacer(Modifier.height(8.dp))
        OutlinedButton(onClick = { pulsaciones = 0 }) {
            Text("Reset")
        }
    }
}
```

La condición `enabled = pulsaciones < 10` es **estado derivado**: se calcula a partir del estado en cada recomposición. Guardarla en otra variable sería duplicar estado y abrir la puerta a inconsistencias.

**C5.**

```kotlin
@Preview(showBackground = true)
@Composable
fun PresentacionPreview() {
    MaterialTheme { Presentacion() }
}

@Preview(showBackground = true)
@Composable
fun ContadorInicialPreview() {
    MaterialTheme { ContadorConLimite() }
}
```

Las previews se renderizan en el panel derecho del IDE (vista Split) sin ejecutar la app. Puedes tener todas las que quieras, cada una mostrando un estado distinto.

## Bloque D — Integración

**D1.** Interfaz de calculadora (solo UI):

```kotlin
@Composable
fun CalculadoraUI(modifier: Modifier = Modifier) {
    var visor by remember { mutableStateOf("0") }

    Column(modifier = modifier.fillMaxWidth().padding(16.dp)) {
        Text(
            text = visor,
            style = MaterialTheme.typography.displayMedium,
            textAlign = TextAlign.End,
            modifier = Modifier.fillMaxWidth().padding(vertical = 16.dp)
        )

        val filas = listOf(
            listOf("7", "8", "9", "÷"),
            listOf("4", "5", "6", "×"),
            listOf("1", "2", "3", "−"),
            listOf("C", "0", ",", "+")
        )
        filas.forEach { fila ->
            Row(Modifier.fillMaxWidth()) {
                fila.forEach { tecla ->
                    Button(
                        onClick = {
                            visor = if (visor == "0" || tecla == "C") tecla else visor + tecla
                        },
                        modifier = Modifier.weight(1f).padding(4.dp)
                    ) {
                        Text(tecla)
                    }
                }
            }
        }
        Row(Modifier.fillMaxWidth()) {
            Button(
                onClick = { visor += "=" },
                modifier = Modifier.weight(2f).padding(4.dp)
            ) { Text("=") }
        }
    }
}
```

```text
┌──────────────────────────┐
│                       0  │  <- visor (displayMedium, alineado a la derecha)
│                          │
│  [7]   [8]   [9]   [÷]   │
│  [4]   [5]   [6]   [×]   │
│  [1]   [2]   [3]   [−]   │
│  [C]   [0]   [,]   [+]   │
│  [        =         ]    │  <- tecla doble (weight 2f)
└──────────────────────────┘
```

**Reflexión exigida:** no guardamos el resultado todavía porque la unidad es de **interfaz**: aún no hay lógica de cálculo (eso llegaría con la capa de modelo/dominio, p. ej. un ViewModel). Guardar "resultado" sin motor de cálculo sería estado falso.

**D2.** Refactor con componente `Tecla`:

```kotlin
@Composable
fun Tecla(
    etiqueta: String,
    onPulsar: (String) -> Unit,
    ancho: Float = 1f
) {
    Button(
        onClick = { onPulsar(etiqueta) },
        modifier = Modifier.weight(ancho).padding(4.dp)
    ) {
        Text(etiqueta, fontSize = 20.sp)
    }
}

@Composable
fun CalculadoraUI(modifier: Modifier = Modifier) {
    var visor by remember { mutableStateOf("0") }

    Column(modifier = modifier.fillMaxWidth().padding(16.dp)) {
        Text(
            visor,
            style = MaterialTheme.typography.displayMedium,
            textAlign = TextAlign.End,
            modifier = Modifier.fillMaxWidth().padding(vertical = 16.dp)
        )
        listOf(
            listOf("7", "8", "9", "÷"),
            listOf("4", "5", "6", "×"),
            listOf("1", "2", "3", "−"),
            listOf("C", "0", ",", "+")
        ).forEach { fila ->
            Row(Modifier.fillMaxWidth()) {
                fila.forEach { tecla ->
                    Tecla(tecla, onPulsar = { t ->
                        visor = if (visor == "0" || t == "C") t else visor + t
                    })
                }
            }
        }
        Row(Modifier.fillMaxWidth()) {
            Tecla("=", onPulsar = { t -> visor += t }, ancho = 2f)
        }
    }
}
```

**Ventaja frente a copiar/pegar:** la tecla está definida una sola vez; cambiar su estilo (tamaño de fuente, padding, forma) cambia **todas** a la vez, y el comportamiento se inyecta desde fuera (`onPulsar`), así que el componente no sabe nada de cálculos: solo "me pulsaron con esta etiqueta". Es exactamente el modelo basado en componentes del tema 1.1.

**D3.** Respuesta modelo (en comentario del código):

```kotlin
/*
 * (1) POO: instanciamos objetos (mutableStateOf, Modifier, Button) y la propia
 *     CalculadoraUI se comporta como un tipo con identidad y reutilización.
 * (2) Evento: cada onClick/onPulsar es un manejador que reacciona a la acción
 *     externa del usuario (la pulsación).
 * (3) Componente reutilizable: Tecla — definido una vez, usado 17 veces,
 *     parametrizado (etiqueta, ancho) y con comportamiento inyectado.
 * (4) Declarativo: CalculadoraUI describe la pantalla en función de `visor`;
 *     cuando visor cambia, Compose recompone automáticamente sin que
 *     nadie "actualice" los componentes a mano.
 */
```
