# Zap Project Concierge

Introduction
This document provides an overview of the Project Concierge, a working prototype developed for the Zap AI home task. This concept was selected for implementation from three innovative AI capabilities I proposed, all designed to be embedded directly into the zap.co.il e-commerce site.
My goal with this prototype is to demonstrate a feature that showcases a more sophisticated, consultative AI interaction. Instead of simply returning search results, the Concierge first engages the user to understand their needs, then provides tailored recommendations, directly addressing the core e-commerce challenge of simplifying complex, multi-product purchases.

How to Run This Project (What You'll Need)
To get this demo running on your own machine, you'll need two things:
OpenAI API Key: The app's intelligence is powered by OpenAI's gpt-4o. You'll need to get an API key from your OpenAI account and paste it into the sidebar of the Streamlit app when it runs.
Ngrok Auth Token: To expose the local Streamlit app to a public URL, I've used ngrok. You'll need a free account and your auth token. You can add it to the final cell in the notebook where it says ngrok.set_auth_token(...).
Once you have those, you can run the notebook cells in order, and a public URL for the live demo will be generated.

Project Vision & Core Concept
The zap.co.il Project Concierge is designed to transform the user experience for project-based shopping. Shoppers often come to zap.co.il to purchase multiple, related items for a specific goal (e.g., setting up a home office). The traditional item-by-item search process is inefficient and fails to guide the user.
The Concierge solves this by acting as a conversational shopping assistant. It mimics an expert sales consultant by first asking clarifying questions (about budget, use case, etc.) before presenting a curated list of individual products, complete with detailed specifications and clear justifications.

Rationale for Synthetic Dataset Generation
To build a functional and convincing prototype without access to live zap.co.il data, it was essential to engineer a high-quality synthetic dataset. The following explains the methodology and strategic assumptions that guided its creation, ensuring the data realistically modeled the target e-commerce environment.
Guiding Principles for Data Realism
My primary goal was to create more than just random data; I aimed to build a dataset that reflects the specific challenges and opportunities of the zap.co.il platform.
Focus on Project-Based Shopping: The core assumption is that users often shop for a "project," not just an "item." I focused the dataset on multi-item purchase scenarios (e.g., "Home Office Setup," "Baby Preparation") because this is where a consultative AI provides the most value and where the business has the highest potential to increase average order value (AOV).
Simulate the Israeli Market Context: To ensure authenticity, all data—including pricing in ILS, the mix of international and local brands, and language nuances—was designed to mirror the realities of the Israeli e-commerce market.
Structure for an AI-First World: The data was explicitly structured for AI consumption. Products are enriched with semantic tags related to projects, styles, and user needs, allowing the AI to understand context and make connections beyond simple keyword matching.

Dataset Characteristics
Based on these principles, I engineered a structured JSON dataset with the following key characteristics to enable a robust demonstration:
Curated Categories: The dataset focuses on three project-heavy categories: Home Office, Baby Preparation, and Home Entertainment.
Realistic Brand Mix: The data includes a realistic mix of international brands (Samsung, IKEA, Logitech) and local Israeli brands (Keter, Shilav) common to the market.
Rich Product Attributes: Each product contains detailed information, including technical specs, features, warranty information, seller details, and realistic review data (ratings, counts).
Semantic Tagging: Products are tagged with their relevant project types (e.g., video_conferencing_setup) and the specific user needs they address (e.g., ergonomics, audio_clarity). This is the most critical element for the AI's recommendation logic.

System Architecture
Here’s a high-level diagram of the system's architecture. The core of the logic is the conditional, two-stage interaction flow.
+--------------------------+
| User Interaction Layer   |
| (Streamlit UI)           |
+--------------------------+
             |
             v
+--------------------------+
| Intent & Need Detection  |
| (From User Input)        |
+--------------------------+
             |
             v
+--------------------------------+
| IF Conversation is New THEN    |
|   -> Ask Clarifying Questions  |
| ELSE                           |
|   -> Suggest Products          |
+--------------------------------+
             |
             v
+--------------------------+
| RAG: Context Retrieval   |
| (From JSON Database)     |
+--------------------------+
             |
             v
+--------------------------+
| Plan/Recommendation Engine |
| (LLM synthesizes response) |
+--------------------------+
             |
             v
+--------------------------+
| Interactive Cart & Order |
| (User adds items)        |
+--------------------------+



Use Case Example
A user on zap.co.il initiates a request: "I need to set up a home office for daily video calls."
AI Asks Questions: Instead of immediately showing products, the Concierge responds: "Great! To find the perfect setup, could you tell me: 1. What's your approximate budget? 2. How important is a high-quality microphone for your calls?"
User Provides Context: The user replies: "My budget is around ₪4,000, and yes, clear audio is very important."
AI Recommends Products: Now armed with context, the Concierge generates a tailored list:
For Audio Clarity: It suggests two specific USB microphones, explaining why each is a good fit for professional calls.
For Video Quality: It recommends a high-resolution webcam.
For Ergonomics: It presents two ergonomic chairs, highlighting their features and benefits for long work sessions.
This converts a vague goal into a set of specific, justified product choices, significantly increasing user confidence and the likelihood of a successful purchase.
Limitations & Future Roadmap
This prototype serves as a proof-of-concept. The following are its primary limitations and a proposed roadmap for development.

Current Limitations:
Static Data: The app uses a fixed sample of the synthetic database, not a live, real-time zap.co.il inventory.
Simple Need Detection: The system currently uses keyword matching to detect user needs.
No User History: The system does not yet leverage user profile data for deeper personalization.

Proposed Future Enhancements:
Live zap.co.il Integration: The highest priority is to connect the system to the live product catalog and inventory APIs to ensure all recommendations are accurate and in-stock.
Advanced Need Detection: Upgrade from keyword matching to an LLM-based or embedding-based approach for more nuanced and accurate understanding of user intent.
Deep Personalization: Integrate with user profiles to incorporate browsing history, past purchases, and stated preferences into the recommendation engine.
Full-Cycle Automation with Agents: The ultimate vision is to evolve the Concierge from a planner into an autonomous agent. After plan approval, the AI could automatically add all selected items to the user's shopping cart, creating a truly frictionless path to checkout.
