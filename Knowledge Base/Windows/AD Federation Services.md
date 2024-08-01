# Endpoints
- `/adfs/ls`: responsible for handling browser-based authentication flows
- `/adfs/services/trust/2005/usernamemixed`: Same as above, in legacy.

# Authentication
If sent in `multipart/form-data`, try to see if wildcard(\*) creds accepted.
# Tools
- `Get-AdfsEndpoint`