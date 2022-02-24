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
