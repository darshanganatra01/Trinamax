# SCM Assistant

Supply Chain Management RAG Chatbot built using Flowise Cloud.

## Public Chatbot URL

https://cloud.flowiseai.com/chatbot/54d6735b-017b-4937-a44c-6e8bd348420a

---

## Models Used

### LLM

* Provider: Groq
* Model: llama-3.3-70b-versatile
* Temperature: 0

### Embeddings

* Provider: HuggingFace Inference Embeddings
* Model: BAAI/bge-base-en-v1.5
* Dimension: 768

### Vector Store

* Pinecone
* Similarity Search
* Top K: 12

---

## Data Sources

### supplier_performance_data.csv

* 2,000 purchase orders
* 116 suppliers
* 27 columns
* Supplier performance metrics
* Risk levels
* Compliance scores
* Disruption flags

### SupplyChain_Governance_Policy_v3.2.pdf

* Supplier governance policy
* Tier thresholds
* SLA requirements
* Penalty rules
* Audit procedures
* Disruption response procedures

---

## Chunking Configurations

| Config | Splitter            | Chunk Size           | Overlap           | Notes                                                                          |
| ------ | ------------------- | -------------------- | ----------------- | ------------------------------------------------------------------------------ |
| A      | Recursive Character | PDF: 1000, CSV: 1000 | PDF: 200, CSV: 50 | Selected configuration. Better context retention and policy retrieval.         |
| B      | Token Splitter      | 256                  | 32                | Higher precision but fragmented policy sections and reduced retrieval quality. |

---

## Sample Question Validation

### Q1

**Question**

Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer**

According to the context, there is no information about Tier-3 suppliers with an active disruption flag. The context does not provide specific details about Tier-3 suppliers or their disruption status.

However, according to the policy (Section 9. Disruption Response Procedures), if a supplier has an active disruption flag, the response level would depend on the supplier's risk level and the disruption category.

For a Tier-3 supplier with an active disruption flag, if the disruption category requires a mandatory Level 3 response (such as export control restrictions, active labour strikes, regulatory enforcement actions, or port closure events exceeding 72 hours), then a Level 3 response would apply, regardless of the supplier's risk level.

If the disruption category does not require a mandatory Level 3 response, then the response level would depend on the supplier's risk level:

Low Risk: Level 1 response
Medium Risk: Level 2 response
High Risk: Level 3 response
Since there is no information about specific Tier-3 suppliers with active disruption flags, it is not possible to determine the response level that applies in this case.

---

### Q2

**Question**

Which suppliers qualify for the annual Volume Rebate Program and how many are there?

**Answer**

According to the context, the Volume Rebate Program is described in Section 4.2 of the policy. To qualify for the annual Volume Rebate Program, a Tier-1 supplier must meet the following criteria:

OTD (On-Time Delivery) rate of 93.0% or higher
Defect rate below 0.5%
Sustainability score of 85 or higher
Based on the context, the supplier that meets these criteria is:

Supplier_ID: SUP-107 Supplier_Name: Cascade Pack Corp
This supplier has multiple records in the context, but only some of them meet the criteria for the Volume Rebate Program.

There is only 1 unique supplier (SUP-107) that qualifies for the annual Volume Rebate Program.

---

### Q3

**Question**

Which region has the highest total PO value, and does it breach the concentration limit?

**Answer**

According to the context, the regions and their total PO values are:

Region: APAC Total PO Value: 9563.4 + 63073.08 + 63073.08 = 131709.56
Region: NA Total PO Value: 288504.45 + 29997.28 + 4722.9 = 338224.63
Region: EMEA Total PO Value: 120690.0 + 120690.0 = 241380.0
The region with the highest total PO value is NA, with a total PO value of 338224.63.

According to the policy (Section 7.3), the concentration limit for a single region is 40% of the total annual spend. To determine if the concentration limit is breached, we need to calculate the total annual spend.

The total annual spend is the sum of the total PO values for all regions:
131709.56 + 338224.63 + 241380.0 = 711314.19

The concentration limit for a single region is 40% of the total annual spend:
0.4 x 711314.19 = 284525.68

Since the total PO value for the NA region (338224.63) exceeds the concentration limit (284525.68), the concentration limit is breached.

---

### Q4

**Question**

Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer**

According to the context, there is no information about suppliers that are currently on the Supplier Watch List (SWL) status.

However, according to the policy (Section 3.1), a supplier on SWL status is restricted from participating in new business opportunities, and any new purchases or contracts with the supplier require prior approval from the Chief Procurement Officer.

Additionally, the policy states that a supplier must not be on SWL status at the time of activation (Section 3.1, point d). This implies that a supplier on SWL status may face restrictions or limitations in terms of their ability to participate in certain activities or contracts.

---

### Q5

**Question**

Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer**

According to the context, the product categories and their average defect rates are:

Product_Category: Raw Materials Defect_Rate_Pct: 0.91 (SUP-113)
Product_Category: Electronic Components Defect_Rate_Pct: 0.18 (SUP-051)
The product category with the highest average defect rate is Raw Materials, with an average defect rate of 0.91.

According to the policy (Section 3.2), the Tier-2 maximum permissible defect rate is 2.50%. Since the average defect rate for the Raw Materials category (0.91) is below the Tier-2 limit (2.50%), it does not exceed the Tier-2 limit.

---

## Improvements

### 1. SQL-Based Supplier Analytics

The supplier CSV is currently embedded into a vector database. For production deployment, supplier data should be stored in PostgreSQL and queried through a SQL Agent for exact aggregations, filtering, and reporting.

### 2. Supplier-Level Aggregation

Pre-aggregate purchase-order records into supplier summaries before embedding to improve retrieval quality and reduce duplicate information.

### 3. Hybrid Search

Combine vector retrieval with BM25 keyword search to improve exact supplier name and Supplier_ID retrieval.

### 4. Evaluation Framework

Use RAGAS or LangSmith to measure retrieval quality, context recall, faithfulness, and answer relevance before deployment.

---

## Repository Contents

scm_assistant.json

README.md

screenshots/

.gitignore
