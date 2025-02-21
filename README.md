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
- Algorithm: AES-256-CBC
- Authentication Type: Authentication Code Flow
- PKCE: No
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
</details>


