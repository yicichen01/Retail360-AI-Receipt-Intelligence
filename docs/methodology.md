# Methodology and Technical Approach

## Overview

Retail360 was built as an MVP, so the methodology emphasized practicality, integration, and system reliability over model complexity. The technical approach focused on converting real-world receipt images into structured, analysis-ready records that could support both dashboards and natural-language summaries. The design intentionally balanced speed of implementation with architectural clarity and future extensibility.

## Design Principles

Several design principles shaped the implementation:

### 1. Start with the highest-value data bottleneck

The most important technical bottleneck in the business problem was the absence of structured cross-retailer purchase data. Receipt OCR and structured ingestion were therefore treated as the foundation of the system. If receipt data could not be extracted and stored reliably, later features such as dashboards, categorization, or AI summaries would not matter.

### 2. Prefer integrated services for MVP speed

The project prioritized tools that could be connected quickly and maintained with lower engineering overhead. This is why the MVP relied heavily on Power Platform components instead of custom-coded alternatives. For example, Power Automate was chosen over Azure Functions for orchestration because it required less setup effort and fit the prototyping goal better.

### 3. Build for future extension even if features are deferred

Although the MVP did not include all planned AI features, the system was designed so that new components could be added later. The Dataverse schema, service boundaries, and workflow structure all support future insertion points for classification, prediction, and deeper summarization capabilities.

## Core Technical Workflow

### Receipt ingestion

Users upload or capture receipt images in Power Apps. This interaction serves as the trigger for the rest of the system. The design keeps the front-end lightweight and focuses the complexity in the integration and processing layers.

### OCR extraction

The uploaded receipt is passed into Azure AI Document Intelligence through the Power Platform integration path. The receipt model extracts key fields such as merchant information, transaction dates, quantities, prices, totals, and line-item descriptions. This is the system’s key unstructured-to-structured conversion step.

### JSON parsing and normalization

Once OCR output is received, Power Automate parses the JSON and transforms it into a normalized schema. This includes:

- converting dates into standardized formats
- cleaning OCR noise such as stray spaces and formatting inconsistencies
- converting numeric strings into machine-readable numeric values
- generating IDs for records
- preserving parent-child structure between receipt headers and line items

This step is methodologically important because the value of OCR is limited unless the output becomes clean, structured, and analytics-ready.

### Structured storage

Normalized data is written into Dataverse. The schema separates higher-level receipt records from detailed line-item records, allowing the system to support both transaction-level and item-level analysis. This also makes it possible to join, aggregate, and visualize the data downstream.

### Dashboard generation

Power BI connects directly to Dataverse and provides visual analytics around totals, categories, spending trends, and merchant-level behavior. Because the storage layer is structured, dashboard logic can remain relatively straightforward while still producing useful outputs.

### Natural-language insight generation

To add an AI assistant style experience within MVP constraints, the project serialized receipt-level data into JSON and used a ChatGPT-based workaround to generate plain-language insights. This preserved the intended feature logic while working around unavailable Azure OpenAI access.

## Methods by Component

### Document AI and extraction

The extraction method relies on the pretrained receipt model in Azure AI Document Intelligence. This model combines OCR, layout understanding, and line-item extraction to interpret semi-structured retail receipts. The decision to use a pretrained service instead of building a custom OCR pipeline reduced implementation complexity and accelerated prototyping.

### Data transformation and normalization

Power Automate handled transformation logic rather than pushing raw OCR output straight into storage. This was an important architectural and methodological decision because raw OCR output often contains inconsistencies that make downstream analytics fragile. The normalization layer was therefore treated as a first-class component, not a minor cleanup step.

### Categorization logic

Automated categorization was designed but not fully implemented. The intended approach involved multi-class classification of item descriptions into categories such as food, household, or health, using Azure Text Analytics. Because the available labeled dataset was too limited for a credible MVP, categories were assigned manually instead.

This is actually a strength in how the project was scoped. It shows the ability to distinguish between what is theoretically attractive and what is responsible to ship under current data constraints.

### Predictive modeling

The longer-term design included purchase-cycle prediction using Azure ML Designer and regression-based methods. Planned features included inter-purchase interval engineering, merchant-specific buying patterns, and seasonality features. These were not implemented in the MVP because of limited historical data and subscription limits.

### Generative AI

The MVP’s natural-language summary functionality used a workaround rather than embedded enterprise-grade LLM integration. Even so, the system preserved a practical generative AI use case: turning structured purchase history into readable, user-facing summaries. That is significant because it shows AI being used as part of a workflow rather than as an isolated demo.

## Reliability and Error Handling

The system incorporated several reliability-oriented design choices:

- retry logic in Power Automate for transient issues
- null checks before database writes
- duplicate detection through GUID logic
- modular separation between ingestion, extraction, storage, and analytics

These mechanisms matter because real receipt processing is messy. A robust MVP has to account for service interruptions, OCR imperfections, and incomplete data.

## Non-Functional Considerations

The presentation also identifies key non-functional requirements that shaped the methodology:

- performance: key screens should load quickly and receipt processing should be asynchronous
- security and privacy: storage and transit should be protected, and data sharing should be user-triggered
- scalability: ingestion should support growing receipt volume
- reliability and maintainability: components should be modular and independently updatable
- usability: the upload experience should remain simple and mobile-friendly 

These considerations strengthened the project by ensuring it was not only technically functional, but also designed with realistic system qualities in mind.

