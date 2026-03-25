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

---

## Q13. What is an "application" in the context of the Dev Portal developer workflow?

**A)** A mobile or web application built by Kong that wraps the Dev Portal UI
**B)** A developer-registered entity that represents a client application consuming the API — credentials are generated and attached to this application for authentication
**C)** A Kong Gateway service object exposed in the portal
**D)** A group of APIs bundled together for discovery purposes

**Answer: B**
*Applications are developer-side entities. A developer creates an application (e.g., "My Mobile App"), registers it against an API, and receives credentials — the registration lifecycle.*

---

## Q14. What happens when a developer registers an application against an API in the Dev Portal?

**A)** The gateway automatically creates a new route for the application
**B)** Konnect generates credentials (e.g., API key or OAuth2 client credentials) for the application, associated with the configured authentication strategy, which the developer then uses to call the API
**C)** The API is automatically published to the public internet
**D)** The developer immediately gains admin access to the gateway configuration for that API

**Answer: B**
*Application registration triggers credential generation. The developer gets the credential (API key, client ID/secret, etc.) that they must include in API calls to authenticate.*

---

## Q15. How does the Dev Portal support the "shift-left" approach to API consumption?

**A)** By automatically generating API implementations from OpenAPI specs
**B)** By allowing developers to discover APIs, read documentation, and test APIs early in their development process — before writing any integration code
**C)** By enforcing coding standards during API development
**D)** By requiring approval workflows before any API is developed

**Answer: B**
*Self-service discovery and Try It let developers explore APIs without waiting for a platform team to set up access. Earlier exploration = earlier feedback and faster integration.*

---

## Q16. What is the role of the "portal user" in the Dev Portal access model?

**A)** A portal user is an internal Konnect admin who manages the portal configuration
**B)** A portal user is an API consumer (external developer) who has registered an account on the Dev Portal to access documentation and manage applications
**C)** A portal user is a service account used by the gateway to authenticate to the portal
**D)** Portal users are the same as Konnect organization members

**Answer: B**
*Portal users are external (or internal) developers — separate from Konnect org members. They interact with the portal as API consumers, not as platform administrators.*

---

## Q17. What is the difference between "publishing" an API to the Dev Portal and "linking" it to a gateway service?

**A)** Publishing and linking are the same action with different names
**B)** Publishing makes the API specification visible in the portal catalog; linking connects the published spec to a live gateway service so that Try It requests are actually proxied
**C)** Publishing is for private portals; linking is for public portals
**D)** Linking is required before publishing, and they cannot be done in reverse order

**Answer: B**
*Two distinct steps: Publishing = "show this API spec in the portal." Linking = "route Try It requests through this specific gateway service." Both are needed for interactive testing.*

---

## Q18. A company runs two separate portals — one for external partners and one for internal developers. How should they configure visibility for each?

**A)** Both portals should be public to ensure discoverability
**B)** The partner portal should be public (accessible without login); the internal portal should be private (requires authentication) — or both private depending on sensitivity
**C)** Both portals must have the same visibility setting
**D)** Visibility settings cannot be configured per portal — they apply org-wide

**Answer: B**
*Portal visibility is configurable independently. Partner portal = public (or restricted to partner accounts). Internal portal = private, requiring corporate SSO login.*

---

## Q19. How does SSO (Single Sign-On) integration benefit enterprise API consumers using the Dev Portal?

**A)** SSO eliminates the need for application credentials, allowing API calls without authentication
**B)** SSO allows enterprise developers to log into the Dev Portal using their existing corporate identity (e.g., Okta, Active Directory), removing the need to create and manage separate portal accounts
**C)** SSO automatically approves all application registrations
**D)** SSO is only available for public portals

**Answer: B**
*SSO = seamless developer experience. Enterprise developers use their existing corporate identity. No new account to create, no password to manage — and the company retains identity control.*

---

## Q20. What approval workflows can be configured in the Dev Portal?

**A)** Approvals are not supported — all registrations are automatic
**B)** Admin approval can be required for developer registrations and/or application registrations, giving the portal team control over who accesses which APIs
**C)** Only application registrations can require approval; developer registrations are always automatic
**D)** Approvals are only available in private portals

**Answer: B**
*Approval workflows can be configured at both levels: (1) who can join the portal (developer registration approval) and (2) who can use a specific API (application registration approval).*

---

## Q21. Which of the following would be a reason to configure "auto-approval" for developer registrations in a Dev Portal?

**A)** When the portal contains highly sensitive internal APIs
**B)** When the portal is public and the goal is frictionless developer onboarding — allowing any developer to register and start exploring APIs immediately
**C)** When the portal is private and requires strict vetting of all users
**D)** When SSO integration is enabled with a corporate IdP

**Answer: B**
*Auto-approval is appropriate for open developer ecosystems (like public APIs for a developer platform) where friction-free onboarding is more valuable than manual vetting.*

---

## Q22. What is the purpose of the "custom domain" feature in Dev Portal configuration?

**A)** It allows the gateway to use a custom domain for API traffic
**B)** It allows organizations to host the Dev Portal under their own domain name (e.g., developers.mycompany.com) instead of the default Konnect-provided URL
**C)** Custom domains are only used for private portals
**D)** The custom domain feature changes the Admin API endpoint address

**Answer: B**
*Custom domain = white-labeled portal experience. "developers.mycompany.com" instead of "mycompany.us.portal.konghq.com" — important for brand consistency and trust.*

---

## Q23. In the secured Try It flow, what is the purpose of step 3 (Configure app authentication strategy)?

**A)** To determine the geographic region where the API is deployed
**B)** To specify which credential type (API key, OAuth2, etc.) will be used to authenticate calls through the portal, ensuring that registered applications can generate the appropriate credential type
**C)** To configure the rate limiting policy for Try It requests
**D)** To set the visibility of the API in the portal

**Answer: B**
*The auth strategy defines the credential handshake. It ensures that when a developer registers an application and calls the API, the gateway knows what credential type to expect and validate.*

---

## Q24. Can a Dev Portal serve APIs from multiple Gateway Control Planes simultaneously?

**A)** No — each Dev Portal is limited to APIs from a single Control Plane
**B)** Yes — a Dev Portal can publish APIs from multiple Control Planes, providing a unified catalog for developers regardless of which backend gateway manages the API
**C)** Only if all Control Planes are in the same geo
**D)** Only if all Control Planes use the same authentication strategy

**Answer: B**
*Dev Portal provides a unified developer experience across the platform. APIs from different environments or Control Planes can be published to the same portal for a single catalog view.*

---

## Q25. A developer has registered an application and received an API key, but their API calls are still being rejected with 401 Unauthorized. They used the correct API key. What is the most likely cause?

**A)** The developer's portal account has expired
**B)** The API is published but not linked to the gateway service — Try It works for documentation but the gateway has no corresponding consumer/credential configured
**C)** The Control Plane is unavailable
**D)** The portal is set to private visibility

**Answer: B**
*If the gateway service is not linked (or the consumer/credential wasn't propagated to the gateway), the gateway has no knowledge of the credential — resulting in 401 rejections.*

---

## Q26. What role does OpenAPI (Swagger) specification play in the Dev Portal?

**A)** OpenAPI specs are used by the gateway to generate routing rules automatically
**B)** OpenAPI specs are uploaded to the Dev Portal to provide structured, interactive API documentation — including endpoint descriptions, request/response schemas, and Try It functionality
**C)** OpenAPI specs replace the need for application authentication strategies
**D)** OpenAPI specs are only used for internal portals

**Answer: B**
*OpenAPI specs are the documentation layer. They power the interactive docs and Try It UI in the portal — giving developers a structured, explorable view of the API.*

---

## Q27. How does the Dev Portal contribute to the overall Konnect value proposition of "developer self-service"?

**A)** By providing a tool for platform teams to monitor developer activity
**B)** By enabling API consumers to independently discover APIs, read documentation, test endpoints, register applications, and obtain credentials — without requiring manual intervention from the platform team
**C)** By automating the deployment of backend services
**D)** By generating Kong gateway configuration from developer-submitted API specs

**Answer: B**
*Self-service is the core portal value: developers operate independently through the portal lifecycle. This reduces platform team workload and accelerates developer onboarding.*

---

## Q28. Which Dev Portal feature ensures that sensitive internal APIs are not accidentally exposed to unauthenticated external users?

**A)** Rate limiting on the Try It feature
**B)** Private portal visibility — requiring authentication to access any portal content, combined with approval workflows for developer and application registration
**C)** Custom domain configuration
**D)** SSO-only login prevents all unauthenticated access automatically

**Answer: B**
*Private visibility = authentication required to see anything. Approval workflows = controlled access even for authenticated users. Together they protect sensitive APIs from unauthorized exposure.*

---

## Q29. A partner company needs to test your APIs before signing a contract. You want to give them temporary, limited access without creating full Konnect organization accounts. What is the best approach?

**A)** Share the Admin API credentials temporarily
**B)** Create a private Dev Portal, invite the partner developers as portal users, configure application authentication, and provide limited-access credentials through the portal workflow
**C)** Give the partner a read-only decK export of the gateway configuration
**D)** Enable public portal visibility for the duration of the trial

**Answer: B**
*Dev Portal is the right tool for external access: isolated from internal Konnect admin functions, controlled access through portal user registration, and scoped credentials via application registration.*

---

## Q30. What is the relationship between the Dev Portal and the Service Catalog in Konnect?

**A)** They are the same product with different names
**B)** Dev Portal is the consumer-facing surface for external/partner developers to discover and consume APIs; Service Catalog is the internal-facing inventory for platform teams to track, govern, and manage all technical assets
**C)** Service Catalog replaces the Dev Portal for internal API consumers
**D)** Dev Portal is used for governance; Service Catalog is used for developer self-service

**Answer: B**
*Portal = outward-facing (external developers, API consumption, self-service). Catalog = inward-facing (internal teams, asset inventory, governance). Complementary products serving different audiences.*
