# Finops

Demo materials and step-by-step guides for FinOps discussions about cloud costs, starting with Azure OpenAI cost estimation.

## Contents

| Path | Description |
| --- | --- |
| [Finops/AI/Azure-OpenAI-Pricing-Calculator-Demo-Guide.md](Finops/AI/Azure-OpenAI-Pricing-Calculator-Demo-Guide.md) | Step-by-step guide to estimate Azure OpenAI costs with the Azure pricing calculator, from workload assumptions to commitment planning. |

## Prerequisites

- A web browser.
- The [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator/). Sign in when saving an estimate or viewing negotiated pricing; negotiated prices also require selecting the applicable billing account/profile or agreement.
- The [Azure OpenAI pricing page](https://azure.microsoft.com/en-us/pricing/details/azure-openai/), open in a second tab to confirm live rates.

## Getting started

1. Open the [Azure OpenAI pricing calculator demo guide](Finops/AI/Azure-OpenAI-Pricing-Calculator-Demo-Guide.md).
2. Add **Azure OpenAI** to the estimate, then set **Region**, **Model type**, **Model**, **Pricing Strategy**, and **Deployment type** as shown in the guide. Confirm the live rates.
3. Follow the steps in order: workload assumptions, model consumption, supporting services and operating cost, scenario change, and value and commitment planning.

## Important notes

- Rates and volumes in the demo are illustrative. They are not price quotes.
- Prices, model lists, and product names change. Verify them before you use any figure.
- The supplied calculator screenshots use **x1,000 tokens**, not millions. For the baseline, enter **Input = 330000**, **Cached Input = 0**, and **Output = 82500**. At the reference rates, the model costs **$1,485/month**; entering Input = 330 and Output = 0 produces only **$0.66/month**.
- The calculator estimates Azure consumption only. Labor, licenses, and external tools are tracked separately.
- Pricing references and calculator field labels were last reviewed against documentation and supplied screenshots on 2026-10-11.

## Key references

- [Understanding deployment types in Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- [Estimate costs with the Azure pricing calculator](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/pricing-calculator)

## Contributing

Edit the Markdown files, then verify links and rates before you commit.
