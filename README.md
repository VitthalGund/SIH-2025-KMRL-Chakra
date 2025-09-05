# SIH-2025-KMRL-Chakra


### The Scenario

An employee from the **Maintenance department** and an employee from the **Finance department** ask the exact same question. The API request will include a `userId` which the system uses to identify the user's role and tailor the response.

-----

## 🔌 API Request

The frontend application would send a `POST` request to an endpoint like `/api/query`. The body of the request includes the user's question and their unique identifier.

**`POST /api/query`**

```json
{
  "query": "What's the status of the new Aluva line signaling system?",
  "userId": "ENG_123" // The ID of the Maintenance Engineer
}
```

*(If the user was from finance, the ID might be "FIN\_456")*

-----

## ✅ API Response (200 OK)

The backend processes the query, identifies the user's role as "Maintenance Engineer," retrieves the relevant documents, and generates a tailored response.

### Response for the Maintenance Engineer 👷

```json
{
  // The direct, synthesized answer to the question
  "answer": "The new signaling system on the Aluva line, model 'AX-500', is 95% installed and currently in the final testing phase. One minor overheating issue was reported and resolved last week. Full operational handover is scheduled for October 15, 2025.",

  // The USP: A summary tailored specifically to the user's role
  "summary": {
    "role": "Maintenance Engineer",
    "content": "Focus on technical specs: The AX-500 system's installation is near completion. A minor overheating issue in relay unit SR-7 was logged on Aug 29, 2025, and resolved via firmware patch v1.2.1. The attached maintenance SOPs have been updated. Be prepared for full diagnostic runs starting the week of October 1st."
  },

  // The primary source documents used to generate the answer
  "sources": [
    {
      "documentId": "doc_789123",
      "title": "Weekly Project Report - Aluva Signaling System - Sep 1, 2025.pdf",
      "url": "/documents/doc_789123",
      "relevanceScore": 0.94
    },
    {
      "documentId": "doc_456001",
      "title": "AX-500 Signaling System - Technical & Maintenance Manual v2.pdf",
      "url": "/documents/doc_456001",
      "relevanceScore": 0.89
    }
  ],

  // The cross-referencing feature: linking to other relevant documents
  "relatedDocuments": [
    {
      "documentId": "inc_112233",
      "title": "Incident Report - SR-7 Relay Overheating - Aug 29, 2025.docx",
      "url": "/documents/inc_112233"
    }
  ],
  
  "metadata": {
    "queryId": "qid_xyz_987",
    "timestamp": "2025-09-05T18:47:55Z",
    "processingTime": 2.8 // Time in seconds
  }
}
```

### How the Response Would Change for Finance 💰

If the same request was made by a user with ID `"FIN_456"`, the `answer` would be similar, but the **`summary`** object would be completely different:

```json
  "summary": {
    "role": "Finance Manager",
    "content": "Focus on financial implications: The project is on-track and 98% of the budget has been utilized. The final invoice from 'SignalPro Inc.' is pending project completion. The reported overheating incident was resolved under warranty with no additional cost to KMRL. A budget report for Q4 maintenance is attached."
  }
```

This API structure effectively delivers on all your key features: a direct LLM-powered answer, verifiable sources, automated cross-referencing, and most importantly, the hyper-personalized summaries that make your solution unique.
