# Section 6: Dev Portal

> **Exam Theme:** Developer self-service, secured Try It flow, branding, visibility, roles, and SSO integration.

---

## Q1. What is the primary purpose of the Dev Portal?

**A)** To manage gateway routing rules and upstream services
**B)** To provide a public or private storefront where API consumers can discover, test, and self-service consume APIs
**C)** To track internal technical assets and provide governance scorecards
**D)** To manage user roles and permissions across the Konnect organization

**Answer: B**
*The Dev Portal is the consumer-facing surface of the API platform. It enables developer self-service: browse the API catalog, read documentation, test APIs, and manage applications.*

---

## Q2. What is the correct sequence a developer must follow to use "Try It" for a secured API?

**A)** Register application → Enable user auth → Configure app auth → Link to service → Publish API
**B)** Publish API → Link to gateway service → Configure app authentication strategy → Enable portal user authentication → Developer registers application and uses credential
**C)** Enable user auth → Publish API → Configure app auth → Developer registers application → Link to service
**D)** Configure app auth → Publish API → Developer registers application → Enable user auth → Link to service

**Answer: B**
*The secured Try It sequence is: (1) Publish API, (2) Link to gateway service, (3) Configure app auth strategy, (4) Enable portal user auth, (5) Developer registers app and uses credential.*

---

## Q3. Why is linking the API to an underlying Gateway service necessary in the portal flow?

**A)** It makes the API documentation publicly visible without authentication
**B)** It connects the portal's API specification to the live gateway service so that Try It requests are actually proxied through Kong to the upstream
**C)** It automatically generates authentication credentials for developers
**D)** It enables SSO for portal users

**Answer: B**
*Linking is what connects the "documentation layer" (portal) to the "runtime layer" (gateway). Without this link, Try It has no service to route requests to.*

---

## Q4. What is the role of an application authentication strategy in the Try It workflow?

**A)** It defines how the portal itself authenticates to the Control Plane
**B)** It specifies the authentication method (e.g., API key, OAuth2) that developers must use when calling the API through their registered application
**C)** It controls whether the Dev Portal is public or private
**D)** It manages SSO federation between the portal and an external IdP

**Answer: B**
*The app auth strategy defines the credential type (API key, OAuth2 client credentials, etc.) that the API requires. Developers generate a credential of this type when they register an application.*

---

## Q5. Why is enabling portal user authentication separate from securing the API itself?

**A)** They are not separate — enabling portal auth automatically secures the API
**B)** Portal user authentication controls who can log in to the Dev Portal; API authentication controls what credentials are needed to call the API — these are independent layers
**C)** Portal user authentication is only needed for private portals; public portals never need it
**D)** API security is managed in the Service Catalog, not the Dev Portal

**Answer: B**
*Two distinct controls: (1) Portal user auth = login to the portal UI (who can browse and register). (2) API auth = credentials to call the API (what you need to make a request).*

---

## Q6. When should you choose private visibility instead of public visibility for a Dev Portal?

**A)** When the portal has more than 100 APIs
**B)** When SSO integration is not available
**C)** When the APIs should only be accessible to authenticated, approved developers — not exposed to the general public or web search
**D)** When the APIs are hosted on a Dedicated Cloud Gateway

**Answer: C**
*Private visibility means the portal is not publicly accessible. Developers must authenticate to view API documentation — appropriate for internal or partner-only APIs.*

---

## Q7. What kind of access does the API Viewer role provide?

**A)** Full read/write access to all portal configuration
**B)** The ability to publish and manage APIs in the portal
**C)** Read-only access to API documentation — viewers can browse and read specs but cannot create applications or make authenticated requests
**D)** Admin access to manage developer registrations and approvals

**Answer: C**
*API Viewer is a read-only role. It allows a developer to view API specs and documentation in the portal without being able to register applications or generate credentials.*

---

## Q8. What customization options are explicitly mentioned in the guide for the Dev Portal?

**A)** Custom plugin development and data plane routing rules
**B)** Branding (logo, colors, custom domain), page layouts, and content sections
**C)** Custom Kubernetes operators and Helm chart overrides
**D)** Custom AI model selection and prompt templates

**Answer: B**
*Dev Portal customization covers branding and appearance: your organization's logo, color scheme, custom domain, and the ability to tailor the portal's look to match your brand.*

---

## Q9. How does SSO change the developer onboarding and approval experience?

**A)** SSO removes all approval workflows, allowing any user to register automatically
**B)** SSO allows developers to log in with their existing corporate identity (via an IdP), reducing friction and enabling centralized identity management — approvals can still be configured
**C)** SSO is only available for internal portals with fewer than 50 users
**D)** SSO replaces the need for application authentication strategies

**Answer: B**
*SSO integration (via OIDC/SAML with an external IdP) means developers use existing credentials (e.g., corporate SSO, Google, GitHub) rather than creating new portal accounts.*

---

## Q10. What is the easiest memory phrase to reconstruct the secured Try It sequence?

**A)** "Auth, Link, Publish, Register, Test"
**B)** "Publish, Link, Configure, Enable, Register" — PLCER
**C)** "Register, Configure, Publish, Enable, Link"
**D)** "Enable, Publish, Link, Auth, Register"

**Answer: B**
*PLCER: **P**ublish API → **L**ink to gateway service → **C**onfigure app auth strategy → **E**nable portal user auth → **R**egister application and use credential.*

---

## Q11. A developer reports they can see the API documentation but cannot make authenticated test calls via Try It. The portal admin confirms the API is published and linked to a gateway service. What is the most likely missing step?

**A)** The developer has not been granted the API Viewer role
**B)** Portal user authentication has not been enabled, so the developer cannot register an application and obtain a credential
**C)** The Control Plane is disconnected from the Data Plane
**D)** The API is set to private visibility

**Answer: B**
*Can see docs (published + linked = OK) but cannot authenticate = portal user authentication is not enabled, so the developer registration and credential steps are blocked.*

---

## Q12. Which statement about Dev Portal visibility is TRUE?

**A)** A Dev Portal can only be either fully public or fully private — individual API visibility cannot be controlled
**B)** Public visibility means any internet user can browse API documentation; private visibility restricts access to authenticated portal users only
**C)** Private portals automatically apply OAuth2 to all APIs within them
**D)** Visibility settings apply to the entire Konnect organization, not just the portal

**Answer: B**
*Public = open to anyone (good for external developer ecosystem). Private = must authenticate to the portal to see anything (good for internal or partner APIs).*
