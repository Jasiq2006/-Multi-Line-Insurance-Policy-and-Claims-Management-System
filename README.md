# Multi-Line Insurance Policy & Claims Management System

## Overview

A Salesforce implementation that centralizes insurance policy data, guides agents through quoting, calculates premiums automatically, routes claims to the right queue, and gives adjusters a focused claims workspace. It covers three policy lines: **Auto, Property and Life**.

## Features

- **Shared Policy object** adapted per product line using Record Types and Field Sets
- **Guided quoting** through the `AutoQuotingFlow` Screen Flow
- **Automatic premium calculation** using the `PremiumCalculator` Apex class
- **Claim routing** to Auto, Property or Life queues by a Record-Triggered Flow
- **High Value Claim Approval** for claims above 50,000 (Senior Adjuster, then Department Manager)
- **Claims dashboard** built with Lightning Web Components, filterable by policy type
- **Data validation** through a VIN rule requiring exactly 17 characters
- **Role-based access** using the Claims Manager and Claims Adjuster permission sets

## Architecture

| Layer | Components |
|-----------------------|--------------------------------------------------------------------|
| Data model | `Policy__c`, `Claim__c`, Contact |
| Configuration | Record Types, Field Sets, Picklists |
| Declarative automation| `AutoQuotingFlow`, claim-routing flow, approval submission action |
| Programmatic logic | `PremiumCalculator`, `ClaimsAdjusterController` |
| User interface | `claimsDashboardLwc`, `claimTileLwc` |
| Security | Permission sets and object permissions |

## Data Model

**Policy** (auto number `P-{0000}`)
- Record types: Auto, Property, Life
- Fields: Customer, Policy Start Date, Policy Term Months, Premium, VIN, Model Year, Square Footage, Year Built, Beneficiary Name

**Claim** (auto number `C-{0000}`)
- Record types: Accident, Property, Life
- Fields: Adjuster, Approval Status, Claim Amount, Date of Loss, Description, Policy
- Approval Status values: New, Submitted for Approval, Approved, Rejected

## Premium Calculation Rules

| Rule | Adjustment |
|--------------------|------------------------|
| Base premium | 1,000.00 |
| Policy State = CA | Base premium x 1.15 |
| Policy State = TX | Base premium x 1.05 |
| Model Year < 2018 | Add 100.00 |

## Claim Processing Flow

1. A claim is created and linked to a Policy.
2. The Record-Triggered Flow reads the Policy Record Type and assigns the claim to the Auto, Property or Life queue.
3. If Claim Amount > 50,000, the claim goes through **High Value Claim Approval**.
4. Adjusters review their assigned claims in the claims dashboard.

## Security

| Permission Set | Access |
|------------------------|-------------------------------------------------------------------------------------|
| Claims Manager Access | Create/Read Policy; Read/Edit Contact; Read-only Claim; Flow User; PremiumCalculator |
| Claims Adjuster Access | Read-only Policy; Read/Edit/Delete Claim; Run Reports; Manage Approvals; ClaimsAdjusterController |

## Setup

1. Create the `Policy__c` and `Claim__c` objects with the fields, record types and field sets above.
2. Deploy the `PremiumCalculator` and `ClaimsAdjusterController` Apex classes.
3. Build the `AutoQuotingFlow` Screen Flow and the claim-routing Record-Triggered Flow.
4. Create the Auto, Property and Life queues.
5. Configure the **High Value Claim Approval** process and the `VIN_Must_Be_17_Characters` validation rule.
6. Deploy the `claimsDashboardLwc` and `claimTileLwc` components.
7. Create the permission sets and assign them to users.

## Tech Stack

Salesforce Lightning, Apex, Flow, Lightning Web Components, Approval Processes
