# Section 16: Insomnia — Design, Test, and Manage APIs with OpenAPI

> **Exam Theme:** Using Insomnia for API design via OpenAPI Specification (OAS), sending requests, organizing collections, and integrating with Kong.

---

## Q1. What is Insomnia primarily used for in an API development workflow?

**A)** Deploying Kong Gateway to production
**B)** Designing, testing, and debugging REST, GraphQL, and gRPC APIs
**C)** Managing Kong's RBAC tokens
**D)** Monitoring upstream health checks

**Answer: B**
*Insomnia is Kong's open-source API client and design tool for making HTTP/GraphQL/gRPC requests, designing APIs via OpenAPI, and organizing collections.*

---

## Q2. What is the OpenAPI Specification (OAS)?

**A)** Kong's proprietary plugin configuration format
**B)** A language-agnostic, machine-readable standard for describing REST APIs
**C)** A security protocol for authenticating API clients
**D)** A binary protocol for high-performance API communication

**Answer: B**
*OAS (formerly Swagger) is a standard YAML/JSON format for describing REST API endpoints, request/response schemas, authentication, and more.*

---

## Q3. Which OAS version is most commonly used with Insomnia and Kong today?

**A)** Swagger 1.2
**B)** OpenAPI 2.0 (Swagger)
**C)** OpenAPI 3.0 / 3.1
**D)** RAML 1.0

**Answer: C**
*OpenAPI 3.0 (and 3.1) is the current standard supported by Insomnia's design editor and by Kong for generating gateway configurations.*

---

## Q4. In Insomnia, what is a "Design Document"?

**A)** A PDF export of API documentation
**B)** An OpenAPI Specification file edited within Insomnia's Design tab
**C)** A collection of saved HTTP requests
**D)** A Kong decK state file

**Answer: B**
*Insomnia's Design tab hosts an OpenAPI editor. A Design Document is the OAS YAML/JSON file you create and edit to define your API contract.*

---

## Q5. What does Insomnia's "Generate Request Collection" feature do?

**A)** Deploys the API to Kong automatically
**B)** Creates a set of pre-populated HTTP requests from an OpenAPI spec
**C)** Generates unit tests for the API backend
**D)** Exports the spec to Kong Manager

**Answer: B**
*From a Design Document, Insomnia can generate a Request Collection with one request per OAS endpoint, pre-filled with parameters, headers, and request bodies.*

---

## Q6. How does Insomnia support environment variables?

**A)** Through system environment variables only
**B)** Via Environment objects that store key-value pairs reusable across requests
**C)** By reading `.env` files from the project directory
**D)** Through Kong's Config Store only

**Answer: B**
*Insomnia Environments store variables (e.g., `base_url`, `api_key`) that can be referenced in requests with `{{ variable_name }}`, enabling easy switching between dev/staging/prod.*

---

## Q7. An OAS document defines a `GET /users/{userId}` endpoint. How does Insomnia handle the `{userId}` path parameter?

**A)** It omits the parameter from the generated request
**B)** It auto-fills the parameter with a UUID
**C)** It creates a path parameter field in the generated request for the user to fill in
**D)** It converts it to a query parameter automatically

**Answer: C**
*Insomnia parses path parameters from the OAS and creates input fields in the generated request so users can provide values before sending.*

---

## Q8. Which Insomnia feature allows you to chain requests by passing a response value from one request as input to the next?

**A)** Request Chaining
**B)** Response References (Template Tags)
**C)** Pre-request Scripts
**D)** Environment Inheritance

**Answer: B**
*Insomnia's Template Tags allow referencing response data from previous requests (e.g., `{% response 'body', 'req_id', '$.token' %}`), enabling dynamic request chaining.*

---

## Q9. What file format does Insomnia use for exporting and sharing collections?

**A)** Postman Collection v2 (JSON)
**B)** Insomnia export format (JSON) or OpenAPI YAML
**C)** HAR (HTTP Archive)
**D)** CSV

**Answer: B**
*Insomnia exports collections in its own JSON format. It can also export/import OpenAPI YAML files, making it interoperable with other OAS-compatible tools.*

---

## Q10. In an OpenAPI document, where are reusable request/response schemas defined?

**A)** `paths`
**B)** `components/schemas`
**C)** `info`
**D)** `servers`

**Answer: B**
*The `components/schemas` section holds reusable object definitions referenced via `$ref` in path definitions, avoiding duplication across the spec.*

---

## Q11. How can an Insomnia Design Document be used to generate Kong configuration?

**A)** By exporting directly from Insomnia to the Kong Admin API
**B)** By using the `inso` CLI to generate a decK state file from the OAS
**C)** By copying the YAML into Kong Manager manually
**D)** Insomnia cannot generate Kong configuration

**Answer: B**
*The `inso` CLI (Insomnia's command-line companion) can run `inso generate config` to produce a Kong-compatible decK state file from an OpenAPI spec.*

---

## Q12. What is the `inso` CLI tool?

**A)** A Kong plugin installer
**B)** Insomnia's command-line interface for running tests, linting specs, and generating configs in CI/CD
**C)** A Kong Admin API client
**D)** An OpenAPI validator provided by Swagger

**Answer: B**
*`inso` is the CLI for Insomnia that enables automation tasks: spec linting, running unit tests, generating Kong decK configs, and integrating with CI/CD pipelines.*

---

## Q13. Which OAS field specifies the authentication mechanisms supported by an API?

**A)** `paths`
**B)** `info`
**C)** `securitySchemes` under `components` + `security` at global/operation level
**D)** `servers`

**Answer: C**
*`components/securitySchemes` defines auth types (apiKey, http, oauth2, openIdConnect). The `security` field at the global or operation level applies them.*

---

## Q14. You want to test that a `POST /orders` endpoint returns `201 Created` for valid input. Where in Insomnia would you write this assertion?

**A)** In the Design tab's OAS document
**B)** In a Unit Test suite using the Test tab
**C)** In the Environment variables
**D)** In the request Body tab

**Answer: B**
*Insomnia's Test tab allows creating JavaScript-based unit tests that assert on response status, headers, and body content.*

---

## Q15. What does the `servers` field in an OpenAPI document specify?

**A)** The list of backend targets for load balancing
**B)** The base URLs where the API is hosted
**C)** The authentication servers for OAuth2
**D)** The Kong Gateway proxy addresses

**Answer: B**
*The `servers` array lists the base URLs for the API (e.g., `https://api.example.com/v1`). Insomnia uses this to pre-fill the base URL for generated requests.*

---

## Q16. How does Insomnia handle authentication for requests that require OAuth2?

**A)** Users must manually add Bearer tokens to headers
**B)** Insomnia has built-in OAuth2 flows (Authorization Code, Client Credentials, etc.) in the Auth tab
**C)** Authentication is not supported in Insomnia
**D)** Only API key authentication is supported

**Answer: B**
*Insomnia's Auth tab supports OAuth 2.0 flows, API Keys, Bearer tokens, Basic Auth, Digest, NTLM, AWS IAM, and more — matching OAS security schemes.*

---

## Q17. Which OAS field describes the expected request body for a POST endpoint?

**A)** `parameters`
**B)** `requestBody`
**C)** `responses`
**D)** `callbacks`

**Answer: B**
*The `requestBody` field in an OAS operation defines the content type(s) and schema for the request body, used by Insomnia to generate body templates.*

---

## Q18. What does `inso lint spec` do?

**A)** Formats the OAS YAML file
**B)** Validates the OpenAPI spec for structural and semantic correctness
**C)** Runs all unit tests in the Insomnia collection
**D)** Generates Kong configuration from the spec

**Answer: B**
*`inso lint spec` validates the OAS document against OpenAPI rules (e.g., missing required fields, invalid references), suitable for CI/CD spec quality gates.*

---

## Q19. In Insomnia, what is a "Collection"?

**A)** A group of Design Documents
**B)** An organized set of saved API requests, folders, and environment variables
**C)** A set of Kong Services grouped by tag
**D)** A decK state file

**Answer: B**
*A Collection in Insomnia is a workspace for organizing related requests into folders, sharing variables through environments, and running requests as a group.*

---

## Q20. How does Insomnia's "Try It" feature (from the Dev Portal) relate to OpenAPI?

**A)** It uses the OAS to dynamically render and execute API requests in the browser
**B)** It generates static documentation from the OAS
**C)** It deploys routes to Kong from the OAS
**D)** It converts OAS to Postman format

**Answer: A**
*Kong's Dev Portal can render an OpenAPI spec as an interactive "Try It" interface (backed by Insomnia technology), allowing developers to send real requests without leaving the portal.*

---

## Q21. Which property in an OAS path item defines the HTTP methods supported?

**A)** `methods: [GET, POST]`
**B)** Individual operation objects keyed by method (`get:`, `post:`, `put:`, etc.)
**C)** `supported_verbs`
**D)** `operations`

**Answer: B**
*OAS path items contain operation objects keyed by lowercase HTTP method names. Each method gets its own `operationId`, parameters, requestBody, and responses.*

---

## Q22. What is the purpose of `operationId` in an OpenAPI operation?

**A)** It sets the Kong route name
**B)** A unique string identifier for the operation, used for code generation and tooling
**C)** It maps the operation to a Consumer
**D)** It sets the priority of the route in Kong

**Answer: B**
*`operationId` provides a machine-readable name for each operation. Tools like `inso` use it to name generated Kong routes and Insomnia requests.*

---

## Q23. How does Insomnia support Git-based collaboration on API designs?

**A)** Through Git Sync — linking an Insomnia project to a Git repository
**B)** By exporting to GitHub Issues
**C)** Insomnia does not support version control
**D)** By storing designs in Kong Manager

**Answer: A**
*Insomnia supports Git Sync, allowing design documents and collections to be stored in and synchronized with a Git repository for team collaboration and version history.*

---

## Q24. Which OAS field documents the possible responses for an operation?

**A)** `requestBody`
**B)** `outputs`
**C)** `responses`
**D)** `callbacks`

**Answer: C**
*The `responses` object maps HTTP status codes to response descriptions and schemas, documenting what clients can expect for success and error cases.*

---

## Q25. You run `inso run test` in a CI pipeline. What does this execute?

**A)** Sends all requests in a Collection and checks for 200 responses
**B)** Runs the unit tests defined in Insomnia's Test suites against a running API
**C)** Deploys the OAS to Kong and runs health checks
**D)** Lints the OAS document

**Answer: B**
*`inso run test` executes Insomnia unit tests (JavaScript assertions on response status, headers, body) against a live API endpoint, suitable for integration testing in CI.*

---

## Q26. What is the relationship between Insomnia and Kong Konnect's Dev Portal?

**A)** Insomnia replaces the Dev Portal
**B)** The Dev Portal uses Insomnia's rendering engine to provide the interactive API explorer
**C)** Insomnia connects directly to the Dev Portal's consumer database
**D)** There is no relationship

**Answer: B**
*Konnect's Dev Portal leverages Insomnia technology to render OpenAPI specs as interactive documentation with a built-in "Try It" request runner.*

---

## Q27. Which OAS feature allows documenting that an endpoint requires a specific security scheme?

**A)** Adding the scheme name to the `tags` field
**B)** Using the `security` array at the operation or global level
**C)** Setting `auth: required` in the path item
**D)** Adding an `x-kong-security` extension

**Answer: B**
*The `security` array at the operation level lists required security schemes (referencing `securitySchemes`). Operations without a `security` key inherit the global `security` setting.*

---

## Q28. In Insomnia, how do you switch between development and production environments for the same collection?

**A)** Create separate collections for each environment
**B)** Use Insomnia Environments — select the target environment from the dropdown before running requests
**C)** Edit the base URL in every request manually
**D)** Deploy different Insomnia versions per environment

**Answer: B**
*Insomnia Environments store per-environment variables. Switching the active environment updates all request variables without modifying the requests themselves.*

---

## Q29. Which OAS extension prefix does Kong use for gateway-specific configurations in an OpenAPI spec?

**A)** `x-kong-`
**B)** `k-config-`
**C)** `kong:`
**D)** `x-gateway-`

**Answer: A**
*Kong uses `x-kong-*` OAS extensions (e.g., `x-kong-plugin-rate-limiting`, `x-kong-route-defaults`) to embed gateway configuration directly in the OpenAPI spec.*

---

## Q30. What is the primary benefit of designing APIs in Insomnia before implementing them?

**A)** It automatically generates backend code
**B)** It enables a contract-first approach, aligning consumers and producers on the API interface before any code is written
**C)** It deploys the API to Kong immediately
**D)** It provides free hosting for the API

**Answer: B**
*Contract-first / design-first development: the OAS spec is the source of truth that teams agree on upfront, reducing rework and misalignment between API providers and consumers.*
