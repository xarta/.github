# xarta

Among other things, homelab AI projects exploring local inference, RAG architectures, and efficient GPU utilization.  Will be mostly AI Vibe Coded / generative code with all the risks and issues that includes i.e. "AI Slop".  It's good enough for me, for now, taking additional precautions with my networking and Proxmox features.  As soon as I was financially able to and when products were available, I made my desktop-cum-workstation.  9950X / ProArt X870E / RTX 5090 / RTX 4000 / T600 based.  And PiKVM etc. and ZFS based file systems mostly.  The premise being to optimise the use of my hardware.  e.g. Proxmox hypervisor, with a choice of VM's to pass through resources to along with CPU CCD allocation and the like.  One VM is a nested Proxmox system with passed through 5090 and 4000.  Setup to share the GPU's among LXC's and Docker Containers.  I've been developing the means to switch between configurations easily to explore AI projects. Configurations are JSON based for which LXC's and Docker containers run and which vLLM models if using vLLM etc. so there's no manual faffing about to switch.  It was my vision to use vLLM extensively for some configurations to take advantage of the awesome parallisation/batching vLLM + 5090 and even the 4000 Blackwell provides.  e.g. in a standard AI Assistant configuration I'll use the 5090 for vLLM with a 30b sparse MOE 3b active 4bit model with quantised context.  Just enough accuracy and context to be useful processing many parallel short requests very quickly.  I want to optimise some of my code to take advantage of that.  The 4000 can run embeddings and reranker as well as STT and TTS simultaneously (and other models like noise reduction for example in a pipecat framework) achieving 300ms latency in conversations with multiple personalities.  I view my system ... as a system, where I want to optimise use of it for different workloads.  I'm still exploring how best to do it, shifting more of it's functionality to local model use where possible and relying less on cloud based services except where it makes most sense.  It's a work in progress.

## Current Focus: Document Analysis & Ingestion Ecosystem

An active project exploring document sanitisation and knowledge retrieval — a collection of Dockerised microservices for cleaning, analysing, and ingesting AI-generated infrastructure code.

### The Challenge

AI-generated infrastructure code (Proxmox, LXC, Docker configurations) contains secrets and environment-specific details that make it unsafe to share and difficult to reuse. Traditional approaches require manual review and editing, which is time-consuming and error-prone.

### The Solution

A composable ecosystem of microservices that:
- Scan for secrets using pattern-driven detection
- Detect content duplications across documents
- Identify contradictory statements
- Chunk content semantically (both embedding-based and LLM-driven approaches)
- Ingest clean documentation into a vector database
- Enable efficient RAG-based knowledge retrieval for AI agents

### Learning Journey

This project serves as a practical exploration of:
- **RAG architectures** — retrieval-augmented generation patterns and vector database integration
- **Semantic chunking** — comparing embedding-based vs LLM-driven approaches
- **GPU optimization** — leveraging batched and parallel operations across RTX 5090 and RTX 4000 Blackwell
- **Microservice composition** — building lightweight containers that delegate compute to shared inference endpoints
- **Local AI inference** — achieving production-quality results without cloud dependencies

### Hardware Utilization

Built for a nested Proxmox homelab, the architecture maximizes local GPU performance:
- **Shared vLLM endpoints** — all services delegate LLM chat, embeddings, and reranking to dedicated inference servers
- **OpenAI-compatible APIs** — standardized interfaces enable parallel and batched operations
- **Lightweight containers** — services contain only business logic; ML workloads run on optimized inference endpoints
- **Concurrent processing** — multiple services can query the same GPU simultaneously, making efficient use of RTX 5090 and RTX 4000 Blackwell

### Components

Repositories tagged with [`doc-sanitiser-ecosystem`](https://github.com/search?q=org%3Axarta+topic%3Adoc-sanitiser-ecosystem&type=repositories):

| Repository | Description |
|---|---|
| [Normalized-Semantic-Chunker](https://github.com/xarta/Normalized-Semantic-Chunker) | Embedding-based semantic text chunking with statistical token-size control. Lightweight fork — delegates embeddings to remote vLLM. |
| [Agentic-Chunker](https://github.com/xarta/Agentic-Chunker) | LLM-driven proposition chunking — uses chat completions to semantically group content. Fork replacing Google Gemini with local vLLM. |
| [gitleaks-validator](https://github.com/xarta/gitleaks-validator) | Dockerised gitleaks wrapper — pattern-driven secret scanning and replacement via REST API. |
| [knowledge-service](https://github.com/xarta/knowledge-service) | Document ingestion into SeekDB (vector database) with RAG query interface. Composes chunking + embedding services. |
| [content-analyser](https://github.com/xarta/content-analyser) | Duplication detection and contradiction analysis across document sets. Composes chunking, embedding, and LLM services. |

### Future Direction

The knowledge base will enable sub-agent patterns — specialized agents that query the ingested documentation for context-aware code generation, infrastructure configuration, and automated problem-solving.

### Status

Active development. Decomposing an original monolithic codebase into manageable microservices. All code is AI-assisted and experimental — suitable for homelab exploration and learning.
