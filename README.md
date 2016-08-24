# OAuth 2.0 Access Token Introspection Policy
This custom policy is a replacement for the Mulesoft EE API Manager "out-of-the-box"
[OAuth 2.0 access token enforcement using external provider policy](https://docs.mulesoft.com/api-manager/external-oauth-2.0-token-validation-policy).
Rather than Mulesoft's (undocumented) approach for token validation, this policy uses the RFC 7662 introspect endpoint to
both validate the Access Token and return the valid scopes and other RFC 7662 response values
for use downstream in the API implementation.

**N.B.** Whereas the Mulesoft policy only needs the
Authorization Token in order to validate it, this policy additionally needs the Client ID and
Secret, since OpenAM will only respond to an introspect that has been validated with Basic Auth
using them.
## Requirements
This policy requires:
 - header Authorization: Bearer *access_token*
 - parameters client\_id and client\_secret

On successful token introspection and validation, it adds the following headers to the flow based
on the keys in the introspection [RFC 7662](https://tools.ietf.org/html/rfc7662#page-6) JSON response.

X-AGW-scope=*scope(s)*
etc. TBD upon further experimentation.

## Developer Notes
### Mulesoft Custom Policy
See https://docs.mulesoft.com/anypoint-studio/v/6/studio-policy-editor and
https://docs.mulesoft.com/api-manager/creating-a-policy-walkthrough

### OpenAM as an OAuth 2.0 server with the RFC 7662 introspect endpoint
OpenAM endpoints are documented here:
https://backstage.forgerock.com/#!/docs/openam/13.5/dev-guide#rest-api-oauth2-client-endpoints

The introspect endpoint requires the following:
 - method POST
 - header Authorization: Basic *HTTPBasicAuth(client\_id,client\_secret)*
 - parameter token=*access\_token*

### Caveats
- This policy has been tested only with OpenAM 14.0, so might not work with other RFC 7662 implementations.
- It has not yet been tested with an HTTPS endpoint (and needs to be fixed to use HTTPS).
- Consider this an Alpha test version!

### Author
Alan Crosswell

Copyright (c) 2016 The Trustees of Columbia University in the City of New York

