---
tags: [account aggregation, account aggregation service, account aggregation software, account balances, ACH account verification, ACH transaction, aggregate data, aggregated data, aggregation service, AllData, asset verification, bank account aggregation, bank account verification, bank verification, data aggregation, data aggregation tools, data aggregator, employment verification, finance technology, financial aggregator, identity verification, income verification, instant bank verification, personal finance management, proof of assets, verify account, verify bank account, verify employment, verify income]
---

# Common Data Aggregation Use Cases and AllData® API Solutions

# Introduction

The purpose of this document is to provide guidance to Fiserv partners on the order in which they should call the AllData APIs as well as the APIs to call for specific use cases and solutions. This document is meant for API partners that do not use Fiserv widgets.

## Overview

In all instances noted below, the partner must first be created in the AllData system. The Fiserv team creates the partner&#39;s administrative account and whitelists the partner&#39;s IP address.

Each partner will have a partner ID and at least one home ID created in the AllData system. Some partners may choose to use multiple home IDs depending on different use cases. A partner can provision multiple customers (users) under its home ID. A user may have more than one financial institution account under his or her profile.

For all use cases outlined below, the partner must create a user profile using the [CreateUser API](../api/?type=post&path=/WealthManagementWeb/ws/UserMgmt/createUser). The user profile consists of the user ID, password, and several optional fields including name, address, etc.

Partners must follow the bootstrapping process to synchronize the Fiserv AllData connections directory on their end. This one-time process uses the [GetFinancialInstInfo API](../api/?type=post&path=/WealthManagementWeb/ws/SeedDataInq/getFinancialInstInfo). Partners can also periodically refresh to get the latest changes made by financial institutions. See the [AllData Web Services API Specifications](?path=docs/alldata-ws-api.md) document for additional details.

# Use Cases

This chapter describes several use cases or solutions and lists the APIs they require.

### Bank account ownership verification

A wide variety of organizations can benefit from bank account ownership verification, such as lenders (including mortgage lenders), ACH payment processors, e-commerce platforms, and wealth management organizations. 

This use case is necessary for a partner to verify the bank account ownership of its end users. This can be accomplished through AllData by exposing the following APIs that allow end users to connect to their FI accounts using their login credentials.

**APIs required:**

- [initiateAddAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts)
- [getAddAccountStatus](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) (1..n)
- [getNewAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts)
    * **Note:** This series of APIs only validates that the user can log in and has ownership of the account. If the partner requires routing and account numbers, see “Add payment account” below.

### Balance verification

Balance verification is useful for lenders, ACH payment processors, e-commerce platforms, and others.

This use case is necessary for a partner to verify the balance(s) of the account(s) linked to the consumer.

**APIs required:**

- [initiateAddAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts)
- [getAddAccountStatus](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) (1..n)
- [getNewAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts)

### Add payment account

Adding a payment account is useful for organizations specializing in ACH money movement, which can include lenders (including mortgage lenders), e-commerce platforms, and bill payment providers. 

AllData can be used to add an ACH payment account of an end user after that account has been verified. The full account number and routing number must be collected for the account to add it as a payment account. 

**APIs required:**

- [initiateAddAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts)
- [getAddAccountStatus](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) (1..n)
- [getNewAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts)
- [createAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/createAccounts)
- [updateAccounts](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/updateAccounts)
- [getHarvestStatus](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/getHarvestStatus) (1..n)
- [getAccountDetails](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountDetails)

### Transaction history

Transaction history is useful for any organization requiring insight into consumer account activity.

AllData can be used to obtain the transaction history for a number of accounts including savings, checking, credit card, money market, and investments. 

**APIs required:**

- [initiateAddAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts)
- [getAddAccountStatus](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) (1..n)
- [getNewAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts)
- [createAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/createAccounts)
- [updateAccounts](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/updateAccounts)
- [getHarvestStatus](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/getHarvestStatus) (1..n)
- [getAccountDetails](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountDetails)
- For banking account transactions, use [getBankingTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getBankingTrans)
- For biller account transactions, use [getOtherAccountTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getOtherAccountTrans)
- For credit card account transactions, use [getCreditCardTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getCreditCardTrans)
- For investment account transactions, use [getInvestmentTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getInvestmentTrans)

### Asset verification

Asset verification is useful for lenders, who often need to know the net worth of their borrowers.

AllData can be used to verify the assets held by an end user. These assets are grouped into categories including bank accounts, investment accounts, retirement accounts, home values, and insurance policies.

**APIs required:**

- [initiateAddAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts)
- [getAddAccountStatus](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) (1..n)
- [getNewAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts)
- [createAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/createAccounts)
- [updateAccounts](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/updateAccounts)
- [getHarvestStatus](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/getHarvestStatus) (1..n)
- [getAccountDetails](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountDetails)
- [getOtherAccountTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getOtherAccountTrans)
- [getEmployerStockOptions](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getEmployerStockOptions)
- [getInvestmentPos](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getInvestmentPos)
- [getInvestmentTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getInvestmentTrans)

### Liability verification

Liability verification is useful for lenders because they often need to know the outstanding financial commitments of their borrowers.

AllData can be used to verify the liabilities of an end user. Liabilities are grouped into categories including credit cards, mortgages, loans, and bills.

**APIs required:**

- [initiateAddAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/initiateAddAccounts)
- [getAddAccountStatus](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getAddAccountStatus) (1..n)
- [getNewAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/getNewAccounts)
- [createAccounts](../api/?type=post&path=/WealthManagementWeb/ws/AccountMgmt/createAccounts)
- [updateAccounts](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/updateAccounts)
- [getHarvestStatus](../api/?type=post&path=/WealthManagementWeb/ws/HarvestAccountData/getHarvestStatus) (1..n)
- [getAccountDetails](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getAccountDetails)
- [getOtherAccountTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getOtherAccountTrans)
- [getCreditCardTrans](../api/?type=post&path=/WealthManagementWeb/ws/AccountDataInq/getCreditCardTrans)

### Personal Financial Management (PFM)

PFM is used for partners who want to provide end users with budgeting tools, savings goals, statements (including bank statements and tax documents), forecasting, home values, and cash flow analysis. 

AllData can be configured to categorize transactions. 

All the APIs noted in the above use cases are applicable to PFM since PFM tools are typically used to provide a complete picture of an end user’s financial position. In addition to the APIs noted above, the following can also be applied to the PFM use case:

- [categorizeTransaction](../api/?type=post&path=/WealthManagementWeb/ws/TxnMgmt/categorizeTransaction)
- [deleteSubCategory](../api/?type=post&path=/WealthManagementWeb/ws/TxnMgmt/deleteSubCategory)
- [createBudget](../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/createBudget)
- [editBudget](../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/editBudget)
- [deleteBudget](../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/deleteBudget)
- [findBudget](../api/?type=post&path=/WealthManagementWeb/ws/BudgetMgmt/findBudget)
- [editGoal](../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/editGoal)
- [deleteGoal](../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/deleteGoal)
- [findGoal](../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/findGoal)
- [createGoal](../api/?type=post&path=/WealthManagementWeb/ws/GoalMgmt/createGoal)
