# Section 6: Dev Portal

> **Exam Theme:** Developer self-service, secured Try It flow, branding, visibility, roles, and SSO integration.

---

## Q1. What is the primary purpose of the Dev Portal?

**A)** To manage user roles and permissions across the Konnect organization
**B)** To track internal technical assets and provide governance scorecards
**C)** To manage gateway routing rules and upstream services
**D)** To provide a public or private storefront where API consumers can discover, test, and self-service consume APIs

**Answer: D**
*The Dev Portal is the consumer-facing surface of the API platform. It enables developer self-service: browse the API catalog, read documentation, test APIs, and manage applications.*

---

## Q2. What is the correct sequence a developer must follow to use "Try It" for a secured API?

**Select TWO steps that must occur BEFORE a developer can register an application.**

**A)** Publish the API to the portal
**B)** Enable portal user authentication
**C)** Link the API to a gateway service
**D)** Configure the app authentication strategy
**E)** Developer registers the application and obtains a credential

**Answer: A, C**
*The secured Try It sequence is: (1) **P**ublish API → (2) **L**ink to gateway service → (3) **C**onfigure app auth strategy → (4) **E**nable portal user auth → (5) **R**egister application. Publishing and linking must happen before user auth and app registration.*

---

## Q3. Why is linking the API to an underlying Gateway service necessary in the portal flow?

**A)** It connects the portal's API spec to the live gateway service so that Try It requests are actually proxied through Kong to the upstream
**B)** It makes the API documentation publicly visible without authentication
**C)** It automatically generates authentication credentials for developers
**D)** It synchronizes OpenAPI spec changes from Insomnia to the portal

**Answer: A**
*Linking is what connects the "documentation layer" (portal) to the "runtime layer" (gateway). Without this link, Try It has no service to route requests to.*

---

## Q4. What is the role of an application authentication strategy in the Try It workflow?

**A)** It controls whether the Dev Portal is public or private
**B)** It manages SSO federation between the portal and an external IdP
**C)** It specifies the authentication method (e.g., API key, OAuth2) that developers must use when calling the API through their registered application
**D)** It defines how the portal itself authenticates to the Control Plane

**Answer: C**
*The app auth strategy defines the credential type (API key, OAuth2 client credentials, etc.) that the API requires. Developers generate a credential of this type when they register an application.*

---

## Q5. Why is enabling portal user authentication a separate step from securing the API itself?

**Select TWO correct statements about the relationship between portal user authentication and API authentication.**

**A)** Portal user authentication controls who can log in to the Dev Portal and register applications
**B)** Enabling portal user auth automatically applies an authentication plugin to the gateway route
**C)** API authentication controls the credentials required to make actual API calls
**D)** Portal user authentication is only required for private portals, not public ones
**E)** API authentication is configured in the Service Catalog, not the Dev Portal

**Answer: A, C**
*Two distinct layers: (1) Portal user auth = login to portal UI (who can browse and register). (2) API auth = credentials to call the API (what you need to make a request). These are independently configured.*

---

## Q6. When should you choose private visibility over public visibility for a Dev Portal?

**A)** When the APIs are hosted on a Dedicated Cloud Gateway
**B)** When SSO integration is not yet available
**C)** When the portal has more than 100 published APIs
**D)** When the APIs should only be accessible to authenticated, approved developers — not exposed to the general public or web crawlers

**Answer: D**
*Private visibility means the portal requires authentication before any content is visible. Appropriate for internal or partner-only APIs that must not be publicly discoverable.*

---

## Q7. What kind of access does the API Viewer role provide in the Dev Portal?

**A)** Read-only access to API documentation — viewers can browse and read specs but cannot create applications or make authenticated requests
**B)** Full read/write access to all portal configuration and API publishing
**C)** Admin access to manage developer registrations and approval queues
**D)** The ability to publish, link, and configure authentication strategies for APIs

**Answer: A**
*API Viewer is a read-only role. It allows a developer to view API specs and documentation without being able to register applications or generate credentials.*

---

## Q8. Which customization options are explicitly available for the Dev Portal?

**A)** Custom plugin development rules and Data Plane routing policies
**B)** Custom Kubernetes operators and Helm chart value overrides
**C)** Branding (logo, colors, custom domain), page layouts, and content sections
**D)** Custom AI model selection and prompt template configuration

**Answer: C**
*Dev Portal customization covers branding and appearance: organization logo, color scheme, custom domain, and tailored page layouts to match the organization's brand.*

---

## Q9. How does SSO integration change the developer onboarding experience?

**A)** SSO is only available for internal portals with fewer than 50 users
**B)** SSO replaces the need for application authentication strategies entirely
**C)** SSO allows developers to log in with existing corporate identity via an IdP, reducing friction and enabling centralized identity management — approval workflows can still be configured separately
**D)** SSO removes all approval workflows, allowing any authenticated user to register automatically

**Answer: C**
*SSO (via OIDC/SAML with an external IdP) means developers use existing credentials (corporate SSO, Google, GitHub) rather than creating new portal accounts.*

---

## Q10. Which mnemonic correctly captures the secured Try It sequence?

**Select TWO answers that accurately describe the PLCER sequence.**

**A)** PLCER stands for: Publish → Link → Configure → Enable → Register
**B)** PLCER stands for: Provision → Launch → Connect → Expose → Route
**C)** "Register" in PLCER refers to a developer registering an application to obtain a credential
**D)** "Enable" in PLCER refers to enabling Advanced Analytics ingestion
**E)** "Link" in PLCER refers to linking Kong nodes to the Control Plane

**Answer: A, C**
*PLCER: **P**ublish API → **L**ink to gateway service → **C**onfigure app auth strategy → **E**nable portal user auth → **R**egister application and use credential.*

---

## Q11. A developer can see API documentation but cannot make authenticated test calls via Try It. The portal admin confirms the API is published and linked. What is the most likely missing step?

**A)** The Control Plane is disconnected from the Data Plane
**B)** The API is set to private visibility blocking the developer
**C)** Portal user authentication has not been enabled, so the developer cannot register an application and obtain a credential
**D)** The developer has not been granted the API Admin role

**Answer: C**
*Can see docs (published + linked = OK) but cannot authenticate = portal user authentication is not enabled, so developer registration and the credential step are blocked.*

---

## Q12. Which statement about Dev Portal visibility settings is TRUE?

**A)** Public visibility automatically applies OAuth2 to all APIs in the portal
**B)** Visibility settings apply to the entire Konnect organization, not just a specific portal
**C)** A Dev Portal can only be either fully public or fully private — per-API visibility cannot be controlled
**D)** Public visibility means any internet user can browse API documentation; private visibility restricts access to authenticated portal users only

**Answer: D**
*Public = open to anyone (good for external developer ecosystems). Private = must authenticate to the portal to see anything (good for internal or partner APIs).*

---

## Q13. What is an "application" in the context of the Dev Portal developer workflow?

**A)** A group of APIs bundled together for discovery purposes
**B)** A Kong Gateway service object exposed in the portal
**C)** A mobile or web application built by Kong that wraps the Dev Portal UI
**D)** A developer-registered entity representing a client application consuming the API — credentials are generated and attached to this application for authentication

**Answer: D**
*Applications are developer-side entities. A developer creates an application (e.g., "My Mobile App"), registers it against an API, and receives credentials — this is the self-service registration lifecycle.*

---

## Q14. What happens when a developer registers an application against an API in the Dev Portal?

**A)** The developer immediately gains admin access to gateway configuration for that API
**B)** Konnect generates credentials (API key or OAuth2 client credentials) for the application, associated with the configured authentication strategy, which the developer uses to call the API
**C)** The API is automatically published to the public internet
**D)** The gateway automatically creates a new route scoped to that application

**Answer: B**
*Application registration triggers credential generation. The developer gets the credential that they must include in API calls to authenticate.*

---

## Q15. How does the Dev Portal support the "shift-left" approach to API consumption?

**A)** By enforcing coding standards and linting during API development
**B)** By requiring approval workflows before any API can be developed
**C)** By automatically generating API implementations from OpenAPI specs
**D)** By allowing developers to discover APIs, read documentation, and test endpoints early in their development process — before writing any integration code

**Answer: D**
*Self-service discovery and Try It let developers explore APIs without waiting for a platform team to set up access. Earlier exploration = earlier feedback and faster integration.*

---

## Q16. What is the role of the "portal user" in the Dev Portal access model?

**A)** A service account used by the gateway to authenticate API calls to the portal
**B)** An API consumer (external or internal developer) who has registered an account on the Dev Portal to access documentation and manage applications
**C)** An internal Konnect admin who manages the portal configuration and visibility settings
**D)** A Konnect organization member with gateway management permissions

**Answer: B**
*Portal users are external (or internal) developers — separate from Konnect org members. They interact with the portal as API consumers, not as platform administrators.*

---

## Q17. What is the precise distinction between "publishing" and "linking" an API in the Dev Portal?

**Select TWO correct statements.**

**A)** Publishing makes the API specification visible in the portal catalog
**B)** Publishing automatically links the API to the gateway service
**C)** Linking connects the published spec to a live gateway service so Try It requests are proxied through Kong
**D)** Linking is required only for private portals; public portals skip the linking step
**E)** Publishing and linking are the same action executed in two different UI panels

**Answer: A, C**
*Two distinct steps: Publishing = "show this API spec in the portal catalog." Linking = "route Try It requests through this specific gateway service." Both are needed for interactive testing.*

---

## Q18. A company runs two portals — one for external partners and one for internal developers. What is the correct visibility configuration?

**A)** Both portals must use the same visibility setting across the organization
**B)** Visibility settings cannot be configured per portal — they apply org-wide
**C)** Both portals should be public to maximize API discoverability and adoption
**D)** The partner portal may be public or private depending on partner vetting requirements; the internal portal should be private, requiring corporate SSO login

**Answer: D**
*Portal visibility is configurable independently. Partner portal = public or restricted. Internal portal = private, requiring corporate SSO. Each portal's visibility is set independently.*

---

## Q19. How does SSO integration benefit enterprise API consumers using the Dev Portal?

**A)** SSO automatically approves all application registration requests
**B)** SSO is only available for public portals and does not apply to private portals
**C)** SSO allows enterprise developers to log into the Dev Portal using existing corporate identity (e.g., Okta, Active Directory), removing the need to create and manage separate portal accounts
**D)** SSO eliminates the need for application credentials, allowing API calls without authentication

**Answer: C**
*SSO = seamless developer experience. Enterprise developers use their existing corporate identity. No new account, no password to manage — and the company retains identity control.*

---

## Q20. What approval workflows can be configured in the Dev Portal?

**A)** Approvals are not supported — all registrations are processed automatically
**B)** Admin approval can be required for developer registrations and/or application registrations, giving the portal team control over who can access which APIs
**C)** Only developer registrations can require approval; application registrations are always automatic
**D)** Approvals are only configurable in private portals, not public portals

**Answer: B**
*Approval workflows at both levels: (1) who can join the portal (developer registration approval) and (2) who can use a specific API (application registration approval).*

---

## Q21. Which scenario justifies enabling auto-approval for developer registrations?

**A)** When the portal contains highly sensitive internal financial APIs requiring vetting
**B)** When SSO is enabled with a strict corporate identity provider
**C)** When the portal is private and requires manual vetting of all users before access
**D)** When the portal is public and the goal is frictionless developer onboarding — allowing any developer to register and start exploring APIs immediately

**Answer: D**
*Auto-approval is appropriate for open developer ecosystems (public APIs for a developer platform) where friction-free onboarding is more valuable than manual vetting of each registrant.*

---

## Q22. What is the purpose of the "custom domain" feature in Dev Portal configuration?

**A)** The custom domain feature changes the Admin API endpoint address alongside the portal URL
**B)** Custom domains are only available for private portals with SSO enabled
**C)** It allows the gateway to use a custom domain for proxied API traffic
**D)** It allows organizations to host the Dev Portal under their own branded domain (e.g., developers.mycompany.com) instead of the default Konnect-provided URL

**Answer: D**
*Custom domain = white-labeled portal experience. "developers.mycompany.com" instead of the default Konnect URL — important for brand consistency and developer trust.*

---

## Q23. In the secured Try It flow, what is the purpose of configuring an app authentication strategy?

**A)** To set the geographic region where the API runtime is deployed
**B)** To configure rate limiting policies specifically for Try It sandbox requests
**C)** To specify which credential type (API key, OAuth2, etc.) will be used to authenticate API calls through the portal, so that registered applications can generate the appropriate credential
**D)** To set the visibility of the API to public or private in the portal catalog

**Answer: C**
*The auth strategy defines the credential handshake. It ensures that when a developer registers an application and calls the API, the gateway knows what credential type to expect and validate.*

---

## Q24. Can a Dev Portal serve APIs from multiple Gateway Control Planes simultaneously?

**A)** Only if all Control Planes share the same authentication strategy configuration
**B)** Only if all Control Planes are deployed in the same geo
**C)** Yes — a Dev Portal can publish APIs from multiple Control Planes, providing a unified catalog for developers regardless of which backend gateway manages each API
**D)** No — each Dev Portal is limited to APIs from a single Control Plane

**Answer: C**
*Dev Portal provides a unified developer experience across the platform. APIs from different environments or Control Planes can be published to the same portal for a single catalog view.*

---

## Q25. A developer received an API key but their calls are still rejected with 401. They are using the correct key. What is the most likely cause?

**A)** The portal is set to private visibility, blocking the API call
**B)** The developer's portal account session has expired
**C)** The Control Plane is unreachable from the developer's network
**D)** The API is published but not linked to the gateway service — the gateway has no corresponding consumer or credential configuration and cannot validate the key

**Answer: D**
*If the gateway service is not linked (or the consumer/credential wasn't propagated to the gateway), the gateway has no knowledge of the credential — resulting in 401 rejections.*

---

## Q26. What role does an OpenAPI (Swagger) specification play in the Dev Portal?

**A)** OpenAPI specs are used by the gateway to auto-generate routing rules for published APIs
**B)** OpenAPI specs are uploaded to the Dev Portal to provide structured, interactive API documentation — including endpoint descriptions, request/response schemas, and the Try It interface
**C)** OpenAPI specs replace the need for application authentication strategies
**D)** OpenAPI specs are only consumed by the Service Catalog, not the Dev Portal

**Answer: B**
*OpenAPI specs are the documentation layer. They power the interactive docs and Try It UI — giving developers a structured, explorable view of the API contract.*

---

## Q27. How does the Dev Portal contribute to the "developer self-service" value proposition?

**Select TWO correct statements.**

**A)** Developers can independently discover, read documentation, and test APIs without manual intervention from the platform team
**B)** The Dev Portal automates deployment of backend services based on developer usage
**C)** Developers can register applications and obtain credentials without filing a support ticket
**D)** The Dev Portal generates Kong gateway configuration from developer-submitted API specs
**E)** The platform team is still required to approve every API call before a developer can test it

**Answer: A, C**
*Self-service: discover → read docs → test → register application → get credential — all without platform team intervention. This reduces platform team workload and accelerates developer onboarding.*

---

## Q28. Which combination of Dev Portal features best protects sensitive internal APIs from unauthenticated external users?

**A)** Rate limiting on the Try It feature and custom domain configuration
**B)** SSO-only login combined with auto-approval for all registrations
**C)** Private portal visibility requiring authentication to access any content, combined with approval workflows for developer and application registration
**D)** Public portal visibility with API-level visibility restrictions applied per route

**Answer: C**
*Private visibility = authentication required to see anything. Approval workflows = controlled access even after authentication. Together they protect sensitive APIs from unauthorized exposure.*

---

## Q29. A partner company needs temporary, limited API access before signing a contract. What is the best approach?

**A)** Enable public portal visibility for the duration of the trial period
**B)** Give the partner a read-only decK export of the gateway configuration for self-review
**C)** Share the Admin API credentials temporarily with a time-limited token
**D)** Create a private Dev Portal, invite partner developers as portal users, configure application authentication, and provide scoped credentials through the portal workflow

**Answer: D**
*Dev Portal is the right tool for external access: isolated from internal Konnect admin functions, controlled via portal user registration, and scoped credentials via application registration.*

---

## Q30. What is the relationship between the Dev Portal and the Service Catalog in Konnect?

**A)** Dev Portal is used for governance; Service Catalog is used for developer self-service
**B)** Service Catalog replaces the Dev Portal for internal API consumers
**C)** They are the same product with different names targeting different UI views
**D)** Dev Portal is the consumer-facing surface for developers to discover and consume APIs; Service Catalog is the internal-facing inventory for platform teams to track, govern, and manage all technical assets

**Answer: D**
*Portal = outward-facing (external developers, API consumption, self-service). Catalog = inward-facing (internal teams, asset inventory, governance). Complementary products serving different audiences.*
