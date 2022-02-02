# Overview

This document describes the web services API usage of the Fiserv AllData application. A RESTful API web service handles communication between the partner application and Fiserv, using HTTP protocol over SSL/TLS as the communication medium.

To extract data from Fiserv, the partner application sends a request with certain partner-identifying parameters and user authentication tokens. Upon receipt, the Fiserv server performs the requested task and responds with the results.

The AllData application supports two data formats: XML and JSON. Please contact your AllData sales representative for a link to the updated JSON schema/sandbox testing environment for the web services.

The AllData system provides web service access to:

- add financial institution (&quot;FI&quot;) seed data
- add, delete, and manage users
- add, delete, and manage user accounts with the FIs
- harvest user account information daily and on demand
- retrieve various kinds of transactions to provide accurate and up-to-date information about user assets and holdings

## Scope and Target Audience

The purpose of this document is to provide information to partner architects, developers, and project/product managers to enable them make design decisions to connect to AllData web services.

The scope of this document is limited to AllData web services. Aspects of AllData services that include APIs are mentioned in the context of web services integration. Detailed information on other AllData PFM (personal finance manager) features such as UI portal, widgets, and batch file integrations for host account management are available in separate documents.

# Web Services Setup and Usage

## Web Services Setup

The following are the prerequisites for web services API integration with Fiserv.

1. Environment setup in AllData system
2. Fiserv provides partner with admin user credentials
3. Server URL to submit web services API requests
4. Session handover URL (for single sign-on (SSO))

## Concurrent Access

Currently the AllData system throttle supports up to 50 concurrent sessions of the WS APIs. Fiserv recommends partners do not exceed 50 sessions for optimal performance.

## URLs

Please note that the server URLs differ between UAT and production environments. The URLs are provided as part of the setup.



---

© 2019-2021 Fiserv, Inc. or its affiliates. All rights reserved. This work is confidential, and its use is strictly limited. Use is permitted only in accordance with the terms of the agreement under which it was furnished. Any other use, duplication, or dissemination without the prior written consent of Fiserv, Inc. or its affiliates is strictly prohibited. The information contained herein is subject to change without notice. Except as specified by the agreement under which the materials are furnished, Fiserv, Inc. and its affiliates do not accept any liabilities with respect to the information contained herein and are not responsible for any direct, indirect, special, consequential or exemplary damages resulting from the use of this information. No warranties, either express or implied, are granted or extended by this document.

[http://www.fiserv.com](http://www.fiserv.com/)

Fiserv is a registered trademark of Fiserv, Inc.

Other brands and their products are trademarks or registered trademarks of their respective holders and should be noted as such.