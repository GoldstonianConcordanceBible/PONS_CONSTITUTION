# PONS EXECUTION INTENT AND CALLDATA VALIDATION STANDARD v0.1

Type: Transaction Integrity Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Control Layer: PONS Control Tower
Applies To: All PONS autonomous and human-gated blockchain execution

## DESCRIPTION

Defines the canonical PONS Execution Intent object and the validation process that proves every signed and broadcast transaction exactly matches what 402 authorized, including asset, amount, recipient, router, calldata, slippage, deadline, policy hash, and transaction-state checks.

## 1. PURPOSE

The purpose of this standard is to establish an immutable transaction-intent object between authorization and signing.

PONS shall not permit:

DECISION

to become

TRANSACTION

without first creating a canonical:

EXECUTION INTENT.

The Execution Intent represents exactly what PONS has been authorized to do.

Canonical rule:

WHAT 402 AUTHORIZES MUST EQUAL WHAT THE SIGNER SIGNS.

## 2. CORE PRINCIPLE

Authorization of an action such as:

TRIM XLAB

is insufficient.

Execution authority requires exact transaction parameters.

PONS must know:

which chain
which Executor
which asset
which amount
which router
which recipient
which method
which slippage
which deadline
which policy
which authorization

before signing.

## 3. CANONICAL EXECUTION FLOW

OBSERVATION

↓

SIGNAL

↓

DECISION

↓

402 AUTHORIZATION

↓

EXECUTION INTENT

↓

TRANSACTION SIMULATION

↓

STATE RECONCILIATION

↓

SIGNER VALIDATION

↓

SIGNATURE

↓

BROADCAST

↓

SETTLEMENT

No required stage may be silently skipped.

## 4. EXECUTION INTENT — INT

Every proposed executable action shall generate an Execution Intent.

The INT shall be a structured machine-readable object.

Preferred formats may include:

canonical JSON
typed structured data
deterministically serialized transaction object

The format must support deterministic hashing.

## 5. REQUIRED INTENT IDENTITY FIELDS

Every INT shall contain:

intent_id
intent_version
created_at
expires_at
strategy_id
lab_id where applicable
decision_id
authorization_id
policy_id
policy_hash
executor_address
chain_id
intent_status
intent_hash

Possible status values:

CREATED
AUTHORIZED
SIMULATED
READY_TO_SIGN
SIGNED
BROADCAST
SETTLED
FAILED
CANCELLED
EXPIRED
REPLACED

## 6. ACTION FIELD

The INT shall specify the authorized action.

Examples:

ADD
TRIM
EXIT
RE_ENTER
SWEEP_TO_TREASURY
RETURN_PRINCIPAL
HARVEST_PROFIT

HOLD and NO_ACTION shall not generate executable INT objects.

## 7. CHAIN FIELD

Every INT shall identify:

chain_id
network
executor_account
expected settlement environment

If transaction chain differs from INT chain:

DENY.

Unknown chain:

DENY.

## 8. EXECUTOR FIELD

Every INT shall identify the exact Executor authorized to act.

If signed transaction originates from a different account:

DENY.

Executor substitution is not permitted without creating a new authorization path.

## 9. INPUT ASSET

Every INT shall identify input asset using:

contract address
chain ID
decimals
symbol for display only

Contract address is authoritative.

Ticker is descriptive.

Unknown input token:

DENY.

## 10. OUTPUT ASSET

Every swap or trade INT shall identify the expected output asset.

Required fields may include:

contract address
chain
minimum output
expected output
reference quote

Unknown output token:

DENY.

## 11. AMOUNT

Every INT shall specify an exact or policy-bounded amount.

Fields may include:

input_amount
reference_value
maximum_input_amount
minimum_output_amount
percentage_of_position

If signed amount exceeds the authorized amount:

DENY.

If deterministic clipping is permitted, it must be explicitly defined by policy.

## 12. RECIPIENT

Every INT shall contain the final approved recipient.

Examples:

approved Executor
approved Treasury
approved router
approved pool
approved protocol

Unknown recipient:

DENY.

Recipient substitution after authorization:

DENY.

## 13. TREASURY DESTINATION

For:

HARVEST_PROFIT
RETURN_PRINCIPAL
EMERGENCY_SWEEP

the destination Treasury address shall be explicitly included.

The autonomous agent shall not be able to rewrite the Treasury destination after authorization.

## 14. ROUTER

Every trade INT shall identify the exact approved router or execution contract.

Fields may include:

router_address
router_version
approved_method
contract_hash where applicable

Router changed after authorization:

NEW INT REQUIRED.

Unknown router:

DENY.

## 15. METHOD SELECTOR

Where technically practical, INT shall identify the exact contract method authorized.

Approval of:

CONTRACT

does not imply approval of:

EVERY FUNCTION.

Method mismatch:

DENY.

## 16. CALLDATA

The complete intended calldata or its canonical representation shall be generated before signing.

The calldata shall be hashed.

Record:

calldata
or protected raw calldata
calldata_hash
decoded_function
decoded_parameters

The hash shall be part of transaction integrity validation.

## 17. INTENT HASH

Every INT shall produce a deterministic:

INTENT HASH.

Material changes must produce a new hash.

Material fields include:

chain
Executor
input token
output token
amount
recipient
router
method
slippage
deadline
policy hash
calldata

Canonical rule:

CHANGE THE TRANSACTION

=

CHANGE THE INTENT HASH.

## 18. AUTHORIZATION BINDING

Each INT shall reference:

authorization_id
policy_id
policy_hash

This proves which 402 decision permitted the transaction.

An INT without a valid authorization:

DENY.

## 19. POLICY IMMUTABILITY

An INT authorized under one policy version shall not silently execute under another.

If policy changes between:

AUTH

and

SIGN

then:

RE-EVALUATE.

If the policy is:

SUSPENDED
REVOKED
SUPERSEDED
EXPIRED

before signing:

DENY OR REAUTHORIZE.

## 20. INTENT EXPIRATION

Every market-sensitive INT shall have an expiration.

This prevents stale instructions from executing after conditions change.

Fields:

created_at
expires_at
maximum_age

Expired INT:

DENY.

A new market observation and authorization shall be required where appropriate.

## 21. MARKET-SNAPSHOT BINDING

The INT should reference the market observation used to create it.

Fields may include:

obs_id
data_snapshot_hash
reference_price
quote_timestamp
oracle_timestamp
basis_at_intent
liquidity_at_intent

This allows later reconstruction of the decision environment.

## 22. SLIPPAGE

Every trade INT shall contain a maximum permitted slippage.

Fields may include:

expected_output
minimum_output
maximum_slippage_bps
maximum_price_impact_bps

If simulation or current quote exceeds the limit:

DENY OR REAUTHORIZE.

## 23. DEADLINE

Every market transaction shall contain an execution deadline where supported.

Deadline prevents delayed execution under materially different market conditions.

Expired deadline:

DENY.

## 24. GAS POLICY

The INT may specify:

maximum_gas
maximum_fee
maximum_priority_fee
maximum_total_execution_cost

Excessive execution cost beyond policy:

DENY OR ESCALATE.

Gas changes that do not alter economic intent may be handled under explicitly defined replacement rules.

## 25. TRANSACTION SIMULATION

Before signing, the exact proposed transaction shall be simulated where technically practical.

Simulation shall verify:

target contract
method
token movement
recipient
expected balances
minimum output
unexpected approvals
unexpected callbacks
unexpected state changes
revert behavior
gas estimate

Simulation is performed against the transaction represented by INT.

## 26. SIMULATION RESULT

Possible simulation states:

PASS
FAIL
INCONCLUSIVE

PASS:

eligible for later validation.

FAIL:

DENY.

INCONCLUSIVE:

DENY OR ESCALATE.

INCONCLUSIVE shall never silently become PASS.

## 27. SIMULATION HASH

The simulation result should be linked to:

intent_id
intent_hash
simulation timestamp
chain state
simulation provider or engine
result hash

A new material INT requires a new simulation.

## 28. STATE RECONCILIATION

Immediately before signing, PONS shall verify that relevant current onchain state remains consistent.

Check may include:

Executor balance
token balances
allowances
nonce
pending transactions
policy state
session-key state
chain ID
emergency state
EABR

Reconciliation failure:

NO SIGNATURE.

## 29. PRE-SIGN VALIDATION

Before signing, validate:

INT exists
AUTH is valid
policy is ACTIVE
policy hash matches
Executor matches
chain matches
token contracts match
router matches
recipient matches
amount within limits
EABR within limit
simulation passed
state reconciled
INT not expired
kill switch inactive
no blocking incident exists

Any hard failure:

DENY.

## 30. INTENT / CALLDATA MATCH

The transaction presented to the signer shall be decoded and compared against the INT.

Compare:

chain
from
to
value
method
token addresses
amounts
recipient
router
deadline
minimum output
calldata hash

Canonical metric:

INTENT / CALLDATA MATCH RATE.

Required target:

100%.

## 31. MATERIAL MISMATCH

Examples of material mismatch:

different token
different recipient
different router
higher amount
different chain
different method
different Treasury destination
unexpected approval
different calldata
higher authority than approved

Material mismatch:

DENY

and

CREATE INCIDENT.

## 32. NON-MATERIAL DIFFERENCES

Some technical fields may legitimately differ if policy explicitly permits them.

Examples may include:

gas estimate
gas fee
nonce replacement under controlled conditions

Permitted tolerances shall be documented.

No field shall be declared non-material merely because execution has already occurred.

## 33. SIGNATURE EVENT

Once all checks pass, signer may produce the signature.

Record:

signer_event_id
intent_id
intent_hash
authorization_id
policy_hash
signer role
timestamp
signed_transaction_hash

Security-sensitive signer material shall not be logged.

## 34. POST-SIGN VALIDATION

Before broadcast, PONS should perform a final transaction-hash and intent relationship check.

If signed transaction no longer corresponds to approved INT:

DO NOT BROADCAST.

## 35. BROADCAST

Broadcast shall produce a TX event.

Record:

transaction hash
broadcast time
RPC endpoint class or provider identifier
nonce
chain
intent_id
intent_hash

TX remains linked to INT permanently.

## 36. TRANSACTION REPLACEMENT

A replacement transaction shall not silently change economic intent.

Permitted replacement examples may include:

fee increase
same recipient
same calldata
same value
same nonce

Material economic change:

NEW AUTHORIZATION AND NEW INT.

## 37. DUPLICATE PREVENTION

Each INT may execute only according to its defined execution policy.

Settled INT:

CANNOT EXECUTE AGAIN.

The system shall track:

intent_id
nonce
transaction hash
settlement state

Duplicate execution attempt:

DENY

and possibly

INCIDENT.

## 38. PENDING TRANSACTION

Pending transactions remain part of active risk.

PONS shall track:

intent
nonce
amount
recipient
elapsed time
replacement status
cancellation status
EABR effect

Stopping the agent does not erase pending transactions.

## 39. SETTLEMENT VALIDATION

After confirmation, settlement shall be compared against INT.

Verify:

actual input
actual output
actual recipient
actual fees
actual slippage
actual token transfers
actual contract interactions

Unexpected settlement difference:

INCIDENT.

## 40. TREASURY IMPACT

Every settled transaction shall generate a Treasury Impact Event where economically relevant.

Record:

capital deployed
principal returned
realized P&L
unrealized impact
fees
gas
EABR change
Treasury balance change

This completes the causal chain.

## 41. HUMAN-GATED INTENT

Human approval shall apply to an exact INT, not merely a natural-language description.

The human should see, where practical:

action
asset
amount
recipient
router
expected output
maximum slippage
simulation result
policy result
intent hash

Human approves:

THIS INT.

Changing material INT fields after approval invalidates the approval.

## 42. AI-GENERATED INTENT

AI may contribute to:

action
size recommendation
trade rationale
risk interpretation

AI shall not directly determine authoritative:

recipient allowlist
Treasury destination
signer authority
policy hash
kill-switch state

Structured AI output shall be validated before becoming an INT.

## 43. MALFORMED AI OUTPUT

Malformed AI output:

REJECT.

Do not infer executable values from ambiguous language.

Examples:

"sell a little"
"move it back"
"probably all of it"
"use the best router"

must not directly become signed transactions.

The Strategy Engine must convert recommendations into deterministic valid parameters under policy.

## 44. TRANSACTION SUBSTITUTION ATTACK

PONS shall explicitly test whether a transaction can be altered between:

authorization

and

signing.

Red-team substitutions should include:

recipient change
router change
amount increase
token change
Treasury address change
method change
hidden approval
calldata modification

Expected result:

ALL SUBSTITUTIONS DENIED.

## 45. FRONT-END DISPLAY ATTACK

The human approval interface shall not be trusted as the sole representation of a transaction.

The system should compare:

human-readable summary

against

decoded calldata.

If the display says:

TRIM 10% SPCX

but calldata represents:

TRANSFER 100% TO UNKNOWN ADDRESS

the signer path must reject it.

## 46. TRANSACTION INTENT PROVENANCE

Public or audit records may expose:

intent_id
intent_hash
policy_hash
authorization status
transaction hash
settlement status

Sensitive raw calldata may be protected if necessary, while hash-based verification remains possible.

## 47. INTENT RECORD RETENTION

INT records shall not be deleted merely because:

transaction failed
transaction was denied
transaction expired
human rejected it

Denied and failed intents are valuable audit evidence.

## 48. FAILED TRANSACTIONS

A failed or reverted transaction shall preserve:

INT
AUTH
simulation result
TX attempt
revert reason
incident linkage where applicable

A failed transaction is not an excuse to erase the decision trail.

## 49. CANCELLED INTENTS

A cancelled INT shall record:

cancellation time
reason
actor
policy state
whether any transaction was signed
whether any transaction was broadcast

Cancellation does not delete the original object.

## 50. INTENT VERSIONING

If an INT is modified before execution:

do not overwrite.

Create:

INT-v2

or a new INT ID.

Reference the original intent.

Maintain append-only history.

## 51. CONTROL TOWER — PUBLIC

The public Control Tower may show:

intent ID
intent hash
action
policy hash
authorization result
transaction hash
settlement state
intent-match status
provenance status

Do not expose future trade details before execution if doing so creates market or security risk.

## 52. CONTROL TOWER — PRIVATE

Private operator view may show:

full INT
decoded calldata
simulation trace
current quote
slippage
nonce
allowances
EABR
authorization rules
failed checks
pending signature
pending broadcast
replacement options

## 53. AUDIT METRICS

Track:

total INT created
total INT authorized
total INT denied
total INT expired
total INT signed
total INT broadcast
total INT settled
total INT reverted
intent/calldata mismatches
transaction substitutions blocked
duplicate attempts blocked
policy mismatches blocked
stale intents blocked

Primary integrity metric:

INTENT / CALLDATA MATCH RATE

=

matching signed transactions

÷

total signed transactions.

Target:

100%.

## 54. HARD-GATE REQUIREMENT

The following conditions are incompatible with bounded autonomy:

unauthorized INT becomes signed
signed transaction differs materially from INT
recipient substituted
router substituted
amount exceeds INT
Treasury destination altered
policy hash bypassed
expired INT executed
duplicate INT executes unexpectedly
simulation failure is ignored
state-reconciliation failure is ignored

Any confirmed occurrence:

ASSURANCE FAIL

and

AUTONOMY OFF

pending investigation.

## 55. RESEARCH VALUE

Execution Intent creates a clear research distinction between:

WHAT THE AGENT WANTED TO DO

WHAT 402 ALLOWED

WHAT THE SIGNER SIGNED

WHAT THE CHAIN EXECUTED

WHAT ECONOMICALLY HAPPENED.

This separation is necessary for valid causal auditing.

## 56. CANONICAL RULES

DECISION ≠ TRANSACTION.

AUTHORIZATION ≠ TRANSACTION.

EVERY EXECUTABLE ACTION REQUIRES AN EXECUTION INTENT.

THE INTENT MUST BE MACHINE-READABLE.

THE INTENT MUST BE HASHABLE.

THE INTENT MUST REFERENCE ITS POLICY.

THE INTENT MUST EXPIRE.

MATERIAL INTENT CHANGES REQUIRE REAUTHORIZATION.

WHAT 402 AUTHORIZES MUST EQUAL WHAT THE SIGNER SIGNS.

WHAT THE SIGNER SIGNS MUST EQUAL WHAT IS BROADCAST.

WHAT IS BROADCAST MUST BE RECONCILED WITH WHAT SETTLES.

UNKNOWN RECIPIENT = DENY.

UNKNOWN ROUTER = DENY.

POLICY MISMATCH = DENY.

EXPIRED INTENT = DENY.

SIMULATION FAILURE = DENY.

STATE RECONCILIATION FAILURE = DENY.

INTENT / CALLDATA MISMATCH = DENY.

PROVENANCE FOLLOWS THE INTENT FROM AUTHORIZATION TO SETTLEMENT.