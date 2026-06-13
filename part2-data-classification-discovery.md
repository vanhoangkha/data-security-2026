# Part 2: Data Classification & Discovery — Nền tảng của mọi Data Security Program

## Giới thiệu

Không thể bảo vệ thứ bạn không biết là gì và nằm ở đâu. **Data Classification & Discovery** là bước đầu tiên và quan trọng nhất trong bất kỳ chương trình Data Security nào. Năm 2026, với data sprawl qua hàng trăm SaaS apps, multi-cloud environments, và AI pipelines, bài toán này phức tạp hơn bao giờ hết — nhưng cũng có nhiều công cụ AI-native mạnh mẽ hơn để giải quyết.

> "Sensitive data discovery and classification is foundational for Zero Trust data security, privacy, and AI governance."  
> — Forrester Wave Q2 2026

---

## 1. Tại sao Data Classification quan trọng hơn bao giờ hết

### 1.1 Data Sprawl — Bài toán 2026

| Thống kê | Nguồn |
|----------|-------|
| 80%+ tổ chức regulated hoạt động cloud-first hoặc hybrid | Sesamedisk DLP Report 2026 |
| Sensitive data chảy vào AI tools qua prompts, RAG, fine-tuning | Forcepoint DSPM Trends |
| 97% tổ chức bị AI breach không có AI access controls | IBM 2025 |
| Overexposed repos, stale accounts, inherited access là root cause phổ biến | Forcepoint DSPM 2026 |

### 1.2 Classification là prerequisite cho mọi thứ khác

```
Classification → DLP Policies → DSPM Posture → Access Control → Encryption → Compliance
```

Nếu không biết dữ liệu nào là sensitive, bạn không thể:
- Áp đúng DLP policy
- Đánh giá data exposure risk
- Enforce encryption/tokenization cho đúng fields
- Demonstrate compliance cho auditors
- Govern data flowing into AI systems

---

## 2. Phương pháp Classification hiện đại

### 2.1 Ba lớp Classification

| Lớp | Phương pháp | Ví dụ |
|-----|-------------|-------|
| **Pattern Matching** | Regex, fingerprinting | Credit card numbers, SSN, API keys |
| **Contextual Analysis** | NLP, semantic understanding | "Lương tháng 6" trong email vs. báo cáo tài chính |
| **Behavioral Signals** | User activity, data flow | File download patterns bất thường |

### 2.2 AI-Powered Classification — 2026 State of Art

**Tại sao AI cần thiết:**
- Data format không consistent — sensitive info hiếm khi "announce itself" theo pattern cố định
- Volume quá lớn cho manual review
- Context quyết định sensitivity — cùng một con số có thể là public metric hoặc salary

**Capabilities mới (2026):**

1. **Large Language Model classification** — Hiểu semantic meaning, không chỉ patterns
2. **Multi-modal classification** — Images, audio, video chứa sensitive info
3. **Contextual sensitivity scoring** — Cùng data type, khác risk level tùy context
4. **Continuous re-classification** — Data sensitivity thay đổi theo thời gian
5. **AI Mesh** (Forcepoint) — Multiple AI models chạy song song cho accuracy

### 2.3 Mô hình 4-Tier cho AI Data Classification

Theo Iternal AI (2026), enterprise CISOs cần mô hình mới cho AI era:

| Tier | Tên | Deployment | Access |
|------|-----|-----------|--------|
| **Tier 1** | Public | Any environment | Unrestricted |
| **Tier 2** | Internal | Corporate-controlled | Authenticated employees |
| **Tier 3** | Confidential | Isolated environments | Role-based, encrypted |
| **Tier 4** | Restricted | Air-gapped / hardware-secured | Named individuals, MFA + audit |

**Đặc biệt cho AI:** Mỗi tier map tới deployment architecture cụ thể:
- Tier 1-2: Có thể dùng trong public AI tools (ChatGPT, Claude)
- Tier 3: Chỉ private/self-hosted AI
- Tier 4: Không được đưa vào bất kỳ AI system nào

---

## 3. Data Discovery — Tìm kiếm dữ liệu ở mọi nơi

### 3.1 Discovery Scope 2026

Data không chỉ nằm trong databases. Discovery phải cover:

```
┌─────────────────────────────────────────────────────────┐
│                  DATA DISCOVERY SCOPE                     │
├─────────────────────────────────────────────────────────┤
│  Cloud Storage: S3, GCS, Azure Blob, MinIO              │
│  SaaS Apps: M365, Google Workspace, Salesforce, Slack   │
│  Databases: RDS, DynamoDB, Snowflake, BigQuery          │
│  Code Repos: GitHub, GitLab (secrets, credentials)      │
│  Endpoints: Laptops, developer machines                 │
│  AI Systems: Vector DBs, RAG corpora, embeddings        │
│  Collaboration: Teams, Confluence, Notion               │
│  Email: Exchange, Gmail (attachments, body text)        │
└─────────────────────────────────────────────────────────┘
```

### 3.2 Shadow Data Discovery

**Shadow data** = dữ liệu nhạy cảm tồn tại ngoài tầm kiểm soát của security team:
- Copies of production data trong dev/test environments
- Exports từ SaaS apps vào personal cloud storage
- Data shared qua consumer AI tools (ChatGPT, Claude consumer)
- Abandoned S3 buckets chứa PII

**Kỹ thuật discovery:**
- Agentless scanning (API-based) cho cloud/SaaS
- Agent-based cho endpoints
- Network traffic analysis cho data-in-motion
- Metadata analysis + permission mapping

### 3.3 Discovery cho AI Systems (Mới 2026)

Đây là gap lớn nhất — hầu hết DSPM tools chưa cover đầy đủ:

| AI Component | Sensitive Data Risk | Discovery Approach |
|-------------|--------------------|--------------------|
| **Training datasets** | PII, proprietary data | Dataset catalog + lineage tracking |
| **Vector databases** | Embedded sensitive content | Semantic search + sampling |
| **RAG corpora** | Documents chứa confidential info | Document provenance + classification |
| **Model weights** | Memorized training data | Model inversion testing |
| **Prompt logs** | User PII, business secrets | Log analysis + classification |
| **Fine-tuning data** | Domain-specific sensitive info | Data pipeline monitoring |

---

## 4. Top Solutions 2026

### 4.1 Leaders (Forrester Wave Q2 2026)

| Vendor | Điểm mạnh | Coverage |
|--------|-----------|----------|
| **BigID** | Highest scores in 11 criteria, broadest discovery | Cloud, on-prem, SaaS, unstructured |
| **Varonis** | Deep access governance + classification | File shares, M365, cloud |
| **Cyera** | AI-native, real-time, agentless | Multi-cloud, SaaS |

### 4.2 Strong Performers

| Vendor | Đặc biệt |
|--------|----------|
| **Sentra** | Cloud-native, extends AI governance to LLMs (Claude, etc.) |
| **Forcepoint** | AI Mesh cho classification accuracy, unified platform |
| **Thales** | Encryption integration, hardware security heritage |
| **Microsoft Purview** | Built-in M365 classifiers, compliance templates |
| **Cyberhaven** | Data lineage tracking — biết data đi từ đâu đến đâu |

### 4.3 Evaluation Criteria

Khi chọn solution, đánh giá theo:

1. **Classification accuracy** — False positive rate? Contextual understanding?
2. **Discovery breadth** — Cover bao nhiêu data stores, SaaS apps?
3. **AI system coverage** — Có discover data trong vector DBs, RAG, training pipelines?
4. **Agentless vs. agent** — Deployment model phù hợp với environment?
5. **Remediation depth** — Chỉ alert hay có thể auto-remediate?
6. **Integration** — Connect với DLP, IAM, SIEM, SOAR?
7. **Scale** — Xử lý petabytes trong reasonable time?

---

## 5. Implementation Framework

### 5.1 Phased Approach

```
Phase 1 (Tuần 1-4): Discovery & Inventory
├── Connect tới cloud accounts (AWS, Azure, GCP)
├── Integrate SaaS connectors (M365, Google, Salesforce)
├── Scan on-prem file shares và databases
└── Build initial data map

Phase 2 (Tuần 5-8): Classification
├── Deploy AI-powered classifiers
├── Define sensitivity levels (4-tier model)
├── Map data to regulatory requirements (GDPR, HIPAA, PCI)
└── Validate accuracy với business stakeholders

Phase 3 (Tuần 9-12): Operationalization
├── Integrate với DLP policies
├── Set up continuous monitoring
├── Build reporting cho compliance
└── Establish feedback loop cho tuning

Phase 4 (Ongoing): AI System Extension
├── Extend discovery tới AI pipelines
├── Classify RAG corpora và training data
├── Monitor data flows into/out of AI systems
└── Update classification khi new AI tools adopted
```

### 5.2 Common Pitfalls

| Pitfall | Impact | Mitigation |
|---------|--------|-----------|
| Over-classification | Alert fatigue, user friction | Start restrictive, tune down |
| Missing shadow data | False sense of security | Include dev/test, personal storage |
| Static classification | Drift over time | Continuous re-scan, event-triggered |
| Ignoring unstructured data | 80%+ of enterprise data missed | Prioritize NLP-based classifiers |
| No business context | Irrelevant results | Involve data owners early |

---

## 6. Metrics & KPIs

| Metric | Target | Measurement |
|--------|--------|-------------|
| Discovery coverage | >95% of data stores | Stores scanned / total stores |
| Classification accuracy | >90% true positive rate | Sampled validation |
| Time-to-classify new data | <24 hours | From creation to label |
| Sensitive data exposure rate | Decreasing quarter-over-quarter | Publicly accessible sensitive files |
| Unclassified data ratio | <10% | Unclassified / total discovered |

---

## 7. Tổng kết

Data Classification & Discovery năm 2026:

1. **AI-powered classification là table stakes** — Manual methods không scale
2. **Discovery phải bao gồm AI systems** — Vector DBs, RAG, training pipelines là blind spots nguy hiểm
3. **Context > Sensitivity label** — Exposure risk = sensitivity × accessibility × sharing pattern
4. **Continuous, not one-time** — Data landscape thay đổi liên tục
5. **Foundation for everything** — DLP, DSPM, encryption, compliance đều bắt đầu từ đây

Trong phần tiếp theo, chúng ta sẽ đi sâu vào **Data Loss Prevention (DLP) & DSPM** — cách enforce protection dựa trên classification results.

---

## Nguồn tham khảo

- [Forrester Wave: Sensitive Data Discovery & Classification Q2 2026](https://www.forrester.com/blogs/what-to-know-when-evaluating-sensitive-data-discovery-and-classification-solutions/)
- [BigID Named Forrester Leader 2026](https://bigid.com/blog/bigid-forrester-wave-leader-sensitive-data-discovery-classification-2026/)
- [Inside AI-Powered Data Classification — Palo Alto Networks](https://www.paloaltonetworks.com/blog/cloud-security/data-classification-ai-era/)
- [The 4-Tier AI Data Classification Model — Iternal AI](https://iternal.ai/ai-data-classification)
- [Best Data Classification Tools 2026 — Sentra](https://sentra.io/blog/best-data-classification-tools)
- [Data Discovery and Classification Tools 2026 — Cyberhaven](https://www.cyberhaven.com/blog/data-discovery-classification-tools)
- [Sentra Extends AI Governance to Claude — TMCnet](https://www.tmcnet.com/usubmit/-sentra-extends-enterprise-ai-governance-claude-with-continuous-/2026/06/12/10399128.htm)
- [7 AI-Native Data Privacy Platforms 2026 — Global Cybersecurity Network](https://globalcybersecuritynetwork.com/blog/best-ai-native-data-privacy-platforms/)
- [Top 8 DSPM Trends 2026 — Forcepoint](https://www.forcepoint.com/blog/insights/dspm-trends)
