# Demo guide: Estimating Azure OpenAI costs with the Azure Pricing Calculator

This guide walks presenters and attendees through the four-step tokenomics demo: workload, model pricing, full FinOps cost, and sensitivity to agent behavior, then value and commitment planning.

All rates and volumes in this guide are **illustrative**. Replace them with measurements from a pilot before using the numbers for a real decision.

Verified against Microsoft documentation on 2026-10-11. Prices, product names, and model lists change frequently, so check the live pages before the session:

- [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/)
- [Azure OpenAI pricing](https://azure.microsoft.com/en-us/pricing/details/azure-openai/)
- [Understanding deployment types in Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- [Estimate costs with the Azure pricing calculator](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/pricing-calculator)

---

## Before you start

1. Open the [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/) in a browser and confirm it loads without signing in.
2. Open the [Azure OpenAI pricing page](https://azure.microsoft.com/en-us/pricing/details/azure-openai/) in a second tab. Use it to find the live rate for your chosen model and deployment type.
3. Sign in to the calculator only if you want to see negotiated or program pricing (for example, an MCA or EA billing account). Otherwise, retail prices are shown.
4. Prepare a worksheet (Excel or similar) with the planning inputs in Step 1. Keep it separate from the calculator. The calculator covers Azure consumption only.
5. Decide which model and deployment type you will use. The rehearsal reference used in this guide is the **GPT-6.1 Sol (short context) Global** row, which lists $2 per 1M input tokens and $10 per 1M output tokens. Confirm that row is still listed and still priced the same before the session. If it is not, use any model and replace the illustrative rates in the worksheet with that model's live rates.

> Important: the calculator uses live retail rates and cannot accept custom rates. If your model's live rate differs from the illustrative rate, the calculator total will differ from the slide. Show the calculator result and the illustrative worksheet side by side, and say so.

---

## Step 1: Establish the workload (1 minute)

Show these assumptions in your worksheet before opening the calculator. They are illustrative inputs.

| Planning input | Demo assumption |
|---|---|
| Active users | 500 |
| Requests per user per working day | 5 |
| Working days per month | 22 |
| Average model calls per request | 3 |
| Average input tokens per model call | 2,000 |
| Average output tokens per model call | 500 |

Input tokens include everything actually sent to that model call: instructions, conversation history, retrieved content, and tool results.

Calculate the volumes in the worksheet:

| Metric | Formula | Result |
|---|---|---|
| Requests per month | 500 × 5 × 22 | 55,000 |
| Model calls per month | 55,000 × 3 | 165,000 |
| Input tokens per month | 165,000 × 2,000 | 330,000,000 (330 million) |
| Output tokens per month | 165,000 × 500 | 82,500,000 (82.5 million) |

Message for the audience: user count alone is not enough. Agent behavior drives the forecast.

---

## Step 2: Price model consumption (2 minutes)

### 2a. Add the Azure OpenAI product

1. On the calculator, use the search box in the product picker and type **Azure OpenAI**.
2. If no tile appears, search for **Foundry Tools**, which is the current product family for AI services in the calculator product list. Confirm the product name in the calculator before the session.
3. Select the tile to add it to your estimate. The calculator adds it with default settings.

### 2b. Choose the model, deployment type, region, and currency

1. In the configuration module, select the **model** (for example, the GPT-6.1 Sol short-context row).
2. Select the **deployment type**. The current deployment types are:

| Deployment type | Data processing | Billing | Best for |
|---|---|---|---|
| Global Standard | Any Azure region | Pay-per-token | Default starting point, lowest price, broadest availability |
| Data Zone Standard | Within US, EU, or APAC data zone | Pay-per-token | Data zone residency |
| Standard | Within an Azure geography | Pay-per-token | Geography residency, low volume |
| Global Provisioned | Any Azure region | Hourly PTU or reservation | Predictable high throughput |
| Data Zone Provisioned | Within data zone | Hourly PTU or reservation | Data zone plus predictable throughput |
| Regional Provisioned | Within Azure geography | Hourly PTU or reservation | Geography residency plus throughput |
| Global / Data Zone Batch | Global or data zone | Discounted, 24-hour turnaround | Large asynchronous jobs |

For the demo, use **Global Standard** (pay-per-token). Provisioned types are covered in Step 5.

3. Select the **region** if the filter is shown. Global deployments are not region-specific.
4. Select the **currency** using the currency control in the estimate. Note that the estimate converts from USD using the published exchange rate.

### 2c. Enter the monthly volumes and check the units

1. Enter input tokens as **330** units of **1 million tokens**. Check that the unit label reads per 1M tokens. If it shows per 1K tokens, enter **330,000**.
2. Enter output tokens as **82.5** units of **1 million tokens** (or **82,500** per 1K).
3. If the model has a cached-input meter, leave it at zero for this demo unless you have measured cache hit rates. Cached input is billed separately from input tokens and must not be blended into the input rate.
4. If the model shows cache-write charges, leave them at zero for the baseline and mention them as a separate category.

### 2d. Read the result and compare it to the worksheet

With the GPT-6.1 Sol (short context) Global rates ($2 input, $10 output), the calculator should show:

| Component | Volume | Rate (per 1M tokens) | Monthly cost |
|---|---|---|---|
| Input tokens | 330 million | $2 | $660 |
| Output tokens | 82.5 million | $10 | $825 |
| **Model total** | | | **$1,485** |

If the calculator total differs, confirm the live rate on the pricing page. Report the difference rather than adjusting the worksheet.

### 2e. Name and save the configuration

1. Give the configuration a unique name, for example **Demo - Agent model consumption**.
2. Select **Save** or **Save as** and name the estimate.
3. Use **Export** if you want an Excel copy for the demo. Use **Share** only for a read-only link to show the estimate (the link lets others view the estimate, but only you can change it).

Message for the audience: there is no single universal token price. The model and deployment configuration determine the rate.

---

## Step 3: Reveal the broader FinOps cost (1 to 2 minutes)

### 3a. Add supporting Azure services in the calculator

Add the supporting services your architecture uses. The calculator lists the services available in its product picker. Examples:

- Azure AI Search
- Azure App Service or Azure Functions (application hosting)
- Storage Accounts (documents and logs)
- Azure Monitor and Application Insights (monitoring)

For each one, add the product, set the region and tier, enter the monthly usage, and name the configuration.

Presenter note: say "the supporting Azure services available in the calculator." Not every architecture component has a calculator entry.

### 3b. Keep labor, licenses, and external costs in the worksheet

These do not belong in the calculator. Record them in the worksheet:

| Monthly cost category | Demo example | Notes |
|---|---|---|
| Model consumption | $1,485 | From the calculator (Step 2) |
| Supporting Azure services | $600 | Sum of the calculator estimate for the supporting services |
| Applicable licenses and external tools | $200 | Third-party or internal licenses |
| Human oversight: 20 hours × $75 | $1,500 | Failure review, knowledge upkeep, quality checks, escalations |
| **Monthly operating cost** | **$3,785** | Sum of the rows above |

Explain the distinction:

- Human oversight includes reviewing failures, maintaining knowledge, checking quality, and handling escalations.
- Incremental cost excludes shared baseline costs and earlier build effort. A shared platform may already exist, but a fully allocated view includes an allocation of its cost.
- One-time implementation and initial testing costs appear in a separate line, not in the monthly estimate.

Label the three-column graphic **Incremental operating cost**.

---

## Step 4: Change one assumption (1 minute)

Keep users constant and change average model calls per request from 3 to 6. Keep tokens per call unchanged.

### 4a. Update the calculator

1. Keep the same estimate and model configuration.
2. Update the input token volume to **660** million (1M-token units) and the output token volume to **165** million.
3. Confirm that the calculator total is about **$2,970**.

### 4b. Confirm the worksheet values

| Metric | 3 calls per request | 6 calls per request |
|---|---|---|
| Monthly model calls | 165,000 | 330,000 |
| Input tokens | 330 million | 660 million |
| Output tokens | 82.5 million | 165 million |
| Model cost at illustrative rates | $1,485 | $2,970 |

Message for the audience: we added no users, but model cost doubled. The extra calls might be useful validation or unnecessary retries. Traces and quality measurements show which.

Recommend a range of scenarios: expected adoption, higher adoption, and more demanding agent behavior. Reassess supporting infrastructure and human review separately for each scenario.

Tip: save this as a second estimate using **Save as** so both scenarios are visible during the demo.

---

## Step 5: Connect cost to value and commitment planning (1 minute)

### 5a. Calculate unit economics

Use the Step 3 operating cost of $3,785:

| Metric | Calculation | Result |
|---|---|---|
| Cost per request | $3,785 ÷ 55,000 | About $0.069 (about 7 cents) |
| Successful resolution rate | Assumed 80 percent, measured in evaluation | 44,000 successful requests |
| Cost per successful resolution | $3,785 ÷ 44,000 | About $0.086 (about 9 cents) |

The success rate must come from evaluation and operational evidence, not from the assumption alone.

### 5b. Commitment options

Explain the planning sequence: estimate the workload, measure it in a pilot, build a realistic cost range, then choose the purchasing approach.

- **Provisioned throughput (PTU)** is billed hourly for each deployed PTU, prorated per minute, whether or not requests run. Estimate it from deployed capacity, not from token volume.
- **Reservations for provisioned throughput** give a discount for a one-month or one-year term. They are purchased separately from the deployment, per deployment type (Global, Data Zone, Regional), and can be scoped to a subscription, resource group, or management group. Confirm the current purchase steps in the Azure portal.

Do not put reservations into the calculator estimate as if they were pay-as-you-go. Note them as a separate commitment decision.

---

## Troubleshooting and common mistakes

| Symptom | Likely cause | Fix |
|---|---|---|
| Model total is 1,000 times too small or too large | Units entered per 1K instead of per 1M, or the reverse | Check the unit label on the input field before entering volumes |
| Total differs from the slide | Live rate differs from the illustrative rate | Show the live total and the illustrative total side by side |
| Azure OpenAI product not found | Product naming changed | Search for Foundry Tools or use the link on the Azure OpenAI pricing page |
| Cached input is missing | Cached input is a separate meter | Enter cache hit assumptions only if measured |
| Provisioned estimate looks low | Provisioned bills on deployed PTUs, not tokens | Use the PTU hourly rate and reservation guidance, not token units |
| Reservation not reflected in the estimate | Reservations are purchased separately | Note the reservation as a separate commitment decision |
| Price differs from the pricing page | Currency conversion or negotiated program pricing | Confirm the currency and whether you are signed in to a program |

---

## Presenter checklist

- [ ] Calculator loads and the Azure OpenAI or Foundry Tools tile is available
- [ ] Chosen model row and deployment type are confirmed on the live pricing page
- [ ] Unit labels are checked (per 1M tokens)
- [ ] Worksheet includes Step 1 volumes, Step 3 operating cost, and Step 4 scenario
- [ ] Both scenarios are saved as separate estimates
- [ ] Illustrative assumptions are labeled as such on every slide
- [ ] The slide edits are applied: "the supporting Azure services available in the calculator" and the "Incremental operating cost" label
