## 🚀 Jenkins and CI/CD Essentials

### 1\. On which language Jenkins is written?

Jenkins is primarily written in **Java**. Pipeline scripts are written in **Groovy**.

### 2\. Types of projects in Jenkins

The main types of jobs/projects are **Freestyle Project**, **Pipeline** (Declarative or Scripted), **Multi-configuration Project** (Matrix Build), and **Multibranch Pipeline**.

### 3\. Syntax of scripted and declarative pipeline

  * **Declarative:** Structured, top-level `pipeline { ... }` block. Easier to read and more restrictive.
  * **Scripted:** Traditional Groovy code within a `node { ... }` block. Highly flexible and complex.

### 4\. Jenkins CLI command to start Jenkins server

Jenkins is typically started via its service wrapper. If installed via a package manager (like `apt` or `yum`), the command is usually **`sudo systemctl start jenkins`** or **`sudo service jenkins start`**.

### 5\. What is CI/CD?

  * **CI (Continuous Integration):** The practice of **merging code changes frequently** (multiple times a day) into a central repository, followed by automated builds and tests.
  * **CD (Continuous Delivery/Deployment):** The practice of ensuring software can be **released reliably** (Delivery) or **automatically deployed** to production (Deployment) after the CI phase.

### 6\. Some tools for CI/CD

**Jenkins**, GitLab CI, GitHub Actions, CircleCI, Travis CI, Azure DevOps, and tools like **Nexus/Artifactory** (artifact management) and **Ansible/Terraform** (deployment).

### 7\. Benefits of Continuous Integration

CI leads to **earlier bug detection**, **reduced integration problems**, **faster release cycles**, and a generally **healthier codebase** because changes are small and frequently validated.

### 8\. Installation steps of Jenkins

1.  Ensure **Java (JDK/JRE)** is installed.
2.  Add the **Jenkins repository key and source list** to the system (OS-dependent).
3.  Install Jenkins using the package manager (`sudo apt install jenkins`).
4.  Start the service (`sudo systemctl start jenkins`).
5.  Access the web interface (default port 8080) and unlock it using the **initial administrator password** found in the logs.

### 9\. Some errors faced while creating pipelines and debugging methods

  * **Syntax Errors:** Usually caught by the **Pipeline Syntax Linter** or the **Replay** feature in the build history.
  * **Missing Dependencies/Permissions:** Debugged by viewing the **Console Output** for class loading errors or access denied messages.
  * **Agent Errors:** Debugged by checking the **Node log** or ensuring the agent is correctly configured and online.

### 10\. Syntax for pipeline

  * **Declarative:** Starts with `pipeline { ... }`
  * **Scripted:** Starts with `node { ... }`

### 11\. Default Jenkins home directory

The default directory is **`/var/lib/jenkins`** on Linux systems installed via package managers. This directory stores all configuration files, job workspaces, build logs, and plugins.

### 12\. Ways to automate build trigger

1.  **SCM Polling:** Jenkins periodically checks the SCM (Git) for changes. (Less preferred due to resource usage).
2.  **Webhooks:** The SCM (GitHub, GitLab) sends a **POST request** to a Jenkins URL whenever a commit is pushed. (Preferred method).
3.  **Upstream/Downstream Jobs:** Triggering Job B after Job A successfully completes.
4.  **Scheduled Builds:** Using **`cron`** syntax.

### 13\. If we have to keep only 5 old builds

This is achieved using **Discard Old Builds** configuration, typically set within the job configuration page under the General section, specifying **Max \# of builds to keep** as 5.

### 14\. How to create Jenkins backup

The simplest method is to **archive the entire `$JENKINS_HOME` directory**. More robust methods involve:

1.  Using a **dedicated backup plugin** (e.g., ThinBackup).
2.  Using **`rsync`** or similar tools to sync `$JENKINS_HOME` to a secure location or S3 bucket.

### 15\. Jenkins security

Key measures include:

1.  Enabling **Global Security** (Jenkins $\rightarrow$ Manage Jenkins $\rightarrow$ Configure Global Security).
2.  Integrating with an **external authentication source** (LDAP, OAuth).
3.  Configuring **Role-Based Access Control (RBAC)** using a plugin.
4.  Ensuring **Node Agents** (slaves) run with minimum necessary privileges.

### 16\. How to deploy an application in Jenkins

Deployment is usually handled in a dedicated **`Deploy` stage** of the pipeline. This involves using:

1.  **SSH** to execute commands on the target server.
2.  **Cloud-specific plugins** (e.g., AWS CodeDeploy).
3.  **Deployment tools** called from the pipeline (e.g., Ansible, Terraform, Kubernetes commands).

### 17\. Multibranch pipeline project

A Multibranch Pipeline automatically **discovers branches** in an SCM repository and creates a separate, dedicated Jenkins job for **each branch** that contains a **`Jenkinsfile`**. It is used to ensure every feature branch is continuously tested.

### 18\. How to set Jenkins build to fail based on a specific word in console output

This is typically done using the **Log Parser Plugin**. You define rules (e.g., treat "ERROR" or "FAILURE" as a failure) and configure the build to check the console output against those rules.

### 19\. Active and reactive parameters

  * **Active (Standard) Parameters:** Simple inputs defined before the build starts (e.g., String, Boolean, Choice).
  * **Reactive Parameters:** Parameters whose **options or visibility change dynamically** based on the value selected for a previous parameter. This requires plugins like the **Active Choices Plugin**.

### 20\. If-else in scripting pipeline

In Scripted Pipeline, standard Groovy `if/else` syntax is used directly:

```groovy
node {
    stage('Check') {
        if (params.ENV == 'prod') {
            sh 'echo "Running production setup"'
        } else {
            sh 'echo "Running development setup"'
        }
    }
}
```

### 21\. Master and slave architecture

The current terms are **Controller (Master)** and **Agent (Slave/Node)**.

  * **Controller:** The central server that schedules jobs, manages configurations, and stores job history. It typically handles the orchestration and lightweight tasks.
  * **Agent:** A machine (physical or virtual) that is connected to the Controller and executes the actual **heavy-lifting build steps** (compilation, testing, deployment), offloading work from the Controller.

### 22\. What is Jenkins shared library, why is it used, and its structure

  * **What:** A **collection of reusable Groovy scripts** (functions, classes) stored in a separate SCM repository.
  * **Why:** To promote **Don't Repeat Yourself (DRY)** principles, centralize pipeline logic, and enable complex logic to be reviewed outside of individual `Jenkinsfiles`.
  * **Structure:** Typically contains **`vars/`** (for global, callable functions/steps) and **`src/`** (for formal Groovy classes).

### 23\. Some most useful Jenkins plugins

  * **SCM:** **Git Plugin** (essential for pulling code).
  * **Build Tools:** **Maven Integration Plugin**, **Pipeline Utility Steps Plugin**.
  * **Deployment:** **SSH Agent Plugin**, **Kubernetes Plugin**.
  * **Notification:** **Email Extension Plugin**.
  * **Quality/Security:** **SonarQube Scanner Plugin**.

### 24\. A Jenkins build fails due to a missing dependency but runs perfectly fine on the local machine - what to do?

The issue is an **inconsistency in environments**.

1.  **Check the Agent Environment:** Verify that the agent has the correct JDK/Maven/Node versions and configurations as the local machine.
2.  **Clear Local Cache:** The agent's Maven/npm cache (`~/.m2` or `~/.npm`) might be corrupt. Use `mvn clean install -U` to force an update.
3.  **Check Repository Access:** Confirm the agent has network access to the remote artifact repository (Nexus/Artifactory) and valid authentication in its `settings.xml`.

### 25\. Jenkins build runs out of disk space, causing build failure - how to resolve?

1.  **Clear Workspace:** Implement an automatic cleanup step at the end of the pipeline.
2.  **Discard Old Builds:** Configure the job to keep fewer build records (see Q13).
3.  **Clear Caches:** Clear temporary directories or Maven/npm local caches on the agent.
4.  **Allocate More Space:** Scale up the agent's disk capacity (often the final solution).

### 26\. After restarting, Jenkins remains stuck at the preparing Jenkins or initialization stage - what to do?

This is often caused by a **corrupt plugin or job configuration**.

1.  **Safe Mode:** Restart Jenkins in **Safe Mode** (`java -jar jenkins.war --disableAllSecurity --enable-safe-plugins`) to isolate plugin issues.
2.  **Check Logs:** Examine the **Jenkins log file** (`/var/log/jenkins/jenkins.log`) for stack traces or `java.io.IOException` errors indicating file corruption.
3.  **Remove Bad Plugin:** Manually remove the suspected plugin's directory from `$JENKINS_HOME/plugins`.

### 27\. Jenkins job is queued but doesn't start - what to do?

The system is waiting for an available **Agent** that matches the job's requirements.

1.  **Check Node Status:** Verify that the required agent (specified in the `agent { label '...' }` block) is **online and connected**.
2.  **Check Executor Availability:** Ensure the agent has **free executors**. If all executors are busy, the job must wait.
3.  **Check Node Requirements:** Verify the job's label/tool requirements match the agent's configuration.

### 28\. Do you have separate Jenkins servers for each environment?

*(Common practice is one server with multiple agents)* No, we use a single **Jenkins Controller** with **multiple dedicated Agents (or agent labels)**. We use labels like `prod-agent` and `dev-agent` to ensure builds and deployments for sensitive environments only run on specific, secured nodes.

### 29\. In Jenkins, have you worked on it?

*(Answer based on your experience)* Yes, I have extensive experience using and administering Jenkins. I primarily focus on **creating and maintaining Declarative Pipelines** for our microservices, integrating them with security scanning and deployment tools.

### 30\. When you run the pipeline, is it on the static space or dynamic space or directly on the master?

The pipeline execution logic (Groovy script) **runs on the Controller (Master)**. However, the heavy-lifting **build steps** (like `sh`, `mvn`, `docker build`) execute on an **Agent (Dynamic/Static Node)** specified by the `agent { ... }` block. We avoid running intensive tasks directly on the Controller.

### 31\. Can you tell me the basic stages and the flow of your pipeline?

Our basic pipeline flow follows these stages:

1.  **Checkout:** Pulls code from SCM (Git).
2.  **Build:** Compiles the code (e.g., `mvn package` or `npm install`).
3.  **Unit Test:** Executes unit tests.
4.  **Code Scan (Quality Gate):** Runs SonarQube or similar security/quality checks.
5.  **Package:** Creates the final deployable artifact (Docker image or JAR/WAR).
6.  **Deploy to Dev:** Deploys the artifact to the development environment.
7.  **Integration Test:** Runs integration tests against the deployed service.
8.  **Manual Approval/Deploy to Prod:** Waits for approval before deploying to Production.

### 32\. Do you monitor build check-ins?

Yes, we monitor check-ins via **Webhooks**. When a developer commits code, the SCM (GitHub/GitLab) sends a notification to Jenkins, immediately triggering a pipeline run for that branch or repository.

### 33\. Is your current project based on Java?

*(Answer based on your experience)* Yes, our primary microservices are developed in **Java (Spring Boot)**. We use the **Maven Integration Plugin** within Jenkins to manage the build process.

### 34\. Do you know how to create projects and configure check-ins?

Yes. Projects are created by selecting a type (e.g., **Pipeline** or **Multibranch Pipeline**). Check-ins are configured by specifying the **SCM URL** (Git repository) and setting up an automatic trigger, preferably a **Webhook**, in the build trigger section.

### 35\. Do you create Jenkins pipelines using declarative or freestyle syntax?

I primarily use the **Declarative syntax**. It's easier to read, enforce structure, and manage within a `Jenkinsfile` stored in the SCM. I use Scripted syntax only for highly complex logic contained within Shared Libraries.

### 36\. What are the different stages in a Jenkins declarative pipeline?

A declarative pipeline is structured into distinct blocks:

  * **`pipeline`**: The root block.
  * **`agent`**: Specifies where the entire pipeline will run (e.g., `label 'docker-agent'`).
  * **`stages`**: Contains one or more **`stage`** blocks.
  * **`stage`**: A logical division of work (e.g., `Build`, `Test`, `Deploy`).
  * **`steps`**: Contains the commands executed within a stage (e.g., `sh 'mvn clean install'`).
  * **`post`**: Defines actions to run *after* the pipeline or stage completes (e.g., `always`, `failure`, `success`).

### 37\. How is the CI/CD pipeline setup in your project? What are the security tools integrated?

Our setup uses **Jenkins Declarative Pipelines** stored as `Jenkinsfiles` in each service repository. The pipeline automatically triggers via **Webhooks** on every commit.

**Security Tools Integrated:**

  * **SAST (Static Analysis):** **SonarQube** (runs in the `Code Scan` stage).
  * **Vulnerability Scanning:** **Trivy** or **Clair** (runs in the `Package` stage to scan the Docker image base layers).

### 38\. How do you manage them?

  * **Pipeline Management:** We use **Shared Libraries** to standardize common security and deployment steps.
  * **Tool Management:** SonarQube and Nexus/Artifactory are managed as separate, centralized services, and Jenkins agents are configured with the necessary credentials and tool paths to interact with them.

### 39\. Write a rough pipeline script for microservices architecture.

```groovy
// Simple Declarative Pipeline for a Spring Boot Microservice
pipeline {
    agent { label 'maven-agent' }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Unit Test & Scan') {
            steps {
                sh 'mvn test'
                // Assume 'sonarScanner' is configured as a shared library step
                sonarScanner 'my-microservice' 
            }
        }
        stage('Package Docker Image') {
            steps {
                script {
                    def image = docker.build("my-repo/service:${env.BUILD_NUMBER}")
                    image.push()
                }
            }
        }
        stage('Deploy to DEV') {
            steps {
                // Assuming 'deployK8s' is a shared library function
                deployK8s cluster: 'dev', service: 'service-name', tag: env.BUILD_NUMBER
            }
        }
    }
}
```

### 40\. How do you find errors in the pipelines?

1.  **Console Output:** The primary source, I look for the exact line where the `sh` command fails.
2.  **Jenkins UI:** Using the **Stage View** to identify the exact stage that failed.
3.  **Replay Feature:** Replaying the pipeline with minor fixes or added `echo` statements to isolate the failure point without committing code.
4.  **Full Debug:** Adding `options { skipDefaultCheckout() }` and manually running steps for debugging, or running the build with verbose output (`-X` for Maven).

### 41\. Explain the CI/CD workflow you follow and the kind of pipeline you use.

We follow a **Trunk-Based Development** model with an automated **CI/CD workflow**. We use **Declarative Pipelines** across a **Multibranch Pipeline** project.

**Workflow:**

1.  Developer commits code to a feature branch.
2.  Webhook triggers the pipeline for that branch.
3.  CI runs (Build, Test, Scan).
4.  Upon merge to `main`, the CD pipeline is triggered.
5.  CD stages include Docker packaging, security scanning, and deployment to Dev/Staging.
6.  Deployment to Production requires a **manual approval gate**.

### 42\. How do you define and invoke pipelines in Jenkins?

**Definition:** Pipelines are primarily defined in a file named **`Jenkinsfile`**, which is committed to the root of the SCM repository.
**Invocation:**

1.  **SCM Polling/Webhooks** (automatic).
2.  **Manually** via the "Build Now" button (often with parameters).
3.  **Programmatically** via the Jenkins CLI or Remote API.

### 43\. What are shared libraries in Jenkins, and how are they written and defined?

  * **What:** A collection of **reusable Groovy code** (functions/steps) to standardize logic across multiple `Jenkinsfiles`.
  * **Written:** As Groovy scripts and classes within a defined **SCM repository structure** (primarily `vars/` and `src/`).
  * **Defined:** By configuring the SCM repository URL and a name in **Manage Jenkins $\rightarrow$ System $\rightarrow$ Global Pipeline Libraries**.

### 44\. What kind of applications do you deploy using Jenkins pipelines, and what deployment tools do you use?

*(Answer based on your experience)* We deploy **containerized microservices** (Java Spring Boot/Node.js). Our primary deployment tools are **Kubernetes** (using the `kubectl` tool or the official Kubernetes Jenkins plugin) and **Helm** (for packaging and managing Kubernetes manifests).

### 45\. If the Jenkins pipeline runs but the build doesn’t happen, what possible issues could be causing it?

This usually means the **orchestration is working, but the execution is failing silently or missing steps**.

1.  **Agent/Node Configuration:** The agent may not have the required tools (e.g., Maven or Docker) installed or configured in its path.
2.  **Missing `sh` or `bat` steps:** The stage might contain Groovy logic but is missing the actual `sh` (shell) or `bat` (Windows) wrapper commands to execute external build tools.
3.  **Incorrect Path:** The pipeline is trying to execute a tool (like `mvn`) that is not in the system's `$PATH`.

### 46\. What is the purpose of a webhook, and how is it used in a CI/CD pipeline?

**Purpose:** A webhook is an **HTTP callback** that triggers an automated action upon a specific event.
**Use in CI/CD:** When code is committed to a Git repository, the SCM (e.g., GitHub) sends a POST request (the webhook payload) to a pre-configured Jenkins URL, which **immediately triggers the corresponding pipeline build**. This eliminates the resource-heavy SCM Polling.

### 47\. What branching strategy do you follow, and how do you handle merges to avoid breaking the release branch?

We follow the **Trunk-Based Development** strategy (or Git Flow). To avoid breaking the release branch (`main`):

1.  **Mandatory Review:** All merges require **Pull Request review** and at least one approval.
2.  **Gating:** The pipeline runs **full CI checks** (tests, SonarQube scan) as a required **status check** before the merge is allowed.
3.  **Merge Strategy:** We enforce **Squash and Merge** to keep the `main` history clean.

### 48\. If a bug appears in production, what’s your approach to resolving it?

1.  **Verify & Triage:** Confirm the bug and its scope by checking monitoring tools/logs.
2.  **Hotfix Branch:** Immediately create a **hotfix branch** from the production tag/commit.
3.  **Fix and Test:** Apply the fix, run local unit tests, and manually test the fix.
4.  **CI/CD:** Run the hotfix branch through a **dedicated hotfix pipeline** with full checks.
5.  **Deploy:** Deploy the hotfix to production.
6.  **Merge Back:** Merge the hotfix branch back into `main` and `develop` to prevent regression.

### 49\. Describe your typical deployment flow and CI/CD workflow.

**CI Workflow:** SCM Commit $\rightarrow$ Webhook $\rightarrow$ Jenkins Pipeline Trigger $\rightarrow$ Checkout $\rightarrow$ Build $\rightarrow$ Unit Test $\rightarrow$ SonarQube Scan $\rightarrow$ Artifact/Image Creation.
**CD Deployment Flow:** CI Success $\rightarrow$ Deploy to **DEV** $\rightarrow$ Integration Test $\rightarrow$ Deploy to **STAGING** $\rightarrow$ QA/Manual Tests $\rightarrow$ **Manual Approval Gate** $\rightarrow$ Deploy to **PROD**.

### 50\. What stages do you define in your Jenkins pipeline, and how do you ensure full quality checks during deployment?

**Standard Stages:** `Checkout`, `Build`, `Test`, `Scan`, `Package`, `Deploy-Dev`, `Deploy-Staging`, `Deploy-Prod`.

**Quality Checks:**

1.  **Test Stage:** Ensures **100% unit test success** and code coverage gates.
2.  **Scan Stage:** Enforces a **security/quality gate** (e.g., no critical SonarQube issues).
3.  **Package Stage:** Runs **vulnerability scanning** (Trivy) on the Docker image.
4.  **Post-Deployment:** Runs **smoke tests** and **integration tests** against the service immediately after it's deployed to Dev/Staging.

### 51\. How do you use Jenkins shared libraries? Explain their typical structure and how they are integrated into your Jenkinsfiles.

I use shared libraries to implement **complex, repetitive logic** like Kubernetes deployment, SonarQube scanning, and Slack notifications.

  * **Structure:**
      * **`vars/`:** Contains Groovy files defining **Global Variables** (e.g., `deployK8s.groovy`). These are invoked directly as functions: `deployK8s('dev')`.
      * **`src/`:** Contains formal **Groovy classes** (e.g., `com/org/Utils.groovy`) for complex, object-oriented logic.
  * **Integration:** Defined globally in Jenkins, and imported into the `Jenkinsfile` using the **`@Library`** annotation or implicitly available via the **`@Library('lib-name') _`** declaration at the top.

### 52\. What have you done with Jenkins?

I have **developed, maintained, and optimized CI/CD pipelines** for microservices, managed **Jenkins security and agent capacity**, integrated **SonarQube** and **Nexus/Artifactory**, and implemented **Shared Libraries** for company-wide build standardization.

### 53\. What was the CI/CD pipeline used for — infra or app deployment?

The pipeline was primarily used for **Application Deployment** (building and deploying microservices). We used a separate tool (**Terraform**) managed by another team for **Infrastructure Deployment (IaC)**, though our app pipeline would sometimes invoke Terraform hooks.

### 54\. Can you pass runtime parameters in a Jenkins pipeline?

Yes. This is done by marking the job as **"This project is parameterized"** (for Freestyle jobs) or by using the **`parameters { ... }`** block within a **Declarative Pipeline**. Parameters are accessed within the script using the global **`params`** variable (e.g., `params.ENV`).

### 55\. Jobs often land in build queue — how can we get notified?

We use the **Queue Notifier Plugin** or implement a custom notification script within the pipeline's **`options`** or **`post`** section to check for long queue times and send alerts via **Slack** or **Email**.

### 56\. Can you explain the Jenkins architecture (nodes and controller)?

Jenkins operates on a **Controller-Agent** architecture. The **Controller** schedules tasks, manages configuration, and monitors agents. **Agents** are external worker machines connected to the controller that execute the actual build steps, distributing the workload and providing different execution environments.

### 57\. Indexes within an application — what is an index?

In the context of databases and applications, an **index** is a data structure (like a B-tree) that **improves the speed of data retrieval** operations on a database table. It acts like an index in a book, allowing the system to locate rows quickly without scanning the entire table.

### 58\. How do you monitor the pipelines?

1.  **Jenkins Blue Ocean/Stage View:** For visual tracking of pipeline flow and stage duration.
2.  **Prometheus/Grafana:** We use the **Prometheus Plugin** to expose build metrics (duration, success/failure rate) and visualize them in a Grafana dashboard.
3.  **Notifications:** Using **Slack/Email** for immediate build failure alerts.

### 59\. What is the difference between scripted and declarative pipelines in Jenkins?

| Feature | Declarative Pipeline | Scripted Pipeline |
| :--- | :--- | :--- |
| **Structure** | Highly **structured** and **rigid** (`pipeline { ... }`). | **Flexible** Groovy code within `node { ... }`. |
| **Readability** | High, easier for non-developers. | Lower, can be complex Groovy code. |
| **Logic** | Limited Groovy logic within `script { }` blocks. | Uses full **Groovy language** for complex flow control. |
| **Use Case** | Simple to medium complexity, standardized CI/CD. | Highly complex tasks, conditional logic, and Shared Libraries. |

### 60\. Have you created a Jenkins pipeline for your project?

*(Answer based on your experience)* Yes, I have created numerous pipelines. I usually start with a declarative structure and then extract complex, reusable parts into a **Shared Library**.

### 61\. What is Groovy, and how is it used in Jenkins?

**Groovy** is a dynamic language for the Java Virtual Machine (JVM). In Jenkins, it is the language used to write **Pipeline scripts** (`Jenkinsfile`) and **Shared Libraries**.

### 62\. Why do you use Groovy in Jenkins, and where do you save Jenkins files?

  * **Why Groovy:** Groovy runs on the JVM, making it **natively compatible** with the Java-based Jenkins. It also has features specifically implemented by Jenkins (like `sh`, `docker`, `git` steps) for building pipelines.
  * **Where to Save:** The primary `Jenkinsfile` is saved in the **root directory of the SCM repository** (e.g., Git). Shared Library scripts are saved in a separate Git repository configured globally in Jenkins.
