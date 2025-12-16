ShopGenie AI Shopping Assistant

ShopGenie is an end-to-end AI Shopping Assistant built using n8n, OpenAI, SerpAPI, and a custom frontend. The system accepts a product query, fetches product data from Amazon, Walmart, and Google Shopping, merges and evaluates results, and returns a ranked list of the top 3 products.
This project satisfies the requirements for the AI Agent Capstone, including workflow orchestration, model integration, cloud deployment, UI interface, and autonomous reasoning.

Key Features
Accepts user shopping queries through a frontend interface.
Uses n8n Webhooks to receive requests from HTML/JavaScript.

Fetches product data from:
Amazon (SerpAPI)
Walmart (SerpAPI)
Google Shopping (SerpAPI)

Merges all incoming API data into a unified structure.
Sends merged results to an OpenAI LLM chain for analysis.
Uses structured prompting and chain-of-thought reasoning.
Returns a clean JSON response with:

summary

top 3 product recommendations

Supports cases where no product data is available.
Includes a clean HTML UI for entering queries and displaying results.
Fully cloud-hosted on n8n.

Architecture Overview

User → index.html → Webhook → Merge Products → API Calls
→ OpenAI LLM → JSON Parsing → Respond to Webhook → Frontend

Workflow Components
1. Webhook Node
Receives POST requests from the frontend.
Extracts:
query
optional product list
Path:
/product-search

2. Code Node (Input Cleaning)
Normalizes the request body.
Ensures query is string and products is always an array.

3. Merge Products Node
Aggregates:
user-provided products
Amazon results
Walmart results
Google Shopping results
Ensures the LLM receives a unified JSON object.

4. API Nodes
Your workflow includes:
Amazon (SerpAPI)
Walmart (SerpAPI)
Google Shopping (SerpAPI)
Each API call uses:
https://serpapi.com/search.json?engine=amazon|walmart|google_shopping&q=QUERY&api_key=YOUR_KEY

5. OpenAI Chain (LLM Ranking Engine)
Prompt includes:
Query interpretation
Product evaluation
Ability to generate recommendations even with zero products
Clear ranking criteria
Required JSON output schema
Model used:
gpt-4.1-mini

6. JSON Parsing Node
Parses the LLM string output into valid JSON.

7. Respond to Webhook
Returns final structured JSON to the frontend.
JSON Output Format
Your workflow outputs:

{
  "summary": "",
  "top_products": [
    {
      "title": "",
      "price": "",
      "store": "",
      "rating": "",
      "discountPercentage": "",
      "link": "",
      "why_ranked": ""
    }
  ]
}

Frontend (index.html)

The UI sends:

POST /product-search
with body:
{
  "query": "laptops"
}

The UI displays the formatted AI response returned by n8n.

Installation and Setup
1. Clone the Repository
git clone https://github.com/yourusername/ShopGenie.git
cd ShopGenie

2. Add Your n8n Webhook URL to index.html
Find:
const url = "YOUR_WEBHOOK_URL";
Replace with your n8n Webhook:
https://yourdomain.n8n.cloud/webhook/product-search

3. Upload Workflow to n8n
Go to n8n
Create a new workflow
Click Import
Paste workflow.json
4. Add SerpAPI and OpenAI Credentials in n8n
OpenAI API Key
SerpAPI Key
5. Deploy and Test

Open the HTML file in a browser and try sending queries such as:
AirPods
Laptop under 700
Best budget TV
Coffee maker

File Structure
ShopGenie/
│
├── frontend/
│   └── index.html
│
├── workflow/
│   └── workflow.json
│
└── README.md

How Recommendation Logic Works
Case 1: Products are provided
The LLM ranks based on:
Rating
Discount
Price
Relevance
Value for money

Case 2: No products available
The LLM generates:
Predicted price range
Known best-selling products
Popular choices
Reasoning for ranking
This ensures your project always responds.

Project Requirements Coverage
Requirement	Status
Workflow orchestration (n8n)	Completed
API integrations	Amazon, Walmart, Google Shopping completed
Advanced prompting	Completed
Chain-of-Thought	Implemented in OpenAI node
Cloud deployment	n8n cloud
Fine-tuning alternative	Synthetic ranking logic implemented
UI/UX	index.html created
End-to-end automation	Completed
Documentation	This README, workflow, and slides

Future Enhancements
Replace SerpAPI with official Walmart/Amazon APIs
Add price-tracking and alert notifications
Add user profiles for personalized recommendations
Store conversation history and preferences
Deploy HTML as a hosted web app
