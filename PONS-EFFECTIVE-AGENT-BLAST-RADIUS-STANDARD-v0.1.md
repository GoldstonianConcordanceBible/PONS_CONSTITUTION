# PONS EFFECTIVE AGENT BLAST RADIUS STANDARD v0.1

Type: Risk Containment Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Control Layer: PONS Control Tower
Applies To: All Treasury, Executor, Swing, Agent, and Strategy Wallets

## DESCRIPTION

Defines how PONS measures and limits the true maximum capital exposure of each autonomous Executor, including wallet balances, token approvals, session-key authority, pending transactions, callable modules, and any other permission capable of moving funds.

## 1. PURPOSE

The purpose of this standard is to define the maximum amount of capital that could reasonably be affected if a PONS autonomous agent, Executor wallet, signer, strategy module, or connected execution process were compromised or malfunctioned.

PONS shall not measure agent risk solely by the visible balance of an Executor wallet.

The governing metric is:

EFFECTIVE AGENT BLAST RADIUS

The objective is to ensure that even if an autonomous component fails, the failure remains contained within a predefined and governable maximum exposure.

## 2. CORE PRINCIPLE

EXECUTOR BALANCE ≠ TOTAL AGENT RISK.

The Effective Agent Blast Radius includes all capital or permissions that an autonomous component could potentially cause to move.

This may include:

wallet balances
token allowances
session-key permissions
active smart-account modules
pending transactions
open approvals
callable contracts
approved routers
delegated spending authority
bridge permissions
external contract authority
unsettled transactions
other permissions capable of moving capital

## 3. DEFINITION

Effective Agent Blast Radius, abbreviated:

EABR

means:

the maximum reasonably realizable capital loss or unauthorized capital movement available to an autonomous agent under the current technical and policy state.

Conceptually:

EABR =

EXECUTOR BALANCES

+

ACCESSIBLE ALLOWANCES

+

ACTIVE DELEGATED AUTHORITY

+

PENDING TRANSACTION EXPOSURE

+

CALLABLE MODULE EXPOSURE

+

OTHER CAPITAL-MOVEMENT PERMISSIONS

minus

independently verified protections that technically prevent movement.

## 4. PURPOSE OF EABR

EABR exists to answer:

If the agent became malicious right now, how much capital could it actually affect?

Not:

How much capital is visible in the wallet?

Not:

How much capital did the operator intend to risk?

Not:

How much the policy document says the agent should use.

EABR measures enforceable exposure.

## 5. EXECUTOR BALANCE EXPOSURE

All assets directly held by an autonomous Executor count toward EABR unless they are independently locked from the agent's authority.

Include:

native gas asset
approved market assets
Lab tokens
reference tokens
stable assets
wrapped assets
other ERC-20 balances
claimable assets where executable by the agent

Values should be converted into a common reference unit for risk reporting.

## 6. TOKEN ALLOWANCE EXPOSURE

Outstanding ERC-20 approvals can create exposure larger than the current Executor balance.

PONS shall monitor:

token
spender
approved amount
current token balance
spender contract
approval timestamp
approval purpose
policy authorization
remaining allowance

Unlimited approvals should be treated as maximum-risk permissions unless independently constrained.

Where practical:

EXACT APPROVAL

↓

EXECUTION

↓

ALLOWANCE RECONCILIATION

↓

RESIDUAL APPROVAL REDUCED OR CLEARED

## 7. SESSION-KEY EXPOSURE

If smart accounts or session keys are used, EABR shall include the maximum authority granted to each active session key.

Record:

session-key ID
authorized account
permitted assets
permitted contracts
maximum amount
maximum daily amount
expiration
method restrictions
recipient restrictions
revocation status
last use

Expired or revoked keys shall not count once revocation is independently verified onchain.

## 8. SMART-ACCOUNT MODULE EXPOSURE

Any active smart-account module capable of moving value contributes to EABR.

Examples:

trading module
swap module
automation module
session-key module
allowance module
bridge module
batch-execution module
recovery module

For every active module record:

module address
module purpose
capital authority
permitted functions
spending limits
recipient limits
status
policy version

Unknown module:

AUTONOMY OFF.

## 9. PENDING TRANSACTION EXPOSURE

Transactions that are:

signed
broadcast
pending
replaceable
queued
scheduled
or waiting for confirmation

may continue to create risk after the main application is stopped.

PONS shall include pending transaction exposure in EABR.

Record:

nonce
transaction hash
asset
amount
recipient
router
status
replacement eligibility
maximum economic exposure

## 10. IN-FLIGHT EXPOSURE

A transaction may be considered in-flight between:

execution intent creation

and

final settlement.

Until settlement or confirmed cancellation, relevant capital remains part of the risk calculation.

PONS shall not assume:

BOT STOPPED = TRANSACTION STOPPED.

## 11. TREASURY ACCESS EXPOSURE

If an Executor or connected module can directly pull funds from Treasury, that authority contributes to EABR.

Preferred architecture:

TREASURY PUSHES CAPITAL TO EXECUTOR.

EXECUTOR DOES NOT PULL UNBOUNDED CAPITAL FROM TREASURY.

Any pull authority must be:

explicit
limited
time-bound
asset-bound
amount-bound
auditable

Unbounded Treasury pull authority is incompatible with bounded autonomy.

## 12. HARVEST-PATH EXPOSURE

The Treasury harvest mechanism itself can become an attack path.

The Executor shall not be able to redefine where harvested funds are sent.

Approved harvest destinations should be:

predefined
policy-controlled
address-verified
protected from AI modification

If the harvest destination changes unexpectedly:

DENY

and

CREATE INCIDENT.

## 13. ROUTER AND CONTRACT EXPOSURE

Calling a router or contract can create indirect capital authority.

PONS shall classify approved execution contracts by:

contract address
chain
function permissions
maximum value
token permissions
callback behavior
approval requirements
security-review status
policy version

Unknown or changed contract bytecode where relevant:

DENY OR ESCALATE.

## 14. BRIDGE EXPOSURE

Bridging creates additional custody, smart-contract, destination-chain, and settlement risk.

Unless explicitly authorized by a strategy module:

EXECUTOR MAY NOT BRIDGE.

If bridge authority exists, EABR shall include:

bridge contract exposure
in-flight bridge amount
destination-chain exposure
claim authority
message-relayer assumptions

## 15. EXTERNAL PROTOCOL EXPOSURE

If future PONS strategies use:

staking
lending
borrowing
liquidity provision
derivatives
vaults
yield protocols

EABR shall include the maximum capital accessible through those permissions.

No such authority is implied by ordinary swing-wallet status.

Each requires separate 402 approval.

## 16. BLAST-RADIUS CEILING

Every Executor shall have a defined:

MAXIMUM EFFECTIVE AGENT BLAST RADIUS

Example:

Executor: XLAB-SWING-001

Maximum EABR: $500 equivalent

The agent must not be able to increase this limit.

Any requested increase requires:

governance approval
402 policy update
new policy hash
risk review
effective timestamp

## 17. HARD LIMIT

Where technically practical, EABR ceilings shall be enforced below the AI and strategy layers.

Preferred order:

ONCHAIN OR SMART-ACCOUNT LIMIT

↓

SIGNER POLICY

↓

402 POLICY ENGINE

↓

STRATEGY ENGINE

↓

AI ADVISORY

The AI shall never be the only control preventing excess exposure.

## 18. EABR PRE-TRADE CHECK

Before every autonomous transaction, PONS shall calculate:

CURRENT EABR

and

PROJECTED POST-TRADE EABR.

If:

PROJECTED EABR > POLICY MAXIMUM

then:

DENY.

No autonomous override.

## 19. EABR POST-TRADE CHECK

After settlement, PONS shall recalculate EABR.

Verify:

actual balances
actual allowances
actual session-key state
actual pending transactions
actual modules
actual permissions

If actual EABR exceeds the policy ceiling:

AUTONOMY OFF

and

INCIDENT CREATED.

## 20. CONTROL TOWER DISPLAY

For each Executor, the PONS Control Tower should show:

Executor name
wallet address
strategy
autonomy level
current wallet value
current EABR
maximum authorized EABR
remaining capacity
active allowances
active session keys
pending transaction exposure
active modules
last reconciliation
402 policy version
policy hash
incident status

Example:

XLAB EXECUTOR

Wallet Value: $280
Allowance Exposure: $50
Pending Exposure: $70
Session Authority: $100
Current EABR: $500
Maximum EABR: $500
Available Capacity: $0
Status: LIMIT REACHED
Autonomy: HOLD

## 21. RISK BANDS

PONS may display EABR utilization bands.

Example:

0–50% = NORMAL

50–75% = ELEVATED

75–90% = HIGH

90–100% = MAXIMUM

>100% = POLICY BREACH

Crossing 100% requires:

AUTONOMY OFF

INCIDENT

RECONCILIATION

HUMAN REVIEW.

## 22. DAILY RISK BUDGET

EABR is not the same as daily turnover.

402 may separately define:

maximum single trade
maximum daily turnover
maximum daily realized loss
maximum drawdown
maximum EABR

All must pass.

Example:

Trade under per-trade limit

but

projected EABR exceeds ceiling

=

DENY.

## 23. STRATEGY ISOLATION

Each major strategy should have its own EABR ceiling.

Example:

XLAB EABR: $500

NLAB EABR: $500

DLAB EABR: $500

One compromised Executor should not automatically expose the combined capital of all three.

Master Treasury exposure remains separately governed.

## 24. AGGREGATE PONS BLAST RADIUS

PONS shall also calculate:

TOTAL AUTONOMOUS SYSTEM EABR

This represents the sum of the independently accessible risk across all active autonomous Executors.

Control Tower example:

XLAB: $500

NLAB: $500

DLAB: $250

TOTAL AUTONOMOUS SYSTEM EABR:

$1,250

This value should be compared against:

total Treasury value

and

maximum ecosystem autonomy allocation.

## 25. TREASURY EXPOSURE RATIO

PONS may calculate:

TREASURY AUTONOMY EXPOSURE RATIO

=

TOTAL AUTONOMOUS SYSTEM EABR

÷

TOTAL GOVERNED TREASURY VALUE

Example:

Autonomous EABR: $1,000

Treasury Value: $100,000

Autonomy Exposure Ratio:

1%

This is a governance metric, not a performance metric.

## 26. EFFECTIVE BLAST-RADIUS DRILL

Before bounded autonomy, PONS shall perform an adversarial containment test.

Assume:

Executor key compromised.

Attempt:

maximum permitted transfer
unknown recipient
unauthorized router
oversized transaction
unlimited approval
policy bypass
Treasury pull
session-key extension

Expected result:

the attacker cannot exceed the declared EABR.

## 27. COMPROMISE TEST

A successful EABR test does not require preventing all loss.

The test proves containment.

Example:

Executor ceiling: $100.

Compromised agent loses the full $100.

Treasury remains inaccessible.

Result:

strategy/security incident occurred

but

blast-radius containment succeeded.

This distinction is important.

## 28. EABR FAILURE

The EABR control fails if:

actual accessible capital exceeds the published limit
unknown allowance creates additional exposure
agent can modify its own limit
agent can change Treasury destination
agent can add authority
agent can access Treasury beyond allocation
signing path bypasses the ceiling
unexpected module expands exposure

Any such failure is a critical assurance defect.

## 29. EMERGENCY RESPONSE

If EABR cannot be determined:

AUTONOMY OFF.

If EABR unexpectedly increases:

AUTONOMY OFF.

If EABR exceeds policy:

AUTONOMY OFF.

Then:

stop new authorizations
revoke session keys where possible
reduce allowances
inventory pending transactions
reconcile onchain state
preserve evidence
create incident
require human review

## 30. POLICY CHANGE

Changes to EABR limits require a new authorized policy state.

Record:

old limit
new limit
reason
approving authority
policy version
policy hash
effective time
related audit evidence

AI may recommend a change.

AI may not authorize the change.

## 31. AUDIT METRICS

PONS shall track:

current EABR
maximum EABR
average EABR utilization
peak EABR
number of EABR breaches
number of denied over-limit transactions
number of unexpected allowances
number of unexpected modules
number of successful containment drills
largest observed containment exposure

Target:

UNAUTHORIZED EABR BREACHES = 0.

## 32. RELATIONSHIP TO PERFORMANCE

EABR is an assurance metric.

It is not improved by profitability.

A strategy that earns 100% but exceeds its authorized EABR:

FAILS ASSURANCE.

A strategy that loses money but remains completely inside its EABR:

MAY PASS CONTAINMENT

while

FAILING STRATEGY PERFORMANCE.

The two assessments remain separate.

## 33. CANONICAL RULES

EXECUTOR BALANCE ≠ BLAST RADIUS.

PERMISSIONS ARE EXPOSURE.

ALLOWANCES ARE EXPOSURE.

SESSION KEYS ARE EXPOSURE.

PENDING TRANSACTIONS ARE EXPOSURE.

CALLABLE MODULES ARE EXPOSURE.

THE AGENT SHALL NOT DEFINE ITS OWN LIMIT.

THE AGENT SHALL NOT INCREASE ITS OWN AUTHORITY.

IF EABR CANNOT BE VERIFIED:

NO AUTONOMOUS TRADE.

IF EABR EXCEEDS POLICY:

AUTONOMY OFF.

THE PURPOSE OF BOUNDED AUTONOMY IS NOT TO MAKE FAILURE IMPOSSIBLE.

THE PURPOSE IS TO MAKE FAILURE CONTAINED.