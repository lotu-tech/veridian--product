# Business Context

## Credit System

The platform uses a credit-based billing model:
- Users purchase subscription plans via Asaas (payment gateway)
- Each plan includes `bonusCredits` that are replenished on billing cycle dates
- **Two credit paths exist — they are NOT duplicates:**
  1. **Webhook credits**: Asaas webhook fires on payment confirmation → adds credits immediately
  2. **Cycle-restart credits (CreditReplenishmentJob)**: BullMQ cron job runs daily, checks confirmed billings with `creditedCredits=false` and `dueDate <= today`, adds bonus credits in a Knex transaction
- The cron job handles the credit cycle restart (when a new billing period begins), which is distinct from the initial payment credit

## OAB Feature (in development, Mar 2026)

"Melhoria na Consulta de Processos por OAB" — improve the OAB (Brazilian Bar Association) process lookup:
- **search-api**: Add OAB search endpoint that queries external OAB data sources
- **investigation-api**: Add endpoint to convert OAB process results into investigation entities (lit.lawsuit)
- **investigation-front**: Add UI for OAB search, process selection, and entity creation
- Split across 3 repos (issues #6, #7, #8 on lotu-tech/veridian--product)

## Domains

- `checkmatch.link` — main web frontend
- `api.checkmatch.link` — investigation-api
- `veridianosint.com.br` — legacy/alternate domain (has CSP configuration)
