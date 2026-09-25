# di-desarrollo-interfaces

Temario del módulo **Desarrollo de Interfaces** (2º DAM, ciclo Desarrollo de Aplicaciones Multiplataforma) modernizado a **Kotlin y Jetpack Compose**.

Publicación automática en cada push a `main`:
**https://ismaelvelasco-star.github.io/di-desarrollo-interfaces**

## Estructura

```
docs/
  section1/            → Módulo Desarrollo de Interfaces
    recursos/          → Normativa oficial (RD 450/2010, módulo 0488)
    uNN/               → Unidades didácticas
      teoria/          → Teoría en Markdown (MkDocs Material)
      ejercicios/      → Ejercicios graduados + solucionario
slides/                → Presentaciones Reveal.js (pareja .md + .html)
.github/workflows/     → Deploy automático a GitHub Pages
```

## Convenciones

- Teoría: `DI-UX.Y.-NombreDescriptivo.md`
- Ejercicios: `DI-UX.-Ejercicios.md` / `DI-UX.-Solucionario.md`
- Slides: mismo nombre base en `.md` y `.html`, prefijo `DI-`
- Normativa de referencia en `docs/section1/recursos/DI Normativa.txt`
- Contenido en español de España, tono didáctico, Kotlin + Jetpack Compose como stack principal

## Créditos

- Estructura del sitio y sistema de slides inspirados en la plantilla de [Eduardo Fdez (revilofe)](https://revilofe.github.io/).
- Contenido elaborado directamente en Kotlin + Jetpack Compose: Ismael Velasco.
