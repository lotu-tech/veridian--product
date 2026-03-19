# Veridian Infrastructure Context

## Architecture

Three ECS Fargate services behind an ALB:
- investigation-api (Express, port 80) → api.checkmatch.link
- investigation-front (Next.js, port 3000) → checkmatch.link
- search-api (NestJS, port 3333) → internal only (AWS Cloud Map)

## Data Stores

| Service | Engine | Notes |
|---|---|---|
| Aurora MySQL | 8.0 Serverless v2 | 0.5–4 ACU, main relational DB |
| DocumentDB | 5.0-compat | MongoDB-compatible, for document storage |
| Valkey | 7 Serverless | Redis-compatible cache (5 GB) |
| S3 | — | File storage, exports, reports |

## Terraform Structure (3 layers)
1. `00-foundation/` — VPC (2 AZs), security groups, GitHub OIDC
2. `01-data/` — Aurora MySQL, DocumentDB, Valkey, S3
3. `02-compute/` — ECS Fargate, ALB, ACM, Route 53, ECR, SSM

## Deployment
- Services deployed as Docker images to ECR → ECS Fargate
- CI/CD via GitHub Actions
- Region: sa-east-1 (São Paulo)
