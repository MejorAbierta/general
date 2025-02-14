

---

#### 1. Definition and Purpose

**SSO (Single Sign-On):**

- It is an authentication system that allows users to log in once and access multiple applications or services without needing to re-authenticate.
- The main goal is to improve the user experience by reducing the need to remember multiple credentials.
- Example: A user logs into Nextcloud and can access Nextcloud Talk, OnlyOffice, Mattermost, etc., without re-entering their credentials.

**OAuth2:**

- It is an authorization protocol (not direct authentication) that allows an application to access protected resources on behalf of a user without sharing the user's credentials.
- The main goal is to allow third-party applications to access a user's resources securely and in a controlled manner.
- Example: A mobile app that requests access to your Mastodon account to read your email or view your toots.

---

#### 2. Security

**SSO:**

- Centralizes authentication, which can improve security by reducing the number of credentials users need to remember.
- However, if the IdP (Identity Provider) is compromised, all connected applications may be affected.

**OAuth2:**

- Provides a secure mechanism for third-party applications to access resources without sharing credentials.
- Access tokens have limited scope and duration, reducing the risk if they are compromised.

---

At the end, I understand that **OAuth + OpenID** is more appropriate. We click "Log in with URJC," and once authenticated, it returns a token to automatically log in to OJS or other systems.

---

### **OAuth + OpenID**:
- **OpenID Plugin for OJS**: [https://github.com/pkp/openid](https://github.com/pkp/openid)  
  Tested on OJS 3.4.0.

#### **Features Already Implemented:**
- Authentication via the OpenID provider, meaning the local OJS login is completely replaced. To maintain login security, the JWT (TOKEN) is validated using a public key before the user logs in.
- Registration of new users via the OpenID provider. User data (email, first name, last name, OpenID identifier) is transferred from the provider to OJS and used for registration.
- Merging of existing user accounts: It is possible to connect existing OJS accounts to the OpenID account. This process must be performed by the users themselves to keep administrative effort as simple as possible. Once accounts are connected, local login is disabled for these accounts, and users must authenticate via OpenID.
- Automatic generation of an OJS-API key to simplify the connection between OJS and third-party software. Currently, users must generate this key manually, which is very cumbersome when developing internal software infrastructure.


#### **Explanatory Video on OpenID for OJS (in English):**
- [https://www.youtube.com/watch?v=70RXxeDP2O0](https://www.youtube.com/watch?v=70RXxeDP2O0)


#### **OpenID is Recommended:**
- [http://forum.pkp.sfu.ca/t/options-for-multifactor-authentication-mfa/84187/2](http://forum.pkp.sfu.ca/t/options-for-multifactor-authentication-mfa/84187/2)

---

#### **OJS API Documentation:**
- [https://pkp.sfu.ca/ojs/doxygen/master/html/annotated.html](https://pkp.sfu.ca/ojs/doxygen/master/html/annotated.html)

---

#### **Other Methods:**

**Shibboleth + SAML:**
- [https://github.com/pkp/shibboleth](https://github.com/pkp/shibboleth)  
  Tested on OJS 3.2 and 3.3 → Unable to Access Settings JSON ERROR.

**LDAP:**
- [https://github.com/shemgp/ojs_ldap_plugin](https://github.com/shemgp/ojs_ldap_plugin)  
  Untested.

---

