# General stuff for the project

This is general stuff for the project MejorAbierta, aimed to produce plugins and other software for OJS, OMP, etc.

---


# State of the art

A brief review of the different SSO projects avaliable in OJS/OMP:


<details>
<summary>
Official openID
</summary>

[Repo Official openID](https://github.com/pkp/openid) 
- Developer: PKP 
- Ver: OJS 3.4.0 (LATEST) 
- Protocol: openID
- Last update: Dec 12, 2024 
- Data: (RS256) JWT : 
```php
  {
   	'id' => $jwtPayload->sub ?? null,
	'email' => $jwtPayload->email ?? null,
	'username' => $jwtPayload->preferred_username ?? null,
	'given_name' => $jwtPayload->given_name ?? null,
	'family_name' => $jwtPayload->family_name ?? null,
	'email_verified' => $jwtPayload->email_verified ?? null,
  }
  ```
- Review: Working
- Tested Ver : OJS 3.4 - stable
- PKCE: No
- OJS config :
  - URL: example.com/.well-known/openid-configuration
  - ClientID
  - Secret
- Endpoint Config:
  - Signature Algorimth : RS256
  - Authentication Type: Authentication Code Flow
  - PKCE enabled : No
  - OJS URL

**CONFIG ORCID OPENID**
- Go to https://orcid.org/developer-tools, sign in and configure URL and name of our service.
  Notes:
> **Only HTTPS** URIs are accepted in production
> Domains registered MUST exactly match the domains used, including subdomains
> Register all redirect URIs fully where possible. This is the most secure option and what we recommend. For more information about redirect URIs, please see our redirect URI FAQ
> The ORCID Public **API is free for non-commercial use by individuals** as stated in the Public APIs Terms of Service. By “non-commercial” we mean that you may not charge any re-use fees for the Public API, and you may not make use of the Public API in connection with any revenue-generating product or service.

</details>

<details>
<summary>
Official shibboleth
</summary>

[Official shibboleth](https://github.com/pkp/shibboleth)
 
 - Developer : Alec (PKP) 
 - Ver: OJS 3.2+ 
 - Protocol: SAMLv2 
 - Last update: 2 y. ago
 -  Data : 
 -- Shibboleth UIN
 -- first or given name<br/>- last, family, or surname<br/>- personal initials<br/>- e-mail address<br/>- telephone number<br/>- postal mailing address 
 - Review: Stuck on error Settings
 - Tested Ver: 3.3, 3.2
</details>


