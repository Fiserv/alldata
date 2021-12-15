# AllData®

## How to Use AllData APIs and Add Account Widget for Various Use Cases

May 2021


© 2021 Fiserv, Inc. or its affiliates. All rights reserved. This work is confidential, and its use is strictly limited. Use is permitted only in accordance with the terms of the agreement under which it was furnished. Anyother use, duplication, or dissemination without the prior written consent of Fiserv, Inc. or its affiliates is strictly prohibited. The information contained herein is subject to change without notice. Except as specified by the agreement under which the materials are furnished , Fiserv, Inc. and its affiliates do not accept any liabilities with respect to the information contained herein and are not responsible for any direct, in direct, special, consequential or exemplary damages resulting from the use of this information. No warranties, either express or implied , are granted or extended by this document.

[http://www.fiserv.com](http://www.fiserv.com)

Fiserv is a registered trademark of Fiserv, Inc.

Other brands and their products are trademarks or registered trademarks of their resp ective holders and should be noted as such.

This document has been created by Fiserv and is classified confidential. This document is restricted to the received party and not to be forwarded or transferred without the approval of Fiserv.

## Introduction Contents

The purpose of this document is to advise Fiserv partners that plan to use the Add Account widget along with APIs to handle specific use cases.

### Overview 

Before utilizing any of the following use cases, you must have a profile in the Fiserv system. The professional services team at Fiserv creates your administrative account and whitelists your IP address(es).

You will be assigned a unique partner ID and a home ID (or multiple home IDs, depending on your needs). You may provision multiple customers (users) under a home ID. A user may have more than one financial institution (“FI”) or any other accounts and connection types offered by AllData under their profile.

![figure 1](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/01-add-account-widget.png)

For all use cases outlined below, you must create a user profile using the CreateUser API. The user profile consists of the user ID, password, and several optional fields such as name and address.
CreateUser returns a value called CEUserID which is the unique identifier for the end user. 

#### Partner flow for Add Account widget

Before launching the Add Account widget, you must call the signOn API to generate a single sign-on (SSO) token. After generating the token, launch the AllData Add Account widget per the instructions in the AllData Next-Gen Widgets Integration Guide. When launched, the widget appears as below.

![figure 2](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/02-add-account-widget.png)
![figure 3](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/03-add-account-widget.png)
![figure 4](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/04-add-account-widget.png)
![figure 5](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/05-add-account-widget.png)
![figure 6](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/06-add-account-widget.png)
![figure 7](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/07-add-account-widget.png)

## Use Cases
This chapter describes use cases for the following scenarios:

- Bank account ownership verification
- Balance verification
- Add payment account
- Transaction history
- Asset verification
- Liability verification
- Personal Financial Management (PFM)

#### Bank account ownership verification
A wide variety of organizations can benefit from bank account ownership verification, such as lenders (including mortgage lenders), ACH payment processors, e-commerce platforms, and wealth management organizations.

This use case is necessary to verify the bank account ownership of your end users. This can be accomplished through AllData by exposing the following APIs that allow end users to connect to their FI
accounts using their login credentials.

**Perform the following actions:**
1. Invoke the createUser API.
2. Invoke the signOn API and get a session token.
3. Launch the Add Account widget per the AllData Next-Gen Widgets Integration Guide instructions.
4. Invoke the getAccountUpdateSummary API. – This is only required if you configure the widget to hide both the account classification page and account confirmation page.
5. Invoke the getAccountDetails API.
6. Invoke the getAccountsSummary API. (optional)
7. Invoke the getBankingTrans API. – This is optional; invoke if you need to see transaction details.

#### Balance verification
Balance verification is useful for lenders, ACH payment processors, e-commerce platforms, and others. This use case is necessary to verify the balance(s) of the account(s) linked to a consumer.

**Perform the following actions:**

1. Invoke the createUser API.
2. Invoke the signOn API and get a session token.
3. Launch the Add Account widget per the AllData Next-Gen Widgets Integration Guide instructions.
4. Invoke the getAccountUpdateSummary API. – This is only required if you configure the widget to hide both the account classification page and account confirmation page.
5. Invoke the getAccountDetails API. – This call returns full account owner name, various account balance types, routing number, and account numbers (both partly masked and fully unmasked, if available).
![figure 8](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/08-add-account-widget.png)

#### Add payment account
Adding a payment account is useful for organizations specializing in ACH money movement. Example organizations are lenders (including mortgage lenders), e-commerce platforms, and bill payment providers.

You can use AllData to retrieve an end user’s full account number(s) and routing number and then use the number to add the account as a payment account. Contact your account executive from Fiserv for the current list of FIs providing this account-level information.

**Perform the following actions:**

1. Invoke the createUser API.
2. Invoke the signOn API and get a session token.
3. Launch the Add Account widget per the AllData Next-Gen Widgets Integration Guide instructions.
4. Invoke the getAccountUpdateSummary API. – This is only required if you configure the widget to hide both the account classification page and account confirmation page.
5. Invoke the getAccountDetails API. – This call returns full account owner name, various account balance types, routing number, and account numbers (both partly masked and fully unmasked, if available).
![figure 8](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/08-add-account-widget.png)

#### Transaction history

Transaction history is useful for any organization requiring insight into consumer account activity.

Where available, AllData can obtain transaction history for the following account types:

- Savings
- Checking
- Credit card
- Money market
- Outstanding loans (including mortgages)
- Bills
- Insurance
- Home values (via Zillow)
- Brokerage
- Retirement savings
- Employee stock options

**Perform the following actions:**

1. Invoke the createUser API.
2. Invoke the signOn API.
3. Launch the Add Account widget per the AllData Next-Gen Widgets Integration Guide instructions.
4. Invoke the getAccountUpdateSummary API. – This is only required if you configure the widget to hide both the account classification page and account confirmation page.
5. Invoke the getAccountDetails API.
6. Invoke the getAccountsSummary API. (optional)
7. Invoke the getBankingTrans API. – This is optional; invoke if you need to see transaction details.
    * For banking account transactions, use the getBankingTrans API.
    * For biller account transactions, use the getOtherAccountTrans API.
    * For credit card account transactions, use the getCreditCardTrans API.
    * For investment account transactions, use the getInvestmentTrans API.

#### Asset verification

Asset verification is useful for lenders, who often need to know the net worth of their borrowers.

You can use AllData to verify your customer assets. Assets are grouped into the following categories:

- Savings
- Checking
- Money market
- Insurance
- Home values (via Zillow)
- Brokerage
- Retirement savings
- Employee stock options

**Perform the following actions:**

1. Invoke the createUser API.
2. Invoke the signOn API.
3. Launch the Add Account widget per the AllData Next-Gen Widgets Integration Guide instructions.
4. Invoke the getAccountUpdateSummary API. – This is only required if you configure the widget to hide both the account classification page and account confirmation page.
5. Invoke the getAccountDetails API. – This call returns full account owner name, various account balance types, routing number, and account numbers (both partly masked and fully unmasked, if available).
![figure 8](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/08-add-account-widget.png)
6. Invoke the getOtherAccountTrans API. – This can include insurance, biller accounts, loans, and other liability accounts.
7. Invoke the getEmployerStockOptions API.
8. Invoke the getInvestmentPos API.
9. Invoke the getInvestmentTrans API.

#### Liability verification
Liability verification is useful for lenders, often need to know the outstanding financial commitments of their borrowers.

You can use AllData to verify your customer liabilities. Liabilities are grouped into categories of bills, credit cards, loans, and mortgages.

**Perform the following actions:**

1. Invoke the createUser API.
2. Invoke the signOn API.
3. Launch the Add Account widget per the AllData Next-Gen Widgets Integration Guide instructions.
4. Invoke the getAccountUpdateSummary API. – This is only required if you configure the widget to hide both the account classification page and account confirmation page.
5. Invoke the getAccountDetails API. – This call returns full account owner name, various account balance types, routing number, and account numbers (both p artly masked and fully unmasked, if available).
![figure 8](https://raw.githubusercontent.com/Fiserv/alldata/md-testing/assets/images/08-add-account-widget.png)
6. Invoke the getOtherAccountTrans API. – This can include insurance, biller accounts, loans, and other liability accounts.
7. Invoke the getCreditCardTrans API. – This is specifically for credit card transactions.

#### Personal Financial Management (PFM)

PFM is useful for partners who want to provide end users with budgeting tools, savings goals, statements (including bank statements and tax documents), forecasting, home values, and cash flow analysis.

AllData’s categorization engine automatically assigns transactions to predefined categories. To activate and configure this premium feature, contact your Fiserv representative.

All the APIs in the above use cases also apply to PFM from a data-collection perspective. The following APIs provide PFM-specific functionality.
- createBudget API
- editBudget API
- findBudget API
- deleteBudget API
- createGoal API
- editGoal API
- findGoal API
- deleteGoal API
- categorizeTransaction API
  Note: This API is used to update categorization in formation for specific transactions by assigning or updating a new subcategory for the transactions. The new subcategory will be auto-assigned to similar transactions in the future.

- deleteSubCategory API


