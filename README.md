# SIH-2025-KMRL-Chakra

# If the documents have low resolution images, then using computer vision techniques, low resolution images would be converted to high resolution images. Some of those techniques are:
## Image Interpolation Techniques : 
1. Linear Interpolation

Linear interpolation is one of the simplest and fastest methods for resizing images. It calculates the pixel value by averaging the nearest neighbors in a straight line.

How it works: For each pixel, the new value is calculated by taking a weighted average of the closest pixels from the original image. This method is suitable for smooth resizing but may not provide the best quality for complex images.

Use case: Best used when speed is important and when working with simpler images (e.g., less complex graphics or basic photographs).


<img width="347" height="288" alt="image" src="https://github.com/user-attachments/assets/c16d4aea-9065-458f-aa82-62c89ec05a74" />

----

2. Cubic Interpolation

Cubic interpolation is more advanced and provides better quality than linear interpolation. It uses the closest 4x4 neighborhood of known pixels to estimate the pixel values. This method gives a smoother and more detailed output, making it suitable for high-quality image enlargements.

How it works: It fits a cubic polynomial to the pixel values in the neighborhood of the desired pixel, offering a higher degree of smoothness.

Use case: Ideal for enlarging images where more detail and sharpness are required, like photographs or images with gradients.

<img width="344" height="277" alt="image" src="https://github.com/user-attachments/assets/13511795-8549-4792-ba73-fc4f0179c6ac" />

----

3. Lanczos Interpolation

Lanczos interpolation is an advanced resampling method that uses a sinc function for interpolation, making it highly effective for image resizing, particularly for enlarging images. It uses a larger neighborhood of pixels (typically 8x8) to calculate the new pixel values, ensuring better preservation of details and sharpness.

How it works: It applies a sinc function to a weighted combination of pixels, making it especially useful for high-quality upscaling. It’s effective at preserving image sharpness while minimizing artifacts like aliasing.

Use case: Used for high-quality enlargements where detail preservation and sharpness are critical.


<img width="337" height="269" alt="image" src="https://github.com/user-attachments/assets/3659e36b-5519-4f1c-8d68-56aed5a2ef11" />


----

Image Denoising

In this project, Non-Local Means Denoising is used for reducing noise in images. This method is effective in preserving the edges while reducing noise in various types of images (e.g., photographs, medical images, etc.).

Non-Local Means Denoising

How it works: Non-local means denoising works by comparing all pixels in an image with each other. It estimates the denoised value of each pixel based on a weighted average of similar pixels within a specified window (search window). It preserves sharp details, making it ideal for images that have noise but require edge details to be maintained.

<img width="261" height="193" alt="image" src="https://github.com/user-attachments/assets/f5f4e51c-5d2a-450e-8efc-442e48acd59b" />


----


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
