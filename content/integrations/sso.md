---
summary: "Help for setting up Single Sign-on"
bodyClass: 'employer-focused'
---

# Single Sign-on

## SAML

Qualified currently supports SAML based single sign-on (SSO). With this feature, you can manage the login credentials
for your Qualified users and allow then to login via a central identity portal.

### Activation

In order to enable SAML SSO on your account, please contact your account executive. This feature is currently only supported on custom plans.

### Setup

You will need to contact us to configure your SAML integration. After you contact us we will provide with a form to fill out. That form will ask you for the following fields:

##### Identity Provider Issuer
This is the unique value that tells us how to identify you with your identity provider. It should be a URL. For example, if you are using Okta as your identity provider, the issuer would look something like `http://www.okta.com/exlm31xb1z3gqaucp3n4`.

##### Identity Provider Metadata
This is the metadata provided by your identity provider. There are two forms of this data that you could provide, either static and dynamic.

**Static** metadata will be in the format of XML, and looking something like
```
<?xml version="1.0" encoding="UTF-8"?><md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata"...
```

**Dynamic** metadata will be in the format of a URL, and will look something like
`https://dev-404632.okta.com/app/exlm31xb1z3gqaucp3n4/sso/saml/metadata` (in this case, we are again using Okta as an example).

If you have access to dynamic metadata, that will be preferred since it will allow us to automatically pick up changes to your setup configuration.

#### Additional Setup
With the Issuer and Metadata information provided, Qualified will be able to configure your SAML integration settings for you. In addition, you may need to configure some settings within your identity provider.

##### Single Sign On URL
If you are asked to provide this URL, you should set it to `https://www.qualified.io/api/saml/consume`

##### Reply URL
If you are required to provide this value, you should set it to `https://www.qualified.io/api/saml/consume`. However if
you are not required to set this value, you should leave it blank. If you set this value, and get a 404 error while testing your flow,
it is likely because this value is set when it should be left blank.


##### Audience Restriction
If you are asked to provide this value, you should set it to `https://qualified.io`

##### Entity ID
If you are asked to provide this value, you should set it to `https://qualified.io`

### Logging in with SAML
Your users should initially login through your identity provider's portal. Once this is done, a cookie to your application specific login URL will be set for each user, which makes it possible for them to login via the login page. They will simply need to click "SSO LOG IN".

### SCIM Integration
If you are looking to support user provisioning with your SSO integration, please contact us.
