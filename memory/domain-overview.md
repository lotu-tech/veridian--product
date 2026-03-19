# Veridian / CheckMatch Domain Overview

Veridian (branded as CheckMatch) is an investigation and background check platform used by law enforcement and security professionals in Brazil.

## Repositories and Responsibilities

| Repo | What it does | Stack |
|------|-------------|-------|
| investigation-api | Backend API — investigation management, data sources, reports | Express + TypeScript, Aurora MySQL, DocumentDB, S3 |
| investigation-front | Web frontend — investigation dashboards, search, reports | Next.js 15 + React 19 + Tailwind + Redux |
| search-api | Internal search service — aggregates data from multiple sources | NestJS + BullMQ, internal only (Cloud Map) |
| veridian--infra | All AWS infrastructure (Terraform) | Terraform, ECS Fargate, Aurora, DocumentDB, Valkey |
| export-scripts | Data export and document generation utilities | Node.js, docx, exceljs |
| oab-api | OAB (Brazilian Bar Association) data service | (empty/new) |

## Infrastructure

- **Cloud:** AWS, region `sa-east-1` (São Paulo)
- **Compute:** ECS Fargate (3 services behind ALB)
- **Database:** Aurora MySQL Serverless v2 + DocumentDB
- **Cache:** Valkey Serverless
- **Storage:** S3
- **Domain:** checkmatch.link (api.checkmatch.link for API)
- **Terraform:** 3-layer stack (foundation → data → compute)

## What Changes Go Where

- New API endpoint → investigation-api
- New UI page/component → investigation-front
- New search data source → search-api
- New AWS resource → veridian--infra
- New export/report format → export-scripts
- OAB integration → oab-api
