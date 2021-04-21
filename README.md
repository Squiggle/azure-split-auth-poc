# Azure API Experiment

We have an interactive website which needs placing behind authentication, with the exception of one
endpoint which needs to be publically accessible.

How do we host this in a cost-effective manner within Azure?

## Constraints

- The web application is authenticated with Azure AD
- The endpoint can be called externally from a web hook EITHER with a long-lived token OR with no token at all
- The expense of hosting is minimal (this means no VPC, API Gateway etc)
