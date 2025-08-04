# README: Zap Project Concierge Prototype

## Overview
This document provides a complete overview of the Zap Project Concierge, a working prototype built for the zap.co.il AI home task. The purpose of this implementation is to demonstrate a consultative, project-oriented shopping assistant that goes beyond simple keyword search. Its core capability is a two-stage conversational flow: it first asks clarifying questions to understand a user's goal, then generates tailored product recommendations to help them complete complex shopping “projects” with confidence.

## Requirements & Setup
- **OpenAI API Key:** Needed for powering the intelligence (gpt-4o). Obtain this from your OpenAI account and supply it to the app via the sidebar or as an environment variable.
- **Ngrok Auth Token:** Used to expose the local Streamlit app to a public URL for demonstration. Set your token in the final cell of the notebook.

## Vision & Core Concept
Users shopping on zap.co.il often aim to complete projects (e.g., “home office setup”) that span multiple correlated products. The Concierge reframes this journey by:

- Engaging the user first — asking clarifying questions about budget, priorities, and use case.
- Retrieving contextual product data — using a synthetic, richly tagged dataset.
- Synthesizing a plan — providing tailored recommendations with clear explanations.
- Facilitating action — enabling users to add chosen items directly to a cart.

## Rationale for Synthetic Dataset Generation
To build a functional and convincing prototype without access to live zap.co.il data, it was essential to engineer a high-quality synthetic dataset. The following explains the methodology and strategic assumptions that guided its creation.

### Guiding Principles for Data Realism
- **Focus on Project-Based Shopping:** The core assumption is that users often shop for a "project," not just an "item." The dataset centers on multi-item purchase scenarios (e.g., "Home Office Setup," "Baby Preparation") because this is where a consultative AI provides the most value and where the business can increase average order value (AOV).
- **Simulate the Israeli Market Context:** To ensure authenticity, all data—including pricing in ILS, the mix of international and local brands, and language nuances—was designed to mirror the realities of the Israeli e-commerce market.
- **Structure for an AI-First World:** Products are explicitly structured for AI consumption. They are enriched with semantic tags related to projects, styles, and user needs, allowing the AI to connect context beyond simple keyword matching.

## Dataset Characteristics
- **Curated Categories:** Home Office, Baby Preparation, and Home Entertainment.
- **Realistic Brand Mix:** Includes international brands (Samsung, IKEA, Logitech) and local Israeli brands (Keter, Shilav).
- **Rich Product Attributes:** Each product includes technical specs, features, warranty information, seller details, and realistic review data.
- **Semantic Tagging:** Products are tagged with relevant project types (e.g., `video_conferencing_setup`) and the specific user needs they address (e.g., `ergonomics`, `audio_clarity`). This semantic layer is critical for the recommendation logic.

## System Architecture & Logic

The core differentiator is the two-stage conversational flow (understand → recommend).

### Visual Flow (Mermaid)

```mermaid
flowchart TB
    UI[User Interaction Layer<br/>(Streamlit UI)]
    Intent[Intent & Need Detection<br/>(From User Input)]
    Clarify{New Conversation?}
    Ask[Ask Clarifying Questions]
    Suggest[Suggest Products]
    Retrieval[Context Retrieval<br/>(filter by need, sort by rating)]
    Engine[Recommendation Engine<br/>(LLM synthesizes plan)]
    Cart[Interactive Cart & Order]

    UI --> Intent --> Clarify
    Clarify -- yes --> Ask --> Retrieval
    Clarify -- no --> Suggest --> Retrieval
    Retrieval --> Engine --> Cart
```

### Textual Flow (fallback)

1. **User Interaction Layer** (Streamlit UI)  
2. **Intent & Need Detection** (from user input)  
3. **Branch:**  
   - If new conversation → ask clarifying questions  
   - Else → suggest products  
4. **Context Retrieval** (filter candidates by need, sort by rating)  
5. **Recommendation Engine** (LLM synthesizes the plan)  
6. **Interactive Cart & Order**

## System Capabilities & Flow

### Initial Interaction & Need Detection
A user starts the conversation by describing their project in natural language (e.g., "I need a comfortable chair for my home office").

**Technical Explanation:** The system processes this input using a keyword-matching function (e.g., `detect_user_needs`). It compares words from the user's query against a predefined dictionary to identify core “needs” (e.g., “comfortable” and “chair” map to the ergonomics need).

### Consultative Questioning Phase
If this is the beginning of the conversation, the AI does not immediately show products. Instead, it behaves like a consultant.

**Technical Explanation:** Detected needs are provided as context to the OpenAI API (`gpt-4o`), and the LLM is prompted to generate 1–2 clarifying questions (e.g., “What’s your budget?” or “How many hours a day will you be sitting?”) to refine intent and preferences.

### Recommendation & Ranking Phase
Once the user answers the clarifying questions, the system has sufficient context to provide recommendations.

**Technical Explanation:** The system selects matches via a two-step process:
1. **Filtering by Need:** It filters the JSON product catalog to candidates matching the required semantic tag (e.g., `ergonomics`).
2. **Sorting by Rating:** The filtered list is sorted by product rating (descending). Top-rated items are treated as best matches. The LLM then synthesizes a natural-language response presenting these products with justifications.

## Example User Flow
- **User:** “I need to set up a home office for daily video calls.”
- **Concierge:** “Great. To tailor recommendations, what’s your approximate budget and how important is microphone quality?”
- **User:** “Budget ≈ ₪4,000. Clear audio is critical.”
- **Concierge:** Recommends specific USB microphones, a webcam, and ergonomic chairs, explaining why each item fits the project and constraints.

## Limitations & Future Roadmap

**Current Limitations:**  
- Static data (no live zap.co.il inventory).  
- Basic keyword-based need detection.  
- No persistent user profile or personalization across sessions.  

**Future Enhancements:**  
- Integrate with live zap.co.il APIs (catalog, inventory, pricing).  
- Replace keyword matching with embedding- or LLM-based intent detection.  
- Add user profiles and session memory for personalization.  
- Evolve into an autonomous agent capable of managing carts and the checkout flow.
