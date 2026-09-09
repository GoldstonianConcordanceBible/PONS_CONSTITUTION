# PONS TREASURY ARCHITECTURE v0.1

Type: Treasury Architecture Standard
Status: PROPOSED FOR GENESIS FREEZE
System: PONS
Governance Authority: 402
Control Layer: PONS Control Tower
Execution Layer: Bounded Executor / Swing Wallets

## DESCRIPTION

Defines the PONS Treasury and Executor wallet architecture, including capital segregation, wallet roles, signer authority, strategy isolation, funding flows, treasury harvest rules, and the requirement that autonomous agents never receive unrestricted Treasury control.

## 1. PURPOSE

The purpose of this standard is to separate protected Treasury capital from active market execution.

PONS shall operate under the principle:

TREASURY OWNS CAPITAL.

EXECUTOR RECEIVES DELEGATED CAPITAL.

AI RECEIVES NO TREASURY OWNERSHIP AUTHORITY.

The Treasury is the protected capital layer.

The Executor is the bounded market-operation layer.

Autonomous agents may eventually operate Executor wallets within explicitly delegated limits.

They shall not receive unrestricted authority over Treasury.

## 2. CORE ARCHITECTURE

Canonical structure:

PONS TREASURY

↓

AUTHORIZED CAPITAL ALLOCATION

↓

PONS EXECUTOR / SWING WALLET

↓

APPROVED MARKET POSITION

↓

REALIZED RESULT

↓

EXECUTOR

↓

TREASURY HARVEST

The Executor is not the Treasury.

The Treasury is not an ordinary trading wallet.

## 3. TREASURY ROLE

The Treasury holds protected ecosystem capital.

Treasury responsibilities include:

capital custody
capital allocation
profit retention
strategy funding
reserve management
approved ecosystem asset holdings
risk budgeting
capital recall
emergency control
long-term balance-sheet management

The Treasury shall not be treated as an unrestricted hot wallet.

## 4. EXECUTOR ROLE

The Executor or Swing Wallet performs bounded market operations.

Executor responsibilities may include:

holding approved assets
entering approved positions
trimming approved positions
exiting approved positions
re-entering approved positions
executing approved swaps
returning realized proceeds
maintaining a limited operating balance

The Executor acts only within delegated authority.

## 5. TREASURY / EXECUTOR SEPARATION

Treasury signing authority shall remain separate from Executor signing authority.

An autonomous trading process shall not possess the Treasury private key.

The same unrestricted signing credential shall not control both Treasury and Executor.

The agent runtime shall not be capable of escalating its Executor authority into Treasury authority.

## 6. STRATEGY WALLET ISOLATION

Where practical, each major strategy or Lab should use a separate Executor.

Example:

XLAB TREASURY

↓

XLAB EXECUTOR

↓

SPCX / APPROVED XLAB POSITIONS

NLAB TREASURY

↓

NLAB EXECUTOR

↓

NVDA-LINKED / APPROVED NLAB POSITIONS

DLAB TREASURY

↓

DLAB EXECUTOR

↓

DELL-LINKED / APPROVED DLAB POSITIONS

Strategy isolation reduces systemic blast radius.

A compromise of one Executor should not automatically expose every Lab.

## 7. PONS MASTER TREASURY

A PONS Master Treasury may sit above individual strategy treasuries.

Conceptual structure:

PONS MASTER TREASURY

↓

XLAB TREASURY

↓

XLAB EXECUTOR

PONS MASTER TREASURY

↓

NLAB TREASURY

↓

NLAB EXECUTOR

PONS MASTER TREASURY

↓

DLAB TREASURY

↓

DLAB EXECUTOR

Capital delegation should be explicit and auditable.

## 8. AUTHORIZED CAPITAL ALLOCATION

Treasury funds shall enter an Executor only through an approved capital-allocation event.

Each allocation should record:

allocation ID
Treasury wallet
Executor wallet
strategy
asset
amount
reference value
timestamp
402 policy ID
402 policy hash
authorizing human or governance process
transaction hash
block number

Capital entering an Executor becomes operating capital subject to Executor limits.

## 9. TREASURY SIGNING AUTHORITY

Treasury signing should use stronger controls than ordinary Executor signing.

Preferred controls may include:

multisig
hardware-backed signing
MPC
separate signer devices
dual control
role-separated signers
emergency revoke authority
transaction review
time delay for higher-risk governance changes

Treasury signing authority shall not reside inside:

an LLM
a public application server
an unrestricted GitHub Action
ordinary trading-agent runtime
public source control
plain-text configuration
unencrypted logs

## 10. EXECUTOR SIGNING AUTHORITY

Executor authority shall be narrower than Treasury authority.

The Executor may eventually be implemented through:

smart account
session key
Safe module
restricted signer
policy-bound account
other permissioned EVM account structure

The preferred design is one where the Executor is technically restricted, not merely instructed.

## 11. EXECUTOR ALLOWLISTS

Each Executor should operate against explicit allowlists.

Potential allowlists include:

approved chains
approved token contracts
approved trading pairs
approved routers
approved pools
approved recipients
approved transaction methods

Unknown destination:

DENY.

Unknown token:

DENY.

Unknown router:

DENY.

Unknown contract:

DENY.

## 12. TREASURY DESTINATION LOCK

The destination used for returning capital to Treasury should not be freely configurable by the autonomous agent.

The Executor shall not independently change:

Treasury destination
Treasury owner
Treasury signer
Treasury recovery address
Treasury governance module

Where technically practical, the Treasury sweep destination should be immutable or protected by higher-level governance.

## 13. EXECUTOR CAPITAL CEILING

Every Executor shall have a maximum authorized exposure.

The limit may include:

maximum wallet balance
maximum position value
maximum transaction value
maximum daily turnover
maximum daily loss
maximum strategy allocation
maximum outstanding allowance
maximum active session-key authority

The agent shall not be able to increase its own ceiling.

## 14. CAPITAL EXPANSION

Capital limits shall not automatically increase because a strategy is profitable.

Expansion requires a governance event.

Potential sequence:

SHADOW

↓

HUMAN-GATED DUST

↓

AUTONOMOUS DUST

↓

LIMITED CAPITAL

↓

EXPANDED BOUNDED CAPITAL

Every escalation shall reference:

audit results
incident status
402 policy
capital limit
risk limit
approval authority
effective date

## 15. PROFIT HARVEST

Realized gains may be transferred from Executor to Treasury.

Canonical flow:

TREASURY

↓

PRINCIPAL ALLOCATION

↓

EXECUTOR

↓

MARKET POSITION

↓

REALIZED EXIT / TRIM

↓

EXECUTOR

↓

TREASURY HARVEST

PONS should distinguish:

RETURNED PRINCIPAL

from

REALIZED PROFIT.

These values shall not be conflated.

## 16. PRINCIPAL ACCOUNTING

If a position is partially trimmed, PONS should calculate the basis attributable to the trimmed portion.

Example:

Initial position cost basis: $10,000

Current position value: $11,000

Trim amount: $2,200

Approximate basis represented by trim: $2,000

Realized profit represented by trim: $200

Accounting should distinguish:

$2,000 returned or recyclable principal

and

$200 realized gain.

Treasury reporting shall not classify returned principal as profit.

## 17. TREASURY HARVEST POLICY

402 policy may specify how realized profit is handled.

Potential destinations include:

Treasury reserve
strategy reserve
PONS token inventory
stable reserve asset
approved reinvestment pool
other governance-approved destination

Any conversion of realized profit into a PONS ecosystem token shall be separately identified in accounting.

## 18. OWN-TOKEN ACCOUNTING

If Treasury acquires PONS or another affiliated token using realized market proceeds, reporting should track:

number of tokens acquired
acquisition timestamp
cost basis
reference value at acquisition
current value
unrealized appreciation
realized market profit used to acquire them

Token appreciation after acquisition shall not be retroactively counted as trading profit generated by the original market strategy.

## 19. TREASURY PERFORMANCE

Treasury reporting should distinguish:

starting Treasury value
capital allocated
capital returned
realized trading profit
unrealized P&L
fees
gas
slippage
Treasury-token appreciation
external deposits
external withdrawals
ending Treasury value

This prevents Treasury growth from being confused with strategy alpha.

## 20. EXECUTOR PROFIT SWEEP

Where technically appropriate, the Executor may automatically sweep excess capital back to Treasury.

A sweep policy may define:

minimum operating balance
maximum Executor balance
harvest threshold
profit threshold
sweep frequency
approved Treasury destination

The autonomous agent shall not be permitted to redirect the sweep destination.

## 21. RESIDUAL CAPITAL

Executor wallets may retain a limited operating balance.

Residual capital should be bounded by policy.

The purpose is to support:

gas
next approved trade
minimum liquidity requirements
operational continuity

An Executor shall not become an uncontrolled secondary Treasury.

## 22. TOKEN APPROVALS

Unlimited token approvals should be avoided where practical.

Preferred behavior:

approve required amount

↓

execute approved transaction

↓

verify resulting allowance

↓

clear or reduce unnecessary residual allowance

Outstanding allowances are part of effective agent exposure.

## 23. EFFECTIVE EXPOSURE

Executor risk shall not be measured only by wallet balance.

Effective exposure may include:

wallet balances
outstanding approvals
session-key permissions
pending transactions
active modules
callable contracts
other authority capable of moving capital

A separate PONS Effective Agent Blast Radius Standard shall define this metric in detail.

## 24. EXECUTOR PROHIBITIONS

Without explicit higher-level authorization, an Executor shall not:

change Treasury destination
add owners
remove owners
add signers
grant itself new permissions
increase capital limits
modify 402 policy
disable the kill switch
interact with unknown contracts
interact with unknown tokens
approve unlimited spending
deploy arbitrary contracts
bridge assets
stake assets
lend assets
provide liquidity
borrow
use leverage
transfer to arbitrary recipients

Any future permission for such actions must be explicitly defined as a separate strategy module and policy version.

## 25. TREASURY RECALL

402 or authorized human governance may recall Executor capital.

A recall may occur because of:

strategy suspension
incident
excess exposure
market anomaly
policy change
security concern
data failure
jurisdiction change
strategy termination
capital reallocation

Executor architecture should make capital recall operationally straightforward.

## 26. EMERGENCY CONTAINMENT

Emergency controls should allow governance to:

disable Executor activity
revoke session authority
reduce or clear allowances
stop new authorizations
inventory pending transactions
reconcile balances
recall accessible capital
preserve transaction evidence

Emergency authority should not require the trading agent's cooperation.

## 27. EXTERNAL TRANSFERS

Transfers outside approved Treasury, Executor, router, pool, or contract relationships shall require higher-level authorization.

Unknown recipients shall fail closed.

The agent shall not infer recipient legitimacy from:

name
ticker
website
social-media post
natural-language instruction
LLM recommendation

Address-level policy controls take precedence.

## 28. CONTROL TOWER MONITORING

The PONS Control Tower should display, for every Treasury and Executor:

wallet address
strategy
current balance
authorized limit
current exposure
available capacity
pending transactions
active allowances
current autonomy level
402 policy
policy hash
latest funding event
latest harvest event
latest incident
current reconciliation status

Sensitive signing information shall remain private.

## 29. PROVENANCE

Every capital movement between Treasury and Executor shall be recorded.

Target evidence chain:

TREASURY ALLOCATION DECISION

↓

402 AUTHORIZATION

↓

EXECUTION INTENT

↓

TREASURY TRANSACTION

↓

EXECUTOR RECEIPT

↓

MARKET OPERATIONS

↓

REALIZED RESULT

↓

HARVEST INTENT

↓

HARVEST TRANSACTION

↓

TREASURY RECEIPT

↓

TREASURY ACCOUNTING RESULT

## 30. GOVERNANCE CHANGES

Changes involving Treasury architecture should receive greater scrutiny than ordinary strategy changes.

Examples:

new Treasury
new Executor
new signer
new module
new routing contract
new capital ceiling
new sweep destination
new strategy authority

These changes should produce:

governance record
policy update
version increment
approval record
transaction record where applicable
audit trail

## 31. RESEARCH BOUNDARY

Treasury research shall distinguish:

proprietary capital

from

third-party capital.

The existence of a Treasury or autonomous strategy does not independently determine whether the operation constitutes a fund, advisory relationship, security, commodity pool, or other regulated structure.

Those determinations remain subject to separate 402 legal/compliance review.

## 32. CANONICAL TREASURY RULES

TREASURY OWNS CAPITAL.

EXECUTOR RECEIVES DELEGATED CAPITAL.

AI DOES NOT OWN TREASURY AUTHORITY.

THE EXECUTOR SHALL NOT DEFINE ITS OWN LIMITS.

THE EXECUTOR SHALL NOT DEFINE ITS OWN TREASURY DESTINATION.

PROFIT SHALL BE DISTINGUISHED FROM RETURNED PRINCIPAL.

STRATEGY WALLETS SHOULD BE ISOLATED WHERE PRACTICAL.

CAPITAL ESCALATION REQUIRES GOVERNANCE.

UNKNOWN DESTINATION = DENY.

FAILURE TO VERIFY = NO TRANSFER.

TREASURY IS THE VAULT.

EXECUTOR IS THE HAND.