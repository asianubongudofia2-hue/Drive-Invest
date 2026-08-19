PROJECT: DRIVE INVEST
MODE: PRIVATE TWO-USER INVESTMENT MANAGEMENT PLATFORM
        ↓
Verify complete source
        ↓
Upload/connect actual source
        ↓
Vercel build
        ↓
Fix compatibility errors
        ↓
Deploy foundation
        ↓
Authentication
        ↓
KYC
        ↓
Investor dashboard
        ↓
Wallet / ledger
        ↓
Portfolio
        ↓
Notifications
        ↓
AI assistant
        ↓
Admin/manager modules
        ↓
Supabase backend integration
        ↓
Final production verification
IMPORTANT:
This is a private platform restricted to two authorized users. Do not create public investor registration or public access to private financial data.

Use the EXISTING Supabase project and existing backend tables/functions wherever possible. Do not create duplicate KYC, wallet, ledger, or transaction systems if equivalent existing structures already exist.

Build the application incrementally and production-safely.

==================================================
1. APPLICATION FLOW
==================================================

Create this exact user journey:

WELCOME
   ↓
LOGIN
   ↓
AUTHENTICATION
   ↓
KYC / ACCOUNT VERIFICATION
   ↓
WITHDRAWAL ACCOUNT SETUP
   ↓
ACCOUNT ACTIVATION
   ↓
$500 ACTIVATION FEE
   ↓
PAYMENT METHOD
   ↓
PAYMENT VERIFICATION
   ↓
ACCOUNT ACTIVATED
   ↓
INVESTOR DASHBOARD
   ↓
PORTFOLIO / INVESTMENT
   ↓
PAYOUT SCHEDULE
   ↓
AI NOTIFICATIONS

The dashboard containing financial balances must remain locked until the required activation conditions have been satisfied.

==================================================
2. $500 ACTIVATION FEE
==================================================

Implement a separate:

ACTIVATION FEE = USD 500

This fee must NOT be treated as an investment.

Display clearly:

"$500 Account Activation Fee"

"Activation fee is separate from investment capital and does not constitute an investment or guaranteed return."

Do NOT automatically credit the $500 to the user's investment balance.

Create an activation status:

PENDING
PAYMENT_REQUIRED
PAYMENT_PROCESSING
PAID
FAILED
REFUNDED
CANCELLED

The dashboard is unlocked only after the backend confirms the payment as successfully settled.

Never unlock the dashboard merely because the frontend says payment succeeded.

==================================================
3. WITHDRAWAL ACCOUNT SETUP
==================================================

Before requesting the activation fee, require the user to configure their withdrawal destination.

Screen:

"Activate Your Withdrawal Account"

Fields should support the appropriate information for the selected withdrawal method.

For bank withdrawal, collect only the information legitimately required by the configured payment provider.

Never store raw card numbers or sensitive payment credentials in our database.

Use a PCI-compliant payment provider/tokenization system for cards.

==================================================
4. PAYMENT METHODS
==================================================

After the user clicks ACTIVATE ACCOUNT, display:

• Gift Card
• Bank Transfer
• Card
• Cryptocurrency

IMPORTANT:

Do not implement gift-card or cryptocurrency payments as arbitrary manual balance credits.

Each payment method must have a legitimate verification/reconciliation process.

For card payments, use a compliant payment processor.

For bank transfers, create a pending payment record and reconcile it against the provider/bank confirmation.

For cryptocurrency, require a legitimate payment processor/wallet verification mechanism and confirmation before activation.

The frontend must never directly modify wallet balances.

==================================================
5. ACCOUNT ACTIVATION
==================================================

Create:

activation_payments
account_activation_status

or reuse an equivalent existing transaction/payment architecture if already present.

The server must verify:

payment amount
currency
payment status
user
provider transaction ID
timestamp
idempotency key

before activating the account.

Prevent:

double payment
double activation
fake payment confirmation
client-side balance manipulation
replayed webhooks

==================================================
6. DASHBOARD
==================================================

After verified activation, show:

CapitalFlow / Drive Invest Dashboard

Sections:

• Available Balance
• Investment Capital
• Portfolio
• Performance
• Bonus / Promotional Credits
• Transactions
• Upcoming Payout
• Withdrawal Account
• Notifications
• AI Assistant

IMPORTANT:

Never display fabricated balances.

If the intended "$83,000" figure is a target, promotional limit, projected amount, or contractual amount, label it precisely.

For example:

"$83,000 Target"

or

"$83,000 Promotional Allocation — subject to applicable conditions"

Do NOT label it "Balance", "Cash", "Available Funds", or "Profit" unless that amount actually exists and is recorded in the backend ledger.

==================================================
7. INVESTMENT MODULE
==================================================

Build an investment module using the existing Supabase:

investments
investment_plans
positions
transactions
ledger_accounts
ledger_entries
wallets

Do not allow the browser to directly create financial ledger entries.

All financial mutations must go through trusted server-side functions.

Every financial event must produce an immutable audit trail.

==================================================
8. PAYOUT SCHEDULE
==================================================

Change the previous payout concept from twice weekly to:

ONE PAYOUT WINDOW PER MONTH.

The manager/admin can configure the approved payout date.

Example:

Payout date: 25th of each month.

The system must calculate the next payout date from the configured schedule.

Do not promise or guarantee profits.

==================================================
9. PAYOUT APPROVAL
==================================================

The AI MUST NOT have permission to move money.

Create this architecture:

USER REQUEST
     ↓
AI PREPARES PAYOUT REQUEST
     ↓
LEDGER / ELIGIBILITY CHECK
     ↓
MANAGER APPROVAL REQUIRED
     ↓
PAYMENT PROCESSOR
     ↓
PAYMENT CONFIRMATION
     ↓
LEDGER UPDATE
     ↓
USER NOTIFICATION

The AI may:

• explain payout status
• calculate dates
• prepare a payout request
• notify the user
• summarize transactions
• identify missing information

The AI may NOT:

• transfer money
• approve payouts
• change balances
• create money
• bypass KYC
• bypass payment verification
• change ledger entries
• change payout dates without authorization
• approve its own requests

==================================================
10. AI ASSISTANT
==================================================

Add a secure server-side AI assistant.

The AI should handle:

• account notifications
• activation reminders
• KYC reminders
• investment reminders
• payout-date reminders
• payment-status notifications
• portfolio explanations
• transaction explanations
• account-status questions

The AI can notify the user when:

activation payment is required
KYC requires action
an investment payment is due
an investment transaction is confirmed
a payout window is approaching
a payout request requires approval
a payout has been approved
a payout has been processed

The AI must only use verified backend data.

Never allow the AI to invent:

balances
profits
transactions
payouts
fees
payment confirmations
investment performance

==================================================
11. MANAGER APPROVAL
==================================================

Create a private manager/admin interface.

Manager can:

• configure payout date
• review payout requests
• approve/reject payout requests
• review transactions
• review KYC status
• review activation payments
• send announcements
• configure notification schedules

Every manager action must create an audit log.

Require strong authentication/MFA for the manager account.

==================================================
12. PRIVATE TWO-USER ACCESS CONTROL
==================================================

Do NOT expose private investor functionality publicly.

Create an explicit allowlist/authorization mechanism for the two authorized users.

Every request must verify:

authenticated user
authorized user ID
role
account status

Do not rely on hidden frontend URLs for security.

Use Supabase RLS and server-side authorization.

==================================================
13. EXISTING KYC BACKEND
==================================================

Reuse the existing:

kyc-start
kyc-submit
kyc-webhook
kyc-admin-review

functions.

Do not create a fake KYC provider.

If no legitimate KYC provider is configured, clearly show:

"KYC provider configuration required."

Never mark a user VERIFIED without an actual verification result.

==================================================
14. NOTIFICATIONS
==================================================

Use the existing:

notifications
notification_preferences

tables where appropriate.

Create notification types:

ACTIVATION_REQUIRED
ACTIVATION_PAYMENT_PENDING
ACTIVATION_CONFIRMED
KYC_ACTION_REQUIRED
INVESTMENT_PAYMENT_DUE
INVESTMENT_CONFIRMED
PAYOUT_UPCOMING
PAYOUT_REQUESTED
PAYOUT_APPROVED
PAYOUT_PROCESSING
PAYOUT_COMPLETED
PAYOUT_REJECTED
SECURITY_ALERT

Notifications must originate from trusted backend events.

==================================================
15. PUBLIC ANNOUNCEMENTS
==================================================

Keep public announcements completely separate from private financial data.

Create an announcement system that can publish authorized promotional/news content.

If content references a real person such as Michael B. Jordan, do not state or imply endorsement, partnership, sponsorship, or participation unless legally authorized and documented.

Use neutral factual wording where authorization is absent.

==================================================
16. SECURITY
==================================================

Implement:

Supabase RLS
MFA for privileged accounts
server-side financial mutations
idempotency keys
audit logging
webhook signature verification
rate limiting
session protection
secure cookies
CSRF protection where applicable
input validation
server-side authorization
least-privilege access
no service-role key in frontend
no raw card storage
no private keys in source code
no secrets committed to GitHub

==================================================
17. VERCEL
==================================================

Prepare Drive Invest for Vercel deployment.

Environment variables must be configured through Vercel:

NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY

Any server-only secrets must remain server-side.

Never expose:

SUPABASE_SERVICE_ROLE_KEY
payment provider secret keys
KYC provider secret keys
AI provider secret keys

to the browser.

==================================================
18. BUILD ORDER
==================================================

Build and test in this order:

PHASE 1
Existing Supabase schema audit

PHASE 2
Authentication

PHASE 3
KYC interface

PHASE 4
Withdrawal account setup

PHASE 5
$500 activation payment

PHASE 6
Activation verification

PHASE 7
Investor dashboard

PHASE 8
Wallet / ledger

PHASE 9
Investment module

PHASE 10
Monthly payout workflow

PHASE 11
Manager approval

PHASE 12
AI assistant

PHASE 13
Notifications

PHASE 14
Announcement system

PHASE 15
Security audit

PHASE 16
Vercel preview deployment

PHASE 17
End-to-end testing

PHASE 18
Production deployment

==================================================
19. CRITICAL FINANCIAL RULE
==================================================

The system must never manufacture or simulate real money.

Every displayed financial balance must originate from a verified backend record.

Every deposit must have a verified payment event.

Every investment must have a recorded transaction.

Every payout must have an approved request and corresponding ledger/payment record.

The AI is an assistant and notification engine only.

It has ZERO authority to move money.

==================================================
20. FINAL TASK
==================================================

First inspect the existing Drive Invest/CapitalFlow Supabase database and Edge Functions.

Reuse existing functionality.

Then build only the missing frontend/backend pieces.

Do not duplicate existing tables or KYC functions.

After each phase:

1. Build
2. Run tests
3. Check TypeScript
4. Check Supabase integration
5. Check RLS
6. Check authorization
7. Fix errors
8. Continue to the next phase

Do NOT claim a phase is complete until it has actually been implemented and tested.

Finally prepare the complete application for Vercel Preview deployment and report:

• files created
• files modified
• Supabase functions used
• database tables used
• environment variables required
• security checks completed
• remaining production requirements
• Vercel build result