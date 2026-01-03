# Context Engineering: What Actually Works (A Research Synthesis) 2026

> *"Most agent failures are not model failures anymore, they are context failures. Context engineering is effectively the #1 job for engineers building AI agents."*
> — Industry, 2025

Research on context window management strategies used by the main players in the market.

---
## Taxonomy of Strategies

According to [Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) and [LangChain](https://blog.langchain.com/context-engineering-for-agents/), the strategies are divided into **4 categories**:

| Strategy     | Description                                      | When to use                                 |
| ------------ | ------------------------------------------------ | ------------------------------------------- |
| **Write**    | Write to external memory (DB, files)             | Data that needs to persist between sessions |
| **Select**   | Select what to retrieve (RAG, similarity search) | Large knowledge bases                       |
| **Compress** | Compress via summarization or trimming           | Long conversations, extensive history       |
| **Isolate**  | Isolate into sub-agents with their own contexts  | Complex multi-domain tasks                  |
> *"Context engineering is the art and science of filling the context window with just the right information at each step of an agent's trajectory."* — LangChain

---
## Strategies by Company/Institution
### 1. Anthropic (Claude Code)

**Source:** [JetBrains Research](https://blog.jetbrains.com/research/2025/12/efficient-context-management/)

**Main Strategy:** Auto-compact + Observation Masking

| Technique                    | Description                                                      |
| ---------------------------- | ---------------------------------------------------------------- |
| **Auto-compact**             | Triggers automatically when it reaches 95% of the context        |
| **Trajectory summarization** | Summarizes the complete sequence of user-agent interactions      |
| **Observation masking**      | Preserves action/reasoning history, compresses only observations |
**Insight:** The distinction between "agent actions" (preserve) and "environment observations" (compress) is key. Observations are generally larger and more redundant.

---
### 2. MemGPT (Letta)

**Source:** [Information Matters](https://informationmatters.org/2025/10/memgpt-engineering-semantic-memory-through-adaptive-retention-and-context-summarization/)

**Main Strategy:** Hierarchical memory inspired by operating systems

| Layer                | Analogy | Function                         |
| -------------------- | ------- | -------------------------------- |
| **Main Context**     | RAM     | Active context, immediate access |
| **External Context** | Disk    | Long-term memory, retrievable    |

**Key Differentiators:**
- LLM manages its own memory (decides what to move to "disk")
- Prioritizes **precision and relevance** over maximum recall
- Explicit operations: `memory_write`, `memory_read`, `memory_search`

**Insight:** Paradigm shift - instead of trying to put everything into context, teach the model to actively manage its memory.

---
### 3. Google (Multi-Agent Framework)

**Source:** [Google Developers Blog](https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/)

**Main Strategy:** Isolation via multi-agent orchestration

| Component                   | Function                                    |
| --------------------------- | ------------------------------------------- |
| **Controller (Lead Agent)** | Orchestrates, maintains compressed overview |
| **Sub-agents**              | Each with specialized and in-depth context  |
**Hierarchical Compression** | Findings from sub-agents are summarized for the controller |

**Benefits:**
- Each agent works with context curated for its specialty
- Avoids giant prompts in the main agent
- Allows guardrails and prevention of infinite loops

---
### 4. Berkeley AI Research Lab

**Source:** [Agenta Blog](https://agenta.ai/blog/top-6-techniques-to-manage-context-length-in-llms)

**Research Findings:**

| Metric                                                           | Result                             |
| ---------------------------------------------------------------- | ---------------------------------- |
| Reduction of hallucinations with RAG                             | **52%** vs pure-context approaches |
| Retention of critical information with multi-level summarization | **91%**                            |
| Reduction of tokens with summarization                           | **68%**                            |
| Reduction with compression (without summarization)               | **40-60%**                         |

**Important distinction:**
- **Summarization**: Creates new sentences (risk of hallucination)
- **Compression**: Maintains original sentences, removes redundancy (safer for accuracy)

---
### 5. LlamaIndex

**Source:** [LlamaIndex Blog](https://www.llamaindex.ai/blog/context-engineering-what-it-is-and-techniques-to-consider)

**Architectural principles:**

| Principle                              | Description                                               |
| -------------------------------------- | --------------------------------------------------------- |
| **Separate storage from presentation** | Distinguish durable state from views per call             |
| **Explicit transformations**           | Named and ordered processors (no ad-hoc concatenation)    |
| **Scope by default**                   | Each call to the model sees the minimum necessary context |

**Central Analogy:**
> *"LLM is like the CPU and its context window is like RAM. Context engineering is like the operating system curating what fits in RAM."*

---
## Recommended Patterns in Medical/Critical Domain


> **👉🏽 RAG with exact retrieval** — When every word matters (no summarization)  
> **👉🏽 Detailed logs in external storage** — Reconstruction when necessary  
> **👉🏽 Compression (no summarization)** — Reduce tokens without hallucination risk


---
## Implementation Gotchas

| Problem                     | Description                                    | Mitigation                              |
| --------------------------- | ---------------------------------------------- | --------------------------------------- |
| **Context Poisoning**       | Hallucinations enter the context and propagate | Validate outputs before persisting      |
| **Context Distraction**     | Very large context overwhelms the model        | Aggressive selection, minimum scope     |
| **Context Confusion**       | Superfluous information influences responses   | Isolation, clear separation of sections |
| **Performance Degradation** | Performance drops with very long context       | Compression, periodic summarization     |

---
## Fonts

1. [Anthropic - Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
2. [LangChain - Context Engineering](https://blog.langchain.com/context-engineering-for-agents/)
3. [JetBrains Research - Efficient Context Management](https://blog.jetbrains.com/research/2025/12/efficient-context-management/)
4. [MemGPT - Adaptive Retention and Context Summarization](https://informationmatters.org/2025/10/memgpt-engineering-semantic-memory-through-adaptive-retention-and-context-summarization/)
5. [Google Developers - Multi-Agent Framework](https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/)
6. [Agenta - Top Techniques to Manage Context Length](https://agenta.ai/blog/top-6-techniques-to-manage-context-length-in-llms)
7. [LlamaIndex - Context Engineering Techniques](https://www.llamaindex.ai/blog/context-engineering-what-it-is-and-techniques-to-consider)
8. [GetMaxim - Context Window Strategies](https://www.getmaxim.ai/articles/context-window-management-strategies-for-long-context-ai-agents-and-chatbots/)
9. [Kubiya - Context Engineering Best Practices](https://www.kubiya.ai/blog/context-engineering-best-practices)
10. [JRodDev - Context Window Management in Agentic Systems](https://blog.jroddev.com/context-window-management-in-agentic-systems/)
