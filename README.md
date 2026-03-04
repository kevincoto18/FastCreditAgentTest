# FastCredit: Agentic AI Loan Underwriter (n8n)

This repository contains an automated system designed to process loan applications from email intake to final decisioning. The agent uses **RAG (Retrieval-Augmented Generation)** to ensure all decisions align with internal company policies stored in a **Qdrant** vector database.

You can watch the full walkthrough and technical explanation here:
**[Link to Video: FastCredit Demo](https://youtu.be/1Jr_Fqx5ONA?si=iltn6kOm__3JU88- )**

> **Quick Access:** If you want to skip the technical setup and go directly to the **3 live examples** (Approved, Flagged, and Missing Data), please jump to **[3:34](https://youtu.be/1Jr_Fqx5ONA?si=FrvcgTLH9H9AaBSM&t=214)**.

## 🛠️ The Architecture

The system is divided into two main workflows to ensure scalability and clean data management:

1.  **Ingestion Pipeline:** A form-based workflow that extracts text from policy PDFs, chunks it using a recursive character splitter, and stores embeddings in a Qdrant collection.
2.  **Main Automated Agent:** The core engine that processes incoming emails and coordinates the following actions:
    * **Intake:** Triggers via Gmail on new messages with PDF attachments that follow the subject Loan Application or Credit Request.
    * **Data Extraction:** AI extracts structured JSON from "messy" PDF application forms.
    * **RAG Validation:** The agent queries the `policy_retriever` tool to fetch real-time lending rules.
    * **Decision Matrix:**
        * ✅ **Approved:** Automatically sends an HTML confirmation email and logs the record.
        * ⚠️ **Flagged:** Creates a task in **ClickUp** for human intervention and notifies the user.
        * ❌ **Missing Data:** The AI identifies specific missing fields and requests them from the applicant.



## 🧠 AI Decision Logic (RAG Implementation)

The Agent doesn't "guess" the rules. It consults a knowledge base for:
* **Age Verification:** (Min 18 years).
* **Employment Stability:** (Min 12 months in current job).
* **Debt-to-Income Ratio:** (Loan must be ≤ 3x monthly income).
* **Risk Flags:** Automated flagging for loans over $15,000 or existing debts.

## 🔑 Required Credentials

To run these workflows, you will need:
* **OpenAI API Key:** For embeddings (`text-embedding-3-small`) and the LLM (`gpt-4.1-mini`).
* **Qdrant Cloud/Local:** (Host & API Key) For the vector store.
* **Google Cloud Console (OAuth2):** For Gmail, Drive, and Sheets integrations.
* **ClickUp API:** (Personal Access Token) for human-in-the-loop task creation.

## 📂 Project Structure

* `FastCredit_Policy_Ingestion.json`: Workflow to populate the knowledge base.
* `FastCredit_Main_Workflow.json`: The core agentic logic and integrations.
* `/samples`: PDF samples for testing (Approved, Flagged, Incomplete cases) and internal policies.

---
*Developed for the Agentic AI Engineer Test Task.*