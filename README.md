# Epistasis: Multi-Agent Binary Analysis Framework

A comprehensive binary analysis platform combining LLM-driven semantic understanding, graph-based code analysis, cross-modal retrieval, and adversarial verification.

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      Epistasis Platform                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │  Discovery   │→ │  Validation  │→ │   Report     │         │
│  │   Phase      │  │    Phase     │  │  Generation  │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│         ↓                 ↓                  ↓                  │
│  ┌──────────────────────────────────────────────────┐         │
│  │        Forest of Agents (FoA) Orchestrator       │         │
│  └──────────────────────────────────────────────────┘         │
│         ↓                                                      │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │              Multi-Modal Analysis Layer                 │  │
│  ├─────────────────────────────────────────────────────────┤  │
│  │ • Raw x86-64 Tokenization  • Graph CFG/DFG Analysis    │  │
│  │ • Multi-View Prompting     • Cross-Modal Retrieval     │  │
│  │ • Semantic Pruning         • Context-Aware Reranking   │  │
│  └─────────────────────────────────────────────────────────┘  │
│         ↓                                                      │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │           Binary Representation Layer                   │  │
│  ├─────────────────────────────────────────────────────────┤  │
│  │ • Binary BPE Tokenization (65K vocab)                   │  │
│  │ • GCN Graph Embeddings (depth 2)                        │  │
│  │ • Decompiled Pseudocode (IDA Pro)                       │  │
│  └─────────────────────────────────────────────────────────┘  │
│         ↓                                                      │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │              Dataset Foundation                         │  │
│  ├─────────────────────────────────────────────────────────┤  │
│  │ Binary-30K: 29,793 binaries                             │  │
│  │ • Windows/Linux/macOS/Android                           │  │
│  │ • 15+ CPU architectures (x86, ARM, MIPS, RISC-V, etc.)  │  │
│  │ • 26.93% malware representation                         │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Core Components

### 1. Forest of Agents (FoA) - Paper 2
- **Discovery Phase**: Parallel agents materialize structure and generate candidate vulnerabilities
- **Validation Phase**: Evidence-constrained execution with adversarial verification
- **Evidence Chains**: Each finding backed by concrete evidence
- **Semantic Pruning**: LLM-guided reduction of false positives

### 2. Multi-View Analysis - Paper 3
- **Permission View**: Security permission patterns
- **API View**: Function call analysis
- **URL & Feature View**: Network/resource access patterns
- **Memory Component**: ~50% token reduction via caching

### 3. Binary Code Representation - Paper 4
- **Raw x86-64 Tokenization**: Direct instruction parsing (prefix, opcode, ModR/M, SIB)
- **Graph-Based Models**: CFG/DFG with GCN (optimal depth: 2 layers)
- **Vulnerability Detection**: Null pointer, array bounds, integer overflow (CWE-476, CWE-129, CWE-190)

### 4. Cross-Modal Retrieval - Paper 5
- **BinSeek-Embedding**: NL query → binary function candidates (Rec@1: 67%)
- **BinSeek-Reranker**: Context-aware refinement (Rec@3: 84.5%, MRR@3: 80.25%)
- **Context Assembly**: Target function + top-k callees
- **LLM Data Synthesis**: Quality-filtered training data generation

### 5. Polymorphic Detection - Paper 1
- **8 Evasion Behaviors**: Junk code, control-flow obfuscation, packing, data encoding, DGA, beacon timing, protocol mimicry, header modification
- **Multi-Layer Detection**: AV (34% DR) + YARA (74% DR) + EDR (76% DR) → Combined 92% DR
- **Iterative Tuning**: FPR-DR trade-off optimization

### 6. Dataset Foundation - Paper 6
- **Binary-30K**: 29,793 unique binaries (SHA-256 deduplicated)
- **Platform Coverage**: Windows 57.86%, Linux 28.37%, macOS 1.91%, Android 0.55%
- **Architecture Diversity**: x86-64 (56.40%), ARM (9.39%), MIPS (2.28%), RISC-V, PowerPC, SuperH, m68k
- **Pre-Tokenization**: Binary BPE (65,536-token vocabulary), transformer-ready Arrow/Parquet
- **Temporal Span**: 2012-2025 (benign), 2017-2025 (malware)

## Key Metrics

| Component | Metric | Performance |
|-----------|--------|-------------|
| BinSeek Embedding | Rec@3 | 80.5% |
| BinSeek Reranker | Rec@3 | 84.5% |
| Full System | MRR@3 | 80.25% |
| AppPoet Multi-View | Accuracy | 97.15% |
| GCN Null Pointer | Accuracy | 93.23% |
| FORGE Discovery | Precision | 72.3% on 3,457 binaries |
| Polymorphic Detection | Combined DR | 92% |

## Technical Stack

- **Binary Analysis**: IDA Pro decompilation, LIEF parsing, GCN graph models
- **LLM Layer**: DeepSeek-V3 (671B) for generation, embeddings for retrieval
- **Tokenization**: Byte-level BPE (65,536 vocab), x86-64 instruction parser
- **Graph Processing**: PyTorch Geometric for CFG/DFG analysis
- **Dataset**: Hugging Face Binary-30K (~11 GB compressed)

## Installation

```bash
# Clone repository
git clone https://github.com/zellkernel/epistasis.git
cd epistasis

# Install dependencies
pip install -r requirements.txt

# Download Binary-30K dataset
python scripts/download_dataset.py

# Configure LLM backend
cp config.example.yaml config.yaml
# Edit config.yaml with your LLM API keys
```

## Quick Start

```python
from epistasis import EpistasisAnalyzer

# Initialize analyzer
analyzer = EpistasisAnalyzer(
    dataset="binary-30k",
    model="deepseek-v3",
    mode="foa"  # Forest of Agents
)

# Analyze binary
result = analyzer.analyze(
    binary_path="target.exe",
    tasks=["vulnerability", "malware", "similarity"]
)

# Discovery phase: parallel agent search
findings = result.discoveries  # Candidate vulnerabilities

# Validation phase: adversarial verification
verified = result.validated  # Evidence-backed findings

# Cross-modal retrieval
similar_funcs = analyzer.search(
    query="buffer overflow in network packet parser",
    top_k=10
)
```

## Multi-Agent Workflow

```python
# Forest of Agents orchestration
foa = ForestOfAgents(
    discovery_agents=5,  # Parallel discovery
    validation_agents=3,  # Adversarial verification
    evidence_threshold=0.7
)

# Discovery: parallel structure materialization
discoveries = foa.discover(
    binary="malware.exe",
    views=["permission", "api", "url"]  # Multi-view analysis
)

# Validation: evidence-constrained execution
validated = foa.validate(
    candidates=discoveries,
    mode="adversarial",  # Skeptical verification
    evidence_chains=True
)

# Report with evidence
report = foa.report(
    findings=validated,
    format="markdown",
    include_evidence=True
)
```

## Cross-Modal Search

```python
# BinSeek: NL → Binary retrieval
binseek = BinSeekRetrieval(
    embedding_model="binseek-embedding-0.3B",
    reranker_model="binseek-reranker-0.6B"
)

# Stage 1: Embedding retrieval (fast)
candidates = binseek.embed_retrieve(
    query="function that parses HTTP headers",
    binary="web_server.exe",
    top_k=10
)

# Stage 2: Context-aware reranking
ranked = binseek.rerank(
    query="function that parses HTTP headers",
    candidates=candidates,
    context_mode="callees",  # Include calling context
    top_k=3
)

# Results with context
for func in ranked:
    print(f"{func.name} @ {func.address} - Score: {func.score}")
    print(f"Callees: {func.callees}")
    print(f"Pseudocode:\n{func.decompiled}")
```

## Polymorphic Testing

```python
# Mutation engine for evasion testing
mutator = PolymorphicMutator(
    behaviors=[
        "junk_code",
        "control_flow_obfuscation",
        "packing",
        "data_encoding",
        "dga",
        "beacon_timing",
        "protocol_mimicry",
        "header_modification"
    ]
)

# Generate variants
variants = mutator.generate(
    seed_binary="malware.exe",
    count=100,
    seed=42  # Reproducibility
)

# Multi-layer detection
detector = MultiLayerDetector(
    layers=["av", "yara", "edr"]
)

results = detector.test(
    variants=variants,
    tune_yara=True  # Iterative rule tuning
)

print(f"AV DR: {results.av_dr}%")
print(f"YARA DR: {results.yara_dr}%")
print(f"EDR DR: {results.edr_dr}%")
print(f"Combined DR: {results.combined_dr}%")
```

## Research Foundation

Built on 6 peer-reviewed papers:

1. **Adaptive Detection of Polymorphic Malware** (arXiv:2511.21764v1)
2. **FORGE: Feedback-Driven Execution for LLM-Based Binary Analysis** (arXiv:2604.15136v1)
3. **AppPoet: LLM-Based Android Malware Detection** (arXiv:2404.18816v3)
4. **Deep Learning-Based Binary Analysis for x86-64 Machine Code** (arXiv:2601.09157v1)
5. **BinSeek: Cross-Modal Retrieval Models for Stripped Binary Analysis** (arXiv:2512.10393v2)
6. **Binary-30K: Heterogeneous Dataset for Deep Learning** (arXiv:2511.22095v1)

## License

MIT License - see LICENSE file for details

## Citation

```bibtex
@software{epistasis_2025,
  title={Epistasis: Multi-Agent Binary Analysis Framework},
  year={2025},
  url={https://github.com/zellkernel/epistasis}
}
```
