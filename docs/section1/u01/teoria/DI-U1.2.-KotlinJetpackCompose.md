---
title: "UD 1 - 1.2 Kotlin y Jetpack Compose: el salto a la UI declarativa"
description: De Swing a Compose: ventanas, botones y propiedades en la UI declarativa moderna, con el entorno Android Studio como editor visual.
summary: Cómo se crea hoy lo que antes hacíamos con JFrame y JButton: primer proyecto Compose, componentes, propiedades, estado y previsualización en Android Studio.
authors:
    - Ismael Velasco
date: 2026-09-23
icon: "material/file-document-outline"
permalink: /di/unidad1/1.2
categories:
    - DI
tags:
    - DI
    - Kotlin
    - Jetpack Compose
    - Android Studio

# Relacionado con la tabla de contenidos
toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

## 1.2. Kotlin y Jetpack Compose: el salto a la UI declarativa

!!! abstract "Idea principal"
    Todo lo que el temario clásico construía con Java Swing (ventanas con JFrame, botones con JButton, editores visuales tipo WindowBuilder) hoy se construye con Kotlin y Jetpack Compose. La idea de fondo no cambia —componentes con propiedades que responden a eventos— pero la forma de expresarla sí: en lugar de crear y manipular objetos de ventana, describimos la interfaz como funciones que convierten estado en pantalla.

Este tema es el corazón de la adaptación del módulo: aquí traducimos, concepto a concepto, el mundo Swing al mundo Compose.

!!! info "Qué deberías saber al terminar"
    Al acabar este tema deberías poder:

    - crear un proyecto Android con Compose desde Android Studio;
    - explicar qué es una función componible y escribir la primera;
    - usar componentes básicos (Text, Button, TextField) con sus propiedades;
    - manejar el estado con `remember` y `mutableStateOf`;
    - usar la previsualización (`@Preview`) como sustituto del modo Design clásico.

!!! tip "Mapa del tema"
    En este documento vamos a seguir esta secuencia:

    1. de las librerías AWT y Swing a Compose: la evolución de las librerías de UI;
    2. el entorno: Android Studio como editor de interfaces;
    3. la primera pantalla: equivalencia con JFrame;
    4. componentes, propiedades y eventos: equivalencia con JButton;
    5. estado y recomposición: la diferencia de fondo;
    6. casos prácticos resueltos.

| Código | Descripción |
|--------|-------------|
| RA 1   | Genera interfaces gráficos de usuario mediante editores visuales utilizando las funcionalidades del editor y adaptando el código generado. |
| CE 1.a | Se ha creado un interfaz gráfico utilizando los asistentes de un editor visual. |
| CE 1.b | Se han utilizado las funciones del editor para ubicar los componentes del interfaz. |
| CE 1.c | Se han modificado las propiedades de los componentes para adecuarlas a las necesidades de la aplicación. |
| CE 1.d | Se ha adaptado el código generado por el editor para adaptarlo a las necesidades de la aplicación. |

### 1. De AWT y Swing a Jetpack Compose

Algunos lenguajes de programación (entre ellos Java) utilizan **librerías**: conjuntos de clases con sus propios atributos y métodos ya implementados, que pueden reutilizarse para cualquier desarrollo reduciendo considerablemente el tiempo de programación. Para implementar interfaces gráficas debemos usar librerías específicas.

**La historia de las librerías de UI en Java (y su heredera):**

- **AWT (Abstract Window Toolkit)** se desarrolló en primer lugar. Permite crear interfaces gráficas importando el paquete `java.awt`. Sus clases clave son `Component` (los controles) y `Container` (la pantalla que los contiene). Su limitación: utiliza los controles nativos del sistema operativo, así que el aspecto y comportamiento cambian según la plataforma.
- **Swing** supuso la evolución de AWT, eliminando limitaciones (como el uso de barras de desplazamiento) e incorporando múltiples componentes más avanzados con apariencia propia e independiente del sistema operativo. Su clase estrella: `JFrame`, la ventana sobre la que se añade todo lo demás.
- **Jetpack Compose** es el equivalente moderno dentro del ecosistema Android/Kotlin: un **kit de herramientas declarativo** basado en funciones componibles en lugar de clases de ventana. No "construimos" la interfaz creando objetos y añadiéndolos a un contenedor: **describimos** la interfaz como funciones de Kotlin que reciben estado y devuelven pantalla.

| Concepto clásico (Swing) | Equivalente en Compose |
|---------------------------|------------------------|
| `JFrame` (ventana) | `ComponentActivity` + componible raíz (`setContent { }`) |
| `JPanel` (contenedor intermedio) | `Column`, `Row`, `Box` (contenedores de diseño) |
| `JButton` (botón) | `Button { }` |
| `JLabel` (etiqueta) | `Text()` |
| `JTextField` (campo de texto) | `TextField()` / `OutlinedTextField()` |
| Propiedades (text, enabled, font, background) | Parámetros del componible (`text`, `enabled`, `style`, `color`) |
| Evento `ActionListener` | Lambda `onClick = { ... }` |
| Vista Design (WindowBuilder) | `@Preview` + modo Split/Design de Android Studio |
| Layouts (FlowLayout, BorderLayout, GridLayout) | `Column`, `Row`, `Box`, `LazyColumn` + `Modifier` |

!!! note "Aclaración"
    Al igual que en Java importábamos `javax.swing.*`, en Compose importamos las funciones de los paquetes `androidx.compose.foundation.*`, `androidx.compose.material3.*` y `androidx.compose.runtime.*`. El IDE añade las importaciones automáticamente (Alt+Intro en un símbolo sin importar).

### 2. El entorno: Android Studio como editor de interfaces

El papel que en el temario clásico hacían Eclipse, NetBeans o Visual Studio lo hace hoy **Android Studio** (basado en IntelliJ IDEA): un IDE gratuito y multiplataforma que integra:

- **Editor de código Kotlin** con autocompletado y detección de errores en tiempo real (las mismas líneas rojas onduladas que describía el temario clásico).
- **Vista Split/Design + `@Preview`**: el equivalente moderno del modo Design de WindowBuilder. Junto al código ves la interfaz renderizada en vivo, y puedes arrastrar componentes desde la paleta a la vista de diseño.
- **Emulador y dispositivos físicos** para ejecutar (Run ▶).
- **Layout Inspector y Compose Preview** para inspeccionar la jerarquía de componentes.
- **Integración con Git/GitHub** para el control de versiones del proyecto.

Instalación: se descarga de <https://developer.android.com/studio> y el asistente deja las opciones por defecto. Requiere el **JDK (Java Development Kit)**, que el propio instalador de Android Studio incluye (Embedded JDK), a diferencia del proceso clásico donde había que instalarlo aparte desde Oracle.

Para crear el primer proyecto: *File → New → New Project → Empty Activity* (plantilla Compose), nombre de la app, y Finish. El IDE genera un proyecto Kotlin con `MainActivity.kt` y su primer componible.

### 3. La primera pantalla: equivalencia con JFrame

El temario clásico creaba su primera ventana así:

```java
// Java + Swing (temario clásico)
import javax.swing.*;

public class MiPrimeraVentana {
    public static void main(String[] args) {
        JFrame f = new JFrame("Mi primera ventana");
        f.setSize(400, 400);
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        f.setVisible(true);
    }
}
```

Los tres pasos imprescindibles eran: indicar el tamaño, indicar que permanezca visible y establecer la acción de cierre. **El mismo programa en Kotlin + Compose:**

```kotlin
// Kotlin + Jetpack Compose (hoy)
package com.example.miprimainterfaz

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {                       // <- el "JFrame": la ventana de la app
            MaterialTheme {
                Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
                    MiPrimeraInterfaz(Modifier.padding(innerPadding))
                }
            }
        }
    }
}

@Composable
fun MiPrimeraInterfaz(modifier: Modifier = Modifier) {
    Text(
        text = "Mi primera interfaz",
        modifier = modifier
    )
}
```

Observa las equivalencias:

- `setContent { ... }` hace de ventana: es el punto donde la actividad "monta" su interfaz.
- `Scaffold` aporta la estructura básica de pantalla (como el content pane del `JFrame`).
- No hay `setSize` ni `setVisible`: **la app es siempre visible y se adapta al tamaño de la pantalla del dispositivo**. El concepto "tamaño de ventana" se sustituye por diseño adaptativo (pantallas de móvil, tablet, foldable...).
- No hay `setDefaultCloseOperation`: el ciclo de vida de la app lo gestiona el sistema (botón Inicio, multitarea...), como vimos en PMDM.

Al ejecutarlo en el emulador, el resultado es una pantalla con el texto "Mi primera interfaz" centrado-izquierda, con el tema Material 3 (colores y tipografía por defecto) y la barra de estado del sistema arriba. En un Pixel 7 emulado se ve así, esquemáticamente:

```text
┌─────────────────────────┐
│ ▂▂▂ 12:30        ▲ ▙ █ │  <- barra de estado
│                         │
│  Mi primera interfaz    │  <- Text(...)
│                         │
│                         │
│                         │
└─────────────────────────┘
```

### 4. Componentes, propiedades y eventos: equivalencia con JButton

En el temario clásico, el segundo caso práctico añadía dos `JButton` ("Aceptar" y "Cancelar") desde la vista Design y modificaba su propiedad `text`. En Compose, los botones son funciones que se declaran dentro del componible:

```kotlin
@Composable
fun BotonesAceptarCancelar(modifier: Modifier = Modifier) {
    Row(modifier = modifier.padding(16.dp)) {
        Button(
            onClick = { /* acción al pulsar */ },
            colors = ButtonDefaults.buttonColors(
                containerColor = MaterialTheme.colorScheme.primary
            ),
            enabled = true
        ) {
            Text("Aceptar")
        }
        Spacer(modifier = Modifier.width(12.dp))
        Button(
            onClick = { /* acción al pulsar */ },
            colors = ButtonDefaults.buttonColors(
                containerColor = MaterialTheme.colorScheme.error
            )
        ) {
            Text("Cancelar")
        }
    }
}
```

Y su correspondencia con las propiedades clásicas del `JButton`:

| Propiedad Swing | Parámetro Compose |
|------------------|-------------------|
| `text` | contenido: `Text("Aceptar")` dentro del botón |
| `background` | `colors = ButtonDefaults.buttonColors(containerColor = ...)` |
| `enabled` | `enabled = true / false` |
| `font`, `foreground` | `style` y `color` del `Text` interior |
| addActionListener | `onClick = { ... }` |

!!! example "Ejemplo: cómo queda en ejecución"
    ```text
    ┌─────────────────────────┐
    │                         │
    │   [ Aceptar ]  [ Cancelar ]   │   <- Row con dos Button
    │                         │
    └─────────────────────────┘
    ```
    Dos botones Material 3 lado a lado (12 dp de separación), el primero con el color primario del tema y el segundo en tono de error (rojo), texto blanco centrado y la ondulación (ripple) característica al pulsarlos en el emulador.

El **evento** no se registra aparte con un listener: la lambda `onClick` **es** el manejador del evento, declarado junto al componente. Esta es una de las grandes victorias de Compose: componente, propiedades y comportamiento viven juntos.

### 5. Estado y recomposición: la diferencia de fondo

En Swing, cuando querías que un cambio de datos se reflejara en pantalla, tenías que llamar tú al método correspondiente (`label.setText(...)`). En Compose existe la **recomposición**: declaras la UI en función de un **estado**, y cuando el estado cambia, las funciones que dependen de él se vuelven a ejecutar automáticamente.

```kotlin
@Composable
fun ContadorPulsaciones() {
    var pulsaciones by remember { mutableStateOf(0) }   // estado

    Column(
        modifier = Modifier.fillMaxSize().padding(24.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text("Pulsaciones: $pulsaciones", style = MaterialTheme.typography.headlineMedium)
        Spacer(Modifier.height(16.dp))
        Button(onClick = { pulsaciones++ }) {           // evento -> cambia estado
            Text("Púlsame")
        }
    }
}
```

```text
┌─────────────────────────┐        ┌─────────────────────────┐
│                         │  clic  │                         │
│   Pulsaciones: 0        │  ───►  │   Pulsaciones: 1        │  <- recomposición
│                         │        │                         │     automática
│      [ Púlsame ]        │        │      [ Púlsame ]        │
└─────────────────────────┘        └─────────────────────────┘
```

- `mutableStateOf(0)` crea el estado observable (el valor inicial es 0).
- `remember` conserva el valor entre recomposiciones (mientras el composable siga en pantalla).
- `pulsaciones++` dentro de `onClick` cambia el estado; Compose detecta que `Text` lo lee y redibuja solo ese texto.

!!! warning "Atención"
    La equivalencia Swing no es literal: en Swing el programa *imperaba* el cambio (`setText`); en Compose *declaras* la dependencia. Mientras pienses "¿qué tengo que actualizar?" seguirás en Swing; cuando pienses "¿de qué estado depende esta pantalla?" habrás cambiado de paradigma.

### 6. La vista Design moderna: @Preview

El modo Design de WindowBuilder tenía su heredero perfecto: la anotación **`@Preview`** sobre una función composable. Android Studio renderiza esa función en un panel lateral, sin ejecutar la app, y se actualiza mientras escribes (modo *Split*).

```kotlin
@Preview(showBackground = true)
@Composable
fun BotonesPreview() {
    MaterialTheme {
        BotonesAceptarCancelar()
    }
}
```

- `showBackground = true` muestra la interfaz sobre fondo blanco (simulando una pantalla real).
- Puedes tener varias previews con distintos contenidos (por ejemplo, un botón habilitado y otro deshabilitado) — algo imposible con el Design clásico de una sola vista.
- El modo *Design* del editor visual (arrastrar componentes de la paleta al lienzo) también existe y genera el código Kotlin correspondiente, igual que WindowBuilder generaba el Java.
- **Interactive Mode** (icono de play en la preview) permite incluso pulsar los botones y navegar dentro de la previsualización sin ejecutar la app.

### 7. Casos prácticos resueltos

#### Caso práctico 1: "Creación de una pantalla" (antes: JFrame)

**Planteamiento.** Crear una interfaz desde cero usando solo código, sin el asistente visual, y comprobar el resultado en el emulador.

**Desarrollo.** Crea un proyecto *Empty Activity* y sustituye el componible por defecto por:

```kotlin
@Composable
fun PantallaBienvenida(modifier: Modifier = Modifier) {
    Column(
        modifier = modifier.fillMaxSize().padding(32.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text("Bienvenid@", style = MaterialTheme.typography.displaySmall)
        Spacer(Modifier.height(8.dp))
        Text("Desarrollo de Interfaces · 2º DAM", style = MaterialTheme.typography.bodyLarge)
    }
}
```

**Desenlace.** En el emulador, pantalla con dos textos centrados vertical y horizontalmente. La gran diferencia respecto a la creación clásica "por asistente": aquí **todo es código desde el primer momento** — el asistente genera la plantilla, pero la interfaz completa la describe tu función; la vista previa (@Preview) es un espejo del código, no un editor que genera código oculto.

#### Caso práctico 2: "Aceptar y Cancelar con comportamiento" (antes: dos JButton)

**Planteamiento.** Dos botones que muestren cuál se ha pulsado en una etiqueta.

**Desarrollo.**

```kotlin
@Composable
fun AceptarCancelar(modifier: Modifier = Modifier) {
    var mensaje by remember { mutableStateOf("Sin acción") }

    Column(
        modifier = modifier.fillMaxSize().padding(24.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(mensaje, style = MaterialTheme.typography.titleLarge)
        Spacer(Modifier.height(24.dp))
        Row {
            Button(onClick = { mensaje = "Has aceptado" }) { Text("Aceptar") }
            Spacer(Modifier.width(12.dp))
            Button(onClick = { mensaje = "Has cancelado" }) { Text("Cancelar") }
        }
    }
}
```

**Desenlace.** Al pulsar cada botón, la etiqueta superior cambia al instante sin una sola línea de "actualizar la etiqueta": el estado `mensaje` cambia y Compose recompone el `Text`. Con Swing habrías necesitado guardar la referencia del `JLabel` y llamar a `setText`; aquí el flujo de datos es automático.

### 8. Buenas prácticas

- **Previews para todo**: cada componente con su `@Preview` (o varias) — es tu vista Design permanente y gratis.
- **Extrae componibles en cuanto repitas** código: si dos pantallas tienen el mismo encabezado, es un componible `Encabezado()`.
- **Estilo al tema, no al componente**: usa `MaterialTheme.colorScheme` y `typography` en vez de colores y fuentes sueltos; cambiar el tema cambia toda la app.
- **Depura el estado, no la pantalla**: si la UI no se actualiza, casi siempre es que el estado no es observable o no es `remember`ado donde toca.

### 9. Errores frecuentes

| Error frecuente | Por qué ocurre | Cómo evitarlo |
|-----------------|----------------|---------------|
| Escribir `Text("...")` sin importar y rendirse | Faltan importaciones de `androidx.compose` | Alt+Intro sobre el símbolo en rojo; el IDE las añade |
| Estado que "se resetea" solo | `mutableStateOf` sin `remember` | Envolver siempre: `remember { mutableStateOf(...) }` |
| Modificar una variable normal y esperar que la UI cambie | Kotlin no sabe que esa variable afecta a la pantalla | Solo el estado observable (`mutableStateOf`) dispara la recomposición |
| Pelear con tamaños fijos | Herencia del pensamiento "ventana de escritorio" | Pensar en adaptativo: `fillMaxSize`, `weight`, constraints |
| Olvidar `dp` en paddings | `24` sin unidad no compila | `24.dp` (y para fuentes, `sp`) |

### 10. Resumen

En este tema has aprendido que:

- las librerías de UI evolucionaron de AWT a Swing y, en el mundo Android/Kotlin, a Jetpack Compose;
- el editor visual moderno es Android Studio con su vista Split/Design y `@Preview`, heredero del modo Design de WindowBuilder;
- `setContent` + `Scaffold` sustituyen a `JFrame`; los componentes (`Button`, `Text`, `TextField`, `Column`, `Row`) sustituyen a los `J*` de Swing;
- las propiedades Swing se convierten en parámetros de las funciones componibles, y los listeners en lambdas `onClick`;
- la diferencia de fondo es el estado observable y la recomposición automática frente a la actualización manual de la UI.

!!! success "Idea clave"
    En Swing describías la ventana una vez y luego la perseguías con `setText`; en Compose describes la pantalla para cada estado y el framework hace el resto. La interfaz es una función del estado.

### 11. Para seguir practicando

- Ejercicios de la unidad: `DI-U1.-Ejercicios.md` (bloques B y C).
- Codelab oficial: *Jetpack Compose basics* <https://developer.android.com/codelabs/jetpack-compose-basics>
- Catálogo de componentes: <https://developer.android.com/develop/ui/compose/components>

## Bibliografía y fuentes

- Android Developers. *Jetpack Compose documentation*. <https://developer.android.com/develop/ui/compose>
- Android Developers. *Thinking in Compose*. <https://developer.android.com/develop/ui/compose/mental-model>
- Kotlin Foundation. *Kotlin docs*. <https://kotlinlang.org/docs/home.html>
- Real Decreto 450/2010. Módulo profesional 0488 Desarrollo de interfaces.
- Temario clásico del módulo (material Java Swing) como base conceptual de la adaptación.
