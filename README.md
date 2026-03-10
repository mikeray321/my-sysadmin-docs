# docs

*Tamdiu discendum est, quoad vivas, quemadmodum vivas*.
-Seneca

### HA

```mermaid
flowchart TD
    %% Styling based on K8s docs guidelines
    classDef k8s fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff;
    classDef storage fill:#f96,stroke:#333,stroke-width:2px;

    User((User)) --> LB[External Load Balancer]
    
    subgraph CP [Control Plane - HA]
        direction TB
        LB --> API1[kube-apiserver 1]
        LB --> API2[kube-apiserver 2]
        LB --> API3[kube-apiserver 3]
        
        API1 <--> ETCD[(etcd cluster)]
        API2 <--> ETCD
        API3 <--> ETCD
    end

    API1 --> W1[Worker Node 1]
    API2 --> W2[Worker Node 2]
    API3 --> W3[Worker Node 3]

    class API1,API2,API3,W1,W2,W3 k8s;
    class ETCD storage;

### Edge Computing Architecture (k3s Style)

```mermad
flowchart LR
    classDef hub fill:#326ce5,stroke:#fff,color:#fff;
    classDef edge fill:#666,stroke:#fff,color:#fff;

    subgraph Central [Cloud / Central Hub]
        direction TB
        Admin[Cluster Admin] --> GitOps[GitOps Controller]
        GitOps --> Registry[Image Registry]
    end

    subgraph SiteA [Edge Site: Factory]
        direction TB
        k3s_A[k3s Cluster] --> Sensor[IoT Sensor Pod]
    end

    subgraph SiteB [Edge Site: Retail]
        direction TB
        k3s_B[k3s Cluster] --> POS[Point of Sale Pod]
    end

    GitOps -- "Deploy Policy" --> k3s_A
    GitOps -- "Deploy Policy" --> k3s_B
    Registry -- "Pull Image" --> k3s_A
    Registry -- "Pull Image" --> k3s_B

    class Admin,GitOps,Registry hub;
    class k3s_A,k3s_B edge;
