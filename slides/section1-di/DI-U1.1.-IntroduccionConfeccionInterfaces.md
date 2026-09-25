# DI-U1.1 - Introducción a la confección de interfaces

Note: Bienvenid@ al módulo de Desarrollo de Interfaces. Este primer tema es la puerta de entrada: qué es una interfaz, qué paradigmas hay detrás y, sobre todo, **el entorno donde trabajaremos todo el módulo: Android Studio con Kotlin y Jetpack Compose**. Al terminar: IDE instalado, primera interfaz creada y entorno de diseño dominado.

---


![Logo Alberti](assets/logo-iesra.png) <!-- .element height="50%" -->

Note: Presentar el módulo dentro de 2º DAM. Este tema replica el guion clásico de introducción (paradigmas, herramientas, instalación, primer contacto) pero con la pila actual: Android Studio, Kotlin y Compose.

---


## Índice

Note: El guion del tema: introducción y paradigmas (breve), herramientas de edición, instalación de Android Studio, primer proyecto con Kotlin, análisis del entorno de diseño y dos casos prácticos.


### Índice I

- Introducción y paradigmas
- Herramientas de edición de interfaces
- Instalación de Android Studio

Note: Primera mitad: contexto conceptual ligero (bajo/alto nivel, imperativo/declarativo, POO-eventos-componentes) y el panorama de herramientas, donde Android Studio entra en la tabla comparativa como nuestro IDE.


### Índice II

- Primer proyecto con Kotlin
- Entorno de diseño: Toolbar, Split, Palette, Structure
- Casos prácticos: pantalla y botones

Note: Segunda mitad: la parte práctica. Crear el primer proyecto Empty Activity, recorrer las zonas del entorno de diseño una a una, y los dos casos prácticos que luego serán los ejercicios del bloque C.

---


## ¿Qué es una interfaz?

Note: Empezamos por la motivación, igual que el temario clásico.


### Lenguajes de bajo y alto nivel

- Bajo nivel: máquina (0 y 1), ensamblador
- Alto nivel: cercano al natural
- La compilación traduce a bajo nivel

Note: Los de bajo nivel controlan el hardware pero son ilegibles. Los de alto nivel se escriben con reglas comprensibles y **el compilador traduce**. Kotlin es de alto nivel: compila a bytecode.


### La interfaz traduce persona ↔ máquina

- Sin GUI: habría que leer el código fuente
- La interfaz es la capa de interacción
- Este módulo: construir esas capas

Note: Sin interfaz, para usar cualquier aplicación habría que ser programador experto. La interfaz es la capa que nos salva a todos — y construirla bien es una disciplina propia, que es lo que viene este módulo.


### Paradigmas: dos estilos

- Imperativo: el paso a paso (Java, C, Kotlin...)
- Declarativo: el resultado (HTML, CSS, SQL)
- POO + eventos + componentes: el trío de las interfaces

Note: Repaso rápido: imperativo dice CÓMO, declarativo dice QUÉ. Y los tres modelos cuya combinación es clave para interfaces: objetos, eventos y componentes. En Compose los tres se dan cita: composables reutilizables que reaccionan a eventos, apoyados en clases Kotlin.

---


## Herramientas de edición

Note: El panorama de IDEs del temario clásico, actualizado con el nuestro.


### El panorama

- Visual Studio, MonoDevelop, Glade, NetBeans, Eclipse
- Todas: paleta + lienzo + propiedades
- **Android Studio**: libre, Kotlin/Java, diseño Compose

Note: La tabla comparativa del tema: licencias y lenguajes de cada IDE. Fíjate en que todas comparten el mismo patrón de editor visual: paleta de componentes, lienzo y panel de propiedades. Android Studio entra en esa tradición y añade la vista de diseño en vivo para Compose y el emulador integrado.


### ¿Por qué Android Studio?

- IDE oficial de Android, basado en IntelliJ
- Gratuito, libre y multiplataforma
- Estándar profesional actual en interfaces móviles

Note: Igual que el temario clásico justificaba Eclipse ("su uso es cada vez más frecuente profesionalmente"), hoy ese argumento lo cumple Android Studio: es el estándar de facto del desarrollo Android profesional. Kotlin es el lenguaje soportado por Google.


### Instalación

- Descargar de developer.android.com/studio
- Asistente con opciones por defecto (Standard)
- Primer arranque: completa los componentes

Note: El proceso completo en tres pasos, ocupando pocos minutos. La gran diferencia con los entornos clásicos: **no hay que instalar nada aparte** — el JDK viene embebido y el asistente de proyectos añade Jetpack Compose automáticamente. Con Eclipse había que instalar el JDK de Oracle aparte.

---


## Primer proyecto

Note: La parte práctica: crear el proyecto y entender qué se ha generado.


### Empty Activity

- New Project → Empty Activity
- Nombre: MiPrimeraInterfaz
- Genera actividad + composable de ejemplo

Note: Dos sencillos pasos: New Project y elegir la plantilla Empty Activity (la básica con Compose). El IDE genera MainActivity.kt con una actividad y un composable de ejemplo. Se recomienda partir de la plantilla porque deja configuradas las dependencias de Compose.


### Actividad y función composable

- Actividad: la pantalla del sistema
- `@Composable`: la ANOTACIÓN que marca la función
- Función composable: la función que describe la interfaz
- setContent monta una dentro de la otra

Note: Doble distinción importante del tema. Primera: @Composable es la ANOTACIÓN (lo que escribes delante), y la función marcada es la función composable. Ojo: la doc oficial en español la llama "función de componibilidad" o "componible", pero en la comunidad y los vídeos se dice composable, que es como la llamaremos aquí. Segunda: la actividad aloja, el componible describe. La conexión es setContent dentro de onCreate: el equivalente funcional de "crear la ventana y añadirle el contenido".


### Importar la librería

```kotlin
import androidx.compose.material3.Button
import androidx.compose.material3.*
```

Note: Como en todo lenguaje con librerías, se importa lo que se usa, tras la declaración del paquete. En la práctica el IDE lo hace solo con Alt+Intro sobre el elemento en rojo. Compose garantiza el mismo aspecto y comportamiento en cualquier dispositivo.

---


## Entorno de diseño

Note: El análisis zona a zona del entorno, igual que el clásico hacía con Toolbar, vista de diseño, Palette y Structure.


### Toolbar

- Acciones genéricas: proyectos, Gradle, SDK, emulador
- **Run ▶**: ejecutar la app
- Su flecha: elegir dispositivo

Note: La barra de herramientas con las acciones genéricas de programación. El botón estrella es Run: ejecuta la app. La flecha a su derecha despliega los dispositivos disponibles (emuladores y físicos conectados).


### Vistas Code, Split y Design

- Code: solo código Kotlin
- Split: código + previsualización a la vez
- Design: solo previsualización (el lienzo)

Note: Las tres pestañas del editor. Split es la joya: escribes Kotlin y ves la interfaz renderizándose al lado, en vivo, sin ejecutar nada. Las funciones @Preview se renderizan aquí. Es el lienzo sobre el que dibujar la interfaz.


### Palette y Component Tree

- Palette: composables para arrastrar (texto, botones, contenedores...)
- Component Tree: jerarquía de lo colocado
- Attributes: propiedades del componente

Note: La paleta recoge los componentes que se arrastran al lienzo; el árbol muestra la jerarquía de lo colocado como un explorador; y el panel de propiedades permite ajustar texto, alineación, color, enabled... El mismo trío paleta-estructura-propiedades de siempre.

---


## Casos prácticos

Note: Los dos casos prácticos del tema, que luego son los ejercicios del bloque C.


### Caso 1: primera pantalla

- Declarar el composable
- Describir el contenido (Column + Text)
- setContent lo asigna a la actividad

Note: Los tres pasos: declarar la función @Composable, describir el contenido y asignarla con setContent. Diferencia clave con las ventanas clásicas: no hay tamaño ni visibilidad que indicar — la interfaz ocupa toda la pantalla y se adapta al dispositivo.


### Caso 2: dos botones

- Row con dos Button
- "Aceptar" y "Cancelar"
- Arrastrar en Design = mismo código Kotlin

Note: Una fila con dos botones. Puedes escribirlos en Code o arrastrarlos desde la paleta en Design: **el resultado es el mismo código Kotlin**, las dos vistas son espejos. No hay código generado "de segunda clase" ni oculto.


### Resumen

- Compose: la librería de componentes visuales
- Componible: la pieza que describe la pantalla
- Dos modos: código y diseño, espejos del mismo archivo

Note: Las ideas del tema: Compose trae todos los componentes visuales (botones, textos, casillas...), el composable es la unidad básica que describe la pantalla, y trabajamos con dos modos de diseño que son dos vistas del mismo código.

---


## Cierre

Note: Cerramos con la resolución del caso de la unidad y el enlace a ejercicios.


### Caso de la unidad

- App de bienvenida del instituto
- ¿Componentes? Textos y un botón "Entrar"
- ¿Lenguaje? Kotlin. ¿IDE? Android Studio. ¿Librería? Compose

Note: La resolución del caso práctico de la unidad: para una app de bienvenida hacen falta textos y un botón; el lenguaje adecuado es Kotlin de alto nivel; el IDE, Android Studio; y la librería, Jetpack Compose. Las cuatro decisiones previas que hay que saber tomar.


### Ejercicios

- Bloque A: conceptos (calentamiento)
- Bloque B: el entorno
- Bloque C: primer proyecto propio

Note: Los ejercicios van de los conceptos al proyecto propio, siguiendo exactamente el guion del tema. Método: intentar primero, solucionario después (que lleva los enunciados citados). Próxima unidad: profundizamos en los componentes y su disposición.
