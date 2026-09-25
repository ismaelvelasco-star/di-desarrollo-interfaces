# DI-U1.1 - Introducción a la confección de interfaces

Note: Bienvenid@ al módulo de Desarrollo de Interfaces. Esta unidad es la puerta de entrada: antes de escribir interfaces modernas tenemos que entender qué paradigmas las sostienen. **El hilo conductor de todo el módulo será Kotlin y Jetpack Compose**, la pila actual de Google para Android. La pregunta que guía la unidad: ¿qué hay debajo de una interfaz?

---


![Logo Alberti](assets/logo-iesra.png) <!-- .element height="50%" -->

Note: Presentamos el módulo dentro de 2º DAM. Este primer tema adapta el temario clásico de Java Swing al mundo actual: los conceptos no cambian, las herramientas sí. Aclarar al inicio que quien entienda los conceptos podrá trabajar con cualquier framework del mercado.

---


## Índice

Note: Seguimos tres bloques: primero los paradigmas y modelos de programación; después el salto de Swing a Compose con el primer proyecto; y cerramos con las herramientas de edición, de Eclipse a Android Studio y Figma.


### Índice I

- Paradigmas: imperativo y declarativo
- Modelos: objetos, eventos, componentes
- Kotlin y Jetpack Compose

Note: El primer bloque es conceptual pero no teórico vacío: cada paradigma lo veremos con código Kotlin real. Insistir en que **los tres modelos juntos son la clave del desarrollo de interfaces**, como dice la propia normativa del módulo.


### Índice II

- De AWT y Swing a Compose
- Primer proyecto y componentes
- Estado y recomposición
- Herramientas: de Eclipse a Android Studio

Note: El segundo bloque es la parte práctica: montar el primer proyecto, escribir componibles y entender el estado. Y el tercero pone el contexto de herramientas, comparando el panorama clásico con el actual. Con eso quedan listos para los ejercicios.

---


## ¿Qué es una interfaz?

Note: Empezamos por la motivación: por qué existen las interfaces y por qué son una disciplina propia dentro del desarrollo.


### Sin interfaz, todos programadores

- Las apps requieren interacción con el usuario
- Sin GUI: usar el lenguaje fuente
- La interfaz traduce persona ↔ máquina

Note: Un lenguaje de programación permite escribir instrucciones interpretables por un ordenador. Sin interfaz, para usar cualquier aplicación habría que ser programador experto y leer su código fuente. **La interfaz es la capa que nos salva a todos.**


### Lenguajes de bajo y alto nivel

- Bajo nivel: máquina (0 y 1), ensamblador
- Alto nivel: cercano al natural
- La compilación traduce a bajo nivel

Note: Los de bajo nivel controlan el hardware directamente pero son ilegibles para personas. Los de alto nivel se escriben con reglas comprensibles y **el compilador los traduce**. Kotlin es de alto nivel: compila a bytecode JVM.

---


## Paradigmas

Note: Un paradigma define un estilo de programación: cómo estructuramos el programa que resuelve el problema. Los dos grandes: imperativo y declarativo.


### Imperativo: el paso a paso

- Estructurada: secuencias, condiciones, bucles
- Procedimental: funciones y subrutinas
- Modular: piezas independientes que se combinan
- Java, C, C#, Kotlin, Python...

Note: El imperativo dice CÓMO: pasos ordenados. Se subdivide en estructurada (estructuras de control), procedimental (subrutinas) y modular (piezas independientes que se ensamblan al final). Ejemplo en pizarra: sumar pares de una lista con un bucle.


### Declarativo: el qué

- Describes el resultado, no los pasos
- HTML, CSS, SQL
- Kotlin también sabe ser declarativo

Note: El declarativo dice QUÉ: SQL pide los datos de 2DAM sin explicar cómo recorrer la tabla. Y ojo: Kotlin con `filter` y `count` también es declarativo. **Jetpack Compose es declarativo**: describimos la pantalla para un estado. Es el corazón del módulo.


### Los tres modelos clave

1. Orientado a objetos: entidades con atributos y métodos
2. Basado en eventos: acciones externas dirigen el flujo
3. Basado en componentes: reutilización de módulos visuales

Note: Los tres modelos que combina toda interfaz. POO: objetos que interactúan. Eventos: el clic gobierna. Componentes: empaquetar y reutilizar. La normativa del módulo pide exactamente esta combinación. En el tema 1.2 la veremos entera en un solo archivo Compose.

---


## De Swing a Compose

Note: Ahora el salto histórico: de las librerías clásicas de Java a la pila moderna. La historia ayuda a entender por qué Compose es como es.


### La evolución de las librerías

- AWT: controles nativos del SO
- Swing: apariencia propia, JFrame, JButton
- Compose: UI declarativa con funciones Kotlin

Note: AWT usaba los controles del sistema y cambiaba de aspecto según plataforma. Swing lo resolvió con componentes propios: nace el JFrame. Y Compose da el salto de paradigma: **ya no hay clases de ventana que manipular, sino funciones que describen**.


### Tabla de equivalencias

| Swing | Compose |
|-------|---------|
| JFrame | setContent + Scaffold |
| JButton | Button |
| JTextField | TextField |
| ActionListener | onClick |

Note: La tabla que hay que memorizar. JFrame pasa a ser la actividad con setContent; JButton es la función Button; el listener es una lambda. Quien venga de Java Swing tiene aquí su diccionario de traducción.


### El entorno hoy

- Eclipse + WindowBuilder entonces
- Android Studio ahora: Split, Preview, paleta
- El concepto no cambia: diseño + código

Note: El rol de Eclipse con WindowBuilder lo hace Android Studio: vista Split con previsualización en vivo, paleta de componentes, arrastrar y soltar. **El concepto de editor visual con generación de código lleva 20 años igual**; solo cambian los nombres. Eso libera: aprende el concepto, sobrevive a cualquier herramienta.

---


## Primer contacto Compose

Note: Vemos el primer código real: la primera pantalla y los primeros componentes, equivalentes de los casos prácticos clásicos de JFrame y JButton.


### Mi primera interfaz

```kotlin
setContent {
    MaterialTheme {
        Scaffold { MiPrimeraInterfaz() }
    }
}
```

Note: El setContent es la ventana. Sin setSize ni setVisible: la app se adapta a la pantalla del dispositivo. Scaffold da la estructura. **Todo es código desde el primer minuto**; la preview es un espejo del código.


### Componentes y propiedades

- Text, Button, TextField
- Propiedades = parámetros
- Evento: onClick junto al componente

Note: Los componentes son funciones con parámetros que equivalen a las propiedades clásicas: text, enabled, colors, style. Y el gran avance de Compose: **componente, propiedades y evento viven juntos** en el mismo sitio. En Swing había que registrar el listener aparte.


### Estado y recomposición

- Estado: `remember { mutableStateOf(0) }`
- Evento cambia el estado
- La pantalla se redibuja sola

Note: La diferencia de fondo con Swing. Antes perseguías la etiqueta con setText; ahora declaras que el Text depende del estado y **el framework actualiza solo**. Mientras pienses "qué tengo que actualizar" sigues en Swing; cuando pienses "de qué estado depende esto", ya piensas en Compose.

---


## Cierre

Note: Recapitulamos y enlazamos con los ejercicios de la unidad.


### Resumen

- Paradigmas: imperativo (cómo) vs declarativo (qué)
- POO + eventos + componentes: el trío de las interfaces
- Compose = declarativo + componentes Kotlin
- Android Studio: el editor visual moderno

Note: Las cuatro ideas de la unidad. La síntesis: una interfaz Compose es un componente orientado a objetos que reacciona a eventos y se describe de forma declarativa. **Los paradigmas del tema trabajando juntos en un solo archivo.**


### Ejercicios y siguiente unidad

- Bloques A-D: de conceptos a la calculadora
- Solucionario disponible: consultar después de intentar
- UD 2: layouts y diseño adaptativo

Note: Los ejercicios van del calentamiento conceptual (bloque A) a la calculadora completa por componentes (bloque D). Recordar el método: intentar primero, solucionario después. Y avanzar el temario: la siguiente unidad profundiza en layouts.
