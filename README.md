# Lab 11 – Infraestructura Big Data como Código (Terraform + Floci)

Laboratorio del curso Business Intelligence y Big Data (UCV). Terraform aprovisiona sobre Floci (AWS local emulado) un Data Lake en S3 y bases de datos de Glue Data Catalog por dominio. No se usa ninguna cuenta AWS real.

## Contenido

- `docker-compose.yml`: levanta Floci en http://localhost:4566 (contenedor `lab11-ucv-floci`).
- `terraform/`: `versions.tf`, `provider.tf`, `variables.tf`, `main.tf` y `outputs.tf`.

## Qué crea

- Bucket `ucv-data-platform`.
- Prefijos `dominio/capa/` para cada dominio (`sales`, `customers`, `products`) y capa (`bronze`, `silver`, `gold`).
- Una base de datos de Glue por dominio (`<dominio>_domain`).

## Uso

```powershell
docker compose up -d
cd terraform
terraform init
terraform validate
terraform plan
terraform apply
```

Verificación con AWS CLI:

```powershell
aws --endpoint-url http://localhost:4566 s3 ls s3://ucv-data-platform/ --recursive
aws --endpoint-url http://localhost:4566 glue get-databases
```

Para eliminar todo: `terraform destroy`.

## Notas

- El archivo `terraform.tfstate`, la carpeta `.terraform/` y `data/` no se versionan.
- El Lab 09 también usa el puerto 4566; no deben ejecutarse a la vez. El contenedor de este laboratorio se llama `lab11-ucv-floci` para no chocar con el nombre del Lab 09.
- Las credenciales `test`/`test` son ficticias.