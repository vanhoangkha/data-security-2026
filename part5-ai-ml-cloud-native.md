# Part 5: Data Security cho AI/ML & Cloud Native

## Giới thiệu

AI và Cloud Native là hai lực lượng đang **định hình lại hoàn toàn** bài toán Data Security. AI tạo ra loại data mới cần bảo vệ (training data, embeddings, model weights), đồng thời tạo ra vectors tấn công chưa từng có (data poisoning, prompt injection, model inversion). Cloud Native với containers, serverless, và distributed storage khiến data boundary trở nên mờ nhạt hơn bao giờ hết.

Phần này tập trung vào: **Bảo vệ dữ liệu trong và xung quanh AI systems**, và **data security cho cloud-native architectures**.

---

## 1. AI Data Security — Threat Landscape 2026

### 1.1 Định nghĩa

> "AI data security is the practice of protecting both the data that fuels AI systems and the AI systems themselves across the machine-learning lifecycle: collection, preprocessing, training, fine-tuning, inference and decommissioning."  
> — CISA/NSA Joint CSI on AI Data Security (May 2025)

### 1.2 AI Data Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                    AI DATA LIFECYCLE                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Collection → Preprocessing → Training → Fine-tuning             │
│      │              │             │            │                  │
│      ▼              ▼             ▼            ▼                  │
│  [Data sources] [Cleaning,   [Model      [Domain-specific       │
│   APIs, DBs,    labeling,    training]    adaptation]            │
│   web scraping]  dedup]                                          │
│                                                                  │
│  → Deployment → Inference → Monitoring → Decommissioning         │
│       │             │            │              │                 │
│       ▼             ▼            ▼              ▼                 │
│  [API serving, [User prompts, [Drift,      [Safe disposal       │
│   integration]  RAG retrieval] anomalies]   of model + data]    │
│                                                                  │
│  ⚠️ SECURITY REQUIRED AT EVERY STAGE ⚠️                         │
└─────────────────────────────────────────────────────────────────┘
```

### 1.3 12 AI-Specific Data Threats (Chi tiết)

#### Critical Threats:

**1. Data Poisoning**
- Adversary chèn crafted examples vào training/fine-tuning/RAG data
- Chỉ 0.1% poisoned data có thể **thay đổi model behavior đáng kể**
- Model continues functioning normally nhưng produce biased/manipulated results
- **Defenses:** Data provenance, dataset signing, version control, anomaly detection

**2. Prompt Injection (Direct)**
- User input chứa instructions bypass system prompt
- Model ignores guardrails, leaks data, performs unauthorized actions
- **Defenses:** System prompt isolation, structured I/O format, input sanitization

**3. Prompt Injection (Indirect)**
- Instructions embedded trong documents, emails, webpages mà model reads
- Model trusts its own pipeline → follows malicious instructions
- **Defenses:** Content sanitization from untrusted sources, gated tool calls

**4. Supply Chain Compromise**
- Malicious models trên HuggingFace, poisoned dependencies, compromised training pipelines
- **Defenses:** Model scanning, artifact verification, SBOM for AI

#### High Threats:

**5. Model Inversion**
- Adversary dùng specially crafted queries để deduce training data
- PII, medical data, proprietary info bị reconstruct
- **Defenses:** Differential privacy, output perturbation, query rate limits

**6. Training Data Leakage**
- Model memorizes training data → reproduce in outputs
- LLMs đặc biệt vulnerable do massive dataset memorization
- **Defenses:** Data deduplication, memorization audits, output filtering

**7. Shadow AI Exposure**
- Employees dùng AI tools không được approve
- Data paste vào consumer ChatGPT, undocumented LLM APIs integrated vào production
- **Cost:** $670K thêm/breach so với organizations có controlled AI usage
- **2026 Evolution (CIO.com, June 2026):** Shadow AI morphed từ "data leaks" sang **"shadow operations"** — shadow agents với high-privilege access risk enterprise integrity without DevSecOps oversight
- **Scale (Grip Security):** 100% analyzed companies operate SaaS environments with embedded AI; 490% YoY spike in public SaaS attacks; 80% incidents involve PII/customer data
- **Defenses:** AI inventory, sanctioned gateway, employee policy, MCP monitoring

**8. Excessive Agency**
- AI agents có too many permissions → perform unauthorized actions
- RAG + tool use = expanded attack surface
- **Defenses:** Least privilege for agents, human-in-the-loop for side effects

---

## 2. Frameworks cho AI Data Security

### 2.1 NIST AI Risk Management Framework (AI RMF)

```
GOVERN → Establish AI governance, accountability, policies
   │
MAP → Identify AI systems, data flows, risk context
   │
MEASURE → Test, red-team, monitor continuously
   │
MANAGE → Incident response, retirement, improvement
```

**Key requirement:** 63% breached organizations had no AI governance policy (IBM 2025). GOVERN function closes this gap.

### 2.2 OWASP Top 10 for LLM Applications (2025)

| # | Risk | Data Security Implication |
|---|------|--------------------------|
| LLM01 | Prompt Injection | Unauthorized data access via manipulated prompts |
| LLM02 | Sensitive Information Disclosure | Training data leakage in outputs |
| LLM03 | Supply Chain | Poisoned models/data from third parties |
| LLM04 | Data and Model Poisoning | Corrupted training data → wrong outputs |
| LLM05 | Insecure Output Handling | Outputs containing sensitive data passed downstream |
| LLM06 | Excessive Agency | Agent performing unauthorized data operations |
| LLM07 | System Prompt Leakage | Revealing system instructions containing secrets |
| LLM08 | Vector and Embedding Weaknesses | Poisoned or extracted embeddings |
| LLM09 | Misinformation | Hallucinated data presented as fact |
| LLM10 | Unbounded Consumption | Resource exhaustion, denial of service |

### 2.3 CISA/NSA Joint Guidance (May 2025)

**Core requirements:**
- Cryptographic protection cho training datasets và model weights (at rest + in transit)
- Digital signatures trên training data (detect tampering)
- Data provenance tracking across AI pipeline
- Access controls cho model APIs, fine-tuning jobs, vector DBs

### 2.4 EU AI Act Article 15 (Deadline: Aug 2, 2026)

- High-risk AI phải resilient against data poisoning + adversarial attacks
- Continuous red teaming là cách demonstrate compliance
- Fine: **€35M hoặc 7% global turnover**
- Data governance obligations trong Article 10

---

## 3. Securing AI Data — Practical Controls

### 3.1 Training Data Security

| Control | Implementation |
|---------|---------------|
| **Data provenance** | Track origin của every training sample |
| **Dataset signing** | Cryptographic signatures detect tampering |
| **Version control** | Git-like versioning cho datasets (DVC, LakeFS) |
| **Access control** | RBAC + MFA cho training data repositories |
| **Deduplication** | Reduce memorization risk |
| **PII scrubbing** | Remove/tokenize PII trước training |
| **Anomaly detection** | Flag statistical outliers trong training data |

### 3.2 Inference Security

| Control | Implementation |
|---------|---------------|
| **Prompt filtering** | Block sensitive data in input prompts |
| **Output sanitization** | Filter PII/secrets from model outputs |
| **Rate limiting** | Prevent model inversion via query volume |
| **Logging** | Log all prompts + outputs for audit |
| **DLP integration** | Apply DLP rules to AI I/O |
| **Sanctioned AI gateway** | Route all AI usage through approved tools |

### 3.3 RAG & Agent Security

```
RAG Security Checklist:
├── Document provenance — Track source of every doc in corpus
├── Content classification — Classify docs before indexing
├── Access-aware retrieval — Only retrieve docs user is authorized to see
├── Indirect injection defense — Sanitize retrieved content
├── Embedding encryption — Encrypt vector DB at rest
├── Agent tool gating — Human approval for side-effect actions
├── Observability — Log every retrieval, tool call, output
└── Continuous red teaming — Test agent's full pipeline adversarially
```

### 3.4 Shadow AI Mitigation

**3-control framework (IBM/Mindgard):**

1. **Live AI Inventory** — Discover every AI system, model, agent, integration
2. **Sanctioned AI Gateway** — Route employee AI usage through approved tools with DLP + logging
3. **Employee Policy** — Define what data may/may not be shared with AI tools

---

## 4. Cloud Native Data Security

### 4.1 Cloud Data Security Challenges

| Challenge | Root Cause | Impact |
|-----------|-----------|--------|
| **Data sprawl** | Easy provisioning, copy-on-read | Unknown sensitive data locations |
| **Ephemeral workloads** | Containers, serverless | Data exists briefly, hard to track |
| **Shared responsibility** | Provider secures infra, customer secures data | Gaps at boundary |
| **Multi-cloud** | Different APIs, policies, tools | Inconsistent protection |
| **East-west traffic** | Service-to-service calls | Lateral movement risk |
| **Secrets in code** | API keys, tokens in repos/configs | Credential compromise |

### 4.2 Cloud Data Security Architecture

```
┌──────────────────────────────────────────────────────┐
│                  CLOUD DATA SECURITY                  │
├──────────────────────────────────────────────────────┤
│                                                       │
│  Layer 1: Data Discovery & Classification             │
│  ├── DSPM scanning all cloud storage                  │
│  ├── Automated classification                         │
│  └── Shadow data discovery                            │
│                                                       │
│  Layer 2: Access & Identity                           │
│  ├── IAM least privilege                              │
│  ├── Service-to-service mTLS                          │
│  ├── Secrets management (Vault, AWS SM)               │
│  └── Just-in-time access                              │
│                                                       │
│  Layer 3: Encryption & Key Management                 │
│  ├── Customer-managed keys (CMK)                      │
│  ├── Column/field-level encryption                    │
│  ├── Envelope encryption pattern                      │
│  └── Cloud HSM for key storage                        │
│                                                       │
│  Layer 4: DLP & Monitoring                            │
│  ├── Cloud DLP (API-integrated)                       │
│  ├── Data flow monitoring                             │
│  ├── Anomaly detection (UEBA)                         │
│  └── SIEM integration                                 │
│                                                       │
│  Layer 5: Compliance & Governance                     │
│  ├── Data residency enforcement                       │
│  ├── Retention policies                               │
│  ├── Audit logging                                    │
│  └── Compliance-as-code                               │
└──────────────────────────────────────────────────────┘
```

### 4.3 Kubernetes Data Security

| Control Area | Implementation |
|-------------|---------------|
| **Secrets Management** | External Secrets Operator → Vault/AWS SM (không dùng K8s secrets) |
| **Encryption at Rest** | etcd encryption, encrypted PV |
| **Network Policies** | Zero-trust east-west với Calico/Cilium |
| **Service Mesh** | Istio/Linkerd mTLS cho all service-to-service |
| **RBAC** | Namespace-scoped, least privilege |
| **Admission Control** | OPA/Kyverno block containers với sensitive mounts |
| **Image Security** | Signed images, vulnerability scanning |
| **Audit Logging** | K8s audit log → SIEM |

### 4.4 Serverless & Container Data Security

- **Ephemeral by nature** — data trong function memory biến mất sau execution
- **Risk:** Temporary storage (e.g., /tmp) có thể chứa sensitive data giữa invocations
- **Controls:** 
  - Memory encryption (Confidential VMs)
  - No persistent storage cho sensitive workloads
  - Environment variable encryption
  - Function-level IAM least privilege

---

## 5. Data Security cho Specific AI Architectures

### 5.1 RAG (Retrieval-Augmented Generation)

| Component | Data Risk | Protection |
|-----------|-----------|-----------|
| Document store | Confidential docs indexed | Classification before indexing |
| Vector DB | Embeddings contain data semantics | Encryption at rest, access control |
| Retrieval layer | Over-retrieval → data leakage | Access-aware filtering |
| Prompt assembly | Retrieved docs + user query | Content sanitization |
| Output | May include verbatim doc content | Output filtering, citations only |

### 5.2 Agentic AI

| Risk | Scenario | Control |
|------|----------|---------|
| Data exfiltration via tools | Agent sends sensitive data to external API | Tool-call gating, DLP on tool outputs |
| Unauthorized data access | Agent queries database beyond its scope | Fine-grained DB permissions per agent |
| Persistence of sensitive data | Agent stores conversation history with PII | Auto-purge, tokenization |
| Multi-hop attacks | Prompt injection causes agent to chain actions | Action budget limits, human approval |
| **MCP tool poisoning** | Malicious metadata in tool descriptions | Tool description validation, allowlisting |
| **Semantic data exfiltration** | Data transformed before leaving via MCP | Semantic DLP at MCP gateway |
| **Privilege escalation** | Compromised MCP server grants agent elevated access | Least privilege per tool, attestation |

### 5.2.1 MCP Security — 2026's Critical New Domain

**Five Eyes Guidance (May 1, 2026):** "Careful Adoption of Agentic AI Services" — treat agents as privileged identities:

```
MCP Security Checklist (Nightfall AI + Strac.io, 2026):
├── 1. Inventory all MCP connections and tool catalogs
├── 2. Classify data accessible via each MCP server
├── 3. Implement least-privilege per tool function
├── 4. Monitor all tool invocations as data movement events
├── 5. Detect semantic transformation of sensitive data
├── 6. Rate-limit agent actions (action budget)
├── 7. Human-in-the-loop for high-risk operations
├── 8. Validate tool descriptions against poisoning
├── 9. Audit trail for every MCP interaction
└── 10. Regular red teaming of full agent pipeline
```

**Key insight:** MCP traffic evades traditional DLP because:
- Looks like authorized API calls
- Data semantically transformed (not raw copy)
- Exfiltration via tool invocations, not file transfers

### 5.3 Federated Learning

- Training data **stays local** — only model gradients shared
- **Risks:** Gradient inversion can reveal training data
- **Protections:**
  - Secure aggregation (SMPC)
  - Differential privacy on gradients
  - Confidential computing cho aggregation server

---

## 6. Enterprise AI Data Security Checklist

### 10-Step Implementation (aligned with NIST AI RMF)

**GOVERN:**
1. ☐ Assign executive owner for AI security
2. ☐ Build live inventory of all AI systems (including shadow AI)

**MAP:**
3. ☐ Classify each AI system by risk level
4. ☐ Map all data flows in/out of AI systems

**MEASURE:**
5. ☐ Implement continuous AI red teaming (OWASP LLM Top 10)
6. ☐ Deploy runtime monitoring (prompt injection, jailbreak detection)

**MANAGE:**
7. ☐ Wire AI findings into SIEM/SOAR workflows
8. ☐ Build AI-specific incident response playbooks
9. ☐ Apply DLP + RBAC + MFA to all AI access points
10. ☐ Quarterly review cycle as new AI capabilities deploy

---

## 7. Tổng kết

Data Security cho AI/ML & Cloud Native 2026:

1. **AI creates entirely new threat surface** — 12 threats legacy tools cannot address
2. **Data poisoning chỉ cần 0.1%** — Nhỏ nhưng devastating
3. **Shadow AI = $670K extra per breach** — Governance không phải optional
4. **Frameworks đã mature** — NIST AI RMF, OWASP LLM Top 10, CISA/NSA guidance sẵn sàng
5. **EU AI Act deadline Aug 2, 2026** — High-risk systems phải compliant
6. **Cloud-native cần defense-in-depth** — Discovery, IAM, encryption, DLP, monitoring layered
7. **RAG và Agentic AI** — Mở rộng attack surface exponentially, cần controls mới

Phần cuối sẽ cover **Compliance, Governance & Lộ trình triển khai** — tổng hợp tất cả thành action plan.

---

## Nguồn tham khảo

- [AI Data Security 2026: Threats, Controls and Costs — Mindgard](https://mindgard.ai/blog/what-is-ai-data-security)
- [CISA/NSA Joint CSI on AI Data Security (May 2025)](https://media.defense.gov/2025/May/22/2003720601/-1/-1/0/CSI_AI_DATA_SECURITY.PDF)
- [Enterprise AI Security Overview — HiFlyLabs](https://hiflylabs.com/blog/2026/5/20/enterprise-ai-security-guide)
- [AI Data Protection 2026 — ConnectWise](https://www.connectwise.com/blog/ai-data-protection)
- [LLM Security Challenges & Best Practices 2026 — Lasso Security](https://www.lasso.security/blog/llm-security)
- [Generative AI Data Privacy in LLMs — Blockchain Council](https://www.blockchain-council.org/generative-ai/generative-ai-data-privacy-and-security-protecting-sensitive-data-in-llm-workflows/)
- [GDPR AI Training Data Compliance 2026 — Lyceum Technology](https://lyceum.technology/magazine/gdpr-ai-training-data-processing-guide/)
- [Privacy Risks in LLMs Survey — arXiv 2505.01976](https://arxiv.org/abs/2505.01976)
- [IBM Cost of a Data Breach Report 2025](https://www.ibm.com/reports/data-breach)
- [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/llm-top-10/)
- [EU AI Act Compliance Guide 2026 — Legiscope](https://legiscope.com/blog/eu-ai-act-compliance-guide.html)
- [Five Eyes Agentic AI Guidance — Crowell](https://www.crowell.com/en/insights/client-alerts/american-and-allied-cyber-agencies-issue-first-joint-guidance-on-securing-agentic-ai)
- [Shadow AI Morphs into Shadow Operations — CIO.com](https://www.cio.com/article/4162664/shadow-ai-morphs-into-shadow-operations.html)
- [Shadow AI: Hidden Risk Expanding — CrowdStrike](https://www.crowdstrike.com/en-us/blog/shadow-ai-hidden-risk-expanding-across-the-enterprise/)
- [Shadow AI SaaS Breaches — SecurityWeek/Grip Security](https://www.securityweek.com/the-shadow-ai-problem-how-saas-apps-are-quietly-enabling-massive-breaches/)
- [MCP Security: 9 Things Every CISO Needs to Know — Nightfall AI](https://nightfall.ai/blog/what-is-mcp-security-9-things-every-ciso-needs-to-know)
- [MCP Tool Poisoning Enterprise AI 2026 — ITECSOnline](https://itecsonline.com/post/mcp-tool-poisoning-enterprise-ai-agent-security-2026)
- [How to Monitor MCP Usage: 10-Step Checklist — Nightfall AI](https://nightfall.ai/blog/how-to-monitor-mcp-usage-a-10-step-security-checklist-for-2026)
- [Secure MCP Servers & Data 2026 — Strac.io](https://www.strac.io/blog/mcp-security)
- [Five Eyes Agentic AI Notes — CodiLime](https://codilime.com/blog/notes-on-five-eyes-agentic-ai-guidance/)
- [Treating Agentic AI as IAM Identities — Kiteworks](https://www.kiteworks.com/regulatory-compliance/cisa-agentic-ai-governance-iam/)
- [Trump EO: AI Innovation & Security — White House](https://www.whitehouse.gov/presidential-actions/2026/06/promoting-advanced-artificial-intelligence-innovation-and-security/)
