## ☁️ Amazon EKS Interview Questions

### 1. What is EKS and how does it differ from self-managed Kubernetes?
**EKS (Elastic Kubernetes Service)** is a managed service that makes it easy to deploy, run, and scale Kubernetes on AWS.

* **EKS:** AWS fully manages and operates the Kubernetes **Control Plane** (API server, etcd, scheduler, controllers). Users only manage the worker nodes and application layer. It offers built-in high availability and automated patching for the control plane.
* **Self-Managed:** The user is responsible for provisioning, configuring, operating, scaling, and upgrading *all* components, including the master nodes and the underlying infrastructure.

### 2. How do you upgrade an EKS cluster and its worker nodes?
Upgrades are performed in two parts:

1.  **Control Plane Upgrade:** Performed via the **AWS Management Console, AWS CLI, or `eksctl`**. This is a managed, non-disruptive, in-place upgrade (e.g., from v1.28 to v1.29).
2.  **Worker Node Upgrade:**
    * **Managed Node Groups:** AWS handles the rolling update of the **AMIs** and the **`kubelet`** versions.
    * **Self-Managed Nodes:** Requires the user to manually update the AMI, drain the node (`kubectl drain`), and update the `kubelet` and `kube-proxy` binaries.

### 3. What are managed node groups vs self-managed nodes vs Fargate in EKS?
| Type | Infrastructure Management | Use Case |
| :--- | :--- | :--- |
| **Managed Node Groups** | AWS manages the lifecycle of the underlying Auto Scaling Groups (ASGs), including rolling updates and AMI versioning. | Standard, recommended approach for dedicated EC2 nodes. |
| **Self-Managed Nodes** | User completely manages the ASG, EC2 instances, and update process. | Required for custom AMIs, specific network setups, or complex node taints/labels. |
| **Fargate** | **Serverless compute** for containers. AWS manages the Worker Node (EC2 instance) abstraction entirely. | Running Pods without managing any EC2 instances; ideal for isolated, short-lived workloads. |

### 4. How do you replace AMIs in EKS worker nodes?
* **Managed Node Groups:** The update is triggered by updating the Node Group to use a newer Kubernetes version or by selecting a newer **AMI version** in the AWS console/CLI. AWS performs a rolling update, safely draining and replacing the old nodes.
* **Self-Managed Nodes:** Update the **AMI ID** referenced in the underlying **Auto Scaling Group** and trigger a **rolling instance replacement** on the ASG.

### 5. How do you perform zero-downtime upgrades in Kubernetes?
Zero-downtime is ensured by:
1.  **RollingUpdate Strategy:** Using the **`RollingUpdate`** strategy (the default) in the Deployment manifest with appropriate `maxSurge` (new Pods created before old ones are terminated) and `maxUnavailable` (max Pods allowed to be unavailable).
2.  **Readiness Probes:** Defining a precise **`readinessProbe`** so new Pods only receive traffic after they are truly ready.
3.  **Graceful Termination:** Implementing a **`preStop` hook** and adequate `terminationGracePeriodSeconds` to allow the application to finish handling existing requests before exiting.

### 6. Explain Pod Security Admission (PSA). How do you enforce restricted policies?
**Pod Security Admission (PSA)** is the native Kubernetes mechanism that implements the **Pod Security Standards (PSS)**. It is a built-in admission controller that enforces security requirements on Pods based on three policy levels: **`Privileged`**, **`Baseline`**, and **`Restricted`**.

* **Enforcement:** You enforce restricted policies by applying a **Label** to the Namespace: `pod-security.kubernetes.io/enforce: restricted`. This label tells the PSA controller to reject any Pod that violates the Restricted policy standards.

### 7. How do you handle cluster auto-scaling in EKS? (Cluster Autoscaler vs Karpenter)
| Tool | Mechanism | Complexity | Best For |
| :--- | :--- | :--- | :--- |
| **Cluster Autoscaler (CA)** | Adjusts the **size of existing Auto Scaling Groups (ASGs)** based on pending Pods. | Simple, stable, works with ASGs. | Workloads with stable node requirements. |
| **Karpenter** | Directly monitors pending Pods and **launches custom EC2 instances** (nodes) on demand, without ASGs. | More dynamic, complex setup but simple runtime. | Cost optimization, rapid scaling, diverse node types. |

### 8. Explain Karpenter. Why use it over Cluster Autoscaler?
**Karpenter** is an open-source, flexible, high-performance Kubernetes cluster autoscaler built by AWS. It works by observing pending Pods and **directly launching the optimal EC2 instances** based on the Pod's resource requests and node constraints.

**Advantages over CA:**
* **Faster Scaling:** Bypasses ASG warm-up times.
* **Cost Optimization:** Can select the *cheapest* instance type that satisfies the Pod's requirements (spot instances, specific CPUs).
* **No ASG Dependency:** Doesn't require creating and managing numerous ASGs.

### 9. What is the difference between Horizontal Pod Autoscaler (HPA), Vertical Pod Autoscaler (VPA), and Cluster Autoscaler?
* **HPA (Horizontal Pod Autoscaler):** Scales the **number of Pods** (replicas) based on metrics (CPU, memory, custom metrics).
* **VPA (Vertical Pod Autoscaler):** Recommends or automatically sets the **CPU and memory resource requests/limits** for the containers within the Pods.
* **Cluster Autoscaler (CA):** Scales the **number of Worker Nodes** based on the number of Pods that cannot be scheduled due to resource scarcity.

### 10. How do you troubleshoot a pod stuck in `Pending` state?
The `Pending` state usually means the Pod cannot be scheduled onto a node.
1.  **Check Events:** Run **`kubectl describe pod [pod_name]`** and examine the **`Events`** section. The scheduler will post the reason here (e.g., "no nodes available to schedule pods," "insufficient cpu," "taints").
2.  **Resource Check:** Verify that the Pod's **`resources.requests`** (CPU/memory) can be met by the available nodes.
3.  **Taints/Tolerations:** Check if the Pod lacks the necessary **`tolerations`** for taints present on the nodes.

### 11. What are taints and tolerations? How are they used with node groups?
* **Taints:** Applied to a **Node**. They repel Pods, preventing them from being scheduled unless the Pod has the corresponding toleration. Used to reserve nodes for specific purposes.
* **Tolerations:** Applied to a **Pod**. They allow the Pod to be scheduled onto a tainted node, but they don't *attract* the Pod.

**Usage with Node Groups:** Managed Node Groups can be configured in EKS to automatically apply specific taints to all nodes provisioned in that group (e.g., taint a GPU node group to repel non-GPU workloads).

### 12. How do you secure EKS with IAM roles for service accounts (IRSA)?
**IRSA** allows you to associate an **AWS IAM Role** directly with a Kubernetes **Service Account**. This lets Pods assume the role's permissions, enabling them to securely access AWS resources (S3, DynamoDB, SQS) without having to rely on EC2 instance credentials or storing AWS keys as secrets.

### 13. Explain EKS control plane logging and monitoring.
EKS integrates its **Control Plane Logs** directly with **Amazon CloudWatch Logs**. These logs include:
* **`kube-apiserver`:** Records all REST API requests (for auditing).
* **`kube-scheduler`:** Records scheduling decisions.
* **`kube-controller-manager`:** Records activities of built-in controllers.
* **`etcd`:** Records database events.
* **Monitoring:** For application and worker node metrics, the standard approach is to use **Prometheus** (running in the cluster) and **Amazon Managed Service for Prometheus (AMP)**.

### 14. How do you perform a rolling deployment in Kubernetes?
See Q5. A rolling deployment is performed by updating the **image tag or configuration** within the Deployment manifest. The Deployment controller then gradually replaces the old Pods with new ones according to the defined update strategy (`maxSurge` and `maxUnavailable`). Command: **`kubectl apply -f deployment.yaml`**.

### 15. What’s the difference between ReplicaSet, Deployment, and StatefulSet?
* **ReplicaSet:** Ensures a specified number of identical Pods are running. Low-level controller.
* **Deployment:** Manages the **ReplicaSet** and provides higher-level features like **rolling updates, rollbacks, and versioning**. Used for stateless applications.
* **StatefulSet:** Used for applications that require **stable, unique network identities** and **persistent storage** (e.g., databases).

### 16. How do you implement pod-to-pod security in EKS?
Pod-to-Pod security (micro-segmentation) is implemented using **Network Policies**.
1.  **Enable:** Ensure a Network Policy CNI (like Calico or Cilium) is installed on the EKS cluster (the default AWS VPC CNI only supports basic policies).
2.  **Define:** Create Network Policy manifests that use Pod/Namespace selectors to explicitly define which Pods are allowed to communicate with others.

### 17. How do you run VAPT hardening in EKS with Twistlock / Prisma Cloud?
Twistlock/Prisma Cloud (by Palo Alto Networks) is a **Container Security Platform** used for hardening and monitoring:
1.  **Vulnerability Scanning:** Scans ECR images and Pods in real-time for known vulnerabilities.
2.  **Compliance:** Checks cluster configurations against security benchmarks (CIS, NIST).
3.  **Runtime Protection:** Deploys a daemonset agent on each worker node to monitor and enforce behavior-based policies on running containers (blocking unauthorized file access or network connections).

### 18. What is the default CNI used in EKS? How do you troubleshoot CNI issues?
The default CNI (Container Network Interface) used in EKS is the **AWS VPC CNI**. It assigns a primary **VPC IP address** directly to each Pod, making Pods first-class citizens in the VPC.

**Troubleshooting CNI Issues:**
1.  **Check CNI Pod Status:** **`kubectl get pods -n kube-system -l k8s-app=aws-node`**
2.  **Check Logs:** **`kubectl logs -n kube-system -l k8s-app=aws-node`**
3.  **IP Exhaustion:** Check the **`aws-node`** logs for messages about exhausting the available IP addresses in the subnet.

### 19. What is the difference between AWS VPC CNI, Calico, and Cilium in EKS?
| CNI | IP Assignment | Network Policy Enforcement | Primary Use Case |
| :--- | :--- | :--- | :--- |
| **AWS VPC CNI** | Assigns **VPC IPs** directly to Pods. | **Limited** policy enforcement (requires other tools). | Simple setup, integrating with VPC features. |
| **Calico** | Can use VPC IPs (via CNI-mode) or its own overlay network. | **Robust, widely adopted Network Policy** enforcement. | Standard requirement for Kubernetes NetworkPolicy. |
| **Cilium** | Uses **eBPF** for high-performance data plane and policies. | **Advanced Network Policy, transparent encryption**, and observability. | High-performance, highly secure environments. |

### 20. How do you enable audit logging in Kubernetes/EKS?
For EKS, audit logging is enabled by configuring **Control Plane Logging** (see Q13) through the AWS console or `eksctl`. You specifically enable the **`audit`** log type, which sends a detailed record of every API server request (who did what, when) to **CloudWatch Logs**.

### 21. What are the tasks you have done on the Kubernetes level?
My tasks include: **Developing and maintaining Helm charts, managing cluster authentication via IRSA, troubleshooting Pod failures, configuring HPA and VPA, implementing Network Policies, and supporting EKS cluster upgrades.**

### 22. Did you get a chance to upgrade the EKS cluster? What tips do you follow?
Yes, I assisted in upgrading EKS from (e.g., v1.27 to v1.28). **Tips followed:**
1.  **Test in Dev/Staging first.**
2.  **Check compatibility** of all add-ons (CNI, CoreDNS) with the target K8s version.
3.  Ensure all critical services have a **Pod Disruption Budget (PDB)**.
4.  Upgrade the **Control Plane first**, then the **Managed Node Groups** (allows AWS to handle draining).

### 23. What is the command to access a pod and how can you define or create a Kubernetes class or object?
* **Access Pod (Shell):** **`kubectl exec -it [pod_name] -- /bin/bash`** (or `/bin/sh`).
* **Define/Create Object:** Objects are **defined** in a **YAML manifest file** and **created** (or updated) by running **`kubectl apply -f [manifest_file.yaml]`**.

### 24. Explain the folder structure of a basic Helm chart. What commands do you use to deploy with Helm?
**Helm Chart Folder Structure:**
* **`Chart.yaml`**: Metadata.
* **`values.yaml`**: Default configuration values.
* **`templates/`**: Kubernetes YAML manifest templates (the core of the chart).
* **`charts/`**: Dependencies (subcharts).

**Deployment Commands:**
* **Install/Upgrade:** **`helm upgrade --install [release_name] [chart_path] -f values-staging.yaml`**
* **View Status:** **`helm status [release_name]`**

### 25. How do you handle authentication for EKS clusters and store secrets securely in your environment?
* **EKS Authentication:** Handled using **AWS IAM** combined with **IRSA** (for Pods) and the **`aws eks update-kubeconfig`** command (for client access).
* **Secrets Storage:** We use **AWS Secrets Manager** or **HashiCorp Vault**, integrated with Kubernetes using an **External Secrets Operator** which securely injects secrets into K8s Secret objects.

### 26. What is email signing and Helm chart signing? Which tools do you use to sign Helm charts?
* **Helm Chart Signing:** The process of digitally signing a packaged Helm chart (`.tgz`) using a cryptographic key to verify its **authenticity and integrity**.
* **Tool:** The standard tool for Helm chart signing is **GnuPG (GPG)**, integrated using the Helm **`helm package --sign`** command.

### 27. How would you investigate issues in EKS/Kubernetes? What commands and tools would you use?
1.  **Initial Status:** **`kubectl get pods`** (identify the failing Pod/Deployment).
2.  **Details:** **`kubectl describe pod [pod_name]`** (check Events for scheduling, image, or OOM errors).
3.  **Logs:** **`kubectl logs -f [pod_name]`** (check application logs).
4.  **Cluster Health:** Check **CloudWatch Logs** for Control Plane errors (EKS-specific).
5.  **Metrics:** Check **Prometheus/Grafana** for historical resource utilization.

### 28. If there is a sudden spike in traffic (example: 100 requests to 500 requests), how will you scale your system?
The system scales automatically using a cascading mechanism:
1.  **Horizontal Pod Autoscaler (HPA):** The HPA detects the spike (e.g., CPU goes from 20% to 80%) and rapidly increases the **number of Pods** (e.g., 5 to 10 replicas).
2.  **Cluster Autoscaler (CA):** If the new Pods are stuck in a `Pending` state due to lack of resources, the CA detects this and triggers the **provisioning of new Worker Nodes** (EC2 instances) via the AWS ASG.

### 29. What is EKS, and how is it different from ECS?
* **EKS (Elastic Kubernetes Service):** Uses the **open-source Kubernetes** orchestration platform. Ideal for cross-cloud compatibility, mature ecosystems, and complex deployments.
* **ECS (Elastic Container Service):** AWS's **proprietary container orchestration service**. Simpler to set up, highly integrated with the AWS ecosystem, and often preferred for simpler, AWS-locked workloads.

### 30. What are task definitions and services in ECS?
* **Task Definition:** A **blueprint** (equivalent to a Pod/Deployment manifest in K8s) that describes the containers, network mode, CPU/memory, and other parameters required to run an application.
* **Service:** Defines **how many copies** (desired count) of a Task Definition should run and **how to access** them (load balancer integration).

### 31. If you created a service and associated it with a load balancer, how can you change the listener of the load balancer?
This is managed outside of ECS/EKS. You must modify the **AWS Load Balancer** configuration directly (usually an **Application Load Balancer or Network Load Balancer**) using the **AWS Console, AWS CLI, or Terraform**. You would update the listener's port or rule configuration.

### 32. Can you change the target group of any ECS service if you created these ones?
Yes. The ECS service is configured to use a specific **Target Group**. You can modify the service configuration via the **AWS console, AWS CLI (`update-service`), or infrastructure code (Terraform)** to point to a different, pre-existing Target Group.

### 33. Do you know about ECR Replication?
**ECR Replication** is an AWS feature that allows you to automatically copy Docker images from a **source ECR repository** to a **destination ECR repository** in the same or a different AWS region. This is used for disaster recovery, multi-region deployments, or reducing image pull latency.

### 34. Have you created or deployed applications on EKS?
Yes, I have created and managed applications on EKS using **Helm charts** deployed via **Jenkins/GitLab CI pipelines**.

### 35. Do you know the EKS architecture or governance policies?
Yes. The EKS architecture is the standard **Kubernetes Master (Control Plane) - Worker Node** model, with the Control Plane being a managed, highly available service. **Governance policies** involve **RBAC** for cluster access, **IRSA** for AWS resource access, and **Network Policies** for network control.

### 36. Pod is in a failed state in EKS — how do you troubleshoot it?
A failed state means the container terminated and will not be restarted (e.g., Pod's `restartPolicy` is `Never`).
1.  **`kubectl logs -p [pod_name]`**: Get logs from the *previous* terminated container.
2.  **`kubectl describe pod [pod_name]`**: Check the `Reason` in the `State` (e.g., `Error`, `Completed`) and look for termination messages in the `Events`.

### 37. ECS vs EKS — when would you choose each?
* **Choose EKS when:** You need **multi-cloud compatibility**, require the **rich, flexible ecosystem of Kubernetes** (e.g., specific Helm charts, advanced CNI/Service Mesh), or have an engineering team already proficient in K8s.
* **Choose ECS when:** You are **committed to AWS**, value **simplicity and tight integration** with other AWS services, and have simpler container orchestration needs.

### 38. How do you handle authentication for EKS clusters and store secrets securely in your environment?
* **Authentication:** See Q25. We use **IRSA** for Pods to authenticate to AWS and **IAM** integrated with `kubeconfig` for human/tool access.
* **Secrets:** See Q25. We use the **External Secrets Operator** to pull secrets from **AWS Secrets Manager** and inject them into the cluster.
