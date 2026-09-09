# PONS 402 MACHINE-READABLE POLICY STANDARD v0.1

Type: Authorization Policy Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Control Layer: PONS Control Tower
Applies To: All PONS autonomous and human-gated execution

## DESCRIPTION

Defines the machine-readable 402 policy object used by PONS to authorize or deny autonomous actions, including asset, wallet, contract, capital, market-data, risk, jurisdiction, autonomy, emergency, and policy-version controls.

## 1. PURPOSE

The purpose of this standard is to convert 402 governance from human-readable doctrine into a machine-verifiable authorization policy.

The 402 Policy Object shall determine whether a proposed PONS action is permitted.

Canonical rule:

POLICY MUST BE VERIFIED BEFORE EXECUTION.

If policy cannot be verified:

NO AUTONOMOUS EXECUTION.

## 2. CORE PRINCIPLE

The 402 Policy Object shall answer:

WHO may act?

WHAT may be traded?

WHERE may value move?

HOW MUCH may be moved?

UNDER WHAT CONDITIONS may execution occur?

WHEN is the policy valid?

WHICH STRATEGY is authorized?

WHICH JURISDICTIONAL OR ELIGIBILITY RESTRICTIONS apply?

WHAT happens during an emergency?

## 3. POLICY OBJECT

Every active 402 policy shall be represented as a structured machine-readable object.

Preferred representation:

JSON

or another deterministic structured format suitable for hashing, validation, signing, storage, and policy-engine evaluation.

The policy object shall not depend on natural-language interpretation for critical execution controls.

## 4. REQUIRED POLICY IDENTITY FIELDS

Each policy shall contain:

policy_id
policy_version
policy_status
created_at
effective_at
expires_at where applicable
governance_authority
approving_authority
policy_hash
previous_policy_hash where applicable

Permitted status values:

DRAFT
APPROVED
ACTIVE
SUSPENDED
SUPERSEDED
REVOKED

Only ACTIVE policy may authorize autonomous execution.

## 5. CHAIN POLICY

The policy shall define permitted blockchain environments.

Required fields may include:

chain_id
chain_name
approved_rpc_policy
minimum_confirmation_requirement
finality_requirement
network_status_requirement

If current chain does not equal approved chain:

DENY.

If chain cannot be verified:

DENY.

## 6. TREASURY POLICY

The policy shall define approved Treasury addresses.

Fields may include:

treasury_id
treasury_address
permitted_funding_actions
permitted_harvest_actions
maximum_allocation
minimum_reserve
governance_required_for_withdrawal
emergency_status

An autonomous agent shall not modify Treasury identity.

## 7. EXECUTOR POLICY

The policy shall define approved Executor wallets.

Fields may include:

executor_id
executor_address
strategy_id
autonomy_level
maximum_wallet_value
maximum_eabr
maximum_single_trade
maximum_daily_turnover
maximum_daily_loss
maximum_position_size
session_key_expiration
approved_modules

Unknown Executor:

DENY.

## 8. ASSET ALLOWLIST

Every tradable asset shall be explicitly identified by contract address.

Fields may include:

asset_id
asset_symbol
asset_name
contract_address
decimals
asset_class
lab_id
reference_asset
status
maximum_position
maximum_trade
minimum_liquidity
maximum_basis_deviation

Ticker alone shall never establish asset identity.

Contract address controls.

Unknown token contract:

DENY.

## 9. ASSET STATUS

Permitted asset states may include:

ACTIVE
WATCH
RESTRICTED
SUSPENDED
DISABLED

ACTIVE:

may be used according to policy.

WATCH:

observation only.

RESTRICTED:

requires additional authorization.

SUSPENDED:

no new autonomous execution.

DISABLED:

no execution.

## 10. CONTRACT ALLOWLIST

The policy shall define contracts an Executor may interact with.

Fields may include:

contract_id
contract_address
contract_type
chain_id
approved_methods
security_review_status
maximum_value
callback_policy
approval_policy
version
bytecode_hash where appropriate

Unknown contract:

DENY.

## 11. ROUTER ALLOWLIST

Approved routers shall be explicitly listed.

Fields may include:

router_id
router_address
protocol
approved_methods
maximum_slippage
maximum_trade_value
security_status
policy_status

The agent shall not select an unapproved router based solely on:

price
AI recommendation
website
social-media source
natural-language instruction.

## 12. RECIPIENT POLICY

The policy shall define permitted destinations.

Potential classes:

APPROVED_ROUTER
APPROVED_POOL
APPROVED_TREASURY
APPROVED_EXECUTOR
APPROVED_PROTOCOL
OTHER_EXPLICITLY_AUTHORIZED_ADDRESS

Unknown recipient:

DENY.

The AI shall not create new recipient authority.

## 13. TRADE-SIZE POLICY

Every policy shall specify transaction ceilings.

Fields may include:

maximum_single_trade_value
maximum_single_trade_percentage
maximum_daily_turnover
maximum_asset_turnover
maximum_executor_allocation
maximum_strategy_allocation

If proposed transaction exceeds any hard limit:

DENY.

Do not silently resize unless the policy explicitly permits deterministic clipping.

## 14. POSITION-LIMIT POLICY

The policy shall define maximum permitted concentration.

Fields may include:

maximum_asset_weight
maximum_reference_asset_weight
maximum_lab_weight
maximum_strategy_weight
maximum_correlated_exposure
maximum_total_autonomous_exposure

Projected post-trade state must pass position-limit validation.

## 15. EFFECTIVE AGENT BLAST RADIUS POLICY

The 402 Policy Object shall reference the permitted EABR ceiling.

Required values may include:

maximum_eabr
current_eabr_requirement
projected_eabr_requirement
maximum_allowance_exposure
maximum_pending_exposure
maximum_session_authority

If projected EABR exceeds policy:

DENY.

## 16. SLIPPAGE POLICY

The policy shall define maximum acceptable execution slippage.

Fields may include:

maximum_slippage_bps
maximum_price_impact_bps
maximum_spread_bps
maximum_deviation_from_quote

If expected or simulated slippage exceeds policy:

DENY OR ESCALATE.

## 17. LIQUIDITY POLICY

Every autonomous strategy shall define minimum liquidity requirements.

Possible measurements:

minimum pool liquidity
minimum daily volume
maximum participation rate
minimum quoted depth
maximum position as percentage of market liquidity

If liquidity cannot support the proposed transaction within policy:

DENY.

## 18. BASIS POLICY

Where a Lab token and reference asset are economically related but distinct, the policy shall define maximum permitted basis or tracking deviation.

Fields may include:

lab_asset
reference_asset
basis_formula
maximum_basis_percentage
maximum_tracking_error
basis_warning_threshold
basis_stop_threshold

If basis exceeds stop threshold:

NO AUTONOMOUS TRADE.

## 19. MARKET-DATA POLICY

The policy shall define approved market-data sources.

Fields may include:

source_id
source_type
asset
maximum_data_age
minimum_sources_required
maximum_source_divergence
halt_status_requirement
required_fields

A single unverified natural-language source shall not authorize market execution.

## 20. ORACLE POLICY

For strategies requiring oracle verification, policy shall define:

approved_oracles
minimum_number_of_oracles
maximum_oracle_age
maximum_oracle_divergence
fallback_behavior
halt_behavior

If required oracle state is unavailable:

DENY.

If oracles diverge beyond limit:

DENY OR ESCALATE.

## 21. HALT POLICY

The system shall define behavior for:

market halt
reference halt
trading suspension
oracle suspension
asset freeze
contract freeze

Default behavior:

NO NEW AUTONOMOUS EXECUTION.

Any exception requires explicit policy.

## 22. DRAWDOWN POLICY

The policy shall define drawdown limits.

Possible fields:

warning_drawdown
autonomy_reduction_drawdown
hard_stop_drawdown
daily_loss_limit
weekly_loss_limit
strategy_loss_limit

Example:

warning threshold crossed:

ESCALATE.

hard-stop threshold crossed:

AUTONOMY OFF.

## 23. RISK-BUDGET POLICY

PONS may define separate risk budgets for:

strategy
asset
Lab
Executor
Treasury
aggregate autonomous system

All applicable limits must pass simultaneously.

One passing limit does not override another failing limit.

## 24. AUTONOMY POLICY

Every strategy shall have an explicit autonomy level.

Example states:

L0_OBSERVE

L1_RECOMMEND

L2_HUMAN_GATED

L3_AUTONOMOUS_DUST

L4_LIMITED_AUTONOMY

L5_EXPANDED_BOUNDED_AUTONOMY

The policy shall specify which action types are permitted at each level.

## 25. ACTION POLICY

Allowed strategy actions may include:

HOLD
ADD
TRIM
EXIT
RE_ENTER
SWEEP_TO_TREASURY

Each action may have separate limits.

Example:

TRIM may be autonomous.

NEW_ASSET_ENTRY may require human approval.

## 26. HUMAN-APPROVAL POLICY

The policy shall define which actions require human review.

Possible triggers:

new asset
new contract
new router
oversized trade
new strategy
basis breach
market anomaly
jurisdiction uncertainty
open material incident
policy change
capital escalation

Return state:

ESCALATE.

## 27. JURISDICTION / ELIGIBILITY POLICY

Where relevant, 402 policy shall encode operating restrictions arising from legal or compliance review.

Fields may include:

permitted_jurisdictions
restricted_jurisdictions
user_or_entity_eligibility
asset_eligibility
transaction_eligibility
required_attestation
required_human_review

If eligibility cannot be established:

DENY OR ESCALATE.

Technical capability does not override eligibility restrictions.

## 28. EMERGENCY POLICY

The policy shall define emergency states.

Potential states:

NORMAL
ELEVATED
HUMAN_ONLY
PAUSED
AUTONOMY_OFF
SYSTEM_STOP

Emergency state may override ordinary strategy permissions.

Example:

AUTONOMY_OFF

means:

NO NEW AUTONOMOUS AUTHORIZATION.

## 29. INCIDENT POLICY

The policy shall define autonomy effects for incidents.

Example:

SEV-0:

no automatic change.

SEV-1:

monitor.

SEV-2:

possible human review.

SEV-3:

AUTONOMY OFF.

SEV-4:

SYSTEM STOP.

Open critical incidents shall prevent capital escalation.

## 30. POLICY PRECEDENCE

Where multiple policy rules apply, the most restrictive applicable control shall govern unless explicitly defined otherwise.

Example:

strategy says ADD.

risk policy says PASS.

liquidity policy says DENY.

Final result:

DENY.

No lower-level PASS may override a higher-priority DENY.

## 31. POLICY RESULT

The deterministic 402 evaluator shall return:

PASS
DENY
ESCALATE

The result should include:

policy_id
policy_hash
evaluation_timestamp
execution_intent_id
rules_evaluated
failed_rules
escalation_reason
decision_hash

## 32. PASS

PASS means all applicable mandatory rules were satisfied.

PASS does not itself guarantee execution.

The proposed transaction must still pass:

state reconciliation
transaction simulation
intent/calldata match
signer-level validation
execution-time checks

## 33. DENY

DENY means the proposed action shall not execute autonomously.

Examples:

unknown token
unknown router
wrong chain
oversized transaction
policy mismatch
stale data
EABR breach
kill switch active
invalid jurisdiction
unknown Treasury destination

DENY shall generate an audit event.

## 34. ESCALATE

ESCALATE means human governance review is required.

The human may:

APPROVE
DENY
REQUEST MODIFICATION

Human approval must not silently rewrite production policy.

If an exception is intended to become future policy, it should create a formal governance change.

## 35. POLICY HASHING

Every policy version shall be deterministically hashable.

The hash shall represent the exact policy used for authorization.

The policy hash may be:

stored in execution records
referenced in authorization events
anchored onchain
included in audit provenance
published in the Control Tower

Changing any material policy field shall change the hash.

## 36. POLICY SIGNING

Production policy should be signed or otherwise authenticated by authorized governance.

The system should verify:

policy integrity
policy source
policy version
policy status
policy signature or equivalent authorization

Invalid signature:

DENY.

## 37. POLICY IMMUTABILITY DURING EXECUTION

Once an Execution Intent enters authorization, the applicable policy version shall be fixed for that intent.

The system shall not evaluate one policy and sign under another without creating a new authorization event.

Policy changed after authorization but before execution:

RE-EVALUATE.

## 38. POLICY CHANGE CONTROL

Material changes require:

governance approval
new version
new policy hash
change reason
effective timestamp
audit record

Examples:

new asset
new router
higher trade limit
higher EABR
new strategy
new jurisdiction
new autonomy level

AI may recommend.

AI may not activate.

## 39. TIMELOCK POLICY

High-risk policy expansions may require a delay before activation.

Examples:

increasing capital ceiling
adding new execution contract
adding new signer
enabling leverage
enabling bridging
raising EABR
expanding autonomy

Emergency restrictions may take effect immediately.

Expansion should be slower than restriction.

## 40. FAIL-CLOSED POLICY ENGINE

If the 402 evaluator:

crashes
times out
returns malformed output
cannot load active policy
cannot verify policy hash
cannot verify required state

the result is:

DENY.

Never:

ASSUME PASS.

## 41. POLICY TESTING

Before activation, every production policy should pass validation tests.

Test categories:

schema validation
address validation
limit validation
hash reproducibility
signature validation
conflicting-rule detection
emergency-state behavior
unknown-input behavior
fail-closed behavior

Invalid policy shall not become ACTIVE.

## 42. POLICY SIMULATION

A proposed policy version should be testable against:

historical events
shadow-mode events
synthetic attack cases
known valid transactions
known invalid transactions

The goal is to determine whether policy creates unexpected authorization paths.

## 43. POLICY VERSION RECORD

Every version should preserve:

full policy object
hash
approval record
activation time
deactivation time
previous version
change summary
incident linkage if applicable

Policy history shall not be silently overwritten.

## 44. CONTROL TOWER DISPLAY

The public Control Tower may show:

active policy ID
policy version
policy hash
effective timestamp
autonomy level
emergency state
policy status
last policy change

The private operator Control Tower may additionally show:

full rule set
rule failures
pending policy changes
exact execution thresholds
simulation results
approval queue

Security-sensitive details may remain private.

## 45. PROVENANCE

Every execution should support reconstruction of:

OBSERVATION

↓

SIGNAL

↓

DECISION

↓

POLICY ID

↓

POLICY HASH

↓

402 EVALUATION

↓

AUTHORIZATION RESULT

↓

EXECUTION INTENT

↓

SIGNER VALIDATION

↓

TRANSACTION

↓

SETTLEMENT

## 46. SAMPLE POLICY OBJECT

Conceptual example:

policy_id: PONS-402-XLAB-0001

policy_version: 0.1

policy_status: ACTIVE

chain_id: approved Robinhood Chain ID

strategy_id: XLAB-SWING

treasury_address: approved XLAB Treasury

executor_address: approved XLAB Executor

allowed_assets:
XLAB
SPCX

allowed_routers:
approved router addresses only

maximum_single_trade:
policy-defined amount

maximum_daily_turnover:
policy-defined amount

maximum_eabr:
policy-defined amount

maximum_slippage:
policy-defined basis points

minimum_liquidity:
policy-defined threshold

maximum_basis_deviation:
policy-defined threshold

oracle_requirements:
policy-defined sources and freshness

autonomy_level:
L2 or L3 as approved

emergency_state:
NORMAL

policy_hash:
deterministically generated

This example is descriptive only.

Production values require separate governance approval.

## 47. RELATIONSHIP TO x402

This 402 Policy Object governs authorization.

If execution later uses x402:

402 POLICY

↓

402 AUTHORIZATION

↓

AUTHORIZED EXECUTION INTENT

↓

x402 TRANSPORT / PAYMENT MECHANISM

The x402 rail does not modify the governing 402 policy.

## 48. RELATIONSHIP TO AI

AI may produce:

market interpretation
risk interpretation
signal
recommended action
recommended size
explanation

The machine-readable 402 policy determines whether the resulting action is permitted.

AI output shall never be treated as a policy update.

## 49. RELATIONSHIP TO SIGNER

402 authorization is necessary but not sufficient.

Critical restrictions should also be enforced at the signer or smart-account layer where practical.

Canonical principle:

POLICY APPROVES.

SIGNER VERIFIES.

CHAIN EXECUTES.

## 50. CANONICAL RULES

402 POLICY MUST BE MACHINE-READABLE.

402 POLICY MUST BE VERSIONED.

402 POLICY MUST BE HASHABLE.

PRODUCTION POLICY MUST BE AUTHENTICATED.

ONLY ACTIVE POLICY MAY AUTHORIZE AUTONOMOUS EXECUTION.

UNKNOWN ASSET = DENY.

UNKNOWN CONTRACT = DENY.

UNKNOWN RECIPIENT = DENY.

UNKNOWN POLICY = DENY.

STALE REQUIRED DATA = DENY.

EABR ABOVE LIMIT = DENY.

KILL SWITCH ACTIVE = DENY.

POLICY ENGINE FAILURE = DENY.

AI MAY RECOMMEND POLICY CHANGES.

AI MAY NOT ACTIVATE POLICY CHANGES.

THE AGENT SHALL NOT DEFINE ITS OWN AUTHORITY.

402 DEFINES MAY.

CAN ≠ MAY.