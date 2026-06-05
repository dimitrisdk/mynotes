# Gmail email signals scenario matrix

Date: 2026-06-05 07:45 UTC
Kanban: `t_42bbf352`
Status: Completed
Artifact: `/root/.hermes/kanban/workspaces/t_42bbf352/gmail-email-scenario-matrix.md`

## Συμπέρασμα

- Public sources do not confirm exact Skroutz transactional sender addresses. Treat Skroutz sender/domain patterns as assumptions until verified against Dimitris's real mailbox samples.
- Gmail API search (`q`) is good for candidate retrieval, but final classification should inspect individual message headers/body because API search has UI differences around aliases/thread-wide behavior.
- Receipts and subscriptions are fuzzy classes; `List-Unsubscribe` is a newsletter/bulk-mail signal, not proof of paid subscription.
- Security alerts must stay read-only: summarize, flag severity, and direct Dimitris to official account/security pages; never click links or mark resolved automatically.

## Scenario matrix

| Category | Confirmed signals | Assumptions / likely patterns | Subject/body cues | Downstream action | Safety |
|---|---|---|---|---|---|
| Skroutz order confirmation | Skroutz Help Center says successful submitted orders should have a confirmation email; order code exists in account Orders and emails Skroutz sent. | Sender likely `skroutz.gr` / subdomain, but exact address unconfirmed. | `παραγγελία`, `κωδικός παραγγελίας`, `καταχωρήθηκε`, `επιβεβαίωση`, `order confirmation`, `submitted`. | Propose/create Orders Kanban card; auto-update only on exact known `order_id` match. | Medium; first-seen sender remains read-only. |
| Skroutz status/shipment | Skroutz Help references tracking progress, delivery, order stages. | Same transactional infra or separate courier messages. | `αποστολή`, `παραδόθηκε`, `tracking`, `Skroutz Point`, `delivered`, `shipped`. | Update existing order timeline/status if exact order match. | Medium if matched to known order; otherwise read-only. |
| Receipts/invoices | Stripe docs confirm email receipts/paid invoices; Gmail `updates` category covers confirmations/statements/reminders; Gmail has `purchases` search value. | Senders vary: merchant, Stripe/custom domain, PayPal, Apple, Amazon, banks. | `receipt`, `invoice`, `paid`, `απόδειξη`, `τιμολόγιο`, `πληρωμή`, `total`, `VAT/ΦΠΑ`, PDF attachments. | Propose expense entry with fields. Auto-book only after sender/template allowlist. | Medium-high for known templates; low for unknown senders. |
| Subscriptions/recurring | RFC 8058 and Google sender guidelines define one-click unsubscribe headers; Stripe subscriptions generate invoices. | `List-Unsubscribe` implies list/bulk mail, not paid subscription by itself. | `subscription`, `renewal`, `next billing`, `monthly`, `annual`, `trial`, `συνδρομή`, `ανανέωση`. | Propose recurring expense/subscription record; set renewal reminder. | Never unsubscribe automatically; explicit confirmation required. |
| Google security alerts | Google says suspicious sign-in emails indicate unusual sign-in and should be checked through Google Account; Gmail says Google won't ask for password by email. | Sender like `no-reply@accounts.google.com` is not enough; verify auth/alignment when available. | `Critical security alert`, `Suspicious sign-in prevented`, `new sign-in`, `password changed`, `2-Step Verification`. | High-priority read-only alert; route to official account security page. | Always read-only. |
| Other security alerts | PayPal/Apple/Amazon/ACS/Gmail guidance emphasizes direct login/verification and phishing caution. ACS confirms official ACS email domain ends in `@acscourier.gr`. | Domains must be learned per provider. | `verify`, `account suspended`, `unauthorized`, `action required`, urgent pay/login links, unknown attachments. | Security triage; if also purchase/receipt, security precedence. | Read-only until provider template verified. |

## Fields to extract

- Orders: `order_id`, merchant, items, total, currency, order date, ETA, carrier/tracking, status.
- Expenses: merchant, amount, currency, date, VAT/tax, invoice/receipt number, payment method, line items, attachments.
- Subscriptions: service, plan, amount, period, next renewal/trial end, payment method, unsubscribe-header presence.
- Security: provider, affected account, event type, time, device/location/IP, requested action, URLs/domains, authentication results.

## Ambiguous cases that remain read-only

- First-seen sender/domain.
- Missing amount/currency for expense.
- Missing order id for order update.
- Newsletter with unsubscribe header but no paid/renewal language.
- Urgent login/pay/update-details request.
- Unknown attachments.
- Courier/payment email not tied to a known order.

## Τι σημαίνει για τον Δημήτρη

Start with a read-only Gmail classifier that creates suggestions, not actions. The only safe early automation is: label/categorize emails, extract fields, propose expense/order/subscription/security records, and auto-update existing Orders only when the message has a known sender/template and exact order-id match. Anything involving payments, unsubscribe, credential/security resolution, or unknown senders needs Dimitris confirmation.

## Πηγές

- Gmail API search/filter docs: https://developers.google.com/workspace/gmail/api/guides/filtering
- Gmail search operators: https://support.google.com/mail/answer/7190?hl=en
- Gmail categories: https://support.google.com/mail/answer/3094499?hl=en
- Gmail purchase tracking blog: https://blog.google/products-and-platforms/products/gmail/one-stop-purchase-tracking-in-gmail/
- Skroutz order/account Help Center: https://help.skroutz.gr/hc/en-us/articles/4403544892689-Why-do-I-not-see-the-order-in-my-account
- Skroutz contact/order support: https://help.skroutz.gr/hc/el-gr/articles/360019507237-%CE%A0%CF%8E%CF%82-%CE%BD%CE%B1-%CE%B5%CF%80%CE%B9%CE%BA%CE%BF%CE%B9%CE%BD%CF%89%CE%BD%CE%AE%CF%83%CE%B5%CE%B9%CF%82-%CE%BC%CE%B5-%CF%84%CE%B7%CE%BD-%CE%BF%CE%BC%CE%AC%CE%B4%CE%B1-%CF%84%CE%BF%CF%85-Customer-Care
- Stripe receipts/invoices: https://docs.stripe.com/payments/checkout/receipts
- RFC 8058 one-click unsubscribe: https://datatracker.ietf.org/doc/html/rfc8058
- Google sender guidelines: https://support.google.com/a/answer/81126?hl=en
- Gmail phishing guidance: https://support.google.com/mail/answer/8253?hl=en
- Google suspicious sign-in guidance: https://support.google.com/accounts/answer/6063333?hl=en
- ACS courier anti-phishing / official sender pattern: https://www.acscourier.net/en/important-updates/frauds-using-the-acs-brand-1622026/
- PayPal phishing guidance: https://www.paypal.com/us/cshelp/article/how-do-i-spot-a-fake-fraudulent-or-phishing-paypal-email-or-website-help164
