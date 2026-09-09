# PONS SIGNER-LEVEL ENFORCEMENT STANDARD v0.1

Type: Execution Security Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Control Layer: PONS Control Tower
Applies To: All PONS autonomous, human-gated, and bounded execution accounts

## DESCRIPTION

Defines the signer and smart-account controls that physically prevent PONS autonomous agents from exceeding delegated authority, including contract allowlists, recipient restrictions, transaction caps, policy-hash checks, session-key limits, simulation requirements, and fail-closed execution.

## 1. PURPOSE

The purpose of this standard is to ensure that PONS authorization is enforced at the transaction-signing boundary.

The system shall not rely only on:

AI instructions
strategy rules
Python logic
dashboard flags
human-readable policies
GitHub configuration

to prevent unauthorized capital movement.

Critical restrictions should be independently enforced at the signer, smart-account, module, or equivalent execution-control layer wherever technically practical.

Canonical rule:

AI MAY PROPOSE.

402 MAY AUTHORIZE.

THE SIGNER MUST VERIFY.

THE CHAIN EXECUTES ONLY WHAT THE SIGNER PERMITS.

## 2. SECURITY PRINCIPLE

If the agent runtime can sign arbitrary calldata, the system is not considered bounded.

The signer must not behave as:

SIGN ANYTHING THE BOT SENDS.

The signer should behave as:

SIGN ONLY A TRANSACTION THAT SATISFIES CURRENT DELEGATED AUTHORITY.

## 3. AUTHORITY STACK

Canonical authority stack:

HUMAN GOVERNANCE

↓

TREASURY MULTISIG / EMERGENCY AUTHORITY

↓

ONCHAIN OR SMART-ACCOUNT INVARIANTS

↓

SIGNED 402 POLICY

↓

DETERMINISTIC 402 EVALUATOR

↓

PONS STRATEGY ENGINE

↓

AI ADVISORY

↓

EXECUTION INTENT

↓

SIGNER-LEVEL VALIDATION

↓

SIGNATURE

↓

BROADCAST

No lower layer may expand the authority of a higher layer.

## 4. TREASURY SIGNER

Treasury signing authority shall remain separate from autonomous Executor authority.

Treasury signer may authorize:

capital allocation
capital recall
new Executor funding
governance changes
emergency recovery
other explicitly approved Treasury actions

The autonomous agent shall not possess unrestricted Treasury signing authority.

## 5. EXECUTOR SIGNER

The Executor signer or smart account shall operate with restricted authority.

It should be capable of signing only transactions that satisfy all applicable execution constraints.

Potential implementation patterns include:

smart accounts
Safe modules
session keys
restricted signing services
account-abstraction permission systems
MPC policies
hardware-backed delegated signing
other EVM-compatible constrained-signing architectures

The specific implementation may evolve.

The governing requirement does not change:

DELEGATED AUTHORITY MUST BE TECHNICALLY BOUNDED.

## 6. ALLOWED CHAINS

The signer shall verify the intended chain.

Required checks may include:

chain ID
network identity
policy chain
transaction chain
current signer context

Wrong chain:

DENY.

Unknown chain:

DENY.

## 7. APPROVED EXECUTOR

The signer shall verify that the transaction originates from an approved Executor identity.

Unknown Executor:

DENY.

Revoked Executor:

DENY.

Expired session authority:

DENY.

## 8. POLICY HASH VERIFICATION

Every Execution Intent shall reference an active 402 policy hash.

The signer or signer-control layer should verify:

policy ID
policy version
policy status
policy hash
effective state
emergency state

If transaction intent references an unknown or inactive policy:

DENY.

If policy changed after authorization:

RE-EVALUATE BEFORE SIGNING.

## 9. TOKEN ALLOWLIST

The signer shall enforce approved token contracts.

Token identity shall be based on:

contract address
chain ID
policy status

Ticker or name alone is insufficient.

Unknown token:

DENY.

Suspended token:

DENY.

## 10. CONTRACT ALLOWLIST

The signer shall enforce approved destination contracts.

Permitted contracts may include:

approved router
approved pool
approved Treasury
approved protocol
approved strategy module

Unknown contract:

DENY.

## 11. METHOD ALLOWLIST

Approval of a contract does not automatically approve every function on that contract.

Where practical, signer policy should restrict:

contract
method selector
value
asset
amount
recipient

Example:

Router approved

does not mean:

all router methods approved.

Unknown function:

DENY.

## 12. RECIPIENT RESTRICTIONS

The signer shall verify transaction recipients.

Permitted recipients may include:

approved router
approved pool
approved Treasury
approved Executor
explicit policy-authorized address

Unknown recipient:

DENY.

The AI shall not be able to create recipient authority.

## 13. TREASURY DESTINATION LOCK

Treasury sweep or harvest destination shall be protected from autonomous modification.

If the agent proposes a different Treasury destination:

DENY.

Changing the Treasury destination requires:

governance authorization
policy update
new policy hash
signer-policy update
audit record

## 14. TRANSACTION VALUE LIMIT

Every signer policy shall enforce a maximum value per transaction.

Inputs may include:

maximum native value
maximum ERC-20 amount
maximum reference value
maximum percentage of Executor
maximum percentage of strategy allocation

If proposed amount exceeds limit:

DENY.

## 15. DAILY LIMIT

The signer-control system should enforce a maximum cumulative amount over a defined period.

Potential limits:

maximum daily turnover
maximum daily transfer value
maximum daily loss
maximum daily Treasury sweep
maximum daily trade count

A compliant single trade may still be denied if cumulative limits would be exceeded.

## 16. EABR LIMIT

Signer policy shall support the Effective Agent Blast Radius ceiling.

Before signing, the system shall evaluate:

current EABR
projected EABR after transaction
maximum policy EABR

If:

PROJECTED EABR > MAXIMUM EABR

then:

DENY.

## 17. POSITION LIMIT

The signer-control path may enforce or reference projected portfolio limits.

Examples:

maximum asset concentration
maximum strategy concentration
maximum Lab exposure
maximum reference exposure

If projected state violates hard position policy:

DENY.

## 18. TOKEN APPROVAL POLICY

Unlimited token approvals should be prohibited unless explicitly authorized by separate governance.

Default:

approve exact amount needed.

Preferred process:

CALCULATE REQUIRED ALLOWANCE

↓

APPROVE REQUIRED AMOUNT

↓

EXECUTE

↓

VERIFY RESIDUAL ALLOWANCE

↓

REDUCE OR CLEAR EXCESS

Request for unlimited approval:

DENY OR ESCALATE.

## 19. SESSION KEYS

Session-key authority shall be explicitly bounded.

Each key should define:

account
strategy
chain
allowed contracts
allowed tokens
allowed functions
per-transaction cap
cumulative cap
start time
expiration time
recipient restrictions
revocation authority

Expired key:

DENY.

Revoked key:

DENY.

Unknown key:

DENY.

## 20. SESSION KEY SELF-ESCALATION

A session key shall not be able to:

extend its own expiration
increase its own spending limit
add new contracts
add new recipients
add new assets
grant another session key
modify Treasury permissions
modify 402 policy

Any such action:

DENY.

## 21. TRANSACTION INTENT

Every executable action shall produce a canonical Execution Intent.

Required fields may include:

intent_id
strategy_id
chain_id
executor
action
input_token
output_token
input_amount
minimum_output
router
recipient
slippage
deadline
policy_id
policy_hash
authorization_id
timestamp
intent_hash

The Execution Intent is the object that 402 authorizes.

## 22. INTENT HASH

The Execution Intent shall be deterministically hashable.

Any material change to:

asset
amount
router
recipient
chain
deadline
slippage
policy
calldata

must result in a different intent hash.

## 23. INTENT / CALLDATA MATCH

Before signing, PONS shall verify that actual calldata corresponds to the approved Execution Intent.

Compare:

chain
target contract
method
asset
amount
recipient
value
slippage
deadline
policy hash where represented
expected state change

Mismatch:

DENY.

Create incident if material.

## 24. TRANSACTION SIMULATION

Every autonomous or human-gated transaction should be simulated before signing where technically practical.

Simulation should evaluate:

expected token movement
unexpected token movement
approval changes
balance changes
revert risk
callback behavior
gas
recipient
router
minimum received
contract state changes

Unexpected material state change:

DENY OR ESCALATE.

## 25. SIMULATION RESULT

Possible simulation states:

PASS
FAIL
INCONCLUSIVE

PASS:

continue to signer checks.

FAIL:

DENY.

INCONCLUSIVE:

ESCALATE OR DENY.

Never:

ASSUME SAFE.

## 26. PRICE / SLIPPAGE CHECK

Immediately before signing or broadcast, the system should verify current execution conditions against policy.

Check:

quote
expected output
minimum output
slippage
spread
price impact
basis
liquidity

If execution condition has materially changed since authorization:

RE-EVALUATE.

## 27. NONCE CONTROL

The signer-control system shall monitor nonce state.

Unexpected nonce:

DENY OR RECONCILE.

Nonce collision:

PAUSE EXECUTION.

Replacement transactions should preserve the original authorized intent unless a new authorization event is created.

## 28. DUPLICATE EXECUTION

The signer shall prevent unintended duplicate execution of the same intent.

Each Execution Intent should have a unique identifier and execution state.

Possible states:

CREATED
AUTHORIZED
SIGNED
BROADCAST
SETTLED
FAILED
CANCELLED
EXPIRED

An intent already SETTLED shall not execute again.

## 29. INTENT EXPIRATION

Execution Intents should expire.

After expiration:

NO SIGNATURE.

A new transaction requires:

new market state
new decision or refreshed decision
new authorization
new intent

This prevents stale transactions from executing later under changed conditions.

## 30. POLICY EXPIRATION

If the policy governing an intent expires before signing:

DENY.

Do not sign using an expired policy.

## 31. KILL SWITCH

The signer-control layer must respect emergency state.

If:

AUTONOMY_OFF

or

SYSTEM_STOP

then:

NO NEW AUTONOMOUS SIGNATURES.

A kill switch that merely stops the application but leaves unrestricted signing authority active is insufficient.

## 32. EMERGENCY REVOCATION

Emergency governance should be able to:

disable Executor module
revoke session key
suspend signer permission
reduce limits
clear or reduce approvals
stop new signatures

Emergency reduction of authority should be faster than ordinary expansion of authority.

## 33. UNKNOWN STATE

If signer-control state cannot be verified:

DENY.

Examples:

cannot verify policy
cannot verify account permissions
cannot verify nonce
cannot verify active session key
cannot verify chain
cannot verify recipient
cannot verify current emergency state

Canonical rule:

FAILURE TO VERIFY = NO SIGNATURE.

## 34. SIGNING KEY LOCATION

Live signing credentials shall not be stored in:

GitHub repository
public source control
plain-text config
AI prompt
LLM memory
public dashboard
logs
documentation
client-side application bundle

Signer architecture should minimize raw-key exposure.

## 35. GITHUB BOUNDARY

GitHub may contain:

signer interface code
policy schemas
test cases
simulation logic
deployment definitions
documentation

GitHub compromise alone should not be sufficient to create an unrestricted Treasury transaction.

## 36. SIGNER / BOT SEPARATION

Where practical, the signer should operate as a separate security boundary from the strategy agent.

Conceptual structure:

PONS AGENT

↓

SUBMITS INTENT

↓

402 POLICY SERVICE

↓

SIGNER SERVICE / SMART ACCOUNT

↓

CHAIN

The bot should not directly possess unrestricted raw signing authority.

## 37. COMPROMISED AGENT ASSUMPTION

PONS shall explicitly test the assumption:

THE AGENT IS COMPROMISED.

The red team should attempt to make the agent:

send to attacker
use unapproved token
use unapproved router
increase trade size
approve unlimited amount
modify Treasury
change policy
extend session key
bypass kill switch

Expected result:

ALL UNAUTHORIZED SIGNING ATTEMPTS FAIL.

## 38. COMPROMISED GITHUB ASSUMPTION

PONS shall test:

GITHUB OR CI/CD IS COMPROMISED.

Expected architecture:

malicious code may create bad intents

but

signer-level policy prevents unauthorized transactions.

If repository compromise automatically grants unrestricted signing:

FAIL.

## 39. COMPROMISED EXECUTOR KEY ASSUMPTION

If an Executor signing credential is compromised, hard restrictions should still limit damage where the chosen architecture supports it.

The system should test:

maximum permitted trade
maximum permitted recipient
maximum permitted contract
maximum daily value
maximum EABR

The attacker should not exceed enforced boundaries.

## 40. SIGNER-LEVEL INCIDENTS

Create an incident if:

unauthorized signature produced
unknown contract reaches signer
intent/calldata mismatch
policy mismatch
unexpected signer
session-key violation
limit bypass
unknown recipient
kill-switch bypass
duplicate execution
unverified chain

Severity shall be determined by the PONS Incident Standard.

## 41. HARD FAILURE CONDITIONS

The following are presumptive critical failures:

signer can transfer to arbitrary recipient
signer can interact with arbitrary contract
agent can modify signer limits
agent can modify Treasury destination
agent can disable signer restrictions
session key can self-escalate
policy hash can be bypassed
kill switch can be bypassed
intent and signed transaction differ materially
unlimited approval can be created outside policy

Any such finding:

NO BOUNDED AUTONOMY.

## 42. HUMAN-GATED EXECUTION

Human approval does not replace signer-level validation.

Canonical human-gated flow:

AGENT PROPOSES

↓

402 EVALUATES

↓

SIMULATION

↓

HUMAN REVIEWS

↓

HUMAN APPROVES

↓

SIGNER VALIDATES

↓

SIGNATURE

↓

BROADCAST

A human should not be able to accidentally approve a transaction outside hard technical constraints unless using a separate higher-level governance path.

## 43. AUTONOMOUS EXECUTION

Canonical autonomous flow:

OBSERVATION

↓

SIGNAL

↓

DECISION

↓

402 PASS

↓

EXECUTION INTENT

↓

SIMULATION PASS

↓

STATE RECONCILIATION PASS

↓

SIGNER VALIDATION PASS

↓

SIGNATURE

↓

BROADCAST

↓

SETTLEMENT

Any failed required stage:

NO EXECUTION.

## 44. AUDIT FIELDS

Each signing event should record:

signer_event_id
intent_id
authorization_id
policy_id
policy_hash
executor
signer identity or role
session-key ID where applicable
chain ID
target contract
method
amount
recipient
simulation result
EABR check
limit check
emergency-state check
sign result
timestamp
transaction hash if broadcast

## 45. PUBLIC PROVENANCE

Security-sensitive signing details may remain private.

Public audit may expose:

intent hash
policy hash
transaction hash
signer-policy version
authorization result
execution result
audit classification

This enables verification without exposing credentials.

## 46. SIGNER ASSURANCE METRICS

PONS shall track:

unauthorized signatures
intent/calldata mismatches
unknown-recipient attempts
unknown-contract attempts
limit-bypass attempts
policy-mismatch attempts
expired-session attempts
duplicate-execution attempts
kill-switch violations
successful red-team boundary violations

Target:

UNAUTHORIZED SIGNATURES = 0.

## 47. ASSURANCE REQUIREMENT

Before bounded autonomy:

SIGNER-LEVEL CONSTRAINTS MUST PASS.

No profitability result may compensate for signer-control failure.

If strategy performance is excellent but signer controls fail:

ASSURANCE = FAIL.

AUTONOMY = NO-GO.

## 48. RELATIONSHIP TO 402

402 defines MAY.

Signer enforcement makes MAY technically binding.

Canonical relationship:

402 POLICY

↓

402 AUTHORIZATION

↓

SIGNER RESTRICTIONS

↓

SIGNATURE

Without signer enforcement, 402 may be only advisory.

The PONS objective is to make material authorization constraints enforceable.

## 49. RELATIONSHIP TO AI

AI may generate any recommendation.

Even malicious or incorrect output shall remain subordinate to signer controls.

The security objective is not:

AI NEVER MAKES A BAD REQUEST.

The objective is:

A BAD REQUEST CANNOT BECOME AN UNAUTHORIZED SIGNED TRANSACTION.

## 50. CANONICAL RULES

AI MAY PROPOSE.

402 MAY AUTHORIZE.

THE SIGNER MUST VERIFY.

UNKNOWN CHAIN = DENY.

UNKNOWN TOKEN = DENY.

UNKNOWN CONTRACT = DENY.

UNKNOWN RECIPIENT = DENY.

UNKNOWN POLICY = DENY.

POLICY MISMATCH = DENY.

INTENT / CALLDATA MISMATCH = DENY.

EXPIRED INTENT = DENY.

EXPIRED SESSION KEY = DENY.

EABR ABOVE LIMIT = DENY.

KILL SWITCH ACTIVE = DENY.

SIMULATION FAILURE = DENY.

FAILURE TO VERIFY = NO SIGNATURE.

THE AGENT SHALL NOT DEFINE ITS OWN AUTHORITY.

THE AGENT SHALL NOT INCREASE ITS OWN AUTHORITY.

THE TREASURY SIGNER SHALL NOT BE CONTROLLED BY THE TRADING AGENT.

PONS DOES NOT TRUST THE MODEL TO BE PERFECT.

PONS CONSTRAINS WHAT A WRONG OR COMPROMISED MODEL CAN DO.

POLICY APPROVES.

SIGNER VERIFIES.

CHAIN EXECUTES.