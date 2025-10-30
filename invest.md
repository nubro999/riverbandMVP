
# GPU Rental Platform - Investment Deck

## Executive Summary

**블록체인 기반 탈중앙화 GPU 마켓플레이스**

유휴 GPU 리소스를 필요한 사람에게 연결하는 P2P 플랫폼으로, 중앙화된 중개자 없이 투명하고 효율적인 거래를 실현합니다.

### 핵심 가치 제안

- **투명성**: 스마트 컨트랙트 기반 자동화된 거래
- **효율성**: P2P 직거래로 중개 수수료 최소화 (2%)
- **확장성**: 글로벌 분산 네트워크
- **신뢰성**: 블록체인 기반 평판 시스템

### 시장 기회

- AI/ML 시장의 폭발적 성장 (연평균 40%+)
- GPU 수요 급증 vs 공급 부족
- 클라우드 GPU 비용 절감 니즈 (최대 70% 저렴)

---

# Platform Overview

## System Architecture

```mermaid
graph TB
    subgraph Users["Ecosystem Participants"]
        Renter[GPU Renters<br/>━━━━━━━━<br/>AI/ML 연구자<br/>개발자<br/>스타트업]
        Provider[GPU Providers<br/>━━━━━━━━<br/>데이터센터<br/>마이닝 업체<br/>개인 사용자]
    end
    
    subgraph Platform["GPU Rental Platform"]
        Core[Core Platform<br/>━━━━━━━━<br/>• 스마트 컨트랙트<br/>• 매칭 엔진<br/>• 평판 시스템]
    end
    
    subgraph Infrastructure["Infrastructure"]
        BC[Blockchain<br/>━━━━━━━━<br/>결제 & 계약]
        IPFS[IPFS<br/>━━━━━━━━<br/>파일 저장]
    end
    
    Renter <-->|대여 & 결제| Core
    Provider <-->|제공 & 수익| Core
    Core <--> BC
    Core <--> IPFS
    
    style Users fill:#e8f5e9
    style Platform fill:#e1f5ff
    style Infrastructure fill:#fff4e6
```

### 주요 참여자

**GPU Renters (수요)**

- AI/ML 모델 학습이 필요한 연구자
- 렌더링 작업이 필요한 크리에이터
- 추론 서비스를 운영하는 기업

**GPU Providers (공급)**

- 유휴 GPU를 보유한 데이터센터
- 채굴 중단 후 GPU 활용을 원하는 업체
- 고성능 PC 소유자

---

# Core Architecture

## Technical Stack

```mermaid
graph TB
    subgraph Frontend["사용자 인터페이스"]
        Web[Web Application<br/>━━━━━━━━<br/>React + Web3<br/>━━━━━━━━<br/>• GPU 검색<br/>• 작업 제출<br/>• 실시간 모니터링]
    end
    
    subgraph Backend["플랫폼 서비스"]
        API[API Gateway<br/>━━━━━━━━<br/>REST & WebSocket]
        
        SC[Smart Contracts<br/>━━━━━━━━<br/>• 리소스 등록<br/>• 주문 매칭<br/>• 결제 처리<br/>• 평판 관리]
        
        Oracle[Oracle Service<br/>━━━━━━━━<br/>• 가용성 검증<br/>• 성능 모니터링<br/>• 분쟁 해결]
    end
    
    subgraph Infrastructure["인프라"]
        BC[Blockchain<br/>━━━━━━━━<br/>스마트 컨트랙트 실행]
        IPFS[IPFS Network<br/>━━━━━━━━<br/>분산 파일 저장]
        DB[(Database<br/>━━━━━━━━<br/>메타데이터 캐시)]
    end
    
    Web --> API
    API --> SC
    SC --> BC
    API --> IPFS
    API --> DB
    Oracle --> BC
    
    style Frontend fill:#e8f5e9
    style Backend fill:#e1f5ff
    style Infrastructure fill:#fff4e6
```

### 핵심 컴포넌트

**스마트 컨트랙트**

- 리소스 등록 및 관리
- 자동화된 매칭 및 결제
- 투명한 평판 시스템
- 분쟁 해결 메커니즘

**IPFS (분산 저장소)**

- Docker 이미지 저장
- P2P 파일 공유
- 중앙화된 서버 불필요

**Oracle (검증 시스템)**

- Provider 가용성 실시간 체크
- 성능 및 품질 보증
- 자동화된 분쟁 해결

---

# Business Logic

## Smart Contract Architecture

```mermaid
graph TD
    Token[GPU Token<br/>━━━━━━━━<br/>플랫폼 화폐]
    
    Token --> Market[Marketplace<br/>━━━━━━━━<br/>매칭 & 거래]
    
    Token --> Escrow[Payment Escrow<br/>━━━━━━━━<br/>안전한 결제<br/>수수료: 2%]
    
    Market --> Registry[Resource Registry<br/>━━━━━━━━<br/>GPU 등록 & 관리]
    
    Market --> Reputation[Reputation System<br/>━━━━━━━━<br/>신뢰도 평가]
    
    style Token fill:#fff9c4
    style Market fill:#b3e5fc
    style Escrow fill:#c8e6c9
    style Registry fill:#f8bbd0
    style Reputation fill:#d1c4e9
```

### 거래 흐름

```mermaid
stateDiagram-v2
    [*] --> Created: 작업 생성
    Created --> Accepted: Provider 수락
    Accepted --> Running: 실행 시작
    Running --> Completed: 완료
    Running --> Disputed: 분쟁 발생
    Disputed --> Resolved: 해결
    Resolved --> Completed
    Completed --> [*]
    
    note right of Created
        토큰 에스크로 예치
    end note
    
    note right of Running
        시간당 과금
    end note
    
    note right of Completed
        자동 정산
    end note
```

---

# Key Processes

## User Journey: GPU 대여 프로세스

```mermaid
sequenceDiagram
    autonumber
    participant U as User
    participant P as Platform
    participant Provider as Provider
    participant BC as Blockchain
    
    Note over U,Provider: 1. 검색 & 선택
    U->>P: GPU 검색 (스펙, 가격)
    P->>U: 사용 가능한 GPU 리스트
    U->>U: GPU 선택
    
    Note over U,BC: 2. 작업 생성 & 결제
    U->>P: Docker 이미지 업로드
    P->>U: 이미지 ID (CID) 반환
    U->>BC: 작업 생성 + 토큰 예치
    BC->>BC: 토큰 에스크로에 보관
    
    Note over Provider,BC: 3. Provider 수락 & 실행
    BC->>Provider: 작업 알림
    Provider->>BC: 작업 수락
    Provider->>Provider: 컨테이너 실행
    Provider->>BC: 실행 시작 확인
    
    Note over U,Provider: 4. 모니터링
    U->>Provider: 실시간 로그 확인
    Provider->>U: 진행 상황 전송
    
    Note over Provider,BC: 5. 완료 & 정산
    Provider->>BC: 작업 완료 보고
    BC->>BC: 사용 시간 계산
    BC->>Provider: 수익 지급 (98%)
    BC->>P: 플랫폼 수수료 (2%)
    BC->>U: 초과 금액 환불
```

## Oracle의 신뢰 보증

```mermaid
sequenceDiagram
    participant O as Oracle
    participant P as Provider
    participant BC as Blockchain
    
    loop 매 1분
        O->>P: Heartbeat 요청
        P->>O: 상태 응답
        
        alt 정상 응답
            O->>BC: 가용성 확인 ✓
        else 응답 없음
            O->>BC: 다운타임 보고
            BC->>BC: 평판 점수 하락
        end
    end
    
    Note over O,P: 무작위 검증
    O->>P: Challenge (데이터 증명 요청)
    P->>O: Response (증거 제출)
    O->>O: 검증
    
    alt 검증 성공
        O->>BC: 신뢰도 유지
    else 검증 실패
        O->>BC: 패널티 부과
    end
```

---

## 5.2 Defense in Depth

```mermaid
graph TB
    subgraph Layer1["Layer 1: Network Security"]
        L1A[DDoS Protection<br/>━━━━━━━━<br/>• Cloudflare<br/>• Rate Limiting<br/>• IP Filtering]
        
        L1B[Firewall<br/>━━━━━━━━<br/>• Ingress Rules<br/>• Egress Rules<br/>• Port Restrictions]
        
        L1C[VPC Isolation<br/>━━━━━━━━<br/>• Private Subnets<br/>• Network ACLs<br/>• Security Groups]
    end
    
    subgraph Layer2["Layer 2: Application Security"]
        L2A[Input Validation<br/>━━━━━━━━<br/>• Schema Validation<br/>• Sanitization<br/>• Type Checking]
        
        L2B[Authentication<br/>━━━━━━━━<br/>• Wallet Signature<br/>• JWT Tokens<br/>• Session Management]
        
        L2C[Authorization<br/>━━━━━━━━<br/>• Role-Based Access<br/>• Resource Ownership<br/>• Permission Checks]
    end
    
    subgraph Layer3["Layer 3: Smart Contract Security"]
        L3A[Access Control<br/>━━━━━━━━<br/>• Ownable<br/>• Roles<br/>• Modifiers]
        
        L3B[Safety Checks<br/>━━━━━━━━<br/>• ReentrancyGuard<br/>• Overflow Protection<br/>• State Validation]
        
        L3C[Upgradability<br/>━━━━━━━━<br/>• Proxy Pattern<br/>• Emergency Pause<br/>• Timelock]
    end
    
    subgraph Layer4["Layer 4: Container Security"]
        L4A[Isolation<br/>━━━━━━━━<br/>• Namespace<br/>• cgroups<br/>• Capabilities]
        
        L4B[Resource Limits<br/>━━━━━━━━<br/>• CPU Quota<br/>• Memory Limit<br/>• Disk I/O]
        
        L4C[Scanning<br/>━━━━━━━━<br/>• Vulnerability Scan<br/>• Malware Detection<br/>• Static Analysis]
    end
    
    subgraph Layer5["Layer 5: Data Security"]
        L5A[Encryption at Rest<br/>━━━━━━━━<br/>• Database Encryption<br/>• Disk Encryption<br/>• Key Management]
        
        L5B[Encryption in Transit<br/>━━━━━━━━<br/>• TLS 1.3<br/>• Certificate Pinning<br/>• Perfect Forward Secrecy]
        
        L5C[Access Logging<br/>━━━━━━━━<br/>• Audit Trails<br/>• Anomaly Detection<br/>• SIEM Integration]
    end
    
    Layer1 --> Layer2
    Layer2 --> Layer3
    Layer3 --> Layer4
    Layer4 --> Layer5
    
    style Layer1 fill:#e3f2fd
    style Layer2 fill:#f3e5f5
    style Layer3 fill:#fff9c4
    style Layer4 fill:#ffccbc
    style Layer5 fill:#c8e6c9
```

---

# Deployment Architecture

## 6.1 Infrastructure Layout

```mermaid
graph TB
    subgraph Internet["Internet"]
        Users[End Users<br/>━━━━━━━━<br/>• Web Browsers<br/>• Mobile Apps]
        Providers[GPU Providers<br/>━━━━━━━━<br/>• Provider Agents<br/>• IPFS Nodes]
    end
    
    subgraph CDN["CDN Layer"]
        CF[Cloudflare<br/>━━━━━━━━<br/>• DDoS Protection<br/>• SSL/TLS<br/>• Caching<br/>• WAF]
    end
    
    subgraph Region1["Region 1 - Primary (US-East)"]
        LB1[Load Balancer<br/>━━━━━━━━<br/>• Health Checks<br/>• SSL Termination<br/>• Sticky Sessions]
        
        subgraph AppCluster1["Application Cluster"]
            API1A[API Gateway 1]
            API1B[API Gateway 2]
            API1C[API Gateway 3]
            
            IDX1A[Indexer 1]
            IDX1B[Indexer 2]
            
            ORC1A[Oracle 1]
            ORC1B[Oracle 2]
        end
        
        subgraph DataLayer1["Data Layer"]
            PG1[(PostgreSQL<br/>Primary<br/>━━━━━━━━<br/>• Master Node<br/>• Write/Read<br/>• Streaming Replication)]
            
            Redis1[(Redis Cluster<br/>━━━━━━━━<br/>• Cache<br/>• Session Store<br/>• Pub/Sub)]
        end
        
        subgraph Blockchain1["Blockchain Cluster"]
            BC1A[Validator Node 1<br/>━━━━━━━━<br/>• Block Production<br/>• Consensus<br/>• Full Archive]
            
            BC1B[Validator Node 2<br/>━━━━━━━━<br/>• Block Production<br/>• Consensus<br/>• Full Archive]
            
            BC1C[RPC Node<br/>━━━━━━━━<br/>• Public RPC<br/>• Read-only<br/>• Fast Sync]
        end
        
        subgraph IPFS1["IPFS Cluster"]
            IPFS1A[IPFS Node 1<br/>━━━━━━━━<br/>• Gateway<br/>• Pinning<br/>• 500GB Storage]
            
            IPFS1B[IPFS Node 2<br/>━━━━━━━━<br/>• Gateway<br/>• Pinning<br/>• 500GB Storage]
            
            IPFS1C[IPFS Node 3<br/>━━━━━━━━<br/>• Gateway<br/>• Pinning<br/>• 500GB Storage]
        end
        
        LB1 --> API1A
        LB1 --> API1B
        LB1 --> API1C
        
        API1A --> PG1
        API1A --> Redis1
        API1B --> PG1
        API1B --> Redis1
        API1C --> PG1
        API1C --> Redis1
        
        IDX1A --> PG1
        IDX1B --> PG1
        
        IDX1A --> BC1C
        IDX1B --> BC1C
        
        ORC1A --> BC1C
        ORC1B --> BC1C
    end
    
    subgraph Region2["Region 2 - DR (EU-West)"]
        LB2[Load Balancer<br/>Standby]
        
        subgraph AppCluster2["Application Cluster"]
            API2A[API Gateway<br/>Standby]
        end
        
        PG2[(PostgreSQL<br/>Replica<br/>━━━━━━━━<br/>• Read-only<br/>• Async Replication<br/>• Failover Ready)]
        
        BC2[Blockchain Node<br/>━━━━━━━━<br/>• Full Node<br/>• Standby Validator]
        
        IPFS2[IPFS Cluster<br/>━━━━━━━━<br/>• Backup Gateway<br/>• Popular Content]
    end
    
    subgraph ProviderNetwork["Distributed Provider Network"]
        PA1[Provider Agent 1<br/>━━━━━━━━<br/>• RTX 4090<br/>• 128GB RAM<br/>• San Francisco]
        
        PA2[Provider Agent 2<br/>━━━━━━━━<br/>• A100<br/>• 256GB RAM<br/>• London]
        
        PA3[Provider Agent N<br/>━━━━━━━━<br/>• RTX 3090<br/>• 64GB RAM<br/>• Tokyo]
    end
    
    subgraph Monitoring["Monitoring & Logging"]
        Prom[Prometheus<br/>━━━━━━━━<br/>• Metrics Collection<br/>• Time Series DB<br/>• Alerting]
        
        Graf[Grafana<br/>━━━━━━━━<br/>• Dashboards<br/>• Visualization<br/>• Alerting]
        
        ELK[ELK Stack<br/>━━━━━━━━<br/>• Centralized Logs<br/>• Search<br/>• Analytics]
        
        Jaeger[Jaeger<br/>━━━━━━━━<br/>• Distributed Tracing<br/>• Performance<br/>• Debugging]
    end
    
    Users --> CF
    Providers --> CF
    CF --> LB1
    
    PG1 -.->|Replication| PG2
    BC1A -.->|Sync| BC2
    IPFS1A -.->|Backup| IPFS2
    
    PA1 --> LB1
    PA2 --> LB1
    PA3 --> LB1
    
    API1A --> Prom
    API1B --> Prom
    IDX1A --> Prom
    BC1A --> Prom
    IPFS1A --> Prom
    
    Prom --> Graf
    
    API1A --> ELK
    IDX1A --> ELK
    PA1 --> ELK
    
    API1A --> Jaeger
    
    style Internet fill:#e8f5e9
    style CDN fill:#fff3e0
    style Region1 fill:#e3f2fd
    style Region2 fill:#f3e5f5
    style ProviderNetwork fill:#fce4ec
    style Monitoring fill:#f1f8e9
```

## 6.2 Scalability & High Availability

```mermaid
graph LR
    subgraph Horizontal["Horizontal Scaling"]
        H1[API Gateway<br/>━━━━━━━━<br/>Stateless<br/>━━━━━━━━<br/>Auto-scaling<br/>Min: 3, Max: 20]
        
        H2[Indexer Service<br/>━━━━━━━━<br/>Stateless<br/>━━━━━━━━<br/>Auto-scaling<br/>Min: 2, Max: 10]
        
        H3[Oracle Service<br/>━━━━━━━━<br/>Stateless<br/>━━━━━━━━<br/>Auto-scaling<br/>Min: 2, Max: 5]
    end
    
    subgraph Vertical["Vertical Scaling"]
        V1[PostgreSQL<br/>━━━━━━━━<br/>Scale-up<br/>━━━━━━━━<br/>CPU: 64 cores<br/>RAM: 512GB<br/>SSD: 4TB NVMe]
        
        V2[Blockchain Node<br/>━━━━━━━━<br/>Scale-up<br/>━━━━━━━━<br/>CPU: 32 cores<br/>RAM: 256GB<br/>SSD: 2TB NVMe]
    end
    
    subgraph HA["High Availability"]
        HA1[Multi-AZ Deployment<br/>━━━━━━━━<br/>• 3 Availability Zones<br/>• Auto-failover<br/>• 99.99% SLA]
        
        HA2[Database Replication<br/>━━━━━━━━<br/>• Streaming Replication<br/>• Synchronous Standby<br/>• Auto-promotion]
        
        HA3[Load Balancing<br/>━━━━━━━━<br/>• Health Checks<br/>• Circuit Breaker<br/>• Retry Logic]
    end
    
    style Horizontal fill:#e1f5fe
    style Vertical fill:#f3e5f5
    style HA fill:#c8e6c9
```

---

# Monitoring & Observability

## 7.1 Monitoring Stack

```mermaid
graph TB
    subgraph Services["Services to Monitor"]
        API[API Gateway]
        BC[Blockchain]
        IPFS[IPFS]
        Agent[Provider Agent]
        DB[(Database)]
    end
    
    subgraph Collection["Metrics Collection"]
        Prom[Prometheus<br/>━━━━━━━━<br/>• Pull-based<br/>• Time-series DB<br/>• PromQL]
        
        API --> |/metrics| Prom
        BC --> |/metrics| Prom
        IPFS --> |/metrics| Prom
        Agent --> |/metrics| Prom
        DB --> |pg_exporter| Prom
        
        Exporters[Exporters<br/>━━━━━━━━<br/>• Node Exporter<br/>• Postgres Exporter<br/>• Custom Exporters]
        
        Exporters --> Prom
    end
    
    subgraph Visualization["Visualization"]
        Graf[Grafana<br/>━━━━━━━━<br/>Dashboards]
        
        Prom --> Graf
        
        D1[System Dashboard<br/>━━━━━━━━<br/>• CPU, Memory, Disk<br/>• Network I/O<br/>• Service Health]
        
        D2[Blockchain Dashboard<br/>━━━━━━━━<br/>• Block Height<br/>• TPS<br/>• Gas Usage<br/>• Validator Status]
        
        D3[Business Dashboard<br/>━━━━━━━━<br/>• Active Jobs<br/>• Revenue<br/>• Provider Count<br/>• User Growth]
        
        Graf --> D1
        Graf --> D2
        Graf --> D3
    end
    
    subgraph Alerting["Alerting"]
        AlertMgr[Alert Manager<br/>━━━━━━━━<br/>• Rule Evaluation<br/>• Deduplication<br/>• Routing]
        
        Prom --> AlertMgr
        Graf --> AlertMgr
        
        Channels[Notification Channels]
        
        AlertMgr --> Slack[Slack<br/>━━━━━━━━<br/>Critical: #alerts<br/>Warning: #warnings]
        
        AlertMgr --> Email[Email<br/>━━━━━━━━<br/>On-call team]
        
        AlertMgr --> PD[PagerDuty<br/>━━━━━━━━<br/>Critical only]
    end
    
    subgraph Logging["Centralized Logging"]
        Filebeat[Filebeat<br/>━━━━━━━━<br/>Log Shipper]
        
        API --> Filebeat
        BC --> Filebeat
        Agent --> Filebeat
        
        Logstash[Logstash<br/>━━━━━━━━<br/>• Parse<br/>• Transform<br/>• Filter]
        
        Filebeat --> Logstash
        
        ES[(Elasticsearch<br/>━━━━━━━━<br/>• Index<br/>• Search<br/>• Aggregate)]
        
        Logstash --> ES
        
        Kibana[Kibana<br/>━━━━━━━━<br/>• Search UI<br/>• Log Analysis<br/>• Visualization]
        
        ES --> Kibana
    end
    
    subgraph Tracing["Distributed Tracing"]
        API --> Jaeger[Jaeger<br/>━━━━━━━━<br/>• Trace Collection<br/>• Service Graph<br/>• Performance]
        
        Agent --> Jaeger
        
        JaegerUI[Jaeger UI<br/>━━━━━━━━<br/>• Trace Search<br/>• Latency Analysis<br/>• Dependencies]
        
        Jaeger --> JaegerUI
    end
    
    style Services fill:#e8f5e9
    style Collection fill:#e3f2fd
    style Visualization fill:#f3e5f5
    style Alerting fill:#fff3e0
    style Logging fill:#fce4ec
    style Tracing fill:#f1f8e9
```

## 7.2 Key Metrics

```mermaid
graph TB
    subgraph System["System Metrics"]
        S1[Infrastructure<br/>━━━━━━━━<br/>• CPU Usage %<br/>• Memory Usage %<br/>• Disk I/O<br/>• Network Bandwidth]
        
        S2[Application<br/>━━━━━━━━<br/>• Request Rate<br/>• Error Rate<br/>• Latency p50/p95/p99<br/>• Active Connections]
    end
    
    subgraph Blockchain["Blockchain Metrics"]
        B1[Performance<br/>━━━━━━━━<br/>• Block Time<br/>• Transactions/sec<br/>• Gas Used/Limit<br/>• Pending TX Pool]
        
        B2[Network<br/>━━━━━━━━<br/>• Peer Count<br/>• Sync Status<br/>• Block Height<br/>• Fork Events]
        
        B3[Consensus<br/>━━━━━━━━<br/>• Validator Uptime<br/>• Missed Blocks<br/>• Stake Amount<br/>• Slash Events]
    end
    
    subgraph IPFS["IPFS Metrics"]
        I1[Network<br/>━━━━━━━━<br/>• Connected Peers<br/>• Bandwidth Usage<br/>• DHT Query Latency<br/>• Repo Size]
        
        I2[Content<br/>━━━━━━━━<br/>• Pinned Objects<br/>• Total Storage<br/>• Pin Queue Size<br/>• GC Runs]
    end
    
    subgraph Business["Business Metrics"]
        BM1[Platform Activity<br/>━━━━━━━━<br/>• Active Users<br/>• Active Providers<br/>• Total GPUs<br/>• Available GPUs]
        
        BM2[Jobs<br/>━━━━━━━━<br/>• Jobs Created/day<br/>• Jobs Completed/day<br/>• Success Rate %<br/>• Avg Duration]
        
        BM3[Financial<br/>━━━━━━━━<br/>• Revenue/day<br/>• Token Volume<br/>• Avg Job Cost<br/>• Platform Fees]
        
        BM4[Quality<br/>━━━━━━━━<br/>• Avg Provider Reputation<br/>• Dispute Rate %<br/>• User Satisfaction<br/>• Uptime %]
    end
    
    style System fill:#e3f2fd
    style Blockchain fill:#f3e5f5
    style IPFS fill:#fff9c4
    style Business fill:#c8e6c9
```

---

# Appendix

## A. Technology Stack Summary

```mermaid
graph TB
    subgraph Frontend["Frontend Stack"]
        FE1[React 18<br/>━━━━━━━━<br/>UI Framework]
        FE2[TypeScript<br/>━━━━━━━━<br/>Type Safety]
        FE3[ethers.js<br/>━━━━━━━━<br/>Web3 Library]
        FE4[TailwindCSS<br/>━━━━━━━━<br/>Styling]
        FE5[React Query<br/>━━━━━━━━<br/>Data Fetching]
    end
    
    subgraph Backend["Backend Stack"]
        BE1[Node.js<br/>━━━━━━━━<br/>API Gateway]
        BE2[Go<br/>━━━━━━━━<br/>Services]
        BE3[Express<br/>━━━━━━━━<br/>Web Framework]
        BE4[Socket.io<br/>━━━━━━━━<br/>WebSocket]
        BE5[GraphQL<br/>━━━━━━━━<br/>Query API]
    end
    
    subgraph Blockchain["Blockchain Stack"]
        BC1[Solidity 0.8.20<br/>━━━━━━━━<br/>Smart Contracts]
        BC2[Hardhat<br/>━━━━━━━━<br/>Dev Environment]
        BC3[OpenZeppelin<br/>━━━━━━━━<br/>Security Libs]
        BC4[Geth Fork<br/>━━━━━━━━<br/>EVM L1]
    end
    
    subgraph Storage["Storage Stack"]
        ST1[PostgreSQL 15<br/>━━━━━━━━<br/>Relational DB]
        ST2[Redis 7<br/>━━━━━━━━<br/>Cache & Queue]
        ST3[IPFS Kubo<br/>━━━━━━━━<br/>Distributed Storage]
    end
    
    subgraph Infra["Infrastructure Stack"]
        IN1[Docker<br/>━━━━━━━━<br/>Containerization]
        IN2[Kubernetes<br/>━━━━━━━━<br/>Orchestration]
        IN3[Terraform<br/>━━━━━━━━<br/>IaC]
        IN4[AWS/GCP<br/>━━━━━━━━<br/>Cloud Provider]
    end
    
    subgraph Monitor["Monitoring Stack"]
        MO1[Prometheus<br/>━━━━━━━━<br/>Metrics]
        MO2[Grafana<br/>━━━━━━━━<br/>Visualization]
        MO3[ELK Stack<br/>━━━━━━━━<br/>Logging]
        MO4[Jaeger<br/>━━━━━━━━<br/>Tracing]
    end
    
    style Frontend fill:#e8f5e9
    style Backend fill:#e3f2fd
    style Blockchain fill:#fff9c4
    style Storage fill:#f3e5f5
    style Infra fill:#fce4ec
    style Monitor fill:#f1f8e9
```

## B. API Reference Summary

### Blockchain APIs (Smart Contracts)

```
ResourceRegistry:
- registerResource(gpuModel, vcpu, memoryGB, storageGB, pricePerHour)
- updateAvailability(resourceId, available)
- updatePricing(resourceId, newPrice)
- getResource(resourceId)

Marketplace:
- createJob(resourceId, imageCID, estimatedDuration)
- acceptJob(jobId)
- startJob(jobId)
- completeJob(jobId)
- disputeJob(jobId, reason)

PaymentEscrow:
- lockFunds(jobId, amount, user, provider)
- releaseFunds(jobId, actualCost)
- refund(jobId, amount)
```

### REST APIs

```
GET    /api/v1/gpus
GET    /api/v1/gpus/:id
POST   /api/v1/instances
GET    /api/v1/instances/:id
DELETE /api/v1/instances/:id
GET    /api/v1/images
POST   /api/v1/images/upload
```

### GraphQL APIs

```graphql
query {
  gpus(filters: {...})
  jobs(user: "0x...")
  myJobs(address: "0x...")
}

subscription {
  jobStatusChanged(jobId: "0x...")
  newGPUAvailable
}
```

## C. Glossary

- **CID**: Content Identifier, IPFS의 파일 주소
- **DHT**: Distributed Hash Table, 분산 키-값 저장소
- **Escrow**: 제3자가 보관하는 예치금
- **Pinning**: IPFS에서 파일을 로컬에 유지
- **Reputation**: 제공자의 신뢰도 점수
- **Stake**: 참여를 위해 예치한 토큰
- **Oracle**: 온체인과 오프체인을 연결하는 서비스
- **EVM**: Ethereum Virtual Machine
- **PoS**: Proof of Stake, 지분 증명 합의 알고리즘
- **TPS**: Transactions Per Second
- **Gas**: 블록체인 거래 수수료

---

**Document End**

Last updated: 2025-10-30  
Version: 1.0.0