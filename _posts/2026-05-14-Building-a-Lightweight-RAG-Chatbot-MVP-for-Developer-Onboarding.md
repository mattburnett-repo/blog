---
title: Building a Lightweight RAG Chatbot MVP for Developer Onboarding
---

# Building a Lightweight RAG Chatbot MVP

Over the last few weeks I’ve been spending time learning more about vector databases, retrieval-augmented generation (RAG), and practical LLM integration. After working through tutorials and experimenting with different approaches, I decided the best way to learn the technology was to build and deploy something real.

The result was a lightweight chatbot MVP designed to help onboard developers to the [CIB project](https://cibmangotree.org)
 within the [Civic Tech DC](https://www.civictechdc.org) ecosystem.

* Live app: [chatmvp.streamlit.app](https://chatmvp.streamlit.app)
* Source code: [chat-mvp GitHub repository](https://github.com/mattburnett-repo/chat-mvp)

The scope of the MVP is intentionally small:

* Answer basic questions about the project
* Explain what the CIB tool is
* Help developers understand how to get involved
* Provide lightweight guidance using existing project documentation

The implementation uses a fairly standard RAG-style architecture:

* Python ingestion/embed scripts
* PostgreSQL with pgvector
* Lightweight backend API
* Streamlit frontend UI
* Hosted open-source LLM inference

The ingestion pipeline processes project documentation, chunks the content, generates embeddings, and stores the vectors in PostgreSQL. The chatbot retrieves relevant context from the vector database and supplies it to the LLM as part of the prompt flow.

One thing that became immediately obvious while building the MVP was that inference cost matters, especially for low-traffic civic-tech or volunteer-oriented projects. After discussing the project with stakeholders and thinking more carefully about sustainability, I replaced the original proprietary model with a hosted open-source alternative accessed through the Hugging Face inference API.

That change significantly improved the long-term sustainability story of the project while keeping the architecture lightweight and easy to maintain.

After deploying the MVP, I shared it with several stakeholders within the Civic Tech DC community. The discussions quickly moved beyond novelty reactions and into implementation-oriented questions around onboarding workflows, deployment, and future integrations, which I took as a positive sign.

The project was valuable because it moved beyond tutorials and into a deployed system with real users, real constraints, and real feedback. That transition — from experimenting privately to building something people can actually interact with — changes the learning experience considerably.
