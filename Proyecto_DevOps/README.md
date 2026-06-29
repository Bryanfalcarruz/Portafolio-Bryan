# aws-terraform-docker-app

Despliega en AWS la infraestructura necesaria para ejecutar una pequeña
aplicación en Python ("File Organizer", un organizador de archivos por tipo)
empaquetada en Docker. Toda la infraestructura se provisiona con Terraform,
aplicando modularización y un backend remoto de estado.

## Arquitectura
```
[ Terraform ] --provisiona--> [ AWS ]
                                ├─ VPC + Subnet pública + Route Table + Security Group  (módulo network)
                                ├─ Instancia EC2                                        (módulo compute)
                                └─ Repositorio ECR (imágenes Docker)                    (módulo ecr)

Estado remoto: S3 (tfstate) + DynamoDB (lock)   -> creado por el módulo "bootstrap"
App: contenedor Docker (Python) -> imagen publicada en ECR -> ejecutada en EC2
```

## Stack / Tecnologías
- **IaC:** Terraform (módulos: network, compute, ecr; bootstrap S3+DynamoDB)
- **Cloud:** AWS (VPC, EC2, ECR, S3, DynamoDB, IAM)
- **Contenedores:** Docker
- **App:** Python (CLI con --dry-run, --exclude, --on-collision)
- **Control de versiones:** Git (ramas por feature, PRs)

## Requisitos previos
- Cuenta de AWS y AWS CLI configurado (`aws configure`)
- Terraform >= 1.5
- Docker
- Permisos IAM para VPC, EC2, ECR, S3 y DynamoDB

## Despliegue
```bash
# 1) Crear el backend remoto (una sola vez)
cd Proyecto_DevOps/infra/bootstrap
terraform init
terraform apply

# 2) Desplegar la infraestructura principal
cd ..
terraform init -backend-config=backend.hcl
terraform plan
terraform apply
```

## Objetivos
- Estructurar Terraform en módulos reutilizables (network / compute / ecr).
- Configurar un backend remoto con bloqueo de estado (S3 + DynamoDB).
- Contenerizar una app con Docker y publicar imágenes en ECR.
- Aplicar buenas prácticas de Git y de seguridad (gestión de .gitignore,
  evitar commitear estado y variables sensibles).

## Estado actual
Infraestructura base operativa (network, compute, ecr). Próximos pasos:
push de la imagen a ECR, ejecución automática del contenedor en EC2,
observabilidad con CloudWatch e implementar CI/CD con GitHub Actions.
