# Feature: Move Expiring Transactions to Bull Queue

## Summary
Extract the expiring transactions logic from the current cron job into a Bull queue to eliminate concurrency issues when scaling to multiple ECS tasks. Remove redundant payment billing processing that's already handled by Asaas webhooks.

## User Journeys
1. System administrator: Credits that expire are automatically debited from user wallets once daily without double-processing
2. Development team: Can scale API instances without worrying about concurrent cron jobs affecting user credits

## Acceptance Criteria
- [ ] Remove the current `src/jobs/UpdatedUserCredits.ts` cron job entirely
- [ ] Create a new Bull queue job for expiring transactions that runs once daily
- [ ] Ensure only one instance of the expiring transactions job runs across all ECS tasks
- [ ] Preserve the exact same expiring transactions logic (transaction expiry + wallet balance updates)
- [ ] Configure the job to run daily at a specific time (e.g., 2 AM São Paulo time)
- [ ] Add proper logging for monitoring the queue job execution

## Scale & Volume
- 1000 users expected, with daily transaction expiry processing
- Job runs once per day across multiple ECS tasks without conflicts
- Uses existing BullMQ infrastructure already in place for search-api

## Affected Projects
- investigation-api: Remove cron job, add Bull queue job, configure queue worker

## Constraints
- Must preserve existing expiring transactions logic exactly
- Should not affect current webhook-based payment processing
- Use existing BullMQ setup patterns from search-api