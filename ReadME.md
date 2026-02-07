
# 🎓 AI University Finance Assistant

A professional, AI-powered chatbot designed to assist university students in retrieving their official payment receipts and finance status. The system uses **n8n** for backend logic, **Groq AI** for natural language generation, and a custom **HTML/JS** frontend hosted on AWS.

## 🚀 Features

* **Secure Student Verification:** Implements "Exact Match" logic to prevent data leaks and unauthorized access to student records.
* **AI-Generated Receipts:** Converts raw CSV data into professional, formatted payment receipts using Large Language Models (LLM).
* **PDF Download:** Users can download their generated receipt as a PDF directly from the chat interface.
* **Full-Screen Web Widget:** A responsive, mobile-friendly chat interface that mimics modern apps like ChatGPT.
* **Serverless Architecture:** Hosted on AWS S3 & CloudFront for high availability and low cost.

## 📂 Project Structure

```text
├── index.html              # The frontend chat interface (upload to AWS S3)
├── workflow.json           # The complete n8n backend workflow
├── payments.csv            # Sample dataset of 5,000 students (for testing)
└── README.md               # Project documentation

```

## 🛠️ Prerequisites

* **n8n:** A self-hosted instance or n8n Cloud account.
* **Groq API Key:** For the LLM (or you can swap this for OpenAI/Anthropic in n8n).
* **AWS Account:** Access to S3 and CloudFront for hosting the frontend.

## ⚙️ Setup Guide

### 1. n8n Workflow Setup

1. **Import Workflow:**
* Open your n8n dashboard.
* Click "Add Workflow" > "Import from File".
* Select the `workflow.json` file from this repository.


2. **Configure Nodes:**
* **Get CSV from S3:** Update the credentials to point to your own S3 bucket where the `payments.csv` is stored.
* **Groq Model:** Enter your Groq API Key in the credentials section.
* **Chat Trigger:**
* Open the "Chat Trigger" node.
* Switch to the **Production** tab.
* **Copy the Webhook URL**. You will need this for the HTML file.





### 2. CSV Data Setup

1. Upload the `payments.csv` file to your AWS S3 bucket (or any public URL accessible by n8n).
2. Ensure your n8n "Get CSV" node is pointing to this file.
* *Note: The CSV includes columns for Student ID, Name, Program, Balance, etc.*



### 3. Frontend Hosting (AWS S3 + CloudFront)

1. **Edit `index.html`:**
* Open `index.html` in a text editor.
* Find the line: `webhookUrl: 'YOUR_WEBHOOK_URL_HERE'`
* Replace `'YOUR_WEBHOOK_URL_HERE'` with the **Production URL** you copied from n8n.


2. **Upload to AWS S3:**
* Create a public S3 bucket.
* Upload `index.html`.
* Enable "Static Website Hosting" in the bucket properties.


3. **Deploy via CloudFront (Optional but Recommended):**
* Create a CloudFront distribution pointing to your S3 bucket.
* This provides a secure `https://` domain (e.g., `d1234.cloudfront.net`) which avoids browser security warnings.



## 🧪 Testing

To verify the system is working correctly and securely, try these test cases:

| Test Case | Input | Expected Result |
| --- | --- | --- |
| **Valid Student** | `20275596` | Returns a receipt for "Laila Hadi Samir Al-Rashid". |
| **Invalid/Partial ID** | `2027559` | Returns an apology: "Student ID not found." (Security Check). |
| **Wrong ID** | `99999999` | Returns an apology: "Student ID not found." |

## 🔒 Security Note

This project implements **Strict Equality Matching** (`===`) in the backend logic. This ensures that a user typing a partial ID (e.g., "2021") does **not** receive data for a student with a similar ID (e.g., "20218314").

## 📄 License


This project is open-source and available under the MIT License.
