# SCM Assistant - Supply Chain Governance Chatbot

## Project Overview

This project implements a Retrieval-Augmented Generation (RAG) chatbot using Flowise Cloud for supply chain governance and supplier performance analysis.

The chatbot combines:
   Supplier Performance Dataset (2,000 purchase orders, 116 suppliers)
   Supply Chain Governance Policy Document

The solution allows users to ask natural language questions about supplier performance, risk management, governance policies, compliance requirements, disruption handling, and procurement controls.

## Public Chatbot URL

https://cloud.flowiseai.com/chatbot/dd4cfe94-a6c4-4ec3-b0ab-985d5bfa2f3e

## Technology Stack

| Component | Technology Used |
|------------|----------------|
| Platform | Flowise Cloud |
| LLM | cerebras GPT-OSS-20B |
| Embedding Model | BAAI/bge-base-en-v1.5(Huggingface)| 
| Vector Database | Qdrant |
| Memory | Buffer Window Memory |
| Retrieval Method | Vector Similarity Search |
| Architecture | RAG + Tool Agent |

## Architecture

User Query -> Tool Agent -> policy_search / supplier_search -> Qdrant -> Groq -> Response

## Chunking Experiments

### Configuration 1
- Chunk Size: 2000
- Chunk Overlap: 200

### Configuration 2 (Final)
- Chunk Size: 1000
- Chunk Overlap: 50

## Sample Question Testing

Add all 5 questions and answers here.
Hi there! How can I help?

Me
Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

AI



Tier‑3 suppliers with an active disruption flag

Supplier ID	Active disruption type(s)	Source
SUP‑067	Political instability flag (Q3-2024)	scm---csv – record for SUP‑067 with “Active_Disruptions: Political instability flag”
SUP‑038	Political instability flag (Q3-2024)	scm---csv – record for SUP‑038 with “Active_Disruptions: Political instability flag”
SUP‑081	Political instability flag (Q4-2024)	scm---csv – record for SUP‑081 with “Active_Disruptions: Political instability flag”
Response level that applies (per policy)

The BQBYTE Disruption Response Procedures state:

“Disruption categories requiring mandatory Level 3 response regardless of risk level: export control restrictions, active disruption flags, and historical performance volatility.”

Level 3 – Activate: Immediate escalation to CPO, alternate supplier activated for a minimum of 40 % of volume within 10 business days, safety stock adjusted by +50 %, full root‑cause analysis required within 15 business days.

Because each of the above suppliers has an active disruption flag, they fall under the mandatory Level 3 (Activate) response, irrespective of their individual risk rating.

Summary

SUP‑067, SUP‑038, SUP‑081 are Tier‑3 suppliers with an active disruption flag.
The policy mandates a Level 3 – Activate response for each of them, which includes immediate escalation to the Chief Procurement Officer, activation of an alternate supplier for at least 40 % of the volume within 10 business days, a 50 % safety‑stock increase, and a full root‑cause analysis within 15 business days.
Me
Which suppliers qualify for the annual Volume Rebate Program and how many are there?

AI



Annual Volume Rebate Program – Qualified Suppliers

Supplier ID	Supplier Name	Contract Tier	OTD %	Defect %	Sustainability Score	Source
SUP‑064	Ostwind Composites AG	Tier‑1	94.0 % (PO‑10412)	0.40 % (PO‑10412)	95 % (PO‑10412)	scm---csv – PO‑10412 record
Why only this supplier qualifies

Criterion	Policy Requirement	Supplier Performance
Tier	Must be Tier‑1	SUP‑064 is Tier‑1 (policy §2.1)
On‑Time Delivery (OTD)	≥ 93 % for the full calendar year	94.0 % (PO‑10412)
Defect Rate	< 0.5 %	0.40 % (PO‑10412)
Sustainability Score	≥ 85 %	95 % (PO‑10412)
All other Tier‑1 suppliers in the dataset (e.g., SUP‑055, SUP‑059) either fall short on OTD, defect rate, or sustainability score, so they do not meet the rebate eligibility criteria.

Number of qualified suppliers

1 supplier (SUP‑064 – Ostwind Composites AG) qualifies for the annual volume rebate program.
Note: The rebate is calculated as 2.5 % of the total annual invoice value for each qualifying supplier and is assessed in January of the following year (policy §4.2).

Me
Which region has the highest total PO value, and does it breach the concentration limit?

AI



Region with the highest total PO value

Region	Total PO Value (USD)
APAC	$408,938.19
EMEA	$211,632.57
Source: Supplier‑PO records (PO‑10002, PO‑10001, PO‑10273 for APAC; PO‑11003 for EMEA) – scm---csv.

Concentration‑limit check

Policy rule (Section 5.3, Concentration Risk Rule):

“No single region (APAC, EMEA, LATAM, NA) may account for more than 45 % of total annual procurement spend.” – scm__policy.

Total annual spend (sum of all regions in the dataset) = $408,938.19 (APAC) + $211,632.57 (EMEA) = $620,570.76.

45 % of total spend = 0.45 × $620,570.76 = $279,256.84.

APAC spend ($408,938.19) exceeds the 45 % threshold.

Conclusion

APAC is the region with the highest total PO value.
It breaches the concentration limit set by policy, requiring the Procurement team to submit a Diversification Plan within 60 days.
Me
Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

AI



Product category with the highest average defect rate

Product Category	Average Defect Rate (calculated from available PO records)
Mechanical Components	1.67 %
Packaging Materials	1.01 %
Calculation – The dataset contains four purchase‑order records:

PO	Category	Defect Rate
PO‑10805	Mechanical Components	1.73 %
PO‑10386	Mechanical Components	0.13 %
PO‑11581	Packaging Materials	1.01 %
PO‑10312	Mechanical Components	4.14 %
Average for Mechanical Components = (1.73 + 0.13 + 4.14) / 3 = 1.67 %.

Average for Packaging Materials = 1.01 %.

Does it exceed the Tier‑2 defect‑rate limit?

Policy rule (Section 3.2, Defect Rate):

“Tier‑2 maximum defect rate = 2.50 %.” – scm__policy.

Result – 1.67 % < 2.50 %, so the average defect rate for Mechanical Components does not exceed the Tier‑2 limit.

Bottom line

The Mechanical Components category has the highest average defect rate (≈ 1.67 %).
This rate is within the Tier‑2 permissible limit of 2.50 %.
	

## Screenshots

Include:
 Document Store
 Embeddings
 Qdrant
 Chatflow
 Public Chatbot
 

## Challenges Faced

- Large CSV dataset retrieval and ingestion 
- Tool routing accuracy

## Future Improvements

- SQL based retrival for analytical queries 


## Author

Rahul Gurbani
