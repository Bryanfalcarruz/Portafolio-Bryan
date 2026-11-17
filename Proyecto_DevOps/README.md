# File Organizer | Python, Docker y AWS con Terraform

Proyecto de aprendizaje que combina un organizador de archivos en Python, su empaquetado con Docker y la definición de infraestructura en AWS mediante Terraform.

[Volver al portafolio](../README.md) · [Uso de la aplicación](docs/README.md) · [Evidencias](docs/screenshots/)

## Arquitectura

```text
Terraform
├── network: VPC, subred pública, Internet Gateway, rutas y Security Group
├── compute: EC2, rol IAM, perfil de instancia e instalación de Docker
└── ecr: repositorio de imágenes

bootstrap: bucket S3 y tabla DynamoDB para el futuro backend remoto
Flujo por completar: imagen Docker → ECR → aplicación ejecutada en EC2
```

## Tecnologías y estructura

| Ruta | Contenido |
| --- | --- |
| [src/organizer.py](src/organizer.py) | CLI Python con exclusiones, simulación y políticas de colisión. |
| [docker/Dockerfile](docker/Dockerfile) | Imagen basada en Python 3.11 slim. |
| [infra/](infra/) | Terraform y proveedor AWS; módulos de red, cómputo y ECR. |
| [infra/bootstrap/](infra/bootstrap/README.md) | S3 con versionado, cifrado y bloqueo público; DynamoDB para bloqueo de estado. |
| [docs/](docs/README.md) | Uso local, Docker y capturas. |

## Estado actual

| Componente | Implementado | Pendiente o limitación |
| --- | --- | --- |
| Aplicación | Clasificación por extensión, `--exclude`, `--dry-run` y `--on-collision`. | La simulación no mueve archivos, pero puede crear carpetas de destino. |
| Docker | Dockerfile con punto de entrada a la CLI. | La ejecución requiere `--path` y un volumen con archivos. |
| Red y EC2 | VPC, subred pública, EC2, Docker y permisos SSM, S3 de lectura y ECR de lectura. | Las capturas no certifican disponibilidad actual en AWS. |
| ECR | Repositorio y código de autenticación, descarga y ejecución en `user_data`. | Verificar publicación de la imagen; el `docker run` actual no pasa `--path` ni monta un volumen, por lo que no completa la tarea de la CLI. |
| Estado remoto | Recursos S3/DynamoDB y archivo `backend.hcl`. | Falta el bloque `backend "s3"` en la configuración principal; el archivo por sí solo no activa el backend. |
| Observabilidad | `monitoring = true` en EC2. | No hay configuración de logs de la aplicación, paneles ni alarmas. |
| CI/CD | No implementado. | Automatización futura con GitHub Actions. |

## Requisitos

- Python 3 para uso local; Docker para construir y ejecutar el contenedor.
- Para infraestructura: Terraform **>= 1.6.0 y < 2.0.0** si se utiliza también bootstrap, AWS CLI y una cuenta con permisos sobre los recursos definidos.
- Perfiles AWS configurados: bootstrap usa por defecto `portfolio-dev` y la raíz de infraestructura usa `bryanf606`. Ambos se pueden seleccionar mediante la variable `aws_profile`.

## Uso local y Docker

Desde la raíz del repositorio:

```bash
cd Proyecto_DevOps
python src/organizer.py --help
python src/organizer.py --path "/ruta/a/archivos" --dry-run
docker build -f docker/Dockerfile -t file-organizer .
docker run --rm -v "/ruta/a/archivos:/data" file-organizer --path /data --dry-run
```

Sustituye la ruta de ejemplo por una carpeta de prueba existente. Consulta las [opciones y ejemplos completos](docs/README.md).

## Flujo de infraestructura

Los comandos siguientes documentan el aprovisionamiento manual y pueden crear recursos facturables. No constituyen un despliegue completo de la aplicación.

Desde la raíz del repositorio, para crear los recursos del backend:

```bash
cd Proyecto_DevOps/infra/bootstrap
terraform init
terraform plan -var="aws_profile=TU_PERFIL"
terraform apply -var="aws_profile=TU_PERFIL"
terraform output
```

Bootstrap utiliza estado local. Los outputs identifican el bucket y la tabla creados; deben corresponder a la configuración del backend que se utilice posteriormente.

Para revisar la infraestructura principal, desde `Proyecto_DevOps/infra/bootstrap`:

```bash
cd ..
terraform init
terraform validate
terraform plan -var="aws_profile=TU_PERFIL"
```

Antes de aplicar, resolver la configuración del backend y revisar las limitaciones de ejecución del contenedor. La instrucción `terraform init -backend-config=backend.hcl` solo será válida para el backend S3 una vez declarado ese backend y ajustados sus valores al entorno. Estos cambios de infraestructura están pendientes y no se realizan en esta revisión documental.

## Documentación técnica

- [Bootstrap y estado de Terraform](infra/bootstrap/README.md).
- [Módulo network](infra/modules/network/README.md).
- [Módulo compute](infra/modules/compute/README.md).
- [Definición del módulo ECR](infra/modules/ecr/).
- [Capturas del aprovisionamiento](docs/screenshots/).

El repositorio conserva la separación entre aplicación, contenedor e infraestructura, el uso de módulos reutilizables y exclusiones de Git para estados y variables sensibles.
