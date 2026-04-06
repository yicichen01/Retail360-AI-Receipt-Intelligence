# System Architecture

## Overview

Retail360 was designed as a layered, modular architecture that separates user interaction, workflow orchestration, cognitive processing, structured storage, and analytics. This separation makes the system easier to understand, easier to extend, and more realistic as a product MVP. The architecture also reflects the core product goal: transforming unstructured receipt images into structured insights that support both customer-facing features and internal business decision-making. :contentReference[oaicite:38]{index=38}

The full system consists of five major layers:

- User Interaction Layer
- Workflow and Integration Layer
- AI and Cognitive Processing Layer
- Data Storage Layer
- Analytics and Insight Layer :contentReference[oaicite:39]{index=39}

## 1. User Interaction Layer

### Power Apps

Power Apps serves as the front-end interface for the MVP. It provides the screens through which users upload or capture receipt images, review dashboard outputs, and trigger AI-generated summaries. This choice supported rapid prototyping without the cost and complexity of building a custom front-end from scratch. It also aligned well with the Microsoft-centered architecture because Power Apps integrates naturally with Power Automate, Dataverse, and embedded Power BI content. :contentReference[oaicite:40]{index=40} :contentReference[oaicite:41]{index=41}

From an architecture standpoint, Power Apps acts as the presentation layer. It does not perform the heavy data processing itself. Instead, it captures input, triggers flows, and displays outputs returned by other components.

## 2. Workflow and Integration Layer

### Power Automate

Power Automate functions as the orchestration backbone of the system. Once a receipt is uploaded through Power Apps, Power Automate handles the downstream sequence of actions:

- receiving the file
- sending it to OCR
- parsing returned JSON
- normalizing data fields
- writing structured records into Dataverse :contentReference[oaicite:42]{index=42} :contentReference[oaicite:43]{index=43}

This layer is important because it turns a collection of services into a coherent workflow. Rather than embedding logic separately into every component, the system centralizes integration and transformation logic in one place. That design lowers implementation overhead for an MVP and supports future extensibility.

## 3. AI and Cognitive Processing Layer

### Azure AI Document Intelligence

Azure AI Document Intelligence, accessed through AI Builder’s receipt-processing capability, is the key cognitive component in the MVP. It extracts structured information from receipt images, including merchant names, dates, totals, and line items. This transforms unstructured images into machine-usable data. :contentReference[oaicite:44]{index=44}

This component is essential because the rest of the architecture depends on having OCR output that is structured enough to normalize into a relational schema.

### ChatGPT summary workaround

Because the student environment did not support Azure OpenAI Service, the MVP used a workaround in which Power Apps serialized receipt data into JSON and opened ChatGPT with a prompt containing that data. This preserved the intended functionality of natural-language spending summaries, while clearly identifying a path for future replacement with embedded Azure OpenAI. :contentReference[oaicite:45]{index=45}

### Planned but not implemented AI modules

The full design also envisioned additional AI services:

- Azure ML Designer for purchase-cycle prediction
- Azure OpenAI for integrated in-app financial summaries :contentReference[oaicite:46]{index=46}

These were not part of the MVP due to data and subscription constraints, but they remain important because they show how the architecture was designed with future extensibility in mind.

## 4. Data Storage Layer

### Microsoft Dataverse

Dataverse serves as the structured storage layer for the MVP. It was selected because it offers a relational schema, built-in compatibility with Power Platform services, and governance features such as validation, auditing, and access control. Within Retail360, Dataverse stores both receipt-level metadata and item-level detail, allowing the system to preserve parent-child relationships between receipt headers and line items. :contentReference[oaicite:47]{index=47}

This architectural choice is important for several reasons:

- it enables downstream dashboarding without a separate ETL stack
- it provides a clean structure for linking transactions and items
- it supports future AI tasks that depend on well-modeled data
- it keeps the MVP low-code while still maintaining relational integrity

## 5. Analytics and Insight Layer

### Power BI

Power BI functions as the analytics layer in the architecture. It connects directly to Dataverse and supports dashboard creation for category breakdowns, merchant trends, and spending patterns. In the MVP, dashboards could be embedded inside Power Apps, preserving a consistent user experience while making the data more interpretable. :contentReference[oaicite:48]{index=48} :contentReference[oaicite:49]{index=49}

Power BI is significant because it closes the loop between ingestion and insight. The architecture does not stop at extracting data. It also turns that data into visuals and patterns that support action.

## End-to-End Workflow

At a high level, the system flow is:

1. Power Apps captures receipt input
2. Power Automate receives the file and starts the flow
3. Azure AI Document Intelligence extracts structured receipt fields
4. Power Automate parses and normalizes the output
5. Dataverse stores receipt headers and line items
6. Power BI refreshes dashboards and visual outputs
7. ChatGPT generates natural-language summaries from serialized data :contentReference[oaicite:50]{index=50} :contentReference[oaicite:51]{index=51}

This flow is one of the strongest aspects of the project because it demonstrates a true end-to-end data and AI product pipeline.

## Why the Architecture Is Strong

This architecture is strong for a portfolio project because it demonstrates:

- modular design across clearly defined layers
- thoughtful division of responsibility between services
- real-world integration of AI into a business workflow
- practical MVP prioritization under platform limitations
- a clear upgrade path for future intelligence features

Instead of presenting AI as an isolated model, Retail360 presents AI as one component in a larger system that also includes ingestion, transformation, storage, analytics, and interface design. That is the kind of system thinking that makes the project valuable to recruiters hiring for data, analytics, AI implementation, or product-oriented technical roles.
