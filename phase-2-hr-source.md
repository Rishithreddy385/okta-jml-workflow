# Phase 2: Mock HR Source

## Objective
Establish a source-of-truth dataset representing what an HR system 
would feed into the IAM platform. This file drives provisioning 
automation in Phase 3.

## What was built
- CSV file with 10 sample employees
- Covers all 4 departments configured in Phase 1
- Includes 3 lifecycle scenarios:
  - **Joiner** — E009 Emily Brown (Pending status, future start date)
  - **Leaver** — E010 Mark Davis (Terminated status, past end date)
  - **Contractor** — E008 Tom Wilson (Active with bounded end date)
- Manager hierarchy modeled via `manager_email` field
- All emails use `.test` TLD to avoid collision with real domains

## Design decisions
- **Email as primary identifier** rather than employee_id because 
  most downstream IAM platforms (Okta, Entra) key on email or UPN, 
  not internal HR IDs.
- **Separate domain for contractors** (`@contractors.test` vs 
  `@acmecorp.test`) to make contractor identity visible at the 
  email layer — a real-world pattern that simplifies group rules 
  and conditional access policies.
- **Manager hierarchy is flat-by-department** rather than nested 
  multiple levels deep, to keep Phase 3 SCIM mapping simple while 
  still demonstrating manager-based access patterns.
- **Status field separates lifecycle state from employment type** 
  (Active/Pending/Terminated vs FTE/Contractor) because lifecycle 
  and employment classification drive different automation rules.

## What's next
Phase 3: Connect this CSV to Okta as a directory source and 
configure automated provisioning rules that assign users to the 
correct department groups based on the `department` field.
