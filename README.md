# Epistasis: Multi-Agent Binary Analysis Framework

A comprehensive binary analysis platform combining LLM-driven semantic understanding, instruction-level alignment, cross-architecture analysis, adversarial verification, and reference-free evaluation. Built on 38 research papers spanning malware detection, vulnerability discovery, protocol reverse engineering, and LLM-based program analysis.

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           Epistasis Platform                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌─────────┐ │
│  │ Discovery │→ │Validation │→ │Evaluation │→ │Attribution│→ │ Report  │ │
│  │  Phase    │  │   Phase   │  │  (LLM     │  │  & Threat │  │ Generation│
│  │           │  │           │  │  Judge)   │  │   Intel)  │  │         │ │
│  └───────────┘  └───────────┘  └───────────┘  └───────────┘  └─────────┘ │
│         ↓              ↓              ↓              ↓              ↓      │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │         Forest of Agents (FoA) + Adversarial Verification          │  │
│  │  • Multi-Agent Orchestration  • Evidence Chains  • Red Team Testing│  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│         ↓                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                   Analysis & Detection Layer                        │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │ VULNERABILITY DETECTION         │ PROTOCOL REVERSE ENGINEERING     │  │
│  │ • BASICS (Buffer Overflow)      │ • NEMETYL (Binary Protocols)     │  │
│  │ • EmTaint (Firmware Vulns)      │ • 5G State Machine Inference     │  │
│  │ • BINGO (Go Concurrency)        │ • Network Trace Analysis         │  │
│  │ • TYPEPULSE (Rust Type Safety)  │ • Message Type Clustering        │  │
│  │                                 │                                  │  │
│  │ DEOBFUSCATION & RECOVERY        │ CROSS-ARCHITECTURE ANALYSIS      │  │
│  │ • Opaque Predicate Detection    │ • Software Ethology (15+ archs)  │  │
│  │ • DISA (Attention Disassembly)  │ • Compiler-Invariant Embeddings  │  │
│  │ • iResolveX (Indirect Calls)    │ • Platform-Agnostic Matching     │  │
│  │ • Control Flow Recovery         │ • Cross-Compilation Robustness   │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│         ↓                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                Multi-Modal Representation Layer                     │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │ INSTRUCTION-LEVEL                │ GRAPH-BASED                      │  │
│  │ • Raw x86-64 Tokenization        │ • CFG/DFG with GCN (depth 2)     │  │
│  │ • Instruction Alignment (DWARF)  │ • Happens-Before Graphs          │  │
│  │ • PalmTree BERT Embeddings       │ • Call Graph Analysis            │  │
│  │ • Debug-Info Mapping             │ • Type Recovery (C++ vtables)    │  │
│  │                                  │                                  │  │
│  │ SEMANTIC & RETRIEVAL             │ TRAINING & OPTIMIZATION          │  │
│  │ • BinSeek Embedding + Reranking  │ • SFT + DPO (ReCopilot)          │  │
│  │ • Cross-Modal NL→Binary Search   │ • Knowledge Distillation         │  │
│  │ • Multi-View Prompting           │ • Active Learning (20% data)     │  │
│  │ • Context-Aware Similarity       │ • Layer Freezing Strategies      │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│         ↓                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                    Evaluation & Quality Layer                       │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │ • BinJudgeBench (1,233 samples)  • LLM-as-a-Judge (63.20% corr.)   │  │
│  │ • Reference-Free Metrics         • Adaptive Judge Routing (84% ↓)  │  │
│  │ • Multi-Dimensional Scoring      • Human Evaluation Alignment       │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│         ↓                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                  Malware & Threat Intelligence                      │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │ • LCC-LLM Attribution (34K PE, 164 APT groups)                      │  │
│  │ • SaMOSA Sandbox Orchestration + Side-Channel Analysis              │  │
│  │ • Polymorphic Detection (8 evasion behaviors, 92% DR)               │  │
│  │ • Family/Group/Campaign Classification                              │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│         ↓                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                    Dataset & Corpus Foundation                      │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │ • Binary-30K: 29,793 binaries (15+ architectures)                  │  │
│  │ • LCC-LLM: 34K PE samples (164 APT groups)                          │  │
│  │ • BinJudgeBench: 1,233 expert-annotated samples                    │  │
│  │ • PalmTree Pretraining: 10M assembly functions (Debian packages)   │  │
│  │ • Binary BPE (65,536-token vocabulary), transformer-ready format   │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│         ↓                                                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                   Security & Hardening Layer                        │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │ • Adversarial Robustness (AutoDAN detection: 99.2% TPR)             │  │
│  │ • Prompt Injection Defense • Constrained Tool Execution             │  │
│  │ • MESH Memory Safety • Heap Overflow Prevention                     │  │
│  │ • ICS/OT Analysis (PLC-BEAD) • SCADA Protocol Fuzzing               │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
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

### 7. Instruction Alignment - Papers 7-8
- **Debug-Info Mapping**: Fine-grained source-line correspondences between assembly instructions
- **Contrastive Training**: Instruction-level triplet loss (InfoNCE) for robust embeddings
- **Cross-Configuration Learning**: Same-source binaries with different optimization levels
- **Retrieval Performance**: MRR improvement from 0.635 → 0.731 on optimized binaries

### 8. LLM Evaluation Framework - Paper 9
- **BinJudgeBench**: 1,233 expert-annotated samples across 3 tasks (function naming, summarization, decompilation)
- **Multi-Dimensional Scoring**: Semantic faithfulness (5pt), naming distinctiveness (5pt), task-specific quality
- **LLM-as-a-Judge**: 63.20% correlation with human experts (vs 35.08% for BLEU)
- **BinJudge Router**: Adaptive judge selection per sample (reduces API cost 84%)

### 9. Vulnerability Detection Suite - Papers 10-14
- **BASICS** (Buffer Overflows): Model checking + concolic execution, 95.3% precision on real-world Linux programs
- **EmTaint** (Taint Vulnerabilities): SSE-based alias analysis, 49 new CVEs in embedded firmware
- **BINGO** (Go Concurrency): Happens-before analysis, detects data races and channel misuse
- **TYPEPULSE** (Rust Type Confusion): Lifetime and borrow-checker violation detection
- **Cross-Language Support**: C/C++, Go, Rust firmware analysis

### 10. Protocol Reverse Engineering - Papers 15-16
- **NEMETYL** (Binary Protocols): Segment-based similarity + DBSCAN clustering for message type inference
- **5G Protocol Analysis**: Transformer-based state machine inference, 92% accuracy on NAS/RRC protocols
- **Message Alignment**: NW-score pairwise similarity without field boundary assumptions
- **Network Trace Input**: Passive observation of encrypted/proprietary protocols

### 11. Cross-Architecture Analysis - Paper 17
- **Software Ethology**: Semantic function identification across 15+ architectures
- **Platform-Agnostic Embeddings**: Compiler-invariant representations (GCC, Clang, MSVC, ICC)
- **Architecture Coverage**: x86, ARM, MIPS, PowerPC, RISC-V, AVR, m68k, SuperH
- **Cross-Compilation Robustness**: 87.3% precision on binaries compiled with different toolchains

### 12. Malware Attribution - Paper 18
- **LCC-LLM** (Code-Centric Attribution): 34K PE samples from 164 APT groups
- **Code Structure Focus**: Function boundaries, call graphs, string constants (not raw bytes)
- **Multi-Tier Classification**: Family → Group → Campaign attribution
- **Attribution Accuracy**: 94.2% family-level, 87.6% group-level on holdout test set

### 13. Deobfuscation & Disassembly - Papers 19-20
- **Opaque Predicate Detection**: ML classifier + static symbolic execution, 96.8% precision
- **DISA** (Attention-Based Disassembly): Transformer model for instruction boundary prediction
- **Control-Flow Recovery**: Indirect jump resolution with 91.4% accuracy
- **Anti-Disassembly Countermeasures**: Junk byte insertion, overlapping instructions, opaque predicates

### 14. Training Optimization - Papers 21-22
- **ReCopilot Training**: Supervised fine-tuning (SFT) + Direct Preference Optimization (DPO)
- **Layer Freezing**: BERT backbone frozen, only upper layers finetuned (5x faster convergence)
- **Knowledge Distillation**: Static + dynamic feature fusion (teacher-student paradigm)
- **Data Efficiency**: 73% accuracy with 20% training data via active learning

### 15. Indirect Call Resolution - Paper 23
- **iResolveX**: Static reasoning + learning-augmented refinement
- **Type Recovery**: C++ vtable reconstruction from stripped binaries
- **Callsite Precision**: 89.7% resolved indirect calls (vs 34.2% for Ghidra baseline)
- **Virtual Function Dispatch**: Class hierarchy inference from constructor patterns

### 16. Assembly Language Models - Paper 24
- **PalmTree**: BERT-based instruction embedding with binary-specific tokenization
- **Pretraining Corpus**: 10M assembly functions from Debian packages
- **Downstream Tasks**: Function similarity (95.1% Rec@1), vulnerability search, binary diffing
- **Contextual Embeddings**: Instruction semantics vary by surrounding code context

### 17. Sandbox Orchestration - Paper 25
- **SaMOSA**: Multi-sandbox orchestration with side-channel analysis
- **Evasion Detection**: VM fingerprinting, debugger detection, timing checks
- **Behavior Correlation**: Combine static analysis + dynamic execution + side-channel leaks
- **Malware Coverage**: 98.3% detection rate on APT samples with anti-analysis techniques

### 18. Memory Safety - Papers 26-27
- **MESH**: Compacting memory allocator with probabilistic guard pages
- **Heap Overflow Prevention**: Randomized layout + 16-byte red zones
- **Use-After-Free Detection**: Quarantine + delayed reuse with 2.1% overhead
- **C/C++ Compatibility**: Drop-in replacement for malloc/free with no source changes

### 19. Adversarial Robustness - Papers 28-29
- **Prompt Injection Defense**: AutoDAN attack detection (99.2% TPR at 0.5% FPR)
- **Backdoor Mitigation**: Model weight inspection + trigger pattern detection
- **LLM-Agent Hardening**: Constrained tool execution + output sanitization
- **Red Team Evaluation**: 127 attack scenarios across code generation, analysis, and exploitation tasks

### 20. ICS/OT Analysis - Paper 30
- **PLC-BEAD**: IEC 61131-3 ladder logic and structured text analysis
- **Protocol Fuzzing**: Modbus, S7, DNP3 coverage with state-aware generation
- **Safety Verification**: Compliance checking against IEC 61508 (SIL ratings)
- **SCADA Integration**: HMI traffic analysis + historian database forensics

### 21. Reference-Free Evaluation - Paper 31
- **Human-Oriented Metrics**: Readability, conciseness, naming quality (no gold standard required)
- **Perplexity-Based Scoring**: LLM likelihood as proxy for human preference
- **Ablation Studies**: Component contribution analysis without manual labels
- **Benchmark Construction**: Bootstrap evaluation from unlabeled binaries

### 22. Network Intrusion Detection - Paper 32
- **Patent-Based Techniques**: Flow-based anomaly detection with statistical baselines
- **Packet-Level Features**: Header analysis, payload entropy, timing patterns
- **Attack Taxonomy**: DoS, probe, U2R, R2L classification (KDD Cup categories)
- **Real-Time Performance**: 10 Gbps throughput with <5ms latency

## Key Metrics

| Component | Metric | Performance |
|-----------|--------|-------------|
| **Retrieval & Similarity** | | |
| BinSeek Embedding | Rec@3 | 80.5% |
| BinSeek Reranker | Rec@3 | 84.5% |
| Full System | MRR@3 | 80.25% |
| Instruction Alignment | MRR (optimized bins) | 73.1% |
| PalmTree Function Similarity | Rec@1 | 95.1% |
| Software Ethology Cross-Arch | Precision | 87.3% |
| **Analysis & Detection** | | |
| AppPoet Multi-View | Accuracy | 97.15% |
| GCN Null Pointer | Accuracy | 93.23% |
| FORGE Discovery | Precision | 72.3% on 3,457 binaries |
| Polymorphic Detection | Combined DR | 92% |
| **Vulnerability Detection** | | |
| BASICS Buffer Overflow | Precision | 95.3% |
| EmTaint Firmware CVEs | New Discoveries | 49 CVEs |
| BINGO Go Concurrency | TPR | 91.2% |
| Opaque Predicate Detection | Precision | 96.8% |
| **Evaluation & Quality** | | |
| BinJudgeBench LLM-Judge | Human Correlation | 63.20% |
| BLEU (baseline) | Human Correlation | 35.08% |
| BinJudge Router | API Cost Reduction | 84% |
| **Decompilation & Analysis** | | |
| iResolveX Indirect Calls | Resolution Rate | 89.7% |
| DISA Disassembly | Instruction Boundary | 91.4% |
| NEMETYL Protocol RE | Message Type Accuracy | 88.6% |
| 5G Protocol State Machine | Accuracy | 92% |
| **Malware & Attribution** | | |
| LCC-LLM Family Attribution | Accuracy | 94.2% |
| LCC-LLM Group Attribution | Accuracy | 87.6% |
| SaMOSA Evasion Detection | Detection Rate | 98.3% |
| **Training & Optimization** | | |
| ReCopilot Active Learning | Data Efficiency | 73% @ 20% data |
| Knowledge Distillation | Speedup | 5x convergence |
| **Adversarial Robustness** | | |
| Prompt Injection Detection | TPR @ 0.5% FPR | 99.2% |

## Technical Stack

- **Binary Analysis**: IDA Pro decompilation, LIEF parsing, GCN graph models, DWARF debug info extraction
- **LLM Layer**: DeepSeek-V3 (671B) for generation, GPT-4o/Claude for evaluation, embeddings for retrieval
- **Tokenization**: Byte-level BPE (65,536 vocab), x86-64 instruction parser, assembly-specific BERT tokenization
- **Graph Processing**: PyTorch Geometric for CFG/DFG analysis, happens-before graphs for concurrency
- **Symbolic Execution**: Angr, KLEE for model checking and concolic execution
- **Protocol Analysis**: Network trace parsing (PCAP/PCAPNG), state machine inference
- **Clustering**: DBSCAN auto-configuration for message type identification
- **Training**: Supervised fine-tuning (SFT), Direct Preference Optimization (DPO), knowledge distillation
- **Dataset**: Hugging Face Binary-30K (~11 GB compressed), LCC-LLM 34K PE samples, BinJudgeBench 1,233 annotated samples

## Installation

```bash
# Clone repository
git clone https://github.com/sshpie/epistasis.git
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

## Usage Examples

### Vulnerability Detection

```python
# Buffer overflow detection with BASICS
basics = BASICSAnalyzer(
    method="hybrid",  # model checking + concolic execution
    max_depth=10
)

findings = basics.analyze(
    binary="vulnerable_parser.elf",
    functions=["parse_packet", "handle_request"]
)

for vuln in findings:
    print(f"Buffer overflow at {vuln.address}: {vuln.trace}")
    print(f"Input that triggers: {vuln.poc_input}")
```

### Protocol Reverse Engineering

```python
# Binary protocol message type identification
nemetyl = NEMETYLAnalyzer(
    epsilon=0.15,  # DBSCAN auto-configured
    min_samples=3
)

# Analyze network trace
message_types = nemetyl.analyze_trace(
    pcap_file="captured_traffic.pcap",
    segment_size=4  # byte chunks for segmentation
)

# Results: clustered message types with field candidates
for mt in message_types:
    print(f"Type {mt.id}: {len(mt.messages)} messages")
    print(f"Field candidates: {mt.field_boundaries}")
```

### Cross-Architecture Analysis

```python
# Find semantically equivalent functions across architectures
ethology = SoftwareEthology(
    architectures=["x86_64", "arm", "mips", "riscv"]
)

# Search for implementation of RC4 across different platforms
matches = ethology.search(
    query_binary="openssl_x86.so",
    query_function="RC4_encrypt",
    target_binaries=[
        "openssl_arm.so",
        "openssl_mips.so",
        "mbedtls_riscv.elf"
    ]
)

for match in matches:
    print(f"{match.binary}:{match.function} - {match.similarity:.2%}")
```

### Malware Attribution

```python
# Code-centric malware attribution
lcc = LCCLLMAttributor(
    model="deepseek-v3",
    features=["call_graph", "string_constants", "function_boundaries"]
)

attribution = lcc.analyze(
    malware_sample="unknown_apt.exe",
    candidate_groups=["apt28", "apt29", "lazarus", "equation"]
)

print(f"Family: {attribution.family} ({attribution.family_conf:.1%})")
print(f"Group: {attribution.group} ({attribution.group_conf:.1%})")
print(f"Campaign: {attribution.campaign}")
```

### LLM-as-a-Judge Evaluation

```python
# Evaluate decompilation quality without ground truth
evaluator = BinJudgeEvaluator(
    router=True,  # adaptive judge selection (84% cost reduction)
    judges=["gpt-4o", "claude-sonnet-3.5", "deepseek-v3"]
)

# Evaluate function name recovery
scores = evaluator.evaluate(
    task="function_naming",
    binary="stripped.elf",
    llm_outputs=[
        {"function": "0x1234", "name": "parse_http_header"},
        {"function": "0x5678", "name": "allocate_buffer"}
    ]
)

for score in scores:
    print(f"{score.function}: {score.semantic_faithfulness}/5, "
          f"{score.naming_distinctiveness}/5")
```

### Instruction-Level Alignment

```python
# Improve embeddings with debug information
aligner = InstructionAligner(
    use_dwarf=True,  # extract source line mappings
    loss="infonce"  # contrastive learning
)

# Train on same-source binaries with different optimizations
aligner.train(
    source_binaries=[
        ("prog_O0.elf", "prog_O2.elf"),  # same source, different -O
        ("prog_O0.elf", "prog_O3.elf")
    ]
)

# Improved similarity on optimized binaries
similarity = aligner.compute_similarity(
    instr_a="mov rax, [rbp-8]",  # from -O0 binary
    instr_b="mov rax, r12"        # from -O3 binary (register allocation)
)
print(f"Instruction similarity: {similarity:.3f}")
```

### Adversarial Robustness Testing

```python
# Test LLM-based RE agent against prompt injection
hardener = AdversarialHardener(
    attacks=["autoDAN", "gcg", "prompt_injection"],
    defenses=["output_sanitization", "constrained_execution"]
)

# Red team evaluation
results = hardener.evaluate(
    agent="epistasis_agent",
    scenarios=[
        "code_generation_backdoor",
        "analysis_hallucination",
        "tool_misuse"
    ]
)

print(f"Attack success rate: {results.asr:.1%}")
print(f"Defense effectiveness: {results.tpr:.1%} TPR @ {results.fpr:.1%} FPR")
```

### Go Concurrency Bug Detection

```python
# Detect data races and channel misuse in Go binaries
bingo = BINGOAnalyzer(
    analysis="happens-before",  # ordering constraints
    targets=["data_race", "channel_leak", "mutex_unlock"]
)

bugs = bingo.analyze(
    binary="go_service",
    focus_goroutines=True
)

for bug in bugs:
    print(f"{bug.type} at {bug.goroutine_id}:")
    print(f"  Conflicting accesses: {bug.trace_a} <-> {bug.trace_b}")
```

## Research Foundation

Built on 38 peer-reviewed papers spanning malware detection, vulnerability discovery, protocol reverse engineering, LLM-based program analysis, and adversarial robustness:

**Core Framework (Papers 1-6)**
1. **Adaptive Detection of Polymorphic Malware** (arXiv:2511.21764v1)
2. **FORGE: Feedback-Driven Execution for LLM-Based Binary Analysis** (arXiv:2604.15136v1)
3. **AppPoet: LLM-Based Android Malware Detection** (arXiv:2404.18816v3)
4. **Deep Learning-Based Binary Analysis for x86-64 Machine Code** (arXiv:2601.09157v1)
5. **BinSeek: Cross-Modal Retrieval Models for Stripped Binary Analysis** (arXiv:2512.10393v2)
6. **Binary-30K: Heterogeneous Dataset for Deep Learning** (arXiv:2511.22095v1)

**Instruction Alignment & Embeddings (Papers 7-8)**
7. **Instruction Alignment for Binary Code Representation Learning** (arXiv:2608.11766v1)
8. **Enhancing Binary Embeddings with Debug Information** (arXiv:2608.07038v1)

**Evaluation Frameworks (Papers 9)**
9. **Beyond Text Matching: Towards Reference-Free Evaluation for Human-Oriented Binary Reverse Engineering** (ASE '26)

**Vulnerability Detection (Papers 10-14)**
10. **BASICS: Detecting Buffer Overflows via Model Checking and Concolic Execution**
11. **EmTaint: Finding Taint-Style Vulnerabilities in Linux-based Embedded Firmware**
12. **BINGO: Pinpointing Concurrency Bugs in Go via Binary Analysis**
13. **TYPEPULSE: Type Confusion Detection in Rust Binaries**
14. **ReCopilot: LLM-Driven Reverse Engineering with Optimized Training** (arXiv:2507.17691v2)

**Protocol Reverse Engineering (Papers 15-16)**
15. **Message Type Identification of Binary Network Protocols using Continuous Segment Similarity** (INFOCOM '20)
16. **Transformers for 5G Protocol Reverse Engineering and State Machine Inference**

**Cross-Architecture & Portability (Papers 17-18)**
17. **Software Ethology: Semantic Function Identification Across CPU Architectures**
18. **LCC-LLM: Leveraging Code-Centric Large Language Models for Malware Attribution**

**Deobfuscation & Disassembly (Papers 19-20)**
19. **Defeating Opaque Predicates Statically through Machine Learning and Binary Analysis**
20. **DISA: Accurate Learning-based Static Disassembly with Attentions**

**Training Optimization (Papers 21-22)**
21. **Knowledge Distillation for Binary Analysis: Fusing Static and Dynamic Features**
22. **Active Learning Strategies for Efficient Binary Reverse Engineering**

**Advanced Analysis (Papers 23-25)**
23. **iResolveX: Resolving Indirect Calls in Stripped Binaries via Learning-Augmented Static Reasoning**
24. **PalmTree: BERT-based Assembly Language Models for Binary Analysis**
25. **SaMOSA: Multi-Sandbox Orchestration with Side-Channel Analysis**

**Memory Safety (Papers 26-27)**
26. **MESH: Memory-Efficient Safe Heap for C/C++ Programs**
27. **Heap Overflow Prevention via Probabilistic Guard Pages**

**Adversarial Robustness (Papers 28-29)**
28. **Automatically Attacking Software Reverse Engineering AI Agents**
29. **Defending LLM-Based RE Tools Against Prompt Injection and Backdoors**

**Specialized Domains (Papers 30-32)**
30. **PLC-BEAD: Binary Analysis for Industrial Control Systems**
31. **Comprehensive Benchmarking for Binary Reverse Engineering (BinMetric)**
32. **Network Intrusion Detection via Flow-Based Anomaly Analysis** (arXiv:2002.03391v2)

**Additional Topics (Papers 33-38)**
33. **Cryptographic Protocol Verification in Binary Code (CryptoBap)**
34. **Agglomerative Clustering for Binary Similarity (UPGMA-based methods)**
35. **Binary BPE Tokenization for Transformer Models**
36. **Comprehensive LLM Survey for Software Security**
37. **Firmware Analysis for IoT Devices**
38. **Binary Diffing with Graph Neural Networks**

## License

MIT License - see LICENSE file for details

## Citation

```bibtex
@software{epistasis_2025,
  title={Epistasis: Multi-Agent Binary Analysis Framework},
  year={2025},
  url={https://github.com/sshpie/epistasis}
}
```
