---
title: Evaluating RAG pipelines with RAGAS
---

This post is about a hands-on LangChain tutorial project—less a polished product than a learning lab—and what stood out most: **RAGAS**, a framework that finally made it feel possible to *test* a retrieval-augmented generation (RAG) pipeline the way developers expect to test ordinary software.

## The problem: how do you test an AI pipeline?

In traditional apps, tests are concrete. Input `2 + 2`, expect `4`. Assertions are deterministic.

RAG is different. The same question can produce a valid paraphrase. Retrieval might return overlapping chunks in different orders. "Correct" is often fuzzy. Eyeballing a few answers in a notebook does not scale, and it does not catch regressions when you change chunk size, embedding model, or prompt.

What we wanted was closer to a **test harness** for an AI stack: repeatable runs, numeric scores, and a way to compare variants before shipping. We did not expect to find something that felt as natural as unit tests. **RAGAS was that pleasant surprise.**

## RAGAS: evaluation that feels like engineering

[RAGAS](https://docs.ragas.io/) (Retrieval Augmented Generation Assessment) treats RAG quality as measurable dimensions, not vibes. You prepare a small **evaluation dataset**: questions, your system's answers, the contexts it retrieved, and (where available) reference answers. RAGAS runs metrics—many powered by an LLM and embeddings—and returns scores you can aggregate, chart, and gate on.

In our project, the core tutorial lives in a `RAGEvaluator` class built around **RAGAS 0.4+** and Hugging Face inference (chat model + `sentence-transformers/all-MiniLM-L6-v2` for embedding-based metrics). The workflow is deliberately boring in the best way:

1. **Prepare** test cases in a consistent shape (`question`, `generated_answer`, `retrieved_contexts`, `ground_truth`).
2. **Map** them to RAGAS column names (`user_input`, `response`, `retrieved_contexts`, `reference`).
3. **Call** `ragas.evaluate()` with a chosen metric set.
4. **Summarize** means, standard deviations, per-question breakdowns, and flags for metrics below a threshold (we used **0.7** as a default "needs attention" line).

### Metrics that changed how we think about RAG

RAGAS does not give you one magic number; it decomposes failure modes. That decomposition is what makes it feel like real testing:

- **Faithfulness** — Is the answer supported by retrieved context? (Claims checked against context, often via natural-language inference-style reasoning.) Catches hallucination when retrieval was actually fine.
- **Context precision** — Are the *right* chunks ranked high? Relevant information should not be buried under noise.
- **Context recall** — Did retrieval surface enough to answer, compared to a reference? Needs ground truth but answers "did we even fetch the right material?"
- **Answer relevancy** — Does the response address the question, regardless of phrasing?
- **Answer correctness** — How close is the answer to the reference answer?

Seeing a report where **faithfulness is high but context recall is low** tells you to fix retrieval, not the generator. Where **precision is low**, chunking or reranking may be wrong. That is the same diagnostic clarity unit tests give you—just over stochastic components.

We also hit practical integration details worth mentioning for anyone reproducing this: chat models are not embedding models; RAGAS metrics that call `embed_query` / `embed_documents` need a proper embeddings bridge; `answer_correctness` can emit long JSON and may need generous `max_new_tokens`; evaluation has a real **API cost** because the judge LLM runs per metric. None of that diminishes the value—it is the price of automated judgment, and it is still cheaper than flying blind in production.

## Why RAGAS felt like a surprise

Most AI tutorials stop at "here is a working chain." You get output, maybe a nice streaming UI, and a sense of progress. What they often skip is the question every senior engineer asks on day two: **how do I know I didn't make it worse?**

RAGAS reframes that question. You are not pretending the LLM is deterministic; you are **measuring distributions of quality** on a fixed test set, the same way you might track p95 latency under load. The mental model clicks quickly:

| Traditional software | RAG + RAGAS |
|----------------------|-------------|
| Unit test assertions | Metric scores on a golden dataset |
| CI fails on regression | Re-run evaluation when retrieval or prompts change |
| Coverage reports | Weak metrics (faithfulness, recall, precision) point to subsystems |
| Staging soak tests | A/B variants + production monitors |

It is not a replacement for human review on edge cases and tone. RAGAS itself notes that ground truth matters for some metrics, evaluator LLM choice affects scores, and diverse test sets matter. But it **closes the gap** between "demo" and "discipline." For a tutorial repo, that was the difference between playing with LangChain and **practicing how RAG is maintained in the wild.**

## Practical takeaways

If you are building or learning RAG today, this project's conclusion is simple:

1. **Build a small golden dataset early**—including awkward questions, not only easy wins.
2. **Run RAGAS (or similar) on every meaningful change**—chunking, embeddings, rerankers, prompts, models.
3. **Read the metric vector, not just the average**—fix retrieval vs generation vs grounding based on which score moved.
4. **Add custom metrics** for what your product cares about (latency SLAs, citations, compliance language).
5. **Tie evaluation to A/B tests and monitors** so quality does not decay quietly after launch.

## Closing thought

LangChain gave us the pipes; Hugging Face gave us models. **RAGAS gave us something we did not know we were missing until we had it: a way to evaluate the pipeline.** In AI terms that is not a luxury—it is how you turn a prototype into something you can improve without guessing.
