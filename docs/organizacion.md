# Organización y mantenimiento del portafolio

[Volver al portafolio](../README.md)

## Evaluación de la estructura actual

La separación entre `Proyecto_DevOps/`, `AWS/` y `Algoritmos_y_scripts/` es adecuada para el volumen actual. Dentro del proyecto DevOps, `src/`, `docker/`, `infra/` y `docs/` ya separan responsabilidades. Se mantienen nombres y rutas para conservar los enlaces existentes.

## Cómo incorporar contenido futuro

- Agregar trabajos universitarios únicamente cuando estén publicados, bajo `Algoritmos_y_scripts/proyectos_academicos/<nombre>/` si corresponden a esa colección.
- Mantener las prácticas AWS dentro de la carpeta del servicio correspondiente.
- Incorporar las mejoras del organizador en `Proyecto_DevOps/`. Un proyecto DevOps independiente debe tener su propia carpeta o repositorio y un enlace desde el índice.
- Para aplicaciones de software independientes, elegir una carpeta o repositorio con nombre descriptivo cuando exista contenido real. Crear una categoría adicional solo cuando ayude a navegar varios proyectos.
- Actualizar la tabla del README principal al publicar cada proyecto. No crear carpetas vacías ni incluir proyectos previstos como terminados.

## Criterio común para README

1. Título y objetivo breve.
2. Alcance implementado, estado y limitaciones relevantes.
3. Tecnologías y requisitos reales.
4. Instrucciones con directorio de trabajo explícito, comandos y entradas de ejemplo.
5. Enlaces a código, informes y evidencias existentes.
6. Pendientes separados de las funcionalidades disponibles, cuando corresponda.

Usar enlaces relativos dentro del repositorio, un solo título principal por archivo, bloques de código con lenguaje y tablas breves. Antes de abrir un PR, comprobar rutas, nombres de archivos, fragmentos de enlaces y correspondencia de las instrucciones con el código. No es necesario añadir automatización para mantener esta estructura.

## Perfil público

La [propuesta de perfil](perfil-github.md) está lista para revisión. La consulta de `Bryanfalcarruz/Bryanfalcarruz` devolvió «no encontrado» el 17 de noviembre de 2026. Debe crearse ese repositorio como público antes de implementar el README; esta propuesta no crea ni publica el perfil.

Copiar únicamente el contenido delimitado en la propuesta a `README.md` de ese repositorio, mediante una rama y un Pull Request. Los enlaces absolutos se mantienen porque apuntan a otro repositorio.
