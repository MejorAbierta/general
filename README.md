# MejorAbierta

Welcome to the MejorAbierta project repository! This initiative focuses on developing plugins and software enhancements for Open Journal Systems (OJS), Open Monograph Press (OMP), and related platforms to improve authentication, data management, and interoperability.

## Current Focus
* Expand OpenID Support:
  - [ ] Enhance the existing OpenID plugin with multi-configuration capabilities, integrate common providers.
  - [ ] Test redIris implementation.
  - [ ] Test OperasID.
  - [ ] Add new common configurations(RedIris, OperasID).

* [MejorAbiertaPlugin](https://github.com/MejorAbierta/mejorAbiertaPlugin): Create plugins to facilitate seamless data exportation.


---


### State of the art

A review of the different SSO/Auth projects avaliable in OJS/OMP:

(Click for expand)
<details>
<summary>
Official openID ✅
</summary>

[Repo Official openID](https://github.com/pkp/openid) 
- Developer: PKP 
- Ver: OJS 3.4.0
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
> Register all redirect URIs fully where possible. This is the most secure option and what we recommend.
> The ORCID Public **API is free for non-commercial use by individuals** as stated in the [Public APIs Terms of Service](https://info.orcid.org/public-client-terms-of-service/).
> ORCID's current Rate Limit is 24 requests per second for the Member, Public, and Anonymous APIs

# Rediris
**Rediris** has an OpenID implementation:
[How to implement OpenId with Rediris](https://www.rediris.es/sir/howto-openid.html)
For use rediris is needed sign up here getting in touch with the organization https://www.rediris.es/sir/contacto.html.


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

---

### Fecyt plugin analysis
[Fecyt plugin analysis](fecyt-plugin-analysis.md)

