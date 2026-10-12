# Demo guide: Estimating Azure OpenAI costs with the Azure Pricing Calculator

This guide walks presenters and attendees through the four-step tokenomics demo: workload, model pricing, full FinOps cost, and sensitivity to agent behavior, then value and commitment planning.

All rates and volumes in this guide are **illustrative**. Replace them with measurements from a pilot before using the numbers for a real decision.

Reviewed against Microsoft documentation and the supplied calculator screenshots on 2026-10-11. The walkthrough below uses the field labels and 1,000-token units shown in those screenshots. Prices, product names, and model lists change frequently, so check the live pages before the session:

- [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/)
- [Azure OpenAI pricing](https://azure.microsoft.com/en-us/pricing/details/azure-openai/)
- [Understanding deployment types in Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- [Estimate costs with the Azure pricing calculator](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/pricing-calculator)

---

## Before you start

1. Open the [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/) in a browser and confirm it loads without signing in.
2. Open the [Azure OpenAI pricing page](https://azure.microsoft.com/en-us/pricing/details/azure-openai/) in a second tab. Use it to find the live rate for your chosen model and deployment type.
3. Use retail pricing for the baseline. To view negotiated pricing, sign in to the calculator and select the appropriate licensing program and billing account/profile or agreement. Signing in or selecting MCA alone does not confirm that negotiated prices are applied. Saving an estimate may also require signing in.
4. Prepare a worksheet (Excel or similar) with the planning inputs in Step 1. Keep it separate from the calculator. The calculator covers Azure consumption only.
5. Decide which model and deployment type you will use. The rehearsal reference used in this guide is the **GPT-6.1 Sol (short context) Global** row, which lists $2 per 1M input tokens and $10 per 1M output tokens. Confirm that row is still listed and still priced the same before the session. If it is not, use any model and replace the illustrative rates in the worksheet with that model's live rates.

> Important: the calculator uses live prices for the selected configuration and pricing program; it cannot accept custom rates. If your model's live rate differs from the illustrative rate, the calculator total will differ from the slide. Show the calculator result and the illustrative worksheet side by side, and say so.

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

1. Select the **Products** tab and type **Azure OpenAI** in the product search box.
2. On the **Azure OpenAI** tile, select **Add to estimate**. This is the product name confirmed in the screenshots.
3. Scroll to **Your Estimate** and expand the **Azure OpenAI** configuration if it is collapsed. Replace its default settings in the next step.

### 2b. Match the configuration fields in screen order

Set the four dropdowns in the top row from left to right, then the deployment dropdown below **Language Models**:

| Order | Calculator field | Demo selection |
|---|---|---|
| 1 | **Region** | **East US** |
| 2 | **Model type** | **Language Models** |
| 3 | **Model** | **GPT-6.1 Sol (short context)** |
| 4 | **Pricing Strategy** | **Standard (On-Demand)** |
| 5 | **Deployment type** | **Global** |

These are the exact labels and selections shown in the screenshots. If a model or option is unavailable, use the **service availability** link and check a supported configuration before continuing.

The calculator's **Standard (On-Demand)** pricing strategy plus **Global** deployment type corresponds to **Global Standard** in the deployment documentation. Do not look for a single dropdown value named Global Standard. **Region** is still a required calculator selection for this walkthrough; selecting East US does not restrict Global inference processing to East US.

For reference, the documented deployment types are:

| Deployment type | Data processing | Billing | Best for |
|---|---|---|---|
| Global Standard | Any Azure region | Pay-per-token | Default starting point, lowest price, broadest availability |
| Data Zone Standard | Within US, EU, or APAC data zone | Pay-per-token | Data zone residency |
| Standard | Within an Azure geography | Pay-per-token | Geography residency, low volume |
| Global Provisioned | Any Azure region | Hourly PTU or reservation | Predictable high throughput |
| Data Zone Provisioned | Within data zone | Hourly PTU or reservation | Data zone plus predictable throughput |
| Regional Provisioned | Within Azure geography | Hourly PTU or reservation | Geography residency plus throughput |
| Global / Data Zone Batch | Global or data zone | Discounted asynchronous processing | Large asynchronous jobs |

For the demo, keep the selections in the first table. Provisioned types are covered in Step 5; the reference table is not a list of values in a single calculator dropdown.

### 2c. Enter the monthly volumes and check the units

The screenshots show **x1,000 tokens** below each quantity box and **Per 1,000 tokens** below each rate. Divide monthly tokens by 1,000 to get the number to enter. Enter the numbers without commas:

| Calculator field | Monthly tokens | Enter in quantity box | Displayed rate per 1,000 tokens | Expected monthly cost |
|---|---|---|---|---|
| **Input** | 330,000,000 | `330000` | $0.00200 | $660 |
| **Cached Input** | 0 | `0` | $0.00010 | $0 |
| **Output** | 82,500,000 | `82500` | $0.01000 | $825 |

1. Replace **Input** with `330000`.
2. Leave **Cached Input** at `0` because this baseline assumes no cached tokens. If you later model caching, split input tokens between Input and Cached Input instead of counting the same tokens twice.
3. Replace **Output** with `82500`.

> Screenshot correction: **Input = 330** represents only 330,000 tokens and costs **$0.66** at $0.002 per 1,000 tokens. **Output = 0** adds no output cost. That is why the screenshot total is $0.66, not the demo's $1,485. Use the quantities above.

If the UI later changes to 1-million-token units, enter `330` for Input and `82.5` for Output instead. Always follow the visible unit label. The selected model in the screenshots has no cache-write field; account for one separately only if a different model exposes it.

### 2d. Read the result and compare it to the worksheet

With the GPT-6.1 Sol (short context) Global rates ($2 input, $10 output), the calculator should show:

| Component | Volume | Rate (per 1M tokens) | Monthly cost |
|---|---|---|---|
| Input tokens | 330 million | $2 | $660 |
| Output tokens | 82.5 million | $10 | $825 |
| **Model total** | | | **$1,485** |

The displayed rates of $0.002 and $0.010 per 1,000 tokens equal $2 and $10 per 1 million tokens. The Azure OpenAI module should show **Monthly cost = $1,485**, **Annual cost = $17,820**, and **Upfront cost = $0** for this configuration. The annual figure is 12 times the monthly estimate, not an annual commitment.

If the module total differs, check the quantities, units, model configuration, currency, and licensing program, then confirm the live rate on the pricing page. Report any remaining price difference rather than forcing the calculator to match the slide.

### 2e. Check the estimate footer, name, and save

1. Rename the text box currently labeled **Azure OpenAI** to **Demo - Agent model consumption**.
2. Scroll below the product configurations. Under **Support**, keep **Basic (Included)** for this demo ($0). A paid support plan would add to the overall estimate.
3. Under **Select your program/offer**, check **LICENSING PROGRAM**. The screenshot shows **Microsoft Customer Agreement (MCA)** with **Selected billing profile: None selected (change)**; do not treat that as confirmation of negotiated prices. For an eligible MCA account, select **None selected (change)** and choose the applicable billing account/profile. Other programs have their own agreement-selection controls. Keep **Show Dev/Test Pricing** off for this baseline.
4. At the bottom right, set **CURRENCY** to **United States - Dollar ($) USD**. Other currencies are converted from USD using the published exchange rate.
5. Check **Estimated monthly cost**. With only the Azure OpenAI product and Basic support, it should match the $1,485 model total. Additional products or paid support increase this overall total.
6. Select **Save** and name the baseline estimate; sign in if prompted. Use **Save as** to create a separate named scenario.
7. Use **Export** for an Excel copy or **Share** for an estimate link. Review the contents before sharing; the documentation states that only the estimate owner can edit it.

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

1. Use **Save as** to create a second estimate before changing the saved baseline. Keep the same model configuration.
2. In the **x1,000 tokens** quantity boxes, set **Input** to `660000`, **Cached Input** to `0`, and **Output** to `165000`. These represent 660 million input and 165 million output tokens.
3. Confirm the Azure OpenAI module shows **Input = $1,320**, **Output = $1,650**, and **Monthly cost = $2,970** at the reference rates. Check the model module, not the overall estimate total if supporting services are already included.
4. Select **Save** to preserve the second scenario.

### 4b. Confirm the worksheet values

| Metric | 3 calls per request | 6 calls per request |
|---|---|---|
| Monthly model calls | 165,000 | 330,000 |
| Input tokens | 330 million | 660 million |
| Output tokens | 82.5 million | 165 million |
| Model cost at illustrative rates | $1,485 | $2,970 |

Message for the audience: we added no users, but model cost doubled. The extra calls might be useful validation or unnecessary retries. Traces and quality measurements show which.

Recommend a range of scenarios: expected adoption, higher adoption, and more demanding agent behavior. Reassess supporting infrastructure and human review separately for each scenario.

If the quantity fields instead show 1-million-token units, enter `660` for Input and `165` for Output. Switch between the saved estimates to compare scenarios without adding both model configurations to the same estimate and summing them.

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
| Model monthly cost shows $0.66 | Input is 330 in x1,000-token units and Output is 0 | Set Input to `330000`, Cached Input to `0`, and Output to `82500` for the baseline |
| Model total is 1,000 times too small or too large | Quantities use 1K-token units but the worksheet uses millions, or the reverse | Divide monthly tokens by the unit size shown below each quantity field |
| Total differs from the slide | Live rate differs from the illustrative rate | Show the live total and the illustrative total side by side |
| Azure OpenAI product not found | Product naming changed | Check the current product picker and the Azure OpenAI pricing page; the supplied screenshots confirm the Azure OpenAI tile |
| Global Standard is not in the dropdown | Documentation and calculator labels differ | Select Pricing Strategy = Standard (On-Demand) and Deployment type = Global |
| Cached input is missing | Cached input is a separate meter | Enter cache hit assumptions only if measured |
| Provisioned estimate looks low | Provisioned bills on deployed PTUs, not tokens | Use the PTU hourly rate and reservation guidance, not token units |
| Reservation not reflected in the estimate | Reservations are purchased separately | Note the reservation as a separate commitment decision |
| Price differs from the pricing page | Currency conversion or negotiated program pricing | Confirm currency, licensing program, and the selected billing account/profile or agreement |

---

## Presenter checklist

- [ ] Products search shows Azure OpenAI and its Add to estimate button
- [ ] Region, Model type, Model, Pricing Strategy, and Deployment type match the intended scenario
- [ ] Unit labels are checked (x1,000 tokens in the supplied screenshots)
- [ ] Baseline quantities are Input `330000`, Cached Input `0`, and Output `82500`; model monthly cost is $1,485 at the reference rates
- [ ] Support, licensing program, billing account/profile or agreement, and USD currency are checked
- [ ] Worksheet includes Step 1 volumes, Step 3 operating cost, and Step 4 scenario
- [ ] Both scenarios are saved as separate estimates
- [ ] Illustrative assumptions are labeled as such on every slide
- [ ] The slide edits are applied: "the supporting Azure services available in the calculator" and the "Incremental operating cost" label
