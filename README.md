# Real Estate Lead Capture & Instant Agent Alert (n8n)

An n8n automation that captures real estate leads, cleans the data, prevents
duplicates, and alerts the agent within seconds.

## Problem
Real estate agents receive leads from multiple sources (website forms, ads,
WhatsApp). The data is often messy, the same person contacts them several
times, and slow follow-up costs deals.

## Solution
1. Receives leads through a webhook
2. Validates required fields and formats (returns clear 400 errors)
3. Normalizes phone numbers, emails and names
4. Detects duplicates by normalized phone number
5. Inserts new leads or updates returning leads (contact count + latest message)
6. Sends the agent an email alert (separate "New Lead" and "Returning Lead" messages)
7. Keeps the lead saved even if the alert fails, and notifies me of failures

## Flow
Form/Postman -> Webhook -> Validate -> Normalize -> Duplicate Check
-> Insert/Update -> Build Alert -> Email -> Response

## Tech Stack
n8n, Webhook, n8n Data Tables, Gmail, JavaScript

## How to Use

1. Import `real-estate-lead-capture-v1.json` into your n8n instance
   (Workflows -> ⋯ -> Import from file).
2. Create a Data Table named `leads` with these columns:
   full_name, phone, email, intent, budget_min (number), budget_max (number),
   preferred_area, property_type, source, message, status,
   contact_count (number).
3. Open the Get row(s), Insert row and Update row(s) nodes and select your
   `leads` table.
4. Open the Gmail nodes ("Send a message"), connect your own Gmail account,
   and replace YOUR_EMAIL@example.com in the "To" field with the email
   address that should receive the alerts.
5. Activate the workflow and send a POST request to your webhook URL
   using the sample payload below.

## Sample Payload
{
  "full_name": "Ali Khan",
  "email": "ali@example.com",
  "phone": "0300-1234567",
  "intent": "buy",
  "budget_min": 5000000,
  "budget_max": 8000000,
  "preferred_area": "DHA Phase 6",
  "property_type": "house",
  "source": "website_form",
  "message": "Looking for a 3 bedroom house"
}

## Roadmap
- Real CRM integration (HubSpot / GoHighLevel)
- WhatsApp alerts
- AI lead qualification and property matching
