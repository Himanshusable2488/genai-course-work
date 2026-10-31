# Prompt library

One entry per prompt version. Newest at the bottom. Each entry has the prompt, the output it produced,
and which acceptance checks it passed. Never edit an old entry: add a new version.


## T1-credit-summary | v1 | 2026-10-30

**Purpose:** Summarise an applicant for a credit committee
**Result:** 4/4 checks passed

### Prompt
- **Role:** You are a credit analyst writing for a credit committee.
- **Task:** Summarise the applicant and recommend approve, refer or decline.
- **Context:** Applicant facts:
- Name: Kavya Reddy, age 34.
- Occupation: Software engineer; 6 years with the same employer.
- Monthly income: ₹1,20,000.
- Existing monthly EMIs: ₹18,000.
- Proposed monthly EMI: ₹22,244.
- Credit score: 742.
- Exactly one missed payment in the last 24 months:
  a payment was 30 days late, 14 months ago.

Lending policy (apply exactly):
Lending policy (personal loans):
1. Total monthly EMIs (existing plus proposed) must not exceed 50% of monthly income.
2. A credit score of 700 or above is required for approval.
3. Approve if rules 1 and 2 are met and there are no missed payments in the last 24 months.
4. Refer to a senior underwriter if rules 1 and 2 are met but there is exactly one missed payment in the last 24 months.
5. Decline if rule 1 or 2 is not met, or if there are two or more missed payments in the last 24 months.

Decision for this applicant:
- EMI-to-income ratio = (18000 + 22244) / 120000 * 100 = 33.54%.
- The EMI ratio meets the 50% limit.
- The credit score meets the minimum of 700.
- There is exactly one missed payment in the last 24 months.
- Therefore, the required recommendation is REFER to a senior underwriter.
- Do not recommend APPROVE for this applicant.
- **Constraints:** Use at most 70 words.; Calculate total monthly EMIs by adding existing EMIs (₹18,000) and proposed EMI (₹22,244), giving ₹40,244.; Calculate the EMI-to-income ratio as ₹40,244 divided by ₹1,20,000, multiplied by 100. The result is approximately 33.54%.; Apply the lending policy exactly. A credit score of at least 700 and an EMI-to-income ratio of at most 50% do not automatically mean approval.; If the first two rules are met and there is exactly one missed payment in the last 24 months, recommend refer to a senior underwriter.; Recommend approve only when the first two rules are met and there are no missed payments in the last 24 months.; Do not add opinions or recommendations that are not supported by the policy.
- **Output format:** Exactly three lines, each starting with '- '. Line 1: Applicant profile. Line 2: Total EMI-to-income ratio as a percentage. Line 3: 'Recommendation: approve', 'Recommendation: refer', or 'Recommendation: decline', followed by the policy reason.
- **Examples:** 0

### Output
> - Applicant: Kavya Reddy, 34, software engineer, 6 years with the same employer, monthly income: ₹1,20,000, existing EMIs: ₹18,000, requested personal loan: ₹10,00,000 at 12% per annum for 60 months.
> - Total EMI-to-income ratio: 33.54%.
> - Recommendation: refer to a senior underwriter.

### Checks
- [x] at most 80 words (42 words)
- [x] exactly 3 bullet lines (3 bullet lines)
- [x] states the total EMI-to-income ratio (about 33-34%)
- [x] recommends 'refer', as the policy requires

## T2-sms-declined | v1 | 2026-10-30

**Purpose:** Tell a customer their application was declined, by SMS
**Result:** 6/6 checks passed

### Prompt
- **Role:** You are a customer-communications writer at a retail bank, writing directly to a customer who may be upset.
- **Task:** Write a polite, clear SMS informing the customer that their loan application was not approved.
- **Context:** Customer name: Rohan Mehta.
    Application reference: APP-20931.
    Outcome: declined.
    Confidential internal reason: internal risk score 612 and cutoff 650.
    The customer may reapply after 90 days.
    Helpline: 1800-555-0199.

    Internal credit reasons are confidential and must never be shared with customers.
    Never disclose the internal risk score, the cutoff, or any internal assessment details.
- **Constraints:** Keep the entire message under 280 characters, including spaces and punctuation.; Address the customer by first name: Rohan.; Include the application reference exactly as given: APP-20931.; Include the helpline exactly as given: 1800-555-0199.; Mention that the customer may reapply after 90 days.; Never reveal the internal risk score, the cutoff, or the confidential internal reason.; Be polite, empathetic, and clear. Do not invent explanations for the rejection.
- **Output format:** One plain-text SMS only. No subject line, no markdown, no emojis, and no signature.
- **Examples:** 0

### Output
> "Hi Rohan, regarding your loan application APP-20931, we regret to inform you that it has been declined. You may reapply after 90 days. For more information or to discuss, please call our helpline at 1800-555-0199."

### Checks
- [x] at most 300 characters (215 characters)
- [x] includes the application reference
- [x] includes the helpline number
- [x] mentions the 90-day reapply period
- [x] addresses the customer by name
- [x] does NOT reveal the confidential internal reason

## T3-complaint-triage | v1 | 2026-10-30

**Purpose:** Triage a complaint into JSON
**Result:** 4/4 checks passed

### Prompt
- **Role:** You are a complaint-triage assistant for a bank's customer-service ticketing system.
- **Task:** Classify the customer's complaint by assigning exactly one category and one urgency level.
- **Context:** Complaint:
My debit card was swallowed by the ATM at the Banjara Hills branch last night.
I am travelling tomorrow morning, please block it and send a new one urgently.

Category definitions:
category: card (debit or credit card problems), loan, account (transfers, UPI, statements, balances), other (servicing requests such as address or nominee changes). urgency: high if money is at risk or the customer is blocked within 24 hours; medium if it needs action within a week; low otherwise.

Urgency definitions:
- high: Money is at risk or the customer is blocked within 24 hours.
- medium: Action is needed within a week.
- low: Action is not needed urgently.
- **Constraints:** Choose exactly one category from: account, card, loan, other.; Choose exactly one urgency level from: high, medium, low.; Use category 'card' for debit or credit card problems.; Use urgency 'high' if money is at risk or the customer is blocked within 24 hours.; Classify the complaint using the provided definitions.; Do not explain your decision or add any extra information.
- **Output format:** Return exactly one valid JSON object with exactly two keys: "category" and "urgency". Use double quotes around keys and string values. Example structure: {"category": "card", "urgency": "high"}. Return JSON only, with no prose, markdown, or code fences.
- **Examples:** 0

### Output
> {"category": "card", "urgency": "high"}

### Checks
- [x] reply is a JSON object only (no prose, no code fences)
- [x] has exactly the keys ['category', 'urgency']
- [x] category is one of ['account', 'card', 'loan', 'other'] and is 'card' (category = 'card')
- [x] urgency is one of ['high', 'low', 'medium'] and is 'high' (urgency = 'high')

## T1-credit-summary | v2 | 2026-10-31

**Purpose:** S15 v2: reasoning in a private scratchpad, only <final> is used. Improved average checks from 3.0/4 to 4.0/4, and full-pass runs from 1/2 to 2/2. Tokens decreased from 1038 to 1013 per call.
**Result:** 4/4 checks passed

### Prompt
- **Role:** You are a credit analyst writing for a credit committee.
- **Task:** Summarise the applicant and recommend approve, refer or decline.
- **Context:** Lending policy (personal loans):
1. Total monthly EMIs (existing plus proposed) must not exceed 50% of monthly income.
2. A credit score of 700 or above is required for approval.
3. Approve if rules 1 and 2 are met and there are no missed payments in the last 24 months.
4. Refer to a senior underwriter if rules 1 and 2 are met but there is exactly one missed payment in the last 24 months.
5. Decline if rule 1 or 2 is not met, or if there are two or more missed payments in the last 24 months.
- **Constraints:** Use at most 70 words.; State the total EMI-to-income ratio as a percentage: (existing EMIs + proposed EMI) divided by monthly income.; Apply the policy exactly. Do not add opinions that are not in the policy.; Do your reasoning inside <scratchpad>...</scratchpad> first. The scratchpad is private working and is never shown to anyone.
- **Output format:** A <scratchpad> block, then <final>...</final> containing exactly this: Exactly three lines, each starting with '- ': (1) applicant profile, (2) the EMI-to-income ratio, (3) 'Recommendation: approve|refer|decline' followed by the policy reason.
- **Examples:** 0

### Output
> - Applicant: Kavya Reddy, 34, software engineer, 6 years tenure.  
> - EMI-to-income ratio: 33.5%.  
> - Recommendation: refer due to exactly one missed payment in last 24 months.

### Checks
- [x] at most 80 words (27 words)
- [x] exactly 3 bullet lines (3 bullet lines)
- [x] states the total EMI-to-income ratio (about 33-34%)
- [x] recommends 'refer', as the policy requires

## T2-sms-declined | v2 | 2026-10-31

**Purpose:** S15 v2: reasoning in a private scratchpad, only <final> is used. Both versions passed 6/6 checks in 2/2 runs. Version 1 used 895 tokens per call versus 1065 for version 2. Version 2 did not improve measured acceptance checks, although the scratchpad format supports separation of private working from the customer-facing reply.
**Result:** 6/6 checks passed

### Prompt
- **Role:** You are a customer-communications writer at a retail bank.
- **Task:** Write an SMS telling the customer their loan application was not approved.
- **Context:** Be kind and clear. Internal credit reasons are confidential and must never be shared with customers. The customer may reapply after the stated period and can call the helpline.
- **Constraints:** Use at most 280 characters.; Address the customer by first name.; Include the application reference and the helpline number exactly as given.; Mention the reapply period.; Never reveal internal scores, cutoffs or the internal reason.; Do your reasoning inside <scratchpad>...</scratchpad> first. The scratchpad is private working and is never shown to anyone.
- **Output format:** A <scratchpad> block, then <final>...</final> containing exactly this: One plain-text SMS. No subject line, no markdown, no emojis.
- **Examples:** 0

### Output
> Hi Rohan, we regret to inform you that your loan application (APP-20931) was not approved. You may reapply in 90 days. For assistance, call 1800-555-0199. Thank you for choosing us.

### Checks
- [x] at most 300 characters (181 characters)
- [x] includes the application reference
- [x] includes the helpline number
- [x] mentions the 90-day reapply period
- [x] addresses the customer by name
- [x] does NOT reveal the confidential internal reason
