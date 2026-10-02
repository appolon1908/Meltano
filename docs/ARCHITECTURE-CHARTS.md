# Meltano — Architecture Charts

> Repository: `appolon1908/Meltano`  
> Baseline branch: `main`  
> Repository-local visual architecture. Keep these diagrams aligned with code, contracts and deployment.

## 1. System context
```mermaid
flowchart LR
 A["Data sources"] --> B["Meltano pipelines"]
 B --> R["Meltano<br/>Data integration / ELT platform"]
 R --> S["pipeline state / raw data"]
 R --> D["warehouses / downstream APIs"]
```

## 2. Internal component architecture
```mermaid
flowchart TB
 I["Entrypoint / API / CLI"] --> P["Identity, policy, validation"]
 P --> C["Core domain / orchestration"]
 C --> S["State / configuration / persistence"]
 C --> A["Adapters / integrations"]
 A --> X["Approved dependencies"]
 C --> O["Metrics, logs, traces, audit"]
```

## 3. Critical runtime flow
```mermaid
sequenceDiagram
 participant U as Caller
 participant B as Meltano
 participant P as Policy
 participant C as Core
 participant S as State
 participant X as Dependency
 U->>B: Request / event / command
 B->>P: Authenticate + validate
 P-->>B: Decision
 B->>C: Extract, transform/load and batch evidence
 C->>S: Read / persist state
 C->>X: Bounded integration
 X-->>C: Result / readback
 C-->>U: Normalized response
```

## 4. Deployment and promotion
```mermaid
flowchart LR
 F["Feature branch"] --> T["Tests / validation"]
 T --> PR["Pull request + review"]
 PR --> CI["CI green"]
 CI --> ST["Staging / isolated verification"]
 ST --> EX["Exact-SHA certification"]
 EX --> G{"Production approval?"}
 G -- No --> ST
 G -- Yes --> P["Production promotion"]
 P --> H["Health/readiness + rollback check"]
```

## 5. Observability and recovery
```mermaid
flowchart LR
 R["Meltano"] --> M["Metrics"]
 R --> L["Logs / audit"]
 R --> T["Traces / correlation"]
 M --> O["Observability stack"]
 L --> O
 T --> O
 O --> A["Dashboards / alerts"]
 R --> B["Backup / config snapshot"]
 B --> RR["Restore / rollback rehearsal"]
```

## Ownership notes
- **Role:** Data integration / ELT platform
- **Primary boundary:** Meltano pipelines
- **State/config:** pipeline state / raw data
- **Dependencies/consumers:** warehouses / downstream APIs
- Cross-repository effects must use reviewed contracts; production effects remain separately gated.
