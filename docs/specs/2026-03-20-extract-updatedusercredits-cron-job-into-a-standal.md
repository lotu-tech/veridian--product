# Feature: Extract UpdatedUserCredits Cron Job to Standalone Lambda

## Summary
Move the `UpdatedUserCredits.ts` cron job from the investigation-api Express process into a standalone AWS Lambda function to eliminate race conditions from multiple API instances and enable independent scaling.

## User Journeys
1. EventBridge triggers Lambda on the same cron schedule (every minute, skipping weekends)
2. Lambda connects to Aurora MySQL using existing connection logic
3. Lambda executes credit paid billings logic: finds unbilled confirmed billings, credits users, creates transactions
4. Lambda executes expire old transactions logic: marks expired transactions, creates debit records, adjusts wallets
5. Lambda logs success/failure to CloudWatch for dev team monitoring
6. On failure, Lambda retries with exponential backoff, then sends alerts

## Acceptance Criteria
- [ ] Only one Lambda instance processes billings at any time (no race conditions)
- [ ] Existing business logic from `UpdatedUserCredits.ts` works unchanged in Lambda
- [ ] Same cron schedule maintained (`UPDATED_CREDITS_SCHEDULE_PATTERN`)
- [ ] CloudWatch logs show processing results for troubleshooting
- [ ] Terraform deploys Lambda with proper IAM, VPC, and EventBridge configuration
- [ ] API instances no longer run the cron job

## Scale & Volume
30 billing transactions/day currently, scaling to 1000/year. Lambda runs every minute but only processes on weekdays during São Paulo business hours.

## Affected Projects
- investigation-api: Remove cron job, keep repository/entity code
- veridian--infra: Add Lambda function, EventBridge rule, IAM roles, VPC config

## Constraints
Timeline: immediate. Must reuse existing database entities and business logic without rewriting core credit processing.