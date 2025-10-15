## 🚢 Kubernetes and Orchestration Essentials

### 1\. Kubernetes Architecture

Kubernetes follows a **Master (Control Plane)-Worker (Node)** architecture. The **Control Plane** manages the cluster state, and **Worker Nodes** run the containerized applications.

### 2\. Command to run Kubernetes Manifest

The primary command to apply a manifest (YAML) file to the cluster is **`kubectl apply -f [manifest_file.yaml]`**.

### 3\. What happens when the master node fails; will the worker node keep running

If a **single Control Plane node fails**, the **Worker Nodes will continue to run** their existing applications (Pods). However, no new Pods can be scheduled, no scaling can occur, and state changes (like updates) cannot be performed until the Control Plane is restored or another manager takes over (in a High Availability setup).

### 4\. What is `etcd` used for

**`etcd`** is the **distributed key-value store** that serves as Kubernetes' **single source of truth**. It stores the entire cluster configuration data, state, network configuration, and metadata.

### 5\. Types of scaling used

1.  **Horizontal Pod Autoscaler (HPA):** Scales the **number of Pods** based on CPU utilization or custom metrics.
2.  **Vertical Pod Autoscaler (VPA):** Adjusts the **CPU and memory resource requests/limits** for containers inside Pods.
3.  **Cluster Autoscaler (CA):** Scales the **number of Worker Nodes** based on the presence of unschedulable Pods.

### 6\. Running instructions before deployment

Instructions like database migrations or temporary setup tasks are handled by a **Kubernetes Job** or **Init Containers**. **Init Containers** run to completion before the main application containers start.

### 7\. What is Headless

A **Headless Service** is a service where the `.spec.clusterIP` is set to **`None`**. Kubernetes does not assign a cluster IP and does not perform load balancing. It's used when you need direct access to the Pods, such as with **StatefulSets**.

### 8\. What is StatefulSet

A **StatefulSet** is a workload API object used to manage stateful applications (like databases). It guarantees a **stable, unique network identity and persistent storage** for each Pod, maintaining order and persistence across restarts.

### 9\. Main pointers under "spec" in the manifest file

The `spec` section defines the **desired state** of the object. Key pointers include:

  * **Deployment:** `replicas`, `selector`, `template` (Pod specification).
  * **Pod/Template:** `containers`, `volumes`, `restartPolicy`.
  * **Service:** `type`, `ports`, `selector`.

### 10\. What is difference between docker and Kubernetes

**Docker** is a tool for **containerization** (packaging and running a single application container). **Kubernetes** is a platform for **orchestration** (managing, scaling, and networking many containers across multiple nodes).

### 11\. What is difference between docker swarm and Kubernetes

**Docker Swarm** is a native, simpler, and less complex orchestration tool by Docker. **Kubernetes** is a **powerful, cloud-native standard** orchestration platform with richer features for networking, scaling, and self-healing.

### 12\. What is difference between docker container and a Kubernetes pod

A **Docker Container** is a single running instance of an image. A **Kubernetes Pod** is the **smallest deployable unit** in K8s, often hosting one main container, but capable of running **one or more tightly coupled containers** that share the same network namespace, storage, and lifecycle.

### 13\. What is namespaces in Kubernetes

**Namespaces** are a way to divide cluster resources into multiple **virtual clusters**. They provide a scope for names (preventing conflicts) and a means for resource quota management within an organization.

### 14\. What is role of `kube proxy`

**`kube-proxy`** runs on every Worker Node and manages network rules (using IPTables or IPVS) to facilitate **Service abstraction**. It ensures that network requests sent to a Service's IP and port are correctly routed to the appropriate backend Pods.

### 15\. What are different types of services within Kubernetes

1.  **ClusterIP (Default):** Exposes the Service on an internal IP; only reachable from within the cluster.
2.  **NodePort:** Exposes the Service on a static port on *each* Worker Node's IP.
3.  **LoadBalancer:** Exposes the Service externally using a cloud provider's load balancer (e.g., AWS ELB).
4.  **ExternalName:** Maps the Service to a DNS name.

### 16\. What is difference between `nodeport` and `load balancer`

**NodePort** exposes the Service on a high, static port across *all* nodes in the cluster, which is accessible externally. **LoadBalancer** automatically provisions an external cloud load balancer (like an AWS ALB or ELB) to distribute traffic to the nodes, providing a dedicated external IP and better traffic control.

### 17\. What is the role of `kubelet`

**`kubelet`** is the primary agent running on every Worker Node. It **communicates with the Control Plane**, ensuring that the containers described in the PodSpecs are running and healthy on its node, according to the desired state defined in `etcd`.

### 18\. What are day to day activites on Kubernetes

My daily activities include:

  * **CI/CD Pipeline Management:** Developing/maintaining Jenkinsfiles to deploy microservices using `kubectl` or Helm.
  * **Troubleshooting:** Investigating failing Pods (`CrashLoopBackOff`, `ImagePullBackOff`) using `kubectl logs` and `kubectl describe`.
  * **Resource Management:** Monitoring cluster capacity, managing **HPA** configurations, and defining Pod **resource requests/limits**.
  * **Cluster Upgrades/Maintenance:** Assisting with **EKS** cluster upgrades and configuration changes.

### 19\. What the difference between stateful set and demon set why we use it

| Feature | StatefulSet | DaemonSet |
| :--- | :--- | :--- |
| **Purpose** | Managing stateful applications (databases, ordered rollout). | Ensuring **one copy of the Pod runs on every single node** (or a subset). |
| **Identity** | Stable, unique network and storage identity per Pod. | No unique identity required. |
| **Use Case** | Databases (MySQL, PostgreSQL), messaging queues (Kafka). | Cluster-level services like logging agents (Fluentd), monitoring agents (Node Exporter). |

### 20\. If there is an update in an application and it is already deployed in Kubernetes how will it be done

Updates are handled by updating the Pod template in the **Deployment manifest**. When the manifest is applied (`kubectl apply -f`), the Deployment Controller performs a **Rolling Update**, creating new Pods with the updated image/configuration and gradually terminating the old Pods to ensure zero downtime.

### 21\. In case of blue green deployement how will be it done in Kubernetes

Blue/Green deployment is achieved by having two identical Deployments (**Blue** - current version, **Green** - new version) running simultaneously, fronted by a **single Service**. The deployment involves:

1.  Deploying the **Green** version (new Deployment).
2.  Testing the Green version internally.
3.  Updating the Service's **`selector`** to point traffic from the Blue Deployment to the Green Deployment.
4.  If successful, the old Blue Deployment is deleted.

### 22\. Which file i will make a change for doing blue green deployment

You make changes to **two files**:

1.  The **Deployment manifest** (to deploy the new **Green** version).
2.  The **Service manifest** (to update the `.spec.selector` to match the Pod label of the Green Deployment).

### 23\. How do you add repositories in the Helm chart?

Helm repositories are added using the **`helm repo add`** command on the client machine:
**`helm repo add [REPO_NAME] [REPO_URL]`**
The repositories are then updated using **`helm repo update`**.

### 24\. How do you work on Kubernetes CMD? How do you list the pods?

I primarily use the **`kubectl`** command-line tool.

  * **List Pods:** **`kubectl get pods`** (shows Pods in the current namespace).
  * **List Pods in all namespaces:** **`kubectl get pods --all-namespaces`**
  * **List Pods in a specific namespace:** **`kubectl get pods -n [namespace_name]`**

### 25\. If you want to delete a pod, what’s the command?

The command is **`kubectl delete pod [pod_name]`**. If the Pod is managed by a Deployment or ReplicaSet, the controller will immediately create a replacement Pod.

### 26\. How can we pull image using Kubernetes

Kubernetes **does not use a direct pull command**. The **Kubelet** on the Worker Node automatically pulls the image when it receives the Pod manifest from the Control Plane, based on the **`image:`** field specified in the Pod's container specification.

### 27\. How do we configure pod autoscaling?

Pod autoscaling is configured using the **Horizontal Pod Autoscaler (HPA)** resource:

1.  Create an HPA manifest that references the target Deployment or ReplicaSet.
2.  Define the **`minReplicas`**, **`maxReplicas`**, and the **`targetCPUUtilizationPercentage`** (or a custom metric target).
3.  Apply the manifest: **`kubectl apply -f hpa.yaml`**.

### 28\. Can you give an example of autoscaling?

We use **HPA** to scale our API gateway service. The HPA is configured to maintain an average CPU utilization of **50%**, with a minimum of **3 replicas** and a maximum of **10 replicas**. If CPU utilization exceeds 50%, the HPA increases the replica count to handle the load.

### 29\. What are Helm charts and how do you use them for application deployment?

**Helm Charts** are **packages of pre-configured Kubernetes resources** (YAML manifests) that can be easily deployed, versioned, and managed. I use them to define the Deployment, Service, ConfigMaps, and Ingress for an application in a reusable, templated format, deploying them via **`helm upgrade --install [release_name] [chart_path]`**.

### 30\. How do you add repositories in the Helm chart?

See Q23. Use the **`helm repo add [NAME] [URL]`** command.

### 31\. In EKS, how do you deploy an application?

I deploy applications to EKS using **`kubectl`** or **Helm** from our CI/CD pipeline (Jenkins/GitLab CI). The pipeline authenticates with EKS using a service account or the **`aws eks update-kubeconfig`** command.

### 32\. What are the main components of EKS?

EKS (Elastic Kubernetes Service) is Amazon's managed K8s service. Its main components are:

1.  **Managed Control Plane:** AWS manages the Kube-API server, etcd, etc. (high availability is built-in).
2.  **Worker Nodes:** EC2 instances managed by the customer or using **Managed Node Groups** (recommended).
3.  **VPC Networking:** EKS integrates with AWS VPC for networking.

### 33\. How do you create a Kubernetes pod in EKS?

You don't typically create individual Pods directly. You create a **Deployment** manifest (via `kubectl apply -f`) which creates a ReplicaSet, which then creates and manages the Pods according to the desired state.

### 34\. How do you connect your local system to EKS?

I use the **AWS CLI** to update my local `kubeconfig` file: **`aws eks update-kubeconfig --name [cluster_name] --region [region]`**. This configures `kubectl` to use IAM credentials for authentication with the EKS cluster.

### 35\. Can you pull an image using a URL? For example, from AWS ECR?

Yes. The Kubelet pulls the image using the full repository URL defined in the Pod's manifest, for example: **`image: 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest`**.

### 36\. How do we do autoscaling in EKS?

We use all three scaling types:

1.  **HPA** (Horizontal Pod Autoscaler) for Pod scaling (default K8s feature).
2.  **VPA** (Vertical Pod Autoscaler) for resource recommendations.
3.  **Cluster Autoscaler (CA)** or **Karpenter** for **Node Scaling**. CA scales the **Managed Node Groups** in/out.

### 37\. In the EKS, how do you deploy? Via `kubectl` or something else?

We primarily deploy using **Helm** from our Jenkins CI/CD pipelines, which simplifies manifest management. The Helm client then interfaces with the EKS API server via **`kubectl`** and the `kubeconfig` context.

### 38\. Is Jenkins integrated with Docker in your current setup?

Yes. Jenkins is integrated with Docker in two ways:

1.  **Image Building:** Jenkins pipelines execute **`docker build`** commands to containerize applications.
2.  **Jenkins Agents:** We use **JEP-200 (Docker agents)** where each pipeline stage runs inside a fresh Docker container to ensure a clean, isolated execution environment.

### 39\. Are you working with Kubernetes? What activities do you perform in Kubernetes?

Yes, extensively. My main activities involve **pipeline development, application deployment via Helm, troubleshooting Pod issues, monitoring resource utilization, and managing manifest files**.

### 40\. How do you list pods using `kubectl` commands?

**`kubectl get pods`** is the standard command. Use **`kubectl get pods -o wide`** for more details like node assignment and IP address.

### 41\. How do you troubleshoot pod logs in Kubernetes?

I use **`kubectl logs [pod_name]`**.

  * To follow logs in real-time: **`kubectl logs -f [pod_name]`**.
  * To see logs from a previous failed instance: **`kubectl logs -p [pod_name]`**.
  * To see logs from a specific container in a multi-container pod: **`kubectl logs [pod_name] -c [container_name]`**.

### 42\. You are using an on-prem Kubernetes setup — how would you upgrade Kubernetes from one version to another?

On-prem upgrades are typically performed using tools like **`kubeadm`** or the installer provided by the distribution (e.g., RKE). The process is sequential:

1.  **Upgrade Control Plane:** Upgrade manager components one by one using `kubeadm upgrade`.
2.  **Upgrade Worker Nodes:** **Drain** the worker node, upgrade its `kubelet` and `kubeadm` binary, then **uncordon** it.

### 43\. What is the difference between upgrading the control plane and worker nodes?

  * **Control Plane Upgrade:** Involves upgrading the Kube-API server, etcd, controller manager, and scheduler. This **updates the core logic** of the cluster.
  * **Worker Node Upgrade:** Involves updating the **`kubelet`** and **container runtime** on the node, ensuring they are compatible with the new Control Plane version and can run Pods correctly.

### 44\. Before upgrading, what steps will you take to ensure safety and compatibility?

1.  **Backup `etcd`:** Take a snapshot of the etcd database.
2.  **Check Upgrade Path:** Ensure the jump is supported (K8s usually supports N-2 minor versions).
3.  **Pod Disruption Budget (PDB):** Ensure all critical applications have PDBs defined to prevent mass outages.
4.  **Cordon and Drain:** For worker nodes, **`kubectl cordon`** (stops new Pods) and **`kubectl drain`** (safely evicts existing Pods) before the upgrade.

### 45\. What kind of backup will you take — cold backup or `etcd` backup?

I will take an **`etcd` backup (snapshot)**. This is a **live, logical backup** of the entire cluster state, which is required for restoring the cluster to a specific point in time.

### 46\. From a Kubernetes security perspective, what are the best practices you follow in an ADM environment?

1.  **RBAC (Role-Based Access Control):** Implement granular roles to restrict access to resources.
2.  **Network Policies:** Define rules for Pod-to-Pod and Namespace-to-Namespace communication (micro-segmentation).
3.  **Image Scanning:** Enforce mandatory image scanning at CI/CD and Registry stages.
4.  **Secrets Management:** Use native **Kubernetes Secrets** or external tools like **HashiCorp Vault**.
5.  **Pod Security Standards (PSS):** Ensure Pods run with minimal privileges (e.g., non-root user).

### 47\. Do you enable audit logs in Kubernetes? Why is that important?

Yes, audit logs are crucial. They record every **API request** made to the Kubernetes API server (who did what, when, and where). This is essential for **security forensics, compliance, and debugging** failed configuration changes.

### 48\. What are pre-requisites to upgrade a K8s cluster?

1.  **Sufficient Resources:** Ensure all Control Plane nodes and etcd cluster members are healthy.
2.  **`etcd` Backup** (as per Q45).
3.  **Tool Readiness:** Ensure the latest version of `kubeadm` (or distribution tool) is installed.
4.  **Compatibility Check:** Verify container runtime compatibility with the target K8s version.

### 49\. What is Pod Disruption Budget (PDB) in K8s?

A **Pod Disruption Budget (PDB)** is a manifest that defines the **minimum number or percentage of replicas** that must be available for a given application during a **voluntary disruption** (like node maintenance, upgrades, or `kubectl drain`).

### 50\. How do we make a K8s cluster highly available?

1.  **Multiple Control Plane Nodes:** Use **three or five manager nodes** (requires an odd number for Raft consensus).
2.  **Redundant etcd:** Deploy etcd as a **highly available cluster** across multiple managers.
3.  **Node Redundancy:** Deploy **Worker Nodes** across multiple Availability Zones (AZs).
4.  **HPA/Deployment:** Use **Deployments** with a sufficient number of **replicas** and anti-affinity rules.

### 51\. What monitoring tools are setup? Have you set the alerts and tell me some common errors you faced related to pod management.

We use **Prometheus** for metrics collection and **Grafana** for visualization and dashboards. **Alertmanager** is used for alerts (Slack/PagerDuty).

**Common Pod Management Errors:**

  * **`CrashLoopBackOff`**: Application within the Pod keeps crashing after starting (often due to application config error or code bug).
  * **`ImagePullBackOff`**: Kubelet cannot pull the image (often due to incorrect image name/tag or missing/expired image pull secret).
  * **`Pending` (due to unschedulable):** Pod requires resources (CPU/memory) that no available node can fulfill.

### 52\. What are manifest files?

Manifest files are **YAML (or JSON) files** that contain the **specifications** (desired state) for Kubernetes objects (Pods, Deployments, Services, etc.). These files are the primary way we interact with the K8s API server.

### 53\. How do you create and manage Kubernetes clusters (using tools like Terraform), and what are the master and worker nodes?

I use **Terraform** to define and manage our EKS clusters (Infrastructure as Code).

  * **Master Nodes (Control Plane):** Run the core services (API Server, Scheduler, Controller Manager, etcd). They **manage the cluster state**.
  * **Worker Nodes:** Run the applications (Pods). They contain the **Kubelet** and **container runtime**.

### 54\. What are common Kubernetes errors you’ve faced (like `CrashLoopBackOff`, `ImagePullError`), and how did you resolve them?

  * **`CrashLoopBackOff`**: **Resolution:** Use **`kubectl logs [pod_name]`** immediately to view application error logs and identify the startup failure (e.g., bad environment variable, DB connection failed).
  * **`ImagePullBackOff`/`ErrImagePull`**: **Resolution:** Use **`kubectl describe pod [pod_name]`** to see the exact error message (e.g., authentication required, tag not found). Often fixed by correcting the image tag or ensuring the **`imagePullSecrets`** are defined.

### 55\. What is the command to access a pod and how can you define or create a Kubernetes class or object?

  * **Access Pod (Shell):** **`kubectl exec -it [pod_name] -- /bin/bash`** (or `/bin/sh`).
  * **Define/Create Object:** Objects are **defined** in a YAML **manifest file** and **created** (or updated) by running **`kubectl apply -f [manifest_file.yaml]`**.

### 56\. Explain the folder structure of a basic Helm chart. What commands do you use to deploy with Helm?

**Helm Chart Folder Structure:**

  * **`Chart.yaml`**: Metadata about the chart (name, version, API version).
  * **`values.yaml`**: The **default configuration values** that can be overridden by the user.
  * **`templates/`**: Directory containing the Kubernetes **YAML manifest templates** (Deployment, Service, etc.) that use Go templating and values from `values.yaml`.
  * **`charts/`**: Optional directory for dependent charts.

**Deployment Commands:**

  * **Install/Upgrade:** **`helm upgrade --install [release_name] [chart_path] -f values-staging.yaml`**
  * **View Status:** **`helm status [release_name]`**

### 57\. How do you handle authentication for EKS clusters and store secrets securely in your environment?

  * **EKS Authentication:** Handled using **AWS IAM** combined with the **`aws eks update-kubeconfig`** command. The `kubectl` client uses an **IAM role** (or user) to authenticate via the **`aws-iam-authenticator`** component.
  * **Secrets Storage:** We use **AWS Secrets Manager** or **HashiCorp Vault**, integrated with Kubernetes using an **External Secrets Operator** which injects the secrets into K8s Secrets objects or directly into Pod environment variables.

### 58\. What is email signing and Helm chart signing? Which tools do you use to sign Helm charts?

  * **Email Signing:** Not directly related to K8s/Helm.
  * **Helm Chart Signing:** The process of digitally signing a packaged Helm chart (`.tgz` file) using a cryptographic key to verify its **authenticity and integrity** (ensuring it hasn't been tampered with).
  * **Tools:** The standard tool for Helm chart signing is **GnuPG (GPG)**, integrated using the Helm **`helm package --sign`** command.

### 59\. What are the key components of Kubernetes architecture and their roles?

See Q1.

  * **Control Plane Components (Master):**
      * **API Server:** Front-end for the Control Plane; handles all REST operations.
      * **etcd:** Stores the cluster state.
      * **Scheduler:** Selects the best node for a new Pod to run on.
      * **Controller Manager:** Runs controllers that regulate the cluster state (e.g., Node, Deployment, ReplicaSet controllers).
  * **Node Components (Worker):**
      * **Kubelet:** Ensures containers in Pods are running and healthy.
      * **Kube-proxy:** Manages network rules for Services.
      * **Container Runtime (e.g., containerd/Docker):** Pulls images and runs containers.

### 60\. What is the difference between a pod, a node, and a container?

  * **Container:** The smallest unit of execution, a running instance of a Docker image.
  * **Pod:** The smallest unit of deployment in K8s, a wrapper around one or more containers that share network and storage resources.
  * **Node:** A physical or virtual machine (Worker) that hosts and runs the Pods.

### 61\. What is the difference between a ReplicaSet and a Deployment?

  * **ReplicaSet:** Ensures a **specified number of Pod replicas** are running at any given time. It is a lower-level controller.
  * **Deployment (Preferred):** A **higher-level API object** that manages the state of the application by controlling the **ReplicaSet**. It handles features like **rolling updates, rollbacks, and versioning**, which ReplicaSet does not.

### 62\. What does a Deployment support (updates, rollback, versioning)?

A Deployment supports all three:

  * **Updates:** By creating a new ReplicaSet with the updated Pod template (Rolling Update).
  * **Rollback:** By reverting to a previous, successful ReplicaSet version (**`kubectl rollout undo deployment/[name]`**).
  * **Versioning:** By automatically maintaining a history of all previous ReplicaSets.

### 63\. What is a ValidatingWebhookConfiguration and MutatingWebhookConfiguration?

These are **Admission Controllers** that intercept API requests *before* the object is persisted to `etcd`.

  * **Mutating Webhook:** Can **modify or change** the resource (e.g., adding sidecar containers or labels) before validation.
  * **Validating Webhook:** Can only **validate or reject** the resource based on custom rules (e.g., reject a Pod if it runs as root).

### 64\. PVCs are stuck in Pending state due to storage bottleneck — how do you troubleshoot?

1.  **Check Events:** Use **`kubectl describe pvc [pvc_name]`** and look at the **Events** section for the failure reason (e.g., "no persistent volumes available for this claim").
2.  **Check StorageClass:** Verify the StorageClass referenced by the PVC is correct and that its provisioner is running and healthy.
3.  **Check Provisioner Logs:** If using dynamic provisioning, check the logs of the storage provisioner Pod (e.g., EBS/EFS CSI driver) for capacity or authentication errors.

### 65\. Worker node AMI replacement caused some pods to remain in Terminating state — how do you investigate and remove them safely?

1.  **Investigate:** Use **`kubectl describe pod [pod_name]`** to check the reason for termination. Pods remain terminating if the **Node is gone** and Kubelet can't confirm the Pod has stopped.
2.  **Force Deletion:** If the Node is definitively gone, use **`kubectl delete pod [pod_name] --force --grace-period=0`**. (Use with caution, as it bypasses safety checks).
3.  **Node Deletion:** Ensure the old Node object is deleted: **`kubectl delete node [old_node_name]`**.

### 66\. How do you troubleshoot out-of-memory (OOM) errors?

1.  **Check Events:** **`kubectl describe pod [pod_name]`** and look for the termination reason "OOMKilled."
2.  **Check Resource Limits:** Verify the container's **`resources.limits.memory`** in the PodSpec. If the app exceeds this, it gets OOMKilled.
3.  **Check Logs:** The app usually won't log the OOM, but check for any preceding high memory utilization reported by Prometheus/Grafana.
4.  **Resolution:** Increase the container's memory limit or optimize the application's memory usage.

### 67\. Some pods are Pending and fail to schedule — what could be the reasons?

1.  **Resource Constraints:** No node has enough **free CPU or memory** to satisfy the Pod's **`resources.requests`**.
2.  **Node Selectors/Taints:** The Pod may require a specific **Node Selector** or fail to tolerate a **taint** present on all available nodes.
3.  **PV/PVC Issues:** The Pod is waiting to mount a PVC that is still Pending or in use.

### 68\. Pods fail to start due to image pull or “image full backup” errors — how do you fix it?

The error is **`ImagePullBackOff`** or **`ErrImagePull`**.

1.  **Credentials:** Ensure **`imagePullSecrets`** are defined in the Pod/ServiceAccount and contain valid credentials for the image registry.
2.  **Image Tag:** Verify the image name and tag are spelled correctly and exist in the registry.
3.  **Registry Access:** Check Network Policies/Firewalls to ensure the Worker Node can reach the registry URL.

### 69\. How does the Deployment know which registry and user to pull the image from?

  * **Registry:** The registry is part of the image URL (e.g., `myregistry.com/repo/image`).
  * **User/Auth:** The credentials are provided via **`imagePullSecrets`**. This Secret (of type `kubernetes.io/dockerconfigjson`) contains the login details for the registry and is referenced by the Pod or Service Account.

### 70\. How do you manage pulling thousands of images across nodes automatically?

This is managed by **caching** and **pre-pulling** images:

1.  **Node Level Caching:** The container runtime on each node automatically caches images after the first pull.
2.  **Image Pre-pulling:** Use a **DaemonSet** or an Ansible playbook to pre-pull large, common images (like the base OS) onto all worker nodes to speed up the Pod startup time.

### 71\. Pods in a CrashLoopBackOff — how do you troubleshoot?

See Q54. This is an application-level failure. The process is:

1.  **`kubectl describe pod`**: Check the **Events** and **Restart Count**.
2.  **`kubectl logs [pod_name]`**: Read the application logs to find the root cause (e.g., failed DB connection, configuration error).
3.  **`kubectl exec`**: Temporarily run a shell inside the crashing container if the executable is available to diagnose the environment.

### 72\. Pods failing due to resource constraints — how do you increase memory/CPU?

Increase the **`resources.limits`** and **`resources.requests`** values in the Pod (or Deployment) manifest for the affected container. This requires applying the updated manifest to trigger a rolling update.

### 73\. How do you scale resources at the node level (not pod level)?

Scaling at the node level is managed by the **Cluster Autoscaler (CA)** or **Karpenter**. These tools monitor unschedulable Pods and interact with the underlying cloud provider's auto-scaling group (ASG) or infrastructure APIs to dynamically add or remove Worker Nodes.

### 74\. How do you ensure zero-downtime deployments?

1.  **Rolling Updates (Default):** Deployments use this strategy, gradually replacing old Pods with new ones.
2.  **Readiness Probe:** Define a **`readinessProbe`** in the PodSpec so that new Pods only receive traffic *after* they are truly ready (e.g., finished warming up).
3.  **Pod Disruption Budget (PDB):** Ensure critical services maintain a minimum replica count during voluntary disruptions.
4.  **PreStop Hook:** Use a **`preStop`** hook to gracefully shut down the application and prevent dropped requests.

### 75\. Cluster Autoscaler is enabled but new nodes are not being created — how do you troubleshoot?

1.  **Check CA Logs:** Check the logs of the **Cluster Autoscaler Pod** for the reason it's not scaling (e.g., insufficient permissions, maximum size reached, resource limits).
2.  **Check Unschedulable Pods:** Verify that there are Pods truly stuck in a **Pending** state with a reason indicating resource scarcity.
3.  **Check IAM/Security Group:** Ensure the CA has the necessary **IAM permissions** to interact with the cloud provider's Auto Scaling Groups (ASGs).

### 76\. Some nodes in the cluster are in NotReady state — how do you investigate?

1.  **`kubectl describe node [node_name]`**: Check the **Conditions** section for specific failure reasons (e.g., `KubeletReady: False`, `MemoryPressure`).
2.  **Check Kubelet Logs:** SSH into the affected node and check the **`kubelet` logs** (e.g., `journalctl -u kubelet`) for errors regarding resource exhaustion, network failures, or communication issues with the Control Plane.
3.  **Resource Check:** Verify the node has sufficient CPU, memory, and disk space.

### 77\. How do you manage worker node auto-scaling — Carpenter vs Cluster Autoscaler?

  * **Cluster Autoscaler (CA):** Scales an existing, predefined **Auto Scaling Group (ASG)**. It's safe but slower, as it relies on the ASG definition.
  * **Karpenter:** A newer, more modern autoscaler (especially for EKS) that **launches new nodes directly** based on Pod requirements, optimizing cost and speed. It doesn't rely on pre-configured ASGs.

### 78\. What are the types of containers?

The most common classifications are:

1.  **Application Containers:** The primary container running the main application code.
2.  **Init Containers:** Containers that run to **completion** before the application containers start (used for setup tasks).
3.  **Sidecar Containers:** Auxiliary containers that run **alongside** the main application container within the same Pod (used for logging, monitoring, or service mesh).

### 79\. What is the difference between a liveness probe and a readiness probe? When would you use each?

| Probe | Purpose | Action on Failure | Use Case |
| :--- | :--- | :--- | :--- |
| **Liveness** | Checks if the **application is healthy and running**. | **Restarts** the container. | Detecting deadlocks, unrecoverable failures. |
| **Readiness** | Checks if the **application is ready to serve traffic**. | Removes the Pod's IP from the Service **endpoints**. | Application startup/warmup time, maintenance periods. |

### 80\. Your application deployment in Kubernetes is failing with a `CrashLoopBackOff` error. How would you troubleshoot this?

See Q54 and Q71. The immediate steps are:

1.  **View Logs:** `kubectl logs [pod_name]` to find the application-level reason for the crash (code bug, bad config, missing file).
2.  **Describe Pod:** `kubectl describe pod [pod_name]` to check **Events** for startup errors (OOMKilled, bad command).
3.  **Fix and Retry:** Correct the issue (e.g., fix a bad environment variable) and re-apply the deployment.

### 81\. Probes for unresponsive application (deadlock) and startup time.

  * **Unresponsive (Deadlock):** Use a **`livenessProbe`** (e.g., an HTTP GET to a health endpoint) configured with a **low failure threshold** and a short timeout. If the probe fails, K8s restarts the Pod.
  * **Startup Time:** Use a **`readinessProbe`** configured with a long **`initialDelaySeconds`** and a short **`periodSeconds`**. This tells K8s to wait for the application to fully initialize before sending it traffic. A **`startupProbe`** can also be used to handle long, initial startup times without interfering with the liveness probe.

### 82\. What is a Pod Disruption Budget (PDB) in Kubernetes, and how does it help in maintaining high availability of applications?

See Q49. A **PDB** limits the number of concurrent voluntary disruptions an application can experience. It ensures that during actions like node drain or cluster upgrades, the application's availability is maintained by guaranteeing a **minimum number of running Pods**.

### 83\. Explain the difference between `minAvailable` and `maxUnavailable`.

  * **`minAvailable`:** Specifies the **minimum number** or **percentage** of Pods that **must be available** (running) during the disruption.
  * **`maxUnavailable`:** Specifies the **maximum number** or **percentage** of Pods that **can be unavailable** during the disruption.

### 84\. How does PDB interact with `kubectl drain` or rolling updates?

  * **`kubectl drain`:** When a node is drained, K8s respects the PDB. It will only evict Pods if the PDB's minimum availability constraint is met, pausing the drain if necessary.
  * **Rolling Updates:** PDB **does not interfere** with the default **Rolling Update** strategy of a Deployment, as rolling updates are designed to replace Pods gradually. PDB primarily focuses on **voluntary, non-update** evictions (like maintenance).

### 85\. Give a YAML example of PDB for a Deployment with 5 pods, ensuring at least 3 are always available.

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: my-app-pdb
spec:
  minAvailable: 60%  # 60% of 5 pods = 3 pods
  selector:
    matchLabels:
      app: my-app
```

### 86\. What can cause resource exhaustion in Kubernetes?

1.  **Misconfigured Limits:** Pods without memory or CPU limits can consume all node resources.
2.  **Memory Leaks:** Application-level memory leaks that eventually trigger OOMKilled events.
3.  **Load Spikes:** Sudden, high traffic bursts that exceed the capacity defined by HPA/VPA.
4.  **Unschedulable Pods:** An inadequate number of Worker Nodes to meet all Pod resource requests.

### 87\. Sudden traffic spike scenario: system handles 200 requests, but 500 requests arrive — how would you handle this?

1.  **HPA (Horizontal Pod Autoscaler):** This is the immediate mechanism; the HPA quickly increases the Pod count based on CPU/Request queue metrics.
2.  **CA (Cluster Autoscaler):** If the new Pods can't be scheduled, the CA triggers the provisioning of new Worker Nodes to handle the load.
3.  **Rate Limiting:** Implement an Ingress/Gateway rate-limiting policy to protect backend services from being overwhelmed while scaling occurs.

### 88\. How does HPA (Horizontal Pod Autoscaler) behave? How do you define min/max pods, and how does it react to sudden load increase/decrease?

  * **Behavior:** HPA continuously monitors metrics and calculates the necessary number of Pods required to meet the **target metric value**.
  * **Min/Max:** Defined in the HPA manifest: `minReplicas` and `maxReplicas`.
  * **Increase:** HPA reacts quickly to load increases (aggressive scaling).
  * **Decrease:** HPA applies a **cooldown delay** (e.g., 5 minutes) before scaling down, preventing rapid flapping and ensuring stability if the load spike was transient.

### 89\. Suppose you have a sudden spike in traffic, and HPA scales pods, then the load goes down immediately — what will happen?

The HPA will hold the elevated replica count for a defined **cooldown period** (usually 5-10 minutes) before starting to scale down. This is done to prevent the system from aggressively scaling down immediately after a brief spike, which could lead to rapid, unnecessary scaling oscillations (thrashing).

### 90\. How do you investigate Kubernetes issues using logs, events, and cloud watch?

  * **Logs (Application):** **`kubectl logs`** is used for application debugging.
  * **Events (System):** **`kubectl get events`** or **`kubectl describe [object]`** is used to check for system-level errors (scheduling failures, image pull failures, node failures).
  * **CloudWatch (EKS):** The EKS Control Plane is integrated with **CloudWatch Logs** for accessing Kube-API server, Scheduler, and Controller Manager logs, which is vital for troubleshooting cluster-level control plane issues.

### 91\. How do you handle root cause analysis for production issues?

1.  **Gather Data:** Collect logs (`kubectl logs`), events (`kubectl get events`), and metrics (Prometheus/Grafana) from the time of the incident.
2.  **Recreate & Isolate:** Attempt to reproduce the failure in a staging environment.
3.  **Trace:** Use distributed tracing tools (e.g., Jaeger) to pinpoint the exact failing service and code path.
4.  **Hypothesize & Test:** Formulate hypotheses (e.g., "It's an OOM due to high traffic") and test them.
5.  **Fix & Post-Mortem:** Implement a fix, deploy, and conduct a **blameless post-mortem** to document the cause, remediation steps, and preventive actions (e.g., increasing limits, improving HPA).

-----
