# Results, Evaluation, and Limitations

## Overview

Retail360 was evaluated as an MVP rather than a production deployment. The goal of evaluation was to determine whether the system could reliably convert diverse real-world receipts into structured records, useful dashboards, and coherent natural-language summaries. Results were interpreted in the context of the project’s intended scope and acknowledged platform constraints.

## Dataset and Evaluation Context

The project paper reports that the solution was evaluated using more than 200 real-world receipts from a range of retail environments, including grocery, general merchandise, e-commerce, and food and beverage. The dataset was intentionally diverse in merchant type and receipt quality, including both thermal and non-thermal formats. This mattered because OCR systems often behave very differently under ideal versus realistic image conditions.

The presentation also frames the MVP as limited in scope and not yet stress-tested for large-scale ingestion. That is an important qualifier for interpreting results.

## OCR and Extraction Results

Because the environment did not support formal benchmarking infrastructure, extraction quality was evaluated through structured manual review. According to the paper, key field-level performance was strong on high-value receipt attributes such as:

- merchant name
- transaction date
- quantities
- unit prices
- line totals

Line-item names were more variable, which is consistent with the difficulty of parsing abbreviations, noisy formatting, and inconsistent receipt layouts. This result is realistic and important. It shows that the system performed well on the fields most critical for transaction reconstruction, while still leaving room for improvement in fine-grained item interpretation.

## Data Normalization and Storage Results

A major success of the MVP was not only extraction, but structured normalization. The paper reports that:

- receipts were mapped into a Dataverse relational schema
- receipt and line-item relationships were maintained
- numeric values were converted into machine-readable formats
- date fields were normalized consistently

This is a key result because many OCR demos stop at extraction. Retail360 went further by producing data that could actually support analytics and downstream workflow logic.

## Dashboard Results

The Power BI layer successfully generated dashboards that surfaced:

- category-level spending patterns
- merchant-level trends
- monthly or time-based spending views

These dashboards matter because they prove the system created not just structured records, but usable business outputs. The results indicate that the storage and analytics layers were sufficiently coherent to support decision-oriented visualization.

## AI Summary Results

The ChatGPT-based summary workaround produced coherent natural-language outputs when provided with structured receipt JSON. According to the paper, the summaries captured patterns such as:

- total spending
- category-level distribution
- frequently visited merchants
- month-to-month behavior shifts
- simple budgeting suggestions

This is significant because it shows that even without a fully embedded enterprise LLM service, the MVP could still demonstrate a practical AI assistant experience tied to actual user data.

## Business Impact and ROI Framing

The project included a modeled business case built around the premise that better visibility into cross-retailer behavior can reduce marketing waste, improve retention, and increase customer lifetime value. The paper estimates substantial upside relative to annual operating cost, with an annual ROI above 800% under the project’s assumptions.

Just as importantly, the paper clearly notes that these are modeled outcomes based on assumptions and benchmarks, and that a real pilot would be needed to validate adoption rates, financial uplift, and operational cost under production usage. That caveat improves the credibility of the project because it shows responsible interpretation rather than overclaiming.

## Strengths of the MVP

Several strengths stand out from the project’s results:

### 1. End-to-end coherence

The system successfully connected input, extraction, normalization, storage, dashboarding, and AI summarization into one functioning workflow.

### 2. Strong practical value

The outputs are not purely technical. They connect to real use cases in targeting, customer understanding, and spending visibility.

### 3. Good MVP scoping

The project delivered the highest-value core loop first instead of trying to force advanced AI features without the necessary data or platform support.

### 4. Realistic evaluation framing

I acknowledged what was implemented, what remained manual, and what still required validation at greater scale.

## Limitations

The presentation identifies several important MVP limitations:

- Azure OpenAI and Azure ML Designer were unavailable in the student environment
- OCR performance depended on image quality and receipt condition
- the dataset and scope were still limited
- privacy controls remained partial, especially because the ChatGPT summary flow sent data outside the platform

These limitations matter because they shape how the project should be interpreted. Retail360 demonstrates strong MVP-level systems thinking, but it is not presented as a production-grade enterprise deployment.

## What I Would Improve Next

If extended beyond the MVP, the next priorities would be:

1. replace the external summarization workaround with embedded Azure OpenAI
2. test ingestion reliability at larger scale
3. expand privacy and compliance controls, especially around data minimization and external AI calls
4. explore purchase-cycle prediction once historical depth supports a credible model

These improvements would move the system from a strong prototype toward a more scalable intelligent retail platform.
