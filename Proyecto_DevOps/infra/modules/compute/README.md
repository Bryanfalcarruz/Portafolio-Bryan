# Módulo compute

Define la instancia EC2 y su identidad IAM para el [proyecto DevOps](../../../README.md), con instalación de Docker y permisos para administración mediante AWS Systems Manager (SSM).

## Recursos y comportamiento

- EC2 `t3.micro` por defecto, con AMI Amazon Linux 2 x86_64 seleccionada mediante filtro.
- Subred y Security Group recibidos desde network, IP pública y monitoreo detallado de EC2 habilitados.
- Rol IAM e instance profile con `AmazonSSMManagedInstanceCore`, `AmazonS3ReadOnlyAccess` y `AmazonEC2ContainerRegistryReadOnly`.
- `user_data` instala y habilita Docker, agrega `ec2-user` al grupo Docker y registra mensajes en `/var/log/user-data.log`.
- El script incluye autenticación en ECR, descarga de la imagen y un intento de ejecutar el contenedor `file-organizer`.

El Security Group usado por la raíz no permite tráfico entrante y permite toda la salida IPv4. El acceso administrativo está planteado mediante SSM; no se configura una llave SSH.

## Variables de entrada

| Variable | Uso | Predeterminado |
| --- | --- | --- |
| `project_name` | Nombres y etiquetas. | Obligatorio |
| `subnet_id` | Subred de la instancia. | Obligatorio |
| `sg_id` | Security Group asociado. | Obligatorio |
| `instance_type` | Tipo de instancia. | `t3.micro` |
| `key_name` | Declarada, pero no utilizada por la instancia. | `null` |
| `tags` | Etiquetas comunes. | `{}` |
| `ecr_repository_url` | URL de la imagen para el script de inicio. | `null` |
| `image_tag` | Etiqueta de la imagen. | `latest` |

La raíz proporciona la URL del módulo ECR. Aunque la variable permite `null`, ese valor no debe interpretarse como una ejecución sin ECR validada: se interpola en `user_data`.

## Outputs

| Output | Valor |
| --- | --- |
| `instance_id` | Identificador de EC2. |
| `public_ip` | IP pública de la instancia. |
| `iam_role_name` | Nombre del rol IAM. |

## Estado y limitaciones

El código de arranque desde ECR está presente, pero no acredita un despliegue funcional de la aplicación. El comando `docker run` no incluye el argumento obligatorio `--path` ni un volumen con datos. También debe verificarse la publicación de la imagen y la ejecución completa del arranque.

`monitoring = true` habilita monitoreo detallado de EC2; no configura logs de la aplicación, paneles ni alarmas. Estas mejoras y CI/CD permanecen pendientes.

## Integración y evidencias

Se invoca desde [infra/main.tf](../../main.tf). Consulta las [capturas de EC2 y Docker](../../../docs/screenshots/) y el [estado general](../../../README.md).
