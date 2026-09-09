# PONS 402 / x402 GOVERNANCE STANDARD v0.1

Type: Governance Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Technical Rail: x402 where applicable

## DESCRIPTION

Defines 402 as the PONS governance, compliance, audit, and authorization entity and x402 as a technical payment rail.

Canonical rule:

402 GOVERNS.

x402 TRANSPORTS WHERE APPLICABLE.

CAN ≠ MAY.

## 1. PURPOSE

This standard establishes the formal distinction between 402 and x402 throughout the PONS ecosystem.

The terms shall not be used interchangeably.

402 is the governing entity and policy authority.

x402 is a technical protocol or payment rail that may be used for machine-to-machine transactions where appropriate.

Technical ability to execute a transaction does not establish authorization to execute it.

## 2. 402 ENTITY

402 is responsible for determining what PONS, its agents, Labs, wallets, and automated systems MAY do.

402 responsibilities include:

governance
compliance
ethics
internal audit
tokenized-asset review
wallet authorization
capital policy
risk limits
jurisdictional restrictions
asset eligibility
contract eligibility
autonomy levels
incident review
policy changes
emergency governance
human accountability
public-record standards

## 3. 402 POLICY

402 translates human governance into explicit operating restrictions.

Production policy should ultimately be:

machine-readable
signed
versioned
hashable
auditable

A 402 policy may define:

approved chain IDs
approved Treasury wallets
approved Executor wallets
approved token contracts
approved trading pairs
approved routers
approved smart contracts
maximum transaction value
maximum daily trading value
maximum position size
maximum Executor exposure
maximum slippage
minimum liquidity
maximum basis deviation
approved data sources
oracle freshness requirements
drawdown limits
jurisdiction rules
autonomy level
emergency state
policy version
effective date
expiration date

## 4. 402 AUTHORIZATION GATE

The canonical PONS authorization control shall be called:

402 AUTHORIZATION GATE

or

402 POLICY ENGINE.

The term x402 Gate shall only be used when the technical operation specifically uses the x402 protocol.

The 402 Authorization Gate determines:

CAN the system technically perform this action?

MAY the system perform this action under current policy?

Both must be satisfied before execution.

## 5. AUTHORIZATION STATES

The 402 Policy Engine shall return:

PASS

DENY

ESCALATE

PASS means the proposed action satisfies current machine-enforceable policy.

DENY means the action violates policy or required compliance cannot be verified.

ESCALATE means human governance review is required.

Ambiguity shall never automatically become PASS.

## 6. FAIL-CLOSED RULE

If 402 cannot verify authorization:

NO AUTONOMOUS EXECUTION.

Examples:

unknown policy version
invalid policy signature
unknown asset
unknown contract
unknown recipient
jurisdiction uncertainty
stale market state
unverified wallet state
state-reconciliation failure
policy-engine failure
open critical incident
kill switch active

Required response:

DENY

or

ESCALATE.

## 7. x402

x402 is a technical protocol or transaction rail.

It may support:

machine-to-machine payments
agent payments
API payments
HTTP 402-based transactions
automated settlement
machine-readable payment requests
compatible agent-commerce workflows

x402 does not establish governance authority.

A transaction being technically possible through x402 does not mean PONS is authorized to perform it.

## 8. RELATIONSHIP BETWEEN 402 AND x402

Canonical architecture:

402 ENTITY

↓

402 GOVERNANCE POLICY

↓

402 POLICY ENGINE

↓

PONS DECISION

↓

EXECUTION INTENT

↓

x402 OR OTHER APPROVED EXECUTION RAIL

↓

SIGNER-LEVEL VALIDATION

↓

BLOCKCHAIN / SETTLEMENT

402 remains authoritative regardless of the technical rail used.

## 9. PONS ROLE

PONS is the Control Tower and operating system coordinating:

market observation
strategy
AI analysis
Treasury management
Executor wallets
risk controls
transaction proposals
provenance
audit
human escalation

PONS orchestrates.

402 authorizes.

The signer enforces.

The blockchain records execution.

## 10. AI AUTHORITY

AI systems including ENOCH ONE may:

observe
analyze
rank
recommend
generate signals
propose HOLD
propose ADD
propose TRIM
propose EXIT
propose RE-ENTER
identify anomalies
support research
support audit

AI may not independently modify:

402 policy
Treasury addresses
wallet ownership
signing authority
capital ceilings
asset allowlists
contract allowlists
jurisdiction rules
kill switches
emergency authority

AI output is subordinate to 402.

## 11. HUMAN GOVERNANCE

Human governance retains authority over:

constitutional changes
402 policy approval
capital escalation
new assets
new contracts
new strategies
critical incidents
emergency actions
autonomy expansion
system restart after serious incidents

Human-governed does not mean human-manually-operated.

Routine compliant actions may eventually be delegated within previously approved boundaries.

## 12. POLICY VERSIONING

Every production 402 policy shall have:

policy ID
version
creation timestamp
effective timestamp
approving authority
policy hash
status

Permitted statuses:

DRAFT
APPROVED
ACTIVE
SUSPENDED
SUPERSEDED
REVOKED

PONS may execute autonomously only against an ACTIVE policy.

## 13. POLICY HASH

Every executable decision shall reference its governing policy.

Canonical chain:

DECISION

↓

POLICY ID

↓

POLICY HASH

↓

402 RESULT

↓

EXECUTION INTENT

↓

SIGNER VALIDATION

↓

TRANSACTION

This establishes which exact policy authorized each transaction.

## 14. POLICY CHANGE CONTROL

AI shall not modify production 402 policy.

The Strategy Engine shall not modify production 402 policy.

The Executor shall not modify production 402 policy.

Material changes require authorized governance.

Changes should generate:

new policy version
change record
approval record
new policy hash
effective timestamp

Higher-risk changes may additionally require:

multisig approval
time delay
independent review

## 15. EMERGENCY AUTHORITY

402 shall maintain authority to:

suspend autonomy
revoke policy
reduce capital limits
disable assets
disable contracts
disable strategies
require human approval
initiate incident investigation

Emergency controls should be capable of reducing authority faster than ordinary governance can expand authority.

## 16. AUDIT RESPONSIBILITY

402 owns or supervises the governance audit layer.

Audit records include:

policy history
authorization events
incident records
red-team results
human overrides
autonomy changes
capital-limit changes
legal-assumption records
compliance-assumption records

Blockchain evidence provides independent execution provenance.

## 17. LEGAL BOUNDARY

402 does not create legal clearance merely by labeling an activity compliant.

x402 does not create legal clearance merely because it provides a technical payment mechanism.

PONS does not create legal clearance merely by describing an activity as:

educational
utility
research
tokenized
onchain
autonomous

Relevant legal determinations remain a separate 402 governance workstream and should be translated into operational restrictions where applicable.

## 18. CAN ≠ MAY

CAN means:

the technology is capable of performing the action.

MAY means:

current governance authorizes the action.

A valid transaction requires both.

Examples:

The wallet CAN call a contract.

402 may determine it MAY NOT.

The agent CAN create a transfer proposal.

402 may determine it MAY NOT execute.

x402 CAN transmit a payment request.

402 may determine the payment MAY NOT occur.

Technical capability never overrides governance.

## 19. CANONICAL TERMINOLOGY

Use:

402 ENTITY
402 GOVERNANCE
402 POLICY
402 POLICY ENGINE
402 AUTHORIZATION GATE
402 AUDIT
402 HUMAN GATE

Use:

x402 PROTOCOL
x402 PAYMENT RAIL
x402 TRANSACTION

only when specifically referring to the technical x402 protocol.

Do not use:

x402 ENTITY

when referring to the governing organization.

Do not use:

x402 POLICY

when referring to general PONS governance unless the policy specifically governs x402 activity.

## 20. CANONICAL STATEMENT

402 IS THE GOVERNING ENTITY.

402 DEFINES MAY.

PONS ORCHESTRATES.

AI ADVISES.

THE EXECUTOR ACTS ONLY WITHIN DELEGATED AUTHORITY.

x402 IS A TECHNICAL RAIL WHERE APPLICABLE.

TECHNICAL CAPABILITY DOES NOT CREATE AUTHORIZATION.

402 GOVERNS.

x402 TRANSPORTS.

CAN ≠ MAY.