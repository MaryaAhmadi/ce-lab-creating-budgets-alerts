# Budget Dashboard — Development Account

## Active Budgets

| Budget Name | Scope | Monthly Limit | Alerts |
|-------------|-------|---------------|--------|
| Monthly-Dev-Budget | All services | $50 | 50%, 80%, 100% actual + 100% forecasted |
| EC2-Monthly-Budget | EC2 only | $30 | 80%, 100% actual |
| Dev-Environment-Budget | Tag: Environment=development | $25 | 80%, 100% actual |

## Notification Channel
- **SNS Topic:** budget-alerts
- **Subscribers:** maryamahmadi.apply@gmail.com (email, confirmed)

## Automated Actions
| Budget | Trigger | Action | Target |
|--------|---------|--------|--------|
| Monthly-Dev-Budget | 100% actual | Apply deny policy | developers IAM group |

## Deny Policy Scope
When triggered, the following actions are blocked:
- `ec2:RunInstances`
- `ec2:CreateVolume`
- `rds:CreateDBInstance`
- `lambda:CreateFunction`

## Monthly Review Checklist
- [ ] Check actual vs. budgeted spend for each budget
- [ ] Review any alerts triggered during the month
- [ ] Verify SNS subscription is still active
- [ ] Adjust budget thresholds if spending patterns have changed
- [ ] Confirm automated action has not been triggered (or resolve if it has)
