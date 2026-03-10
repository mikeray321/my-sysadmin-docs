# docs

*Tamdiu discendum est, quoad vivas, quemadmodum vivas*.
-Seneca

---

### A "Bulletproof" Test
Please try copying this extremely simple version to see if *any* diagram renders. If this works, we can add the complexity back in:

```mermaid
graph TD
    Start --> GitHub --> Success

graph TB
    subgraph "Your Laptop (e.g., Mac/Windows/Linux)"
        subgraph "A k3d or kind 'Node' (Docker Container 1)"
            subgraph CP1[Control Plane Node A]
                direction TB
                APIServer1[kube-apiserver]
                etcd1[etcd (Quorum Node)]
                Scheduler1[kube-scheduler]
            end
        end

        subgraph "A k3d or kind 'Node' (Docker Container 2)"
            subgraph CP2[Control Plane Node B]
                direction TB
                APIServer2[kube-apiserver]
                etcd2[etcd (Quorum Node)]
                Scheduler2[kube-scheduler]
            end
        end

        subgraph "A k3d or kind 'Node' (Docker Container 3)"
            subgraph CP3[Control Plane Node C]
                direction TB
                APIServer3[kube-apiserver]
                etcd3[etcd (Quorum Node)]
                Scheduler3[kube-scheduler]
            end
        end

        subgraph "A k3d or kind 'Node' (Docker Container 4)"
            direction TB
            subgraph W1[Worker Node A]
                Kubelet1[Kubelet]
                Proxy1[kube-proxy]
                PodA[Pod 1 (e.g., WebApp)]
            end
        end

        subgraph "A k3d or kind 'Node' (Docker Container 5)"
            direction TB
            subgraph W2[Worker Node B]
                Kubelet2[Kubelet]
                Proxy2[kube-proxy]
                PodB[Pod 2 (e.g., WebApp Replica)]
            end
        end

        %% Connections
        LB[Simulated Internal LoadBalancer] -.-> CP1
        LB -.-> CP2
        LB -.-> CP3
        etcd1 --- etcd2
        etcd2 --- etcd3
        W1 -.-> LB
        W2 -.-> LB
    end
