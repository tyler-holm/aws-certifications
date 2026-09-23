# Section 19: Advanced Identity

## AWS STS (security Token Service)
- Enables you to create temporary, limited-privileges credentials to access your AWS resources
- Short-term credentials: you configure expiration period
- Use cases
    - Identity Federation: manage user identities in external systems and provide them with STS tokens to access AWS resources
    - IAM Roles for cross/same account access
    - IAM Roles for Amazon EC2: provide temporary credentials for EC2 instances to access AWS resources

## AWS Cognito
- Identity for your web and mobile applications users (potentially millions)
- Instead of creating them an IAM user, you create a user in Cognito

## AWS Directory Services
- AWS Managed Microsoft AD
    - create your own AD in AWS, manage users locally, supports MFA
    - can establish "trust" connections with your on-premise AD
- AD Connector
    - Directory Gateway (proxy) to redirect to on-premise AD, supports MFA
    - Users are managed on the on-premise AD
- Simple AD
    - AD compatible managed directory on AWS
    - Cannot be joined with on-premise AD

## AWS IAM Identity Center
- EXAM NOTE: one access to multiple accounts = IAM Identity Center
- successor to AWS single sign-on
- one login for all your 
    - AWS accounts in AWS Organizations
    - Business cloud applications (Salesforce, Box, Microsoft 365, etc)
    - SAML2.0-enabled applications
    - EC2 Windows Instances
- Identity providers
    - built-in identity store in IAM identity center
    - 3rd party active directory (AD), OneLogin, Okta

## Summary
- IAM 
    - Identity and Access Management inside your AWS account
    - For users that you trust and belong to your company
- Organizations: manage multiple accounts
- Security Token Service (STS): temporary, limited-privileges credentials to access AWS resources
- Cognito: create a database of users for your mobile and web applications
- Directory Services: integrate Microsoft Active Directory in AWS
- IAM Identity Center: one login for multiple AWS accounts and applications