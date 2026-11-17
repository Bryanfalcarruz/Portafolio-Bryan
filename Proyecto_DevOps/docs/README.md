# File Organizer | Guía de uso

Aplicación Python que organiza los archivos del primer nivel de una carpeta por extensión. Se empaqueta con Docker y forma parte del [proyecto de infraestructura AWS](../README.md).

## Funcionalidades implementadas

- Categorías `Images`, `Documents`, `Videos`, `Music` y `Others`.
- Exclusión de archivos mediante patrones.
- Simulación sin mover archivos y políticas para nombres duplicados.
- Resumen por categoría en la consola.

No recorre subcarpetas. El resumen cuenta archivos procesados, incluidos los que se omiten por colisión; no equivale necesariamente al total movido.

## Requisitos y ubicación

Python 3, sin dependencias externas, o Docker. Todos los comandos siguientes parten de `Proyecto_DevOps/`; desde la raíz del repositorio, ejecuta primero:

```bash
cd Proyecto_DevOps
```

## Uso con Python

```bash
python src/organizer.py --path "RUTA" [opciones]
```

| Opción | Comportamiento |
| --- | --- |
| `--path RUTA` | Carpeta que se organizará; obligatorio. |
| `--dry-run` | No mueve ni reemplaza archivos, pero puede crear carpetas de destino. |
| `--exclude PATRONES` | Uno o más patrones; reemplazan las exclusiones predeterminadas. |
| `--on-collision skip` | Omite archivos cuyo destino existe; valor predeterminado. |
| `--on-collision overwrite` | Reemplaza el destino existente. |
| `--on-collision rename` | Genera un nombre disponible, por ejemplo `archivo (1).txt`. |

Exclusiones predeterminadas: `*.tmp`, `*.part`, `*.crdownload`, `Thumbs.db` y `.DS_Store`. Usa comillas para evitar que la terminal expanda los patrones.

```bash
# Windows: organizar Descargas
python src/organizer.py --path "C:/Users/TuUsuario/Downloads"

# Linux/macOS: simular sobre una carpeta de prueba
python src/organizer.py --path "/home/usuario/prueba" --dry-run

# Exclusiones personalizadas
python src/organizer.py --path "/home/usuario/prueba" --exclude "*.tmp" "*.part"

# Reemplazar o renombrar ante una colisión
python src/organizer.py --path "C:/Users/TuUsuario/Downloads" --on-collision overwrite
python src/organizer.py --path "/home/usuario/prueba" --on-collision rename
```

## Uso con Docker

### Construcción local

```bash
docker build -f docker/Dockerfile -t file-organizer .
docker run --rm -v "C:/Users/TuUsuario/Downloads:/data" file-organizer --path /data --dry-run
docker run --rm -v "C:/Users/TuUsuario/Downloads:/data" file-organizer --path /data --on-collision overwrite
```

El volumen expone la carpeta del equipo como `/data` dentro del contenedor. `overwrite` puede reemplazar archivos existentes; usa datos de prueba para explorar su comportamiento.

### Imagen referenciada en Docker Hub

La imagen está publicada en [bryanf606/file-organizer](https://hub.docker.com/r/bryanf606/file-organizer). El repositorio y las etiquetas `latest` y `0.1.0` respondieron correctamente el 17 de noviembre de 2026. No se verificó que el contenido de esas imágenes corresponda al código actual; la construcción local usa el Dockerfile del repositorio.

```bash
docker pull bryanf606/file-organizer:latest
docker run --rm -v "C:/Users/TuUsuario/Downloads:/data" bryanf606/file-organizer:latest --path /data --dry-run
```

En Linux/macOS, el equivalente y la variante con etiqueta fija documentada son:

```bash
docker run --rm -v "$HOME/Downloads:/data" bryanf606/file-organizer:latest --path /data --dry-run
docker run --rm -v "$HOME/Downloads:/data" bryanf606/file-organizer:0.1.0 --path /data
```

## Estado y evidencias

Python, Dockerfile y módulos Terraform están presentes. La integración ECR/EC2 y el backend remoto requieren completar los puntos indicados en el [estado técnico del proyecto](../README.md). CI/CD continúa pendiente.

Las [capturas](screenshots/) documentan el aprovisionamiento de recursos y Docker. Autor: Bryan Alcarruz.
