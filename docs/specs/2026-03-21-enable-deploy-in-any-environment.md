# Feature: Manual Branch Deployment Control

## Summary
Replace automatic main-branch deployment with manual deployment control, allowing any branch to be deployed to production on-demand. This enables feature testing in production environment and rollback capabilities.

## User Journeys
1. Developer completes feature work on feature branch
2. Developer navigates to GitHub Actions in the repository
3. Developer selects "Deploy" workflow and clicks "Run workflow"
4. Developer selects target branch from dropdown menu
5. Developer confirms deployment
6. GitHub Actions deploys selected branch to production environment

## Acceptance Criteria
- [ ] Automatic deployment on main branch push is removed
- [ ] Manual workflow dispatch trigger is added with branch selection input
- [ ] Any branch can be selected for deployment to production
- [ ] Deployment process and target environment remain unchanged
- [ ] Workflow can be triggered by authorized team members only

## Scale & Volume
- Low frequency deployments (~every 2 days)
- Small development team
- Single production environment target

## Affected Projects
- Current repository: Modify existing GitHub Actions workflow file to replace automatic trigger with manual dispatch

## Constraints
- Must maintain existing production deployment process and target
- No new environments created (AWS account setup pending)
- Out of scope: Creating dev/staging environments, approval workflows, deployment rollback automation