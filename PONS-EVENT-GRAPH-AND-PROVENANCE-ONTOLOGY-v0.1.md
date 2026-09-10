# PONS EVENT GRAPH AND PROVENANCE ONTOLOGY v0.1

Type: Event and Audit Ontology Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Control Layer: PONS Control Tower
Applies To: All PONS observations, signals, decisions, authorizations, transactions, incidents, recoveries, and audit records

## DESCRIPTION

Defines the PONS event graph used to reconstruct every autonomous action from observation through signal, decision, authorization, intent, transaction, settlement, treasury impact, human override, incident, recovery, and audit classification.

## 1. PURPOSE

The purpose of this standard is to replace a single flat Agent Decision Event with a structured causal event graph.

PONS shall not treat:

monitoring
signal generation
decision
authorization
transaction
settlement
treasury accounting
incident handling

as one event.

Each stage shall be represented independently and linked to its parent events.

The target is complete causal reconstructability.

## 2. CORE PRINCIPLE

A transaction alone does not explain why it happened.

A decision alone does not prove it was authorized.

An authorization alone does not prove the correct transaction was signed.

A blockchain record alone does not prove which policy authorized the action.

Therefore PONS shall maintain an append-only event graph.

Canonical chain:

OBSERVATION

↓

SIGNAL

↓

DECISION

↓

AUTHORIZATION

↓

EXECUTION INTENT

↓

TRANSACTION

↓

SETTLEMENT

↓

TREASURY IMPACT

with parallel events for:

HUMAN OVERRIDE
CONFIGURATION CHANGE
INCIDENT
RECOVERY
AUDIT CLASSIFICATION

## 3. EVENT GRAPH

Each PONS event shall have:

event_id
event_type
timestamp
parent_event_ids
strategy_id
lab_id where applicable
chain_id
policy_id
policy_hash where applicable
git_commit
software_version
actor
event_hash

Events should be immutable after finalization.

Corrections should create new linked events rather than silently overwriting prior history.

## 4. OBSERVATION EVENT — OBS

OBS records what the system observed.

Examples:

market price
wallet balance
oracle value
spread
liquidity
basis
volume
halt state
nonce
allowance
policy state
chain state

Every scheduled monitoring cycle should produce an OBS or an explicit monitoring failure event.

Fields may include:

obs_id
timestamp
asset
contract_address
price
reference_price
basis
spread
liquidity
volume
oracle_sources
oracle_age
wallet_balance
allowances
pending_transactions
nonce
chain_id
data_source_hash
state_reconciliation_status

## 5. WHY OBS MATTERS

Without OBS events, PONS cannot measure:

monitoring uptime
missed observations
stale-data usage
silent system failure
whether the agent should have seen a market condition

OBS establishes the denominator for monitoring reliability.

## 6. SIGNAL EVENT — SIG

SIG records when a predefined condition or model-generated condition becomes actionable.

Examples:

price appreciation threshold reached
drawdown threshold reached
basis deviation reached
re-entry threshold reached
liquidity deterioration
oracle divergence
risk limit warning

Fields may include:

sig_id
parent_obs_id
signal_type
trigger_rule
threshold
observed_value
confidence where applicable
signal_source
deterministic_or_ai
signal_hash

## 7. DECISION EVENT — DEC

DEC records the action proposed by the strategy.

Allowed decision classes may include:

HOLD
ADD
TRIM
EXIT
RE_ENTER
SWEEP_TO_TREASURY
ESCALATE
NO_ACTION

Fields may include:

dec_id
parent_sig_id
decision
proposed_asset
proposed_amount
proposed_percentage
strategy_rule
ai_consulted
ai_model
ai_output_hash
decision_reason
decision_hash

## 8. HOLD DECISIONS

HOLD is a valid decision and should be logged when a relevant signal was evaluated and intentionally rejected.

However, routine observations without actionable signals should not inflate decision-compliance denominators.

PONS shall distinguish:

NO SIGNAL

from

SIGNAL → HOLD.

## 9. AUTHORIZATION EVENT — AUTH

AUTH records the 402 policy decision.

Possible states:

PASS
DENY
ESCALATE

Fields may include:

auth_id
parent_dec_id
policy_id
policy_hash
authorization_state
rules_evaluated
failed_rules
jurisdiction_result
risk_result
eabr_result
emergency_state
human_review_required
authorization_hash

## 10. AUTHORIZATION FAILURE

A denied decision shall terminate the autonomous execution path.

Canonical:

DEC

↓

AUTH = DENY

↓

NO EXECUTION INTENT

unless a separate human-governance process creates a new authorized decision.

## 11. EXECUTION INTENT EVENT — INT

INT records the exact transaction PONS intends to execute.

Fields may include:

intent_id
parent_auth_id
chain_id
executor
action
input_token
output_token
input_amount
minimum_output
router
recipient
value
slippage
deadline
calldata_hash
policy_hash
intent_hash

INT represents what has been authorized.

## 12. INTENT IMMUTABILITY

Once INT is authorized for signing, material fields shall not change silently.

A change to:

token
amount
router
recipient
chain
slippage
deadline
calldata
policy

requires a new INT and, where necessary, new AUTH.

## 13. TRANSACTION EVENT — TX

TX records the actual blockchain transaction attempt.

Fields may include:

tx_event_id
parent_intent_id
transaction_hash
from
to
nonce
value
gas_limit
gas_price_or_fee
calldata_hash
broadcast_timestamp
status
replacement_tx
signer_policy_version
transaction_hash_record

Possible states:

SIGNED
BROADCAST
PENDING
REPLACED
DROPPED
REVERTED
CONFIRMED

## 14. INTENT / TRANSACTION MATCH

Every TX shall be compared with its parent INT.

Required check:

INTENT = SIGNED TRANSACTION

within authorized execution tolerances.

Any material mismatch creates an Incident Event.

## 15. SETTLEMENT EVENT — SET

SET records what actually happened onchain after confirmation.

Fields may include:

set_id
parent_tx_event_id
block_number
block_hash
confirmation_count
receipt_status
actual_input
actual_output
actual_recipient
actual_fee
actual_slippage
actual_balance_changes
token_transfer_logs
settlement_hash

Settlement is the economic truth of execution.

## 16. TREASURY IMPACT EVENT — TRI

TRI records the portfolio and accounting effect.

Fields may include:

tri_id
parent_set_id
strategy
executor
treasury
pre_trade_value
post_trade_value
realized_pnl
unrealized_pnl
principal_returned
profit_harvested
fees
gas
basis_change
eabr_change
treasury_balance_change
tri_hash

This prevents transaction activity from being confused with economic outcome.

## 17. TREASURY TRANSFER EVENT — XFER

Capital movement between Treasury and Executor shall be separately recorded.

Types may include:

FUND_EXECUTOR
RETURN_PRINCIPAL
HARVEST_PROFIT
RECALL_CAPITAL
EMERGENCY_SWEEP

Fields may include:

xfer_id
source
destination
asset
amount
reason
policy_hash
authorization
transaction_hash
accounting_classification

## 18. HUMAN OVERRIDE EVENT — HUM

HUM records accountable human intervention.

Possible actions:

APPROVE
DENY
MODIFY
PAUSE
RESTART
REDUCE_LIMIT
ESCALATE
EMERGENCY_STOP

Fields may include:

hum_id
parent_event_id
human_role
action
reason
timestamp
policy_effect
approval_hash
new_event_required

Human changes shall not erase the original machine decision.

## 19. CONFIGURATION CHANGE EVENT — CFG

CFG records any material system configuration change.

Examples:

new asset
new router
new contract
new policy
new strategy
new signer
new session key
new capital limit
new oracle
new autonomy level

Fields may include:

cfg_id
configuration_type
old_value_hash
new_value_hash
approving_authority
policy_version
effective_time
git_commit
change_reason

## 20. INCIDENT EVENT — INC

INC records abnormal or prohibited behavior.

Fields may include:

inc_id
parent_event_ids
severity
incident_type
detection_time
description
capital_at_risk
eabr_at_detection
autonomy_effect
kill_switch_triggered
containment_status
root_cause_status

Possible incident types:

POLICY_FAILURE
DATA_FAILURE
ORACLE_FAILURE
EXECUTION_FAILURE
SIGNER_FAILURE
TREASURY_FAILURE
STATE_RECONCILIATION_FAILURE
PROMPT_INJECTION
HUMAN_ERROR
GITHUB_SUPPLY_CHAIN
SECURITY_ANOMALY
MARKET_ANOMALY

## 21. RECOVERY EVENT — REC

REC records return to service after an incident.

Fields may include:

rec_id
parent_incident_id
root_cause
remediation
state_reconciliation_result
policy_version
software_version
security_review
human_restart_authorization
restart_timestamp
recovery_hash

No serious incident should disappear merely because the service restarted.

## 22. AUDIT CLASSIFICATION EVENT — AUD

AUD records the reviewer's interpretation of an event or event chain.

Possible classifications:

PASS
FAIL
EXPECTED
ANOMALY
POLICY_BREACH
SECURITY_BREACH
DATA_FAILURE
HUMAN_OVERRIDE
UNRESOLVED

Fields may include:

aud_id
reviewed_event_ids
reviewer_type
reviewer_identity_or_model
classification
severity
rationale
confidence
review_timestamp

## 23. ACTOR CLASSIFICATION

Every event should identify the actor responsible.

Possible actor types:

DETERMINISTIC_ENGINE
AI_MODEL
402_POLICY_ENGINE
SIGNER
SMART_ACCOUNT
HUMAN_OPERATOR
TREASURY_GOVERNANCE
EXTERNAL_CONTRACT
CHAIN
SYSTEM

This supports accountability analysis.

## 24. AI CONSULTED FLAG

Every DEC should explicitly record:

AI_CONSULTED = TRUE/FALSE.

If TRUE, record:

model
version
prompt_hash
response_hash
structured_output_hash

This enables later comparison of:

AI-assisted decisions

versus

deterministic-only decisions.

## 25. EVENT HASHING

Every finalized event should have a deterministic hash.

The event hash should represent the canonical serialized event record.

Changing a material field changes the hash.

Hashes support:

provenance
tamper detection
Merkle anchoring
independent audit

## 26. PARENT HASHES

Each event should reference the hashes or IDs of its parent events.

Example:

SET

references

TX.

TX references

INT.

INT references

AUTH.

AUTH references

DEC.

DEC references

SIG.

SIG references

OBS.

This creates the causal graph.

## 27. GIT COMMIT

Events produced by software should record the software version or Git commit associated with the execution path.

This supports the question:

WHICH EXACT CODE GENERATED THIS DECISION?

GitHub itself is not the root of trust.

The commit is provenance metadata.

## 28. POLICY HASH

Every AUTH, INT, and relevant TX shall reference the active policy hash.

This establishes:

WHICH EXACT 402 POLICY GOVERNED THIS ACTION?

Unknown policy relationship:

PROVENANCE FAILURE.

## 29. DATA SNAPSHOT HASH

OBS should preserve or hash the exact data snapshot used for a decision.

This supports later independent reconstruction.

Examples:

market-data bundle
oracle values
wallet state
liquidity state
basis calculation
policy state

## 30. EVENT COMPLETENESS

An executed market transaction should normally contain the complete path:

OBS

↓

SIG

↓

DEC

↓

AUTH

↓

INT

↓

TX

↓

SET

↓

TRI

Missing required event:

PROVENANCE GAP.

## 31. BLOCKED PATH

A denied action may legitimately terminate early.

Example:

OBS

↓

SIG

↓

DEC

↓

AUTH = DENY

No INT.

No TX.

This is a complete denied-action chain.

## 32. HUMAN-GATED PATH

Canonical human-gated flow:

OBS

↓

SIG

↓

DEC

↓

AUTH = ESCALATE

↓

HUM

↓

new or confirmed AUTH

↓

INT

↓

TX

↓

SET

↓

TRI

Human review remains visible.

## 33. INCIDENT PATH

Example:

INT

↓

TX mismatch detected

↓

INC

↓

AUTONOMY OFF

↓

REC

↓

new CFG or policy version

The incident remains permanently linked to the affected execution path.

## 34. CONFIGURATION VERSIONING

System configuration used during an event should be identifiable.

Fields may include:

strategy_version
policy_version
signer_policy_version
oracle_configuration_version
risk_configuration_version
software_version

Silent configuration mutation is prohibited for audited operation.

## 35. MONITORING DENOMINATOR

PONS shall measure:

expected monitoring cycles
completed OBS events
missing OBS events
failed OBS events

Example:

Expected: 288

Completed: 286

Failed: 1

Missing: 1

Monitoring Completeness:

99.3%

This prevents invisible downtime from being omitted.

## 36. SIGNAL DENOMINATOR

PONS should distinguish:

total OBS
actionable SIG
non-actionable OBS
SIG rejected by strategy
SIG resulting in HOLD
SIG resulting in execution path

This prevents compliance percentages from being inflated by quiet market periods.

## 37. AUTHORIZATION DENOMINATOR

Rule-fidelity measurement should focus on actionable decisions subjected to policy.

Example denominator:

all DEC events requiring AUTH.

Do not count ordinary OBS cycles as successful policy decisions.

## 38. EXECUTION FIDELITY DENOMINATOR

Execution fidelity should compare:

all signed/broadcast TX events

against

their corresponding INT events.

Metric:

EXECUTION INTENT MATCH RATE

=

matching TX / total signed TX.

Target:

100%.

## 39. PROVENANCE COMPLETENESS

For each settled transaction, determine whether all required evidence exists.

Metric:

PROVENANCE COMPLETENESS

=

settled transactions with complete required event graph

÷

total settled transactions.

Target:

100%.

## 40. RECONSTRUCTION TEST

Independent reviewers should periodically receive only the event package and public blockchain evidence.

They should be asked to reconstruct:

what happened
why
under which policy
what was authorized
what was signed
what settled
what changed economically

Failure to reconstruct indicates a provenance weakness.

## 41. APPEND-ONLY PRINCIPLE

PONS audit history should be append-only.

If an event contains an error:

do not delete it.

Create:

CORRECTION EVENT

or

SUPERSEDING EVENT

referencing the original.

This preserves research integrity.

## 42. TIME SYNCHRONIZATION

Event timestamps should use a consistent canonical format.

Where possible record:

system timestamp
block timestamp
block number

Clock drift should be detectable.

## 43. EVENT ORDERING

Event IDs alone shall not be assumed to establish blockchain order.

Relevant chain events should use:

block number
transaction index
log index

where applicable.

## 44. PRIVACY AND SECURITY

Not every event field must be public.

Potentially public:

event ID
event type
policy hash
intent hash
transaction hash
block
audit result
provenance root

Potentially private:

raw prompts
internal RPC architecture
signer device information
unpatched vulnerability details
personal identifiers
security-sensitive thresholds

Public hashes may prove existence without revealing sensitive contents.

## 45. MERKLE ANCHORING

PONS may periodically combine event hashes into a Merkle tree.

Example:

PONS AUDIT ROOT 000001

covers a defined event range.

The Merkle root may be anchored onchain.

This enables later proof that an event existed in the audited dataset without storing every event record directly onchain.

## 46. PUBLIC CONTROL TOWER

The public Control Tower may show:

total OBS
monitoring completeness
signals
decisions
authorizations
transactions
settlements
provenance completeness
unauthorized transactions
incidents
latest audit root
latest policy hash
latest transaction

Users should be able to click from a PONS transaction to the relevant public provenance chain where appropriate.

## 47. PRIVATE CONTROL TOWER

Private operations may additionally show:

raw OBS data
raw model output
full AUTH failure reasons
full INT
calldata
simulation
nonce
allowances
pending transactions
signer checks
incident investigation
recovery state

## 48. DATA EXPORT

The event graph should support export into research-ready structured data.

Preferred representations may include:

JSONL
CSV tables by event class
relational database export
graph database export
Parquet

The canonical raw event structure should be preserved.

## 49. RESEARCH UNIT OF ANALYSIS

Different research questions require different event units.

Rule fidelity:

DEC + AUTH.

Execution fidelity:

INT + TX + SET.

Monitoring reliability:

OBS.

Human governance:

AUTH + HUM.

Incident analysis:

INC + REC.

Treasury performance:

SET + TRI + XFER.

PONS shall not force all questions into one universal denominator.

## 50. CANONICAL RULES

ONE EVENT TYPE IS NOT ENOUGH.

OBSERVATION IS NOT DECISION.

DECISION IS NOT AUTHORIZATION.

AUTHORIZATION IS NOT EXECUTION.

EXECUTION IS NOT SETTLEMENT.

SETTLEMENT IS NOT TREASURY IMPACT.

EVERY SETTLED TRANSACTION MUST HAVE A RECONSTRUCTABLE CAUSAL CHAIN.

MISSING OBSERVATIONS COUNT AS SYSTEM EVIDENCE.

DENIED TRANSACTIONS ARE VALID AUDIT EVIDENCE.

HUMAN OVERRIDES SHALL REMAIN VISIBLE.

INCIDENTS SHALL NOT BE ERASED.

CORRECTIONS SHALL BE APPEND-ONLY.

POLICY HASHES SHALL FOLLOW THE ACTION.

CODE VERSION SHALL FOLLOW THE ACTION.

TRANSACTION HASHES SHALL CONNECT THE RECORD TO THE CHAIN.

PROVENANCE IS NOT A SCREENSHOT.

PROVENANCE IS A CAUSAL GRAPH.