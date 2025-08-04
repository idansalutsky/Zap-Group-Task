# Zap Project Concierge

## Overview
The **Zap Project Concierge** is a working prototype built for the zap.co.il AI home task. Its purpose is to demonstrate a consultative, project-oriented shopping assistant that goes beyond keyword search: it first asks clarifying questions, then generates tailored multi-item recommendations to help users complete complex shopping “projects” (e.g., setting up a home office) with higher confidence and increased average order value.

## Key Goals
- Shift users from item-by-item browsing to goal/project completion.
- Simulate an expert sales consultant via a two-stage conversational flow.
- Surface justified, contextual product bundles instead of isolated suggestions.

---

## Requirements

1. **OpenAI API Key**  
   - Needed for powering the intelligence (uses `gpt-4o` as the primary model).  
   - Obtain from your OpenAI account and supply it to the app via the sidebar or environment variable.

2. **Ngrok Auth Token**  
   - Used to expose the local Streamlit app to a public URL for demonstration.  
   - Create a free ngrok account, grab your auth token, and set it in the notebook (e.g., `ngrok.set_auth_token("...")`).

---

## Setup & Run Instructions

1. Clone the repository / open the notebook.
2. Create a `.env` or configure the following values in the UI/sidebar:
   ```env
   OPENAI_API_KEY=your_openai_key_here
   NGROK_AUTH_TOKEN=your_ngrok_token_here
   ```
3. In the notebook:
   - Install dependencies if not already present.
   - Set the ngrok token in the designated cell:  
     ```python
     from pyngrok import ngrok
     ngrok.set_auth_token("your_ngrok_token_here")
     public_url = ngrok.connect(8501, "http")
     print("Streamlit exposed at:", public_url)
     ```
   - Run the Streamlit app (`streamlit run app.py` or via the provided runner cell).
4. Interact with the Concierge via the generated public URL.

---

## Vision & Core Concept

Users shopping on zap.co.il often aim to complete **projects** (e.g., “home office setup”) that span multiple correlated products. The Concierge reframes the shopping journey by:

1. **Engaging the user first** — asking clarifying questions (budget, priorities, use case).
2. **Retrieving contextual product data** — using a synthetic, richly tagged dataset.
3. **Synthesizing a plan** — tailored recommendations with explanations, addressing the user’s stated needs.
4. **Facilitating action** — enabling users to add chosen items into a cart, moving toward conversion.

---

## Synthetic Dataset: Rationale & Design

Because live zap.co.il data wasn’t available, the prototype uses a **purpose-built synthetic dataset** engineered to model realistic Israeli e-commerce behavior.

### Guiding Assumptions
- **Project-based shopping** drives more value than one-off item searches. Users shop with a goal (e.g., “video conferencing setup”), not just isolated products.
- **Israeli market context** matters: pricing in ILS, a mix of international (Samsung, IKEA, Logitech) and local brands (Keter, Shilav), and relevant language nuances were simulated.
- **AI-first structure**: Products are semantically enriched so the LLM can reason across user needs, project types, and product attributes—not just keyword overlap.

### Dataset Characteristics
- **Focused Categories**: Home Office, Baby Preparation, Home Entertainment.
- **Realistic Brand Mix**: Blends international and local brands reflecting local shopping habits.
- **Rich Attributes**: Technical specs, warranty, seller metadata, etc.
- **Semantic Tagging**: Each product is tagged with project relevance (e.g., `video_conferencing_setup`) and user needs (e.g., `audio_clarity`, `ergonomics`), powering recommendation logic.

---

## System Architecture

```text
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
| RAG: Context Retrieval   |
| (From JSON Dataset)      |
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
```

---

## Example User Flow

1. **User**: “I need to set up a home office for daily video calls.”
2. **Concierge**: “Great. To tailor recommendations:  
   1. What’s your approximate budget?  
   2. How important is microphone quality?  
   3. Do you prefer compact or full-sized desks?”
3. **User**: “Budget ≈ ₪4,000. Clear audio is critical.”
4. **Concierge**:
   - Recommends USB microphones with explanations (e.g., noise cancellation, clarity).  
   - Suggests a webcam for video quality.  
   - Adds two ergonomic chairs for long sessions.  
   - Breaks down why each item fits the project and constraints.

---

## Limitations (Current Prototype)

- **Static Data**: Uses a fixed synthetic JSON dataset; no real-time inventory or pricing.
- **Basic Need Detection**: Relies on keyword matching rather than embeddings or deeper semantic intent modeling.
- **No Persistent User Profile**: Lacks history, personalization across sessions, or behavior-based adaptation.

---

## Roadmap / Future Enhancements

1. **Live Integration**
   - Connect to real zap.co.il product catalog, inventory, and pricing APIs.
2. **Improved Intent Detection**
   - Replace keyword heuristics with embedding-based or LLM-driven understanding for nuance and ambiguity resolution.
3. **Personalization**
   - Incorporate user profiles: past behavior, preferences, saved projects.
4. **Autonomous Agents**
   - Evolve from planning assistant to agent that can pre-fill carts and execute multi-step flows with user approval.
5. **Feedback Loop**
   - Capture downstream conversion signals to refine recommendations (e.g., A/B test project bundles).
6. **Session Continuity**
   - Enable multi-session project draft saving, collaborative editing, and shareable plans.

---

## File / Config Suggestions

- `.env.example`
  ```env
  OPENAI_API_KEY=your_key_here
  NGROK_AUTH_TOKEN=your_token_here
  ```
- `synthetic_dataset.json` — the structured product data with semantic tags.
- `app.py` / Streamlit entrypoint that wires user input → retrieval → LLM → recommendation UI.
- `utils/` — helpers for embedding lookup, prompt templating, and cart management.

---

## Extending the Prototype

- **Add new project categories**: Define new semantic tags and associated product bundles.
- **Swap in advanced retrievers**: Plug in FAISS or vector DB to improve similarity search over products.
- **Introduce multi-turn memory**: Layer session storage (e.g., Redis or lightweight user state) to persist project evolution.
- **Cart automation**: After user approval, programmatically add all recommended items to the e-commerce cart via zap.co.il’s frontend/back channels.

---

## Notes for Reviewers / Stakeholders

- The core differentiator is the **two-stage conversational flow** (understand → recommend) powered by semantics, not surface-level search.  
- Synthetic data was intentionally designed to be **realistic for market validation** while remaining decoupled from live systems for safe prototyping.  
- The current implementation is a **proof-of-concept foundation**; the most value unlock comes from plugging in live product data and refining intent understanding.

---

## Contact / Ownership

Prototype owner: **Idan Salutsky**  
Built as part of the zap.co.il AI home task — intended as a demo of consultative, project-centered commerce assistance.
