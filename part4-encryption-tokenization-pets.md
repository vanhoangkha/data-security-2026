# Part 4: Encryption, Tokenization & Privacy-Enhancing Technologies (PETs)

## Giới thiệu

Classification phát hiện dữ liệu nhạy cảm. DLP/DSPM ngăn chặn và giám sát. Nhưng **Encryption & PETs** là tuyến phòng thủ cuối cùng — nếu dữ liệu bị lộ, nó vẫn không thể đọc được. Năm 2026, công nghệ này đã vượt qua encryption đơn giản, với Fully Homomorphic Encryption (FHE) cho phép **compute on encrypted data** và Confidential Computing bảo vệ **data-in-use** ở hardware level.

---

## 1. Data Protection ở 3 trạng thái

| Trạng thái | Mô tả | Giải pháp truyền thống | Giải pháp tiên tiến 2026 |
|-----------|-------|----------------------|--------------------------|
| **At Rest** | Lưu trữ trong DB, file system, backup | AES-256, TDE, disk encryption | Column-level encryption, KMS-managed |
| **In Transit** | Di chuyển qua network | TLS 1.3, VPN, mTLS | Post-quantum TLS, certificate-pinning |
| **In Use** | Đang xử lý trong memory | Không bảo vệ (traditional) | **FHE, Confidential Computing, TEE** |

> **Gap lớn nhất** = Data-in-use. Đây là nơi data phải decrypt để xử lý, tạo window cho attacks. PETs 2026 đóng gap này.

---

## 2. Encryption Technologies — State of Art 2026

### 2.1 Symmetric & Asymmetric Encryption

| Technology | Strength 2026 | Use Case | Performance |
|-----------|---------------|----------|-------------|
| AES-256-GCM | Standard | Data at rest, TDE | Excellent (hardware-accelerated) |
| ChaCha20-Poly1305 | Standard | TLS, mobile | Excellent |
| RSA-4096 | Adequate (pre-PQC) | Key exchange | Slow for bulk data |
| Ed25519/X25519 | Standard | Digital signatures, key agreement | Fast |

### 2.2 Post-Quantum Cryptography (PQC)

**Timeline 2026:**
- NIST PQC standards finalized (FIPS 203, 204, 205 — ML-KEM, ML-DSA, SLH-DSA) in August 2024
- NSA CNSA 2.0 **mandates PQC for national security systems starting 2027**
- NIST plans **deprecate quantum-vulnerable algorithms by 2035**, high-risk systems much earlier
- "Harvest now, decrypt later" attacks đang diễn ra — data encrypted hôm nay bị decrypt trong 5-10 năm
- **Hybrid mode** là recommended approach: Classical + PQC algorithms chạy song song
- **G7 roadmap** published January 2026 — PQC migration cho global financial systems
- Google publicly called on governments "prepare now" (February 2026) after Willow quantum processor breakthrough
- **NIST mandates** quantum-resistant algorithm implementation **by May 2026** cho enterprise
- BIS Project Leap proved FHE/PQC feasibility in live payment systems

| Algorithm | Type | Status 2026 |
|-----------|------|-------------|
| ML-KEM (Kyber) | Key encapsulation | FIPS 203 — Production ready |
| ML-DSA (Dilithium) | Digital signature | FIPS 204 — Production ready |
| SLH-DSA (SPHINCS+) | Stateless hash signatures | FIPS 205 — Production ready |
| FN-DSA (FALCON) | Fast signatures | Expected 2025 (4th standard) |

**Urgency (2026):** AI accelerating progress toward capable quantum adversary. AI cũng making enterprise PQC migration tractable (IEEE Computer, June 2026). "Readiness is no longer a 2035 problem."

**Action cho enterprise:** Crypto inventory → identify vulnerable systems → hybrid deployment → full migration path.

### 2.3 Format-Preserving Encryption (FPE)

- Encrypt data mà giữ nguyên format (e.g., credit card vẫn là 16 digits)
- Use case: Analytics on encrypted data, legacy system compatibility
- Standard: NIST SP 800-38G (FF1, FF3-1)
- Cho phép encrypted data flow qua systems mà không cần modify schemas

---

## 3. Tokenization

### 3.1 Tokenization vs. Encryption

| Dimension | Tokenization | Encryption |
|-----------|-------------|-----------|
| **Reversibility** | Only with token vault | With correct key |
| **Mathematical relationship** | None (random mapping) | Algorithmic |
| **Performance** | Fast lookup | Compute-intensive (varies) |
| **Compliance scope** | De-scopes systems (PCI DSS) | Reduces risk but stays in scope |
| **Use case** | Payment data, PII in analytics | General data protection |

### 3.2 Tokenization Patterns 2026

| Pattern | Description | Example |
|---------|-----------|---------|
| **Vault-based** | Central vault maps tokens to original values | Traditional PCI tokenization |
| **Vaultless** | Deterministic token generation (no central store) | High-performance analytics |
| **Dynamic** | Different token per context/session | Multi-tenant SaaS |
| **Format-preserving** | Token maintains data format | Credit card: 4532-XXXX-XXXX-7891 |

### 3.3 Tokenization cho AI

**Bài toán mới:** Làm sao dùng sensitive data cho AI training/inference mà không expose original values?

- **Pre-training tokenization:** Replace PII trong training data bằng synthetic tokens
- **Prompt tokenization:** Tokenize sensitive fields trước khi gửi tới LLM API
- **Output de-tokenization:** Restore original values after LLM processing (controlled)
- **Consistent tokenization:** Cùng entity → cùng token để maintain relationships

---

## 4. Fully Homomorphic Encryption (FHE) — The 2026 Breakthrough

### 4.1 FHE là gì?

FHE cho phép **thực hiện computation trên encrypted data mà không cần decrypt**. Kết quả sau khi decrypt giống hệt như compute trên plaintext.

```
Plaintext → Encrypt → Compute on Ciphertext → Decrypt → Same Result
    A                    f(Enc(A))                       f(A) ✓
```

### 4.2 FHE Maturity 2026

| Aspect | Status |
|--------|--------|
| **Correctness** | Proven — mathematically sound |
| **Performance** | 10-1000x overhead (improving) — viable cho specific workloads |
| **Standardization** | ISO/IEC 18033-6 in progress |
| **Production use** | Limited scope — healthcare analytics, financial computations, AI inference |
| **Libraries** | Microsoft SEAL, IBM HElib, TFHE, OpenFHE, Zama's Concrete |
| **Cloud support** | AWS, Azure, GCP offer FHE-compatible instances |

### 4.3 FHE cho AI Inference (2026 Frontier)

**Paper:** "Scaling up Privacy-Preserving ML" (arXiv 2601.18511) — FHE cho confidential LLM inference.

Use cases:
- **Medical AI:** Patient data encrypted → AI diagnosis → result decrypted only by doctor
- **Financial AI:** Transaction patterns analyzed encrypted → fraud detection without raw data exposure
- **Multi-party ML:** Multiple organizations train joint model without sharing raw data

**Hạn chế:**
- Performance overhead vẫn significant (10-100x cho complex operations)
- Token length limitation cho LLM inference
- Key management complexity

### 4.4 FHE vs. Confidential Computing

| Dimension | FHE | Confidential Computing |
|-----------|-----|----------------------|
| Protection mechanism | Mathematical (encryption) | Hardware (TEE) |
| Performance overhead | 10-1000x | 2-15% |
| Trust model | Cryptographic proof | Hardware attestation |
| Compute complexity | Limited operations | Full general-purpose |
| Maturity | Production for specific tasks | Mainstream |
| Best for | Cross-party computation, compliance | General secure processing |

---

## 5. Confidential Computing & TEE

### 5.1 Trusted Execution Environment (TEE) Technologies

| Technology | Vendor | Isolation Level | Use Case 2026 |
|-----------|--------|----------------|---------------|
| **Intel TDX** | Intel | VM-level | Confidential VMs, multi-tenant cloud |
| **AMD SEV-SNP** | AMD | VM-level | Cloud workloads, sovereign cloud |
| **Intel SGX** | Intel | Process-level (enclave) | Key management, attestation |
| **NVIDIA H100/H200 CC** | NVIDIA | GPU-level | **Confidential AI inference** |
| **ARM CCA** | ARM | Realm-level | Edge, IoT, mobile |

### 5.2 Confidential Computing for AI (2026)

```
┌─────────────────────────────────────────────┐
│        Confidential AI Pipeline              │
├─────────────────────────────────────────────┤
│                                              │
│  ┌──────────────┐    ┌───────────────────┐  │
│  │  Encrypted   │───▶│  TEE / Conf. GPU  │  │
│  │  Input Data  │    │  (AMD SEV / TDX)  │  │
│  └──────────────┘    │                   │  │
│                      │  AI Model runs    │  │
│  ┌──────────────┐    │  inside TEE       │  │
│  │  Encrypted   │◀───│                   │  │
│  │  Output      │    │  Attestation ✓    │  │
│  └──────────────┘    └───────────────────┘  │
│                                              │
│  Neither cloud provider nor other tenants    │
│  can access data or model weights            │
└─────────────────────────────────────────────┘
```

**Key developments 2026:**
- NVIDIA H100/H200 confidential GPUs cho AI inference
- VMware Cloud Foundation 9.0 tích hợp confidential computing natively
- 75%+ regulated workloads on cloud yêu cầu TEE (compliance requirement)
- Remote attestation cho phép verify code integrity trước khi send data

### 5.3 Confidential Computing Use Cases

| Use Case | Technology | Benefit |
|----------|-----------|---------|
| Multi-tenant AI hosting | Intel TDX + NVIDIA CC | Model weights protected from cloud provider |
| Healthcare AI | AMD SEV-SNP | Patient data never leaves TEE |
| Financial ML | Confidential VMs | Regulatory compliance for data residency |
| Federated learning | TEE + FHE hybrid | Cross-org collaboration without data sharing |
| Key management | Intel SGX | HSM-equivalent in cloud |

---

## 6. Privacy-Enhancing Technologies (PETs) Portfolio

### 6.1 PETs Landscape 2026

| Technology | Maturity | Performance | Use Case |
|-----------|----------|------------|----------|
| **FHE** | Production (limited scope) | 10-1000x overhead | Cross-party computation |
| **Confidential Computing** | Mainstream | 2-15% overhead | General secure processing |
| **Differential Privacy** | Growing | Minimal overhead | AI training, analytics |
| **Secure Multi-Party Computation (SMPC)** | Production | 100-1000x overhead | Joint computation |
| **Federated Learning** | Mainstream | Network-dependent | Distributed AI training |
| **Synthetic Data** | Mainstream | Generation time | Testing, development |
| **Zero-Knowledge Proofs** | Growing | Variable | Authentication, attestation |

### 6.2 Differential Privacy cho AI

- **Concept:** Thêm calibrated noise vào data/model outputs để prevent individual identification
- **Apple, Google, Microsoft** đều sử dụng cho product telemetry
- **2026 AI application:**
  - Training LLMs với differential privacy guarantees
  - Publishing AI model outputs mà không leak training data
  - Federated learning với privacy budget management

### 6.3 Synthetic Data

- Tạo data có statistical properties giống real data nhưng không chứa actual PII
- **Use cases 2026:**
  - AI/ML model training mà không cần production data
  - Test environments với realistic data
  - Sharing data across organizations without privacy risk
- **Vendors:** Mostly.ai, Hazy, Tonic.ai, Gretel.ai

---

## 7. Enterprise Implementation Strategy

### 7.1 Encryption Decision Matrix

```
Data Sensitivity: HIGH + Data Access Pattern: ANALYTICS
    → Format-Preserving Encryption hoặc Tokenization

Data Sensitivity: HIGH + Data Access Pattern: AI INFERENCE
    → Confidential Computing (TEE) hoặc FHE (nếu cross-party)

Data Sensitivity: MEDIUM + Data Access Pattern: STORAGE
    → AES-256 at rest + TLS 1.3 in transit

Data Sensitivity: CRITICAL + Data Access Pattern: MULTI-PARTY
    → FHE hoặc SMPC

Data Sensitivity: ANY + Environment: PUBLIC CLOUD
    → Confidential VMs (TDX/SEV-SNP) + Customer-managed keys

Data Type: AI TRAINING DATA
    → Differential Privacy + Synthetic Data augmentation
```

### 7.2 Key Management Best Practices 2026

1. **Customer-managed keys (CMK)** cho cloud storage — không dùng provider-managed
2. **Hardware Security Modules (HSM)** cho key storage — cloud HSMs (AWS CloudHSM, Azure Managed HSM)
3. **Key rotation** tự động — 90 days cho data keys, 365 days cho master keys
4. **Separation of duties** — Key admins ≠ data admins
5. **Crypto-agility** — Ability to switch algorithms (post-quantum readiness)
6. **Key escrow** — Disaster recovery cho critical keys

### 7.3 Post-Quantum Migration Roadmap

```
Phase 1 (Now): Crypto Inventory
├── Identify all cryptographic assets
├── Map algorithms to applications
└── Assess "harvest now, decrypt later" risk

Phase 2 (2026-2027): Hybrid Deployment
├── Deploy hybrid classical + PQC for critical systems
├── Upgrade TLS libraries to support ML-KEM
└── Test application compatibility

Phase 3 (2027-2029): Full Migration
├── Replace vulnerable algorithms
├── Update certificate infrastructure
└── Validate performance and interoperability
```

---

## 8. Tổng kết

Encryption & PETs trong 2026:

1. **Data-in-use protection** là frontier mới — FHE và Confidential Computing đóng gap cuối cùng
2. **FHE đã production-ready** cho specific AI inference workloads, nhưng performance overhead vẫn là trade-off
3. **Confidential Computing (TEE)** là mainstream choice cho regulated AI — 2-15% overhead, general-purpose
4. **Post-quantum migration** phải bắt đầu ngay — "harvest now, decrypt later" attacks đang diễn ra
5. **Tokenization cho AI** là pattern mới — protect PII khi data flows qua LLM pipelines
6. **Portfolio approach** — Không có single PET giải quyết mọi bài toán; cần combination

Phần tiếp theo sẽ đi sâu vào **Data Security cho AI/ML & Cloud Native** — bảo vệ dữ liệu trong hệ sinh thái AI đang bùng nổ.

---

## Nguồn tham khảo

- [A Privacy-Preserving Framework using Homomorphic Encryption — arXiv 2604.23245](https://arxiv.org/html/2604.23245v1)
- [Scaling up Privacy-Preserving ML with FHE — arXiv 2601.18511](https://arxiv.org/abs/2601.18511)
- [Post-Quantum Homomorphic Encryption — arXiv 2504.16091](https://arxiv.org/html/2504.16091)
- [FHE in 2026: Computing on Encrypted Data — KuCoin Research](https://www.kucoin.com/news/articles/fhe-in-2026-computing-on-encrypted-data-and-the-projects-making-private-blockchains-a-reality)
- [Confidential Computing: AMD SEV vs Intel TDX — Phala](https://phala.com/learn/AMD-SEV-vs-Intel-TDX-vs-NVIDIA-GPU-TEE)
- [Confidential VMs Explained — ACM](https://dl.acm.org/doi/10.1145/3700418)
- [Confidential Computing in VMware Cloud Foundation 9.0](https://blogs.vmware.com/cloud-foundation/2025/08/06/confidential-computing-vmware-cloud-foundation-9-0/)
- [FHE and Confidential Computing — IBM Research](https://www.research.ibm.com/blog/fhe-cc-security)
- [Confidential Computing for Secure AI Inference — MarkAICode](https://markaicode.com/confidential-computing-secure-ai-inference/)
- [FHE vs Confidential Computing — Cloud Security Alliance](https://cloudsecurityalliance.org/blog/2024/08/22/understanding-the-differences-between-fully-homomorphic-encryption-and-confidential-computing)
- [Protecting User Data with FHE — IBM](https://www.research.ibm.com/blog/fhe-cc-security)
