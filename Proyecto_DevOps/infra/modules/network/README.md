# Módulo network

Define la red base del [proyecto DevOps](../../../README.md): una VPC con una subred pública, salida a Internet y un Security Group sin reglas de entrada.

## Recursos

| Recurso | Configuración |
| --- | --- |
| VPC | DNS y hostnames habilitados; CIDR recibido por variable. |
| Subred pública | IP pública automática; primera zona disponible devuelta por AWS, no una zona fija. |
| Internet Gateway | Asociado a la VPC. |
| Tabla de rutas | Ruta `0.0.0.0/0` al Internet Gateway, asociada a la subred. |
| Security Group | Sin ingress; todo el tráfico de salida IPv4 permitido. |

En la configuración raíz, los CIDR predeterminados son `10.0.0.0/16` para la VPC y `10.0.1.0/24` para la subred. Con `project_name = "file-organizer"`, los nombres son `file-organizer-vpc`, `file-organizer-public-a`, `file-organizer-igw`, `file-organizer-rt-public` y `file-organizer-sg-base`.

## Variables de entrada

| Variable | Uso |
| --- | --- |
| `project_name` | Prefijo de nombres y etiquetas; obligatorio. |
| `vpc_cidr` | Rango CIDR de la VPC; obligatorio. |
| `public_subnet_cidr` | Rango CIDR de la subred; obligatorio. |
| `tags` | Etiquetas adicionales; predeterminado `{}`. |

## Outputs

| Output | Valor |
| --- | --- |
| `vpc_id` | Identificador de la VPC. |
| `public_subnet_id` | Identificador de la subred pública. |
| `route_table_public_id` | Identificador de la tabla de rutas pública. |
| `sg_base_id` | Identificador del Security Group base. |

## Integración y evidencias

Se invoca desde [infra/main.tf](../../main.tf). El módulo compute consume la subred y el Security Group y configura los permisos para administración mediante SSM. La red no implementa subredes privadas, NAT ni filtrado del tráfico saliente.

Consulta las [capturas de red](../../../docs/screenshots/) y el [flujo de infraestructura](../../../README.md).
