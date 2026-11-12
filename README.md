# BESHO People — Live n8n AI Recruitment & RAG Automation

BESHO People is an end-to-end AI automation system built on self-hosted n8n that streamlines candidate intake, AI-powered CV screening, error monitoring, and document-grounded Q&A. The public web interface connects directly to live n8n workflows via a static ngrok tunnel, allowing real visitors to submit applications and query company policies in real time.

**Live Website:** `https://beshoysamehh.github.io/BESHO-People-HR-Automation/` *(update repository name if different)*

---

## Workflows & Triggers

| Workflow File | Trigger | Description |
| :--- | :--- | :--- |
| `recruitment-1-intake.json` | Form Trigger | Validates applicant fields and PDF CV files, uploads CVs to Google Drive, and logs records to Google Sheets. |
| `recruitment-2-ai-screening.json` | Schedule Trigger | Evaluates pending applications with an AI model and sends automated interview invites or decline emails. |
| `recruitment-3-error-handler.json` | Error Trigger | Captures execution errors across all workflows, logs failures to Google Sheets, and sends email alerts. |
| `rag-customer-support-chat.json` | Chat Trigger | Public RAG assistant that retrieves policy chunks from the vector store and answers with source citations. |
| `policy-loader.json` | Instance Start, Daily (7 AM), Manual | Loads Google Docs from Drive, chunks text, creates Gemini embeddings, and refreshes the in-memory vector store. |

---

## Services & Requirements

* **Self-Hosted n8n** (Docker Desktop)
* **ngrok** (Free static domain for public HTTPS webhook & form access)
* **Google Cloud OAuth2** (Google Sheets, Google Drive, and Gmail APIs)
* **Google Gemini API** (Embeddings and LLM reasoning)
* **Groq API** (Fast open-weight LLM inference for CV screening)

---

## How to Import & Run

1. **Start n8n with a public URL:** Configure `WEBHOOK_URL`, `N8N_HOST`, and `N8N_EDITOR_BASE_URL` in `docker-compose.yml` to match your static ngrok domain, then run `docker compose up -d` and start your ngrok tunnel.
2. **Import Workflows:** In n8n, create a project folder and import all 5 `.json` files via **Import from File**.
3. **Connect Credentials:** Create your Google OAuth2 (Sheets, Drive, Gmail), Google Gemini, and Groq credentials in n8n, then assign them to every node.
4. **Link Your Files:** Select your own Google Sheets (Applications & Error Log) and Google Drive folders (CVs & HR Policies) inside the Google nodes.
5. **Configure Triggers & Activate:** Set `recruitment-3-error-handler` as the Error Workflow in all workflow settings, add your GitHub Pages domain to `Allowed Origins (CORS)` in the Chat Trigger, activate all workflows, and run `Policy Loader` once to populate the vector store.
