# PRUDRA — Build the Documentation Site
# Cursor agent prompt

Read this entire prompt before touching any file.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
WHAT YOU ARE DOING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

You are building the complete Prudra developer documentation site
using Mintlify. This means:

  1. Writing all .mdx page files
  2. Updating docs.json with the correct navigation
  3. NOT touching any existing api-reference/ folder files
     (those are auto-generated from openapi.json)

The output is a fully functional Mintlify docs site that passes:
  npx mintlify dev    — no errors, no broken links
  npx mintlify broken-links  — zero broken internal links

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DOCUMENTS TO READ FIRST — MANDATORY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Before writing a single .mdx file, read all of these in full:

  1. prudra-docs/docs.prudra.com-arcitecture.md
     The architecture (file structure + docs.json) AND the quality
     standard (page types, required sections, checklists).
     This is your primary reference for everything.

  2. docs/prudra-spec-v0.5.docx
     The full Prudra product spec. This is your source of truth for
     what each feature does, how it works, and what the architecture is.
     Read it carefully before writing any conceptual content.

  3. docs/dev-agent-workflow.md
     The developer agent workflow. Section "Obligation 2 — Documentation"
     contains additional quality guidance. The checklist at the bottom
     of that document must be completed for every page.

  4. The existing example files (examples/example-01 through example-09)
     These show real working code. Use the code patterns in your
     documentation examples — they must match what actually works.

  5. The swagger comments file (api/swagger-comments-reference.md)
     This contains the API descriptions for every endpoint. Use the
     summary and description fields when writing task pages — they
     describe what each endpoint does precisely.

   6. libraries/packages
     The public sdk. 
     contains additional functions. that users can use, to aid when creating functinoal coding examples

Report that you have read all five documents before proceeding.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 0 — AUDIT WHAT EXISTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Before creating anything, audit the current state:

  ls -R /prudra/prudra-docs/

Report:
  - What .mdx files already exist
  - What folders already exist
  - Whether docs.json or mint.json exists and what it contains
  - Whether the api-reference/ folder exists and has files
  - Whether any existing content is worth preserving

Then report the plan:
  - Which pages need to be created from scratch
  - Which pages need to be updated
  - Which pages already exist and are fine

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 1 — WRITE THE docs.json
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Copy the docs.json exactly from prudra-docs/docs.prudra.com-arcitecture.md.
It is the complete, schema-valid configuration.

The only things you may need to adjust:
  - Logo file paths — check what logo files actually exist in the docs folder
  - Favicon path — check what favicon file actually exists
  - api-reference paths — update these to match whatever files the
    Mintlify scraper has already generated (do not change their content)

After writing docs.json, run:
  npx mintlify dev

If it errors, fix the error before proceeding. Common issues:
  - A page listed in navigation doesn't have a .mdx file yet
    → create a placeholder: ---\ntitle: "Coming soon"\n---
  - Logo file doesn't exist → use a placeholder path for now
  - api-reference pages don't match → adjust those paths to match
    whatever files the scraper generated

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 2 — CREATE ALL .mdx FILES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Create every page listed in the docs.json navigation tree,
in the order listed below. The API Reference tab pages are
excluded — do not touch api-reference/ files.

For each page:
  1. Identify the page type (A=Overview, B=Task, C=Concept)
  2. Use the correct structure from the quality standard
  3. Source content from prudra-spec-v0.5.docx or /api directly
  4. Use code examples from the example files
  5. Complete the quality checklist before moving to the next page

Work through pages in this order (highest priority first):

─── GET STARTED (all Type A or B) ──────────────────────────

get-started/overview.mdx              Type A
  Sources: spec overview, core product description
  Must include: what Prudra is, the three products, architecture diagram,
  links to each product section, quickstart CTA

get-started/quickstart.mdx            Type B
  Sources: example-01-basic-paywall.ts
  Must include: install SDK, initialise, add payMiddleware to one route,
  test with curl, see the vault created. End-to-end in <10 steps.
  Working code from example-01.

get-started/core-concepts.mdx         Type C
  Sources: spec architecture section
  Must include: how agents pay for APIs (the 402 flow), what a vault is,
  what a wallet is, how they relate. Diagram of the full system.

get-started/authentication.mdx        Type B
  Sources: API key format, authenticate middleware
  Must include: how to get an API key, test vs live key formats,
  how to pass it in requests, what 401 means

─── PAYMENTS ────────────────────────────────────────────────

payments/overview.mdx                 Type A
  Sources: spec payments section
  Must include: x402 vs MPP explanation, when to use each,
  recommendation, dual-protocol advantage, CardGroup for x402/MPP/Sessions,
  link to security section

payments/accept-a-payment.mdx         Type B
  Sources: example-01, example-05 (for the full middleware chain)
  Must include: walletMiddleware + payMiddleware + vaultMiddleware chain,
  working code, curl test, what the 402 response looks like (headers),
  what the 200 response includes (vaultId, payment object)

payments/x402/overview.mdx            Type A
  Sources: spec x402 section, x402 ERC-3009 explanation
  Must include: what x402 is, ERC-3009 flow explanation, who issues
  the challenge, who settles, PAYMENT-REQUIRED header, when to use x402

payments/x402/how-it-works.mdx        Type C
  Sources: x402 ERC-3009 implementation details
  Must include: the full flow diagram (402 → agent signs → resubmit →
  verify → settle → 200), what PAYMENT-REQUIRED contains (base64 options),
  what PAYMENT-SIGNATURE contains (ERC-3009 signature), what CDP does

payments/x402/add-to-endpoint.mdx     Type B   ← dashboard tab
  Sources: example-01, payMiddleware options
  Must include: payMiddleware with x402 options, price param,
  curl test without payment (402), curl test with real signature,
  PAYMENT-RESPONSE header on success

payments/x402/test.mdx                Type B
  Sources: example-06-x402-agent.ts
  Must include: the full test script, PAYMENT_STUB_MODE explanation,
  how to use a test private key, what to expect at each step

payments/x402/handle-response.mdx     Type B
  Sources: PAYMENT-RESPONSE header, settlement flow
  Must include: decoding PAYMENT-RESPONSE, txHash, settlementPending,
  what to do if settlement fails

payments/mpp/overview.mdx             Type A
  Sources: spec MPP section
  Must include: what MPP is, WWW-Authenticate header format,
  Authorization: Payment header, Tempo chain, USDC.e, when to use MPP

payments/mpp/how-it-works.mdx         Type C
  Sources: MPP verification flow, challenge HMAC
  Must include: flow diagram (402 → agent sends tx → txHash →
  resubmit → verify receipt → 200), how the HMAC challenge works,
  challenge expiry, MPP vs x402 difference

payments/mpp/add-to-endpoint.mdx      Type B   ← dashboard tab
  Sources: example-01 with MPP, payMiddleware
  Must include: same as x402 add page but MPP-specific headers

payments/mpp/test.mdx                 Type B
  Sources: example-07-mpp-agent.ts
  Must include: the full MPP agent test script, Tempo testnet setup,
  faucet instructions, what to expect

payments/mpp/authorization.mdx        Type C
  Sources: parseMPPCredential, Authorization header spec
  Must include: Authorization: Payment header format, credential
  structure (base64url JSON), parsing the credential, validating it

payments/dual-protocol/overview.mdx   Type A
  Sources: buildDualChallenge, challenge generation
  Must include: what dual-protocol means (both headers on every 402),
  agent picks which to use, developer writes one integration, diagram

payments/dual-protocol/challenge.mdx  Type C
  Sources: challenge.route.service.ts, buildDualChallenge
  Must include: how challenges are built server-side, the HMAC for MPP,
  the base64 options array for x402, rate limiting (20/IP/60s),
  why secrets never leave the server

payments/dual-protocol/pick-protocol.mdx  Type C
  Sources: protocol decision logic
  Must include: decision table (x402 vs MPP by chain, by agent type,
  by use case), recommendation, why dual-protocol is the safe default

payments/sessions/overview.mdx        Type A
  Sources: session payment spec
  Must include: what session payments are, the agent credit card analogy,
  one payment covers a workflow, session ID flow, CardGroup linking to sub-pages

payments/sessions/how-it-works.mdx    Type C
  Sources: session payment middleware, PaymentSession model
  Must include: flow diagram (pay → session created → session ID →
  subsequent requests skip payment → same vault), how session IDs work,
  session scoping by org, expiry

payments/sessions/add.mdx             Type B   ← dashboard tab
  Sources: example-04-session-payments.ts
  Must include: payMiddleware with session:true, X-PRUDRA-SESSION-ID header,
  reading sessionId from response, reusing it, full working example

payments/sessions/multi-step.mdx      Type B
  Sources: example-04
  Must include: the full two-request session flow with curl commands,
  confirming same vaultId, vault accumulation across steps

payments/sessions/expiry.mdx          Type C
  Sources: PaymentSession.expiresAt, sessionTtl option
  Must include: default TTL (24h), custom TTL, what happens on expiry
  (fresh 402 with challenge), how to extend a session

payments/security/replay.mdx          Type C
  Sources: replay protection implementation, txHash UNIQUE constraint
  Must include: what a replay attack is, how the UNIQUE constraint
  works at the DB level, why it's the DB not application code,
  the 409 error response, testing replay protection

payments/security/harvesting.mdx      Type C
  Sources: challenge rate limiting, no-challenge-on-error rule
  Must include: what challenge harvesting is, rate limiting (20/IP/60s),
  why failed credentials get no new challenge, the two defences

payments/security/audit-logs.mdx      Type B   ← dashboard tab
  Sources: PaymentLog model, payment logging
  Must include: what is logged on every payment (org, apiKey, protocol,
  amount, route, txHash), how to query logs, what apiKeyId enables

─── WALLETS ─────────────────────────────────────────────────

wallets/overview.mdx                  Type A
  Sources: spec wallet infrastructure section
  Must include: managed vs BYO explanation, recommendation
  (managed for new builds, BYO if you have an existing wallet),
  CardGroup for managed/BYO, transfers, withdrawals overview

wallets/choose-wallet-type.mdx        Type C
  Sources: wallet type comparison
  Must include: decision table (BYO vs Managed by custody needs,
  by use case, by phase readiness), when child addresses make sense

wallets/managed/overview.mdx          Type A
  Sources: Phase B spec, wallet.service.ts
  Must include: what managed wallets are, KMS custody explanation,
  the master-seed → master-wallet → child-address hierarchy,
  diagram of the derivation tree, what Prudra holds vs what you hold

wallets/managed/how-it-works.mdx      Type C
  Sources: KMS envelope encryption, BIP-32 derivation, withWalletSigner
  Must include: envelope encryption model (KEK encrypts DEK),
  BIP-44 path explanation, what "zero plaintext persistence" means,
  key rotation, what keyVersion is

wallets/managed/provision.mdx         Type B   ← dashboard tab
  Sources: example-07, provisionMasterWallet
  Must include: provisionWallet() SDK call, chain and token selection
  (Chain enum, Token enum), curl equivalent, full response, Plan limits

wallets/managed/child-addresses.mdx   Type B   ← dashboard tab
  Sources: example-07 Step 2, deriveChildAddress
  Must include: deriveChildAddress() SDK call, derivation path shown,
  uniqueness of each address, unlimited on all plans, metadata param

wallets/managed/check-balance.mdx     Type B   ← dashboard tab
  Sources: getBalance SDK function, WalletBalance response
  Must include: getBalance() call, response structure, empty balance
  (wallet unfunded), what tokenSymbol and formattedBalance mean

wallets/managed/transactions.mdx      Type B   ← dashboard tab
  Sources: WalletTransaction model, transactions endpoint
  Must include: listing transactions, filtering by direction,
  what each transaction field means, inbound vs outbound

wallets/managed/key-rotation.mdx      Type C
  Sources: KMS key rotation, keyVersion field, re-encryption plan
  Must include: what rotation means for envelope encryption,
  why old data still decrypts after rotation, keyVersion tracking,
  the re-encryption job (pre-launch requirement)

wallets/managed/supported-chains.mdx  Type C
  Sources: chains.ts config, Chain enum
  Must include: table of all supported chains with chainId,
  which tokens are supported per chain, ERC-3009 support by chain,
  MPP (Tempo only), testnet chains in dev mode

wallets/byo/overview.mdx              Type A
  Sources: BYO wallet spec, Phase A
  Must include: what BYO wallets are, no custody (read-only monitoring),
  Alchemy monitoring explanation, how deposit detection works, use cases

wallets/byo/register.mdx              Type B   ← dashboard tab
  Sources: example-06-byo-wallet.ts, BYO wallet registration route
  Must include: register SDK call, address + chain + supportedTokens params,
  AML screening (async, pending status), Alchemy monitoring registration,
  curl equivalent, what to do after registration

wallets/byo/monitor-deposits.mdx      Type C
  Sources: Alchemy webhook, processActivity, WalletTransaction
  Must include: how deposit detection works (Alchemy → ngrok → API),
  ADDRESS_ACTIVITY webhook, what WalletTransaction gets created,
  deposit.success webhook event, Tempo monitoring limitation

wallets/byo/chains-tokens.mdx         Type C
  Sources: chains.ts, tokens.ts, getSupportedTokensForChain
  Must include: same supported chains table as managed/supported-chains,
  ERC-3009 support matrix, how to pick tokens at registration

wallets/byo/deregister.mdx            Type B   ← dashboard tab
  Sources: deregister route, Alchemy deregistration
  Must include: deregister SDK call, what happens to Alchemy monitoring,
  MonitoredAddress deactivation, BYO wallet limit decrements

wallets/transfers/overview.mdx        Type A
  Sources: transfer.service.ts
  Must include: the three routes (direct, swap, bridge), automatic routing,
  when each route is selected, fees summary, status values, CardGroup

wallets/transfers/routing.mdx         Type C
  Sources: transfer routing logic
  Must include: routing decision tree diagram, direct vs swap vs bridge,
  fee comparison, time comparison, when to expect pending status

wallets/transfers/send.mdx            Type B
  Sources: example-08-transfer.ts Transfer 1
  Must include: transfer() SDK call, all params (Chain/Token enums),
  curl equivalent, response with route and status fields,
  how to use child addresses (fromWalletType: 'child')

wallets/transfers/cross-chain.mdx     Type B
  Sources: example-08 Transfer 3 (bridge)
  Must include: bridge transfer example, pending status explanation,
  deposit.success webhook fires on destination chain,
  estimating bridge time, fees involved

wallets/transfers/fees.mdx            Type C
  Sources: fee structure, direct vs swap vs bridge
  Must include: direct (gas only, fractions of a cent on Base),
  swap/bridge (protocol fees deducted from amount), Prudra takes no markup,
  how to see estimated output before sending (getTransferQuote)

wallets/transfers/track-status.mdx    Type B   ← dashboard tab
  Sources: WalletTransaction, transactions endpoint
  Must include: checking status via transactions endpoint, confirmed vs pending,
  what to do if a bridge is pending for a long time, the webhook event

wallets/withdrawals/overview.mdx      Type A
  Sources: withdrawal.service.ts, Withdrawal model
  Must include: what a withdrawal is, the on-chain + fiat conversion flow,
  Prudra's exchange wallet, manual processing (Phase 1), 2-3 business days,
  reference number tracking

wallets/withdrawals/bank-account.mdx  Type B   ← dashboard tab
  Sources: example-09 Step 1
  Must include: addBankAccount() SDK call, UK/US/IBAN fields,
  account number masking, listBankAccounts, deleteBankAccount

wallets/withdrawals/request.mdx       Type B
  Sources: example-09 Step 3
  Must include: withdraw() SDK call, all params, response (pending_conversion
  status, txHash for verification, reference, estimatedDays),
  withdrawal without a bank account (ops contacts you)

wallets/withdrawals/track.mdx         Type B   ← dashboard tab
  Sources: listWithdrawals, getWithdrawal
  Must include: listWithdrawals() SDK call, status values
  (pending_conversion → converting → sent → failed), reference number,
  txHash verification on-chain, what to do if failed

wallets/withdrawals/currencies.mdx    Type C
  Sources: BankAccount model, currency field
  Must include: supported fiat currencies (GBP, USD, EUR),
  supported bank account types (UK sort/account, US routing, IBAN/SWIFT),
  estimated processing times by currency, regions served

─── STORAGE ─────────────────────────────────────────────────

storage/overview.mdx                  Type A
  Sources: vault spec
  Must include: what storage is (vaults + real-time events + file delivery),
  Obsidian/Google Drive analogy for the directory structure (Phase 3),
  CardGroup for vaults/events/files, when to use storage

storage/vaults/overview.mdx           Type A
  Sources: vault spec, Vault model
  Must include: what a vault is, the three content types (documents/files/events),
  the lifecycle (active → sealed/persisted → expired), shared agent context,
  CardGroup linking to create/add-documents/upload-files/seal/persist

storage/vaults/how-it-works.mdx       Type C
  Sources: vault architecture, VaultDocument/VaultFile/VaultEvent
  Must include: documents (Postgres JSON) vs files (GCS binary),
  vault lifecycle diagram, TTL and expiry, seal vs persist distinction,
  how multiple agents share a vault via sessionId

storage/vaults/create.mdx             Type B
  Sources: example-01, createVault SDK
  Must include: createVault() in SDK context (payMiddleware does it automatically),
  direct API creation, metadata param, sessionId linking

storage/vaults/add-documents.mdx      Type B
  Sources: example-01 vault.addDocument()
  Must include: addDocument() SDK call, data field (JSON object),
  name field, gcsPath/assetUrl null for data-only documents,
  document limits by plan (50 on Hobby)

storage/vaults/upload-files.mdx       Type B
  Sources: example-02-file-upload.ts
  Must include: uploadFile() SDK call, multipart/form-data curl,
  pre-signed URL flow (file → GCS directly, not through API),
  assetUrl response (artifacts.prudra.com CDN), file size limits by plan

storage/vaults/seal.mdx               Type B   ← dashboard tab
  Sources: vault.seal() SDK, sealVault route
  Must include: seal() SDK call, what sealing does (read-only permanently),
  summary param, tracker.vaultClosed() decrements active count,
  plan limit freed on seal

storage/vaults/persist.mdx            Type B   ← dashboard tab
  Sources: vault.persist() SDK, persistVault route
  Must include: persist() SDK call, what persisting does (no expiry),
  persisted vault plan limits, difference from sealing,
  when to persist vs seal

storage/vaults/share-context.mdx      Type C
  Sources: multi-agent vault sharing, sessionId
  Must include: how multiple agents share one vault, the sessionId as
  shared identity, orchestrator + sub-agent pattern, real-time via SSE,
  manifest as the assembly point, diagram of multi-agent vault sharing

storage/vaults/lifecycle.mdx          Type C
  Sources: vault status enum, TTL, expiry
  Must include: lifecycle diagram (active → sealed/persisted → expired),
  TTL by plan (24h Hobby, 7 days Pro), vault.expiring webhook event,
  what happens on expiry (cleanup scheduled), how to prevent expiry

storage/events/overview.mdx           Type A
  Sources: VaultEvent, SSE implementation
  Must include: what vault events are, real-time vs persisted,
  SSE stream explanation, replay from timestamp, reconnection, use cases

storage/events/subscribe.mdx          Type B
  Sources: example-03-streaming.ts
  Must include: SSE subscription curl command, VaultSSE class in SDK,
  event format (data: JSON), what events fire automatically vs manually,
  heartbeat (30s), connection stays open until vault.sealed fires

storage/events/replay.mdx             Type B
  Sources: ?from= query param, VaultEvent timestamp
  Must include: replay curl command with ?from=ISO_TIMESTAMP,
  use case (agent reconnects after failure), all events since timestamp,
  then live stream continues

storage/events/reconnect.mdx          Type C
  Sources: VaultSSE reconnection, exponential backoff
  Must include: what happens on disconnect, exponential backoff (1s→30s),
  last-event-id tracking, how to not miss events, the heartbeat comment

storage/files/overview.mdx            Type A
  Sources: VaultFile, GCS, CDN
  Must include: what file delivery is, artifacts.prudra.com CDN,
  pre-signed URL architecture (file → GCS, not through API),
  file limits by plan, when to use files vs documents

storage/files/upload.mdx              Type B
  Sources: example-02 file upload curl
  Must include: multipart/form-data upload, curl with -F flag,
  response with assetUrl (artifacts.prudra.com/...), file size limits,
  what Content-Length is used for (enforce size limit)

storage/files/cdn.mdx                 Type C
  Sources: artifacts.prudra.com, GCS CDN setup
  Must include: what the CDN does, artifacts.prudra.com
  (artifacts = vault files),
  how URLs are structured, CDN caching, pre-signed URL security

storage/files/limits.mdx              Type C
  Sources: billing plans.ts, file limits
  Must include: table of file limits by plan (size, count, total storage),
  what happens when you hit a limit (429 with LimitError),
  storage decrement on file deletion, upgrading for more storage

─── WEBHOOKS ────────────────────────────────────────────────

webhooks/overview.mdx                 Type A
  Sources: webhook spec, BullMQ worker
  Must include: what webhooks are, HMAC signing, retry behaviour,
  event types overview, diagram of delivery flow

webhooks/register.mdx                 Type B   ← dashboard tab
  Sources: example-05-webhooks.ts registration curl
  Must include: register curl, response (secret shown once!),
  save the secret immediately warning, events array (use ["*"]),
  Pro plan required (429 on Hobby)

webhooks/verify-signatures.mdx        Type B
  Sources: example-05 verifyWebhook function
  Must include: X-Prudra-Signature header, X-Prudra-Timestamp header,
  HMAC-SHA256 verification code, raw body requirement (express.raw()),
  why raw body matters (parsed body breaks HMAC)

webhooks/retry-behaviour.mdx          Type C
  Sources: BullMQ worker, WebhookDelivery model
  Must include: 5 retries with exponential backoff (5s→10s→20s→40s→80s),
  dead-letter queue, deliveredAt timestamp, why you must respond 200 quickly,
  what happens if you respond slowly (duplicate delivery)

webhooks/event-reference.mdx          Type C
  Sources: webhook events emitted throughout the system
  Must include: complete table of all event types:
    payment.received, vault.created, vault.sealed, vault.expiring,
    vault.expired, deposit.success, withdrawal.initiated,
    withdrawal.complete, withdrawal.failed
  For each: when it fires, payload shape, example payload


─── PLATFORM ────────────────────────────────────────────────

platform/billing/plans.mdx            Type A
  Sources: billing/plans.ts PLANS object
  Must include: complete feature comparison table (Hobby/Pro/Enterprise),
  all limits from plans.ts (never hardcode — describe the current values),
  what -1 means (unlimited), how to upgrade, enterprise contact

platform/billing/usage.mdx            Type B   ← dashboard tab
  Sources: GET /billing/usage, UsageReport schema
  Must include: GET /billing/usage curl, full response example,
  what percent means, what unlimited means in the response,
  features object (boolean flags per feature)

platform/billing/upgrade.mdx          Type B   ← dashboard tab
  Sources: Stripe integration (Phase 3)
  Must include: how to upgrade (dashboard → settings → billing),
  what happens immediately on upgrade (limits update, features unlock),
  Note: "Stripe billing integration is coming in Phase 3"

platform/orgs/overview.mdx            Type A
  Sources: Organisation model, OrganisationMember
  Must include: what an organisation is, owner vs admin vs member roles,
  one subscription per org, multiple members, CardGroup

platform/orgs/api-keys.mdx            Type B   ← dashboard tab
  Sources: API key routes
  Must include: create key (raw key shown once warning),
  list keys (keyHash never returned), revoke key,
  key format (prv_test_sk_ vs prv_live_sk_), accountId audit field

platform/orgs/members.mdx             Type B   ← dashboard tab
  Sources: OrganisationMember model
  Must include: roles (owner/admin/member), what each role can do,
  invite flow (dashboard — coming soon note), member management

platform/security/auth.mdx            Type C
  Sources: authenticate middleware, API key format
  Must include: Bearer token authentication, header format,
  test vs live key distinction, 401 error format, key rotation procedure

platform/security/api-keys.mdx        Type C
  Sources: API key best practices
  Must include: never commit keys to git, use env vars, test keys for dev,
  live keys for production, rotate if compromised, one key per service,
  revokedAt soft delete


─── AGENT GUIDES ────────────────────────────────────────────

These are written for AI agents consuming Prudra-protected APIs —
NOT for human developers. Write them to be machine-readable:
short paragraphs, direct answers first, structured data, no marketing.

agent-guides/overview.mdx
  Load this page first. What Prudra is, the three products,
  base URL, authentication header format, error format.

agent-guides/quickstart.mdx
  Sources: example-06-x402-agent.ts
  How to pay for a Prudra-protected endpoint: detect 402,
  decode PAYMENT-REQUIRED, sign ERC-3009, resubmit.

agent-guides/payments.mdx
  Sources: payment flow docs
  The two protocols (x402 and MPP), which header each uses,
  how to detect which one to use, credential format for each.

agent-guides/vault.mdx
  Sources: vault docs
  What vaultId is in the response, how to read vault contents,
  how to subscribe to SSE events, what events mean.

agent-guides/wallet.mdx
  Sources: wallet docs
  How agent wallets work, child addresses, how to check balance.

agent-guides/webhooks.mdx
  Sources: webhook docs
  How to verify a Prudra webhook signature (for agents that receive them).

agent-guides/troubleshooting.mdx
  Common failures and recovery:
  402 with no challenge headers (plan limit),
  402 with challenge headers (no payment),
  409 duplicate-payment (replay),
  429 (rate limit or plan limit),
  session expired (fresh 402 needed)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STEP 3 — VALIDATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

After all pages are written:

  npx mintlify dev
  → must start with no errors

  npx mintlify broken-links
  → must return zero broken internal links

  npx mintlify build
  → must succeed

Fix any errors before marking the task complete.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ABSOLUTE RULES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

NEVER touch files in api-reference/ — they are auto-generated
NEVER use placeholder text (TODO, coming soon, insert here)
  Exception: "Dashboard support coming soon" in Dashboard tabs only
NEVER name third-party vendors unless developers must interact with them
NEVER hardcode limit numbers — always express them descriptively
  ("Hobby plan allows 3 active vaults" not "the limit is 3")
  The quality standard explains why — limits change and docs drift
NEVER write code examples that don't match the working examples
ALWAYS complete the quality checklist before moving to the next page
ALWAYS update docs.json when adding a new page
ALWAYS use the correct page type structure from the quality standard
ALWAYS include at least 3 internal links per page
ALWAYS run npx mintlify dev after every 10 pages to catch errors early

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
WHAT TO PASS TO THIS AGENT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Give this agent access to:

  1. prudra-docs-architecture-quality-v2.md  ← architecture + quality standard
  2. prudra-spec-v0.5.docx                   ← product spec (content source)
  3. dev-agent-workflow.md                   ← workflow + additional quality guidance
  4. swagger-comments-and-tsoa-template.ts   ← API descriptions for every endpoint
  5. examples/example-01 through example-09  ← working code for examples
  6. billing/plans.ts                        ← current plan limits
  7. The existing docs folder                ← audit what exists already