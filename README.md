# Lab M7.05 - Creating Budgets and Cost Alerts

**Repository:** https://github.com/MaryaAhmadi/ce-lab-creating-budgets-alerts.git

**Activity Type:** Individual  
**Estimated Time:** 45-60 minutes

## Learning Objectives
Based on Module 7 Day 3 lessons

## Prerequisites

- [ ] AWS account with appropriate permissions
- [ ] Completed Module 7 lessons through Day 3

## Introduction

This lab applies concepts from the lessons to real AWS resources.


# Lab M7.05 - Creating Budgets and Alerts

## Overview
In this lab, I implemented a complete AWS budget monitoring and enforcement system using AWS Budgets, SNS, and IAM. The goal was to proactively detect cost overruns and automatically restrict resource creation when budget limits are reached.

---

## What I Did
- Created an SNS topic (`budget-alerts`) and confirmed an email subscription for budget notifications
- Configured a **monthly cost budget ($50)** with multiple alert thresholds:
  - 50% (early awareness)
  - 80% (warning)
  - 100% (critical)
  - 100% forecasted (predictive alert)
- Created a **service-specific EC2 budget ($30)** to isolate compute costs
- Created a **tag-based budget ($25)** filtered by `Environment=development`
- Implemented an **automated budget action**:
  - Trigger: 100% actual spend
  - Action: Apply IAM deny policy
  - Target: `developers` IAM group
- Built documentation:
  - `alert-flow.md` → alert behavior and response plan
  - `budget-dashboard.md` → centralized budget configuration view

---

## Architecture / Workflow

1. AWS Budgets monitors cost usage
2. Thresholds trigger notifications
3. Notifications are sent to SNS
4. SNS delivers alerts via email
5. At 100% threshold:
   - AWS Budgets assumes a role
   - Applies a deny IAM policy
   - Prevents new resource creation

---

## Key Findings

- **Multi-threshold alerts are critical**: They provide progressive visibility (early → warning → critical)
- **Forecasted alerts are powerful**: They allow proactive action before overspending occurs
- **Service-level budgets help isolate cost drivers** (e.g., EC2 is often the biggest contributor)
- **Tag-based budgets enable environment-level cost tracking**
- **Automated actions turn monitoring into enforcement**, preventing further cost escalation
- **SNS is reliable for alert delivery**, but requires confirmed subscriptions
- **Cost allocation tags must be enabled** in Billing for tag-based budgets to work correctly

---

## Challenges & Lessons Learned

### 1. SNS Subscription Not Delivering Emails
- Issue: Test emails were not received initially  
- Cause: Subscription was not confirmed or SNS ARN was not set in the shell session  
- Fix:
  - Confirm email via AWS notification link
  - Re-check `SNS_ARN` variable before publishing

---

### 2. Environment Variable Loss (SNS_ARN)
- Issue: `$SNS_ARN` was empty in a new terminal session  
- Impact: Notifications were not sent correctly  
- Fix:
  - Re-fetch ARN using:
    ```bash
    aws sns list-topics
    ```
  - Manually reassign variable

---

### 3. IAM Policy / Role Permissions
- Challenge: Understanding how AWS Budgets assumes roles to apply policies  
- Key Learning:
  - Budgets requires a role with `sts:AssumeRole`
  - Policy attachment permissions are essential

---

### 4. Tag-Based Budget Visibility
- Issue: Tag-based budget may not show data immediately  
- Cause: Cost allocation tags not activated  
- Fix:
  - Enable tags in AWS Billing → Cost Allocation Tags

---

## Screenshots
- `sns-subscription-confirmed.png`
- `budget-list-console.png`
- `budget-alerts-thresholds.png`
- `budget-action-configuration.png`

---

## Budget Configuration Summary

| Budget Name | Scope | Limit | Alerts |
|-------------|-------|------|--------|
| Monthly-Dev-Budget | All services | $50 | 50%, 80%, 100% actual + forecast |
| EC2-Monthly-Budget | EC2 only | $30 | 80%, 100% |
| Dev-Environment-Budget | Tag-based | $25 | 80%, 100% |

---

## Automated Control

| Trigger | Action | Target |
|--------|--------|--------|
| 100% Actual Spend | Apply IAM Deny Policy | developers group |

Blocked actions:
- `ec2:RunInstances`
- `ec2:CreateVolume`
- `rds:CreateDBInstance`
- `lambda:CreateFunction`

---

## Improvements / Future Work

- Implement budgets using **Terraform** for infrastructure as code
- Add **Slack / Teams notifications** via SNS + Lambda
- Create **separate budgets for production environments**
- Add **daily cost anomaly detection** (AWS Cost Anomaly Detection)
- Replace IAMFullAccess with **least-privilege policy**
- Extend automated actions to:
  - Stop EC2 instances
  - Scale down resources automatically

---

## Conclusion

This lab demonstrated how to move from **passive cost monitoring** to **active cost control** in AWS. By combining budgets, alerts, and automated IAM enforcement, it is possible to build a robust financial governance system that prevents unexpected cloud costs.


## Submission

Submit GitHub repository with:
1. Lab report documenting your work
2. Screenshots of results
3. README with summary

## Verification Checklist

- [ ] Completed all required steps
- [ ] Documented findings
- [ ] Captured screenshots
- [ ] Submitted to GitHub

## Additional Resources

Refer to Module 7 Day 3 lessons and AWS documentation.

**Good luck! 🚀**
