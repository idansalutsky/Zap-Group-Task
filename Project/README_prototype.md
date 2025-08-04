# README: Zap Project Concierge Prototype

Overview
This document provides a complete overview of the Zap Project Concierge, a working prototype built for the zap.co.il AI home task. The purpose of this implementation is to demonstrate a consultative, project-oriented shopping assistant that goes beyond simple keyword search. Its core capability is a two-stage conversational flow: it first asks clarifying questions to understand a user's goal, then generates tailored product recommendations to help them complete complex shopping “projects” with confidence.

Requirements & Setup
OpenAI API Key: Needed for powering the intelligence (gpt-4o). Obtain this from your OpenAI account and supply it to the app via the sidebar or as an environment variable.

Ngrok Auth Token: Used to expose the local Streamlit app to a public URL for demonstration. Set your token in the final cell of the notebook.

Vision & Core Concept
Users shopping on zap.co.il often aim to complete projects (e.g., “home office setup”) that span multiple correlated products. The Concierge reframes this journey by:

Engaging the user first — asking clarifying questions about budget, priorities, and use case.

Retrieving contextual product data — using a synthetic, richly tagged dataset.

Synthesizing a plan — providing tailored recommendations with clear explanations.

Facilitating action — enabling users to add chosen items directly to a cart.

Rationale for Synthetic Dataset Generation
To build a functional and convincing prototype without access to live zap.co.il data, it was essential to engineer a high-quality synthetic dataset. The following explains the methodology and strategic assumptions that guided its creation.

Guiding Principles for Data Realism
Focus on Project-Based Shopping: The core assumption is that users often shop for a "project," not just an "item." I focused the dataset on multi-item purchase scenarios (e.g., "Home Office Setup," "Baby Preparation") because this is where a consultative AI provides the most value and where the business has the highest potential to increase average order value (AOV).

Simulate the Israeli Market Context: To ensure authenticity, all data—including pricing in ILS, the mix of international and local brands, and language nuances—was designed to mirror the realities of the Israeli e-commerce market.

Structure for an AI-First World: The data was explicitly structured for AI consumption. Products are enriched with semantic tags related to projects, styles, and user needs, allowing the AI to understand context and make connections beyond simple keyword matching.

Dataset Characteristics
Curated Categories: The dataset focuses on three project-heavy categories: Home Office, Baby Preparation, and Home Entertainment.

Realistic Brand Mix: The data includes a realistic mix of international brands (Samsung, IKEA, Logitech) and local Israeli brands (Keter, Shilav).

Rich Product Attributes: Each product contains detailed information, including technical specs, features, warranty information, seller details, and realistic review data.

Semantic Tagging: Each product is tagged with its relevant project type (e.g., video_conferencing_setup) and the specific user needs it addresses (e.g., ergonomics, audio_clarity). This is the most critical element for the AI's recommendation logic.

System Architecture & Logic
The core differentiator is the two-stage conversational flow (understand → recommend).

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
| IF New Conversation THEN       |
|   -> Ask Clarifying Questions  |
| ELSE                           |
|   -> Suggest Products          |
+--------------------------------+
             |
             v
+--------------------------+
| Context Retrieval        |
| (Filter by Need, Sort by Rating) |
+--------------------------+
             |
             v
+-----------------------------+
| Recommendation Engine       |
| (LLM synthesizes the plan)  |
+-----------------------------+
             |
             v
+--------------------------+
| Interactive Cart & Order |
+--------------------------+

System Capabilities & Flow
Here is a step-by-step breakdown of the user flow and the technical capabilities at each stage:

Initial Interaction & Need Detection:

A user starts the conversation by describing their project in natural language (e.g., "I need a comfortable chair for my home office").

Technical Explanation: The system processes this input using a keyword-matching function (detect_user_needs). It compares words from the user's query against a predefined dictionary of terms to identify core "needs" (e.g., "comfortable" and "chair" map to the ergonomics need).

Consultative Questioning Phase:

If this is the beginning of the conversation, the AI does not immediately show products. Instead, it acts as a consultant.

Technical Explanation: The system uses the detected needs as context for a call to the OpenAI API (gpt-4o). The LLM is prompted to generate 1-2 relevant, clarifying questions (e.g., "What's your budget?" or "How many hours a day will you be sitting?"). This makes the interaction feel intelligent and personalized.

Recommendation & Ranking Phase:

Once the user answers the clarifying questions, the system has enough context to provide recommendations.

Technical Explanation: The system finds the best matches using a two-step filtering process:

Step 1: Filtering by Need: It filters the entire JSON product catalog to create a list of candidates that have the required semantic tag (e.g., ergonomics).

Step 2: Sorting by Rating: It then sorts this filtered list of candidates by a single, clear metric: the product rating, in descending order. The top-rated products are considered the "best matches." The LLM is then used to synthesize a natural language response presenting these products to the user.

Example User Flow
User: “I need to set up a home office for daily video calls.”

Concierge: “Great. To tailor recommendations, what’s your approximate budget and how important is microphone quality?”

User: “Budget ≈ ₪4,000. Clear audio is critical.”

Concierge: Recommends specific USB microphones, a webcam, and ergonomic chairs, explaining why each item fits the project and constraints.

Limitations & Future Roadmap
Current Limitations: The prototype uses static data, basic keyword-based need detection, and has no persistent user profile for personalization.

Future Enhancements: The roadmap includes integrating with live zap.co.il APIs, using embeddings for advanced intent detection, incorporating user profiles for personalization, and evolving the assistant into an autonomous agent that can manage the cart and checkout process.
