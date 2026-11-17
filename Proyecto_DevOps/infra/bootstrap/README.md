# Bootstrap | Recursos para el estado de Terraform

Este directorio define el bucket S3 y la tabla DynamoDB destinados al backend remoto del [proyecto DevOps](../../README.md). Bootstrap utiliza estado local; crear estos recursos no activa automáticamente el backend de la infraestructura principal.

## Recursos y propósito

- Bucket `tfstate-<project_name>-<sufijo aleatorio>`: almacenamiento del estado, versionado, cifrado SSE-S3 AES256, propiedad `BucketOwnerEnforced` y bloqueo de acceso público.
- Tabla `tf-lock-<project_name>`: bloqueo de estado mediante la clave `LockID`, con facturación `PAY_PER_REQUEST` según uso.
- Etiquetas `Project`, `Env` y `Owner` para identificar los recursos.

## Variables y outputs

| Variable | Valor predeterminado |
| --- | --- |
| `aws_region` | `us-west-2` |
| `aws_profile` | `portfolio-dev` |
| `project_name` | `file-organizer` |

Los outputs `tfstate_bucket_name` y `lock_table_name` devuelven los nombres creados.

## Flujo de trabajo

Requiere Terraform >= 1.6.0 y < 2.0.0 y un perfil AWS con permisos para los recursos definidos. Desde la raíz del repositorio:

```bash
cd Proyecto_DevOps/infra/bootstrap
terraform init
terraform validate
terraform plan -var="aws_profile=TU_PERFIL"
terraform apply -var="aws_profile=TU_PERFIL"
terraform output
```

`apply` crea recursos en AWS. Conserva el estado local de bootstrap para gestionar posteriormente esos recursos.

## Integración pendiente del backend

El archivo existente es [../backend.hcl](../backend.hcl), fuera de este directorio. La infraestructura principal todavía no declara `backend "s3"`; por tanto, `terraform init -backend-config=backend.hcl` no basta para habilitar el estado remoto.

Una futura implementación debe declarar el backend, comprobar que bucket, tabla, región y perfil correspondan al entorno, y gestionar la migración del estado existente. No existe `env/dev.tfvars` en el repositorio: los ejemplos utilizan la variable `aws_profile` directamente.

La carpeta [test/](test/) contiene otra configuración de recursos, no una suite de pruebas automatizadas. No forma parte del flujo anterior.

## Evidencias y exclusiones

- [Captura del bucket](../../docs/screenshots/bootstrap/s3-backend-created.png).
- [Reglas de Git](../.gitignore) para `.terraform/`, `*.tfstate`, planes y variables sensibles.
