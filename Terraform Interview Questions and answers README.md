## ⚙️ Terraform Essentials

### 1\. Basic Terraform commands

The most fundamental commands, forming the core workflow, are:

1.  **`terraform init`**: Initializes the working directory, downloads provider plugins, and sets up the backend.
2.  **`terraform plan`**: Creates an execution plan, showing what actions (create, update, destroy) Terraform will take.
3.  **`terraform apply`**: Executes the plan, provisioning or updating the infrastructure.
4.  **`terraform destroy`**: Destroys all resources managed by the current configuration.

### 2\. How to auto-approve without typing yes or no

Use the **`-auto-approve`** flag with the `apply` and `destroy` commands:

  * `terraform apply -auto-approve`
  * `terraform destroy -auto-approve`

### 3\. Workspaces and Terraform modules

  * **Workspaces:** Used to manage **multiple, distinct working environments** (e.g., `dev`, `stage`, `prod`) using the **same configuration**. Each workspace maintains its own isolated state file.
  * **Terraform Modules:** Self-contained, reusable blocks of Terraform configuration (e.g., a module to create a VPC). They promote **reusability, organization, and maintainability**.

### 4\. What is `tfstate` file and best practices to secure it

The **`terraform.tfstate` file** is Terraform's **single source of truth**. It maps the resources defined in your configuration files to the actual resources deployed in the cloud, tracking metadata and resource dependencies.

**Best Practices for Security:**

1.  **Remote Backend:** **NEVER** use local state in a team environment. Use a **remote backend** like **Amazon S3** (with DynamoDB for locking) or **Terraform Cloud**.
2.  **Encryption:** Ensure the remote backend storage (e.g., S3 bucket) is configured with **server-side encryption (SSE)**.
3.  **Access Control:** Use **IAM policies** to strictly limit who can read or modify the state file.
4.  **State Locking:** Use a mechanism (like **DynamoDB** with S3) to prevent concurrent state modifications, which can lead to data corruption.

### 5\. Three or five important Terraform commands

1.  **`terraform init`** (Setup)
2.  **`terraform plan`** (Review changes)
3.  **`terraform apply`** (Execution)
4.  **`terraform import`** (Bringing existing resources under management)
5.  **`terraform fmt`** (Standardizing code format)

### 6\. How to destroy a specific resource

You can use the **`-target`** flag with the `destroy` command, but this is **highly discouraged** as it bypasses dependency checking.

  * `terraform destroy -target=aws_instance.example`
    The safer, preferred method is to **remove the resource block** from the configuration file and run a regular **`terraform apply`**, which will detect the resource is no longer desired and plan its destruction.

### 7\. Terraform file extension

The primary configuration file extension is **`.tf`** (e.g., `main.tf`, `variables.tf`). Variable definitions can also use **`.tfvars`**.

### 8\. Who developed Terraform

Terraform was developed by **HashiCorp**.

### 9\. Bringing manually created resources under Terraform

This is done using the **`terraform import`** command. It requires:

1.  A resource block in the configuration file that matches the type of the existing resource.
2.  The command: `terraform import [resource_type.name] [cloud_resource_ID]`
    *Example: `terraform import aws_s3_bucket.mybucket my-existing-bucket-name`*

### 10\. What are provisioners

**Provisioners** are blocks within a resource definition that are used to **execute scripts or configuration on a local machine or a remote machine** as part of the resource creation or destruction process. They are a *last resort* and are generally discouraged in favor of configuration management tools like Ansible.

### 11\. Difference between provisioner and provider

  * **Provider:** A **plugin** that interfaces with an API to **manage the lifecycle** (create, read, update, delete) of cloud resources (e.g., `aws`, `azurerm`, `google`). **Providers** manage the infrastructure.
  * **Provisioner:** A block of code used to **run scripts** *after* a resource is created or *before* it is destroyed (e.g., running a setup script on a newly created EC2 instance). **Provisioners** configure the software *inside* the infrastructure.

### 12\. How to provide secrets in Terraform

Secrets should be handled outside the configuration files to prevent them from being committed to source control:

1.  **Environment Variables:** Use `TF_VAR_variable_name` environment variables.
2.  **Vault:** Use the **Vault Provider** to dynamically fetch secrets at runtime.
3.  **Cloud Secret Managers:** Use provider data sources to fetch secrets from **AWS Secrets Manager** or **Azure Key Vault**.
4.  **`terraform.tfvars` (Discouraged):** If used, the file must be explicitly added to `.gitignore` and secured.

### 13\. Creating multiple similar configurations in Terraform

Use **`for_each`** or **`count`** meta-arguments:

  * **`count` (Simpler):** Creates a fixed number of identical resources.
  * **`for_each` (Preferred):** Creates multiple instances based on a **map or set**, giving each instance a unique key for better state management.

### 14\. Providing a version range in Terraform

You define version constraints in the **`required_providers`** block within `terraform` configuration:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"  # Allows versions 5.0 up to, but not including, 6.0
    }
  }
}
```

### 15\. Creating resources after other resources (using "depends\_on")

**`depends_on`** is used to explicitly create an ordering dependency between resources that is not automatically inferred by Terraform.
*Example: Ensuring an IAM role is created before the EC2 instance attempts to use it.*

```hcl
resource "aws_instance" "server" {
  # ... configuration ...
  depends_on = [
    aws_iam_role.service_role
  ]
}
```

### 16\. Showing outputs during resource creation

Outputs are shown **after** a successful `terraform apply`. They are defined in **`outputs.tf`** files. They are used to display important information (like the final DNS name or IP address) to the user or for use in other configurations.

### 17\. Ways to store variables in Terraform

1.  **`variables.tf` (Definition):** Defines the variable schema (type, description, default).
2.  **`terraform.tfvars` (Default Input):** Automatically loaded file for variable values.
3.  **`*.auto.tfvars` (Input):** Automatically loaded files (e.g., `prod.auto.tfvars`).
4.  **CLI Flag:** **`-var 'key=value'`** during `plan` or `apply`.
5.  **Environment Variables:** `TF_VAR_variable_name`.

### 18\. Explain Terraform `init`, `plan`, and `apply`

  * **`init` (Initialize):** The setup phase. Downloads the necessary provider code/plugins and initializes the backend where the state file will be stored.
  * **`plan` (Preview):** The inspection phase. Reads the current configuration, compares it with the remote state, and creates an execution plan, showing exactly what will be added, changed, or destroyed.
  * **`apply` (Execute):** The execution phase. Executes the plan against the target infrastructure, updating the remote resources and the local/remote state file.

### 19\. Loops in Terraform

Loops are implemented using meta-arguments and built-in functions:

  * **`count`:** Simple iteration over an integer count.
  * **`for_each`:** Iteration over a map or set to create resources with unique keys (preferred).
  * **`for` Expressions:** Used in output, variable, and resource blocks to transform or filter lists/maps.

### 20\. Difference between Ansible and Terraform

See the Ansible section (Q9).

  * **Terraform:** Declarative IaC, focuses on **provisioning** infrastructure (VMs, networks, load balancers).
  * **Ansible:** Procedural Configuration Management, focuses on **configuring** software and operating systems *inside* the infrastructure.

### 21\. What are terraform lifecycle

The Terraform lifecycle refers to the primary phases of resource management: **`init`**, **`plan`**, and **`apply`**. It also includes the specific resource lifecycle options available inside a resource block via the **`lifecycle`** meta-argument:

  * **`prevent_destroy`**: Prevents the resource from being destroyed (critical resources).
  * **`ignore_changes`**: Tells Terraform to ignore changes to specific attributes of the resource.
  * **`create_before_destroy`**: Forces Terraform to create the new replacement resource before destroying the old one (to ensure continuous availability).

### 22\. How can we import terraform infrastructure cmd and full steps

**Import Command and Steps:**

1.  **Define Resource:** Create a matching `resource` block in your `.tf` files (e.g., `resource "aws_instance" "web" {}`).
2.  **Execute Import:** Run the command providing the Terraform resource address and the cloud resource ID.
      * **Command:** `terraform import aws_instance.web i-0abcdef1234567890`
3.  **Review Changes:** Run **`terraform plan`**. The plan should show "No changes. Infrastructure is up-to-date." If it shows changes, manually adjust the `.tf` file to match the imported resource's current state.

### 23\. What are terraform important components

1.  **Configuration Files (`.tf`):** Define the desired infrastructure state.
2.  **Providers:** Interface with cloud/SaaS APIs.
3.  **State File (`tfstate`):** Tracks the current deployed state.
4.  **Modules:** Reusable configuration blueprints.

### 24\. How we can store sensitive data in terraform

(See Q12). **Avoid storing secrets in `.tf` or committed `.tfvars` files.** Use environment variables, a dedicated secrets manager (Vault, AWS Secrets Manager), or the remote state file itself if highly secured.

### 25\. What is the difference between local repository and global repository

This terminology is generally associated with package/artifact management (like Maven or Nexus). In the context of Terraform:

  * **Local Repository:** Refers to a module source path on the local filesystem.
  * **Global/Remote Repository:** Refers to a remote module source, such as a **Terraform Registry** (public or private), a **Git repository (GitHub/GitLab)**, or an **S3 bucket**.

### 26\. If I apply or if I use a Terraform plan, will it make a permanent change or just a temporary change?

  * **`terraform plan`:** Makes **no changes** to the infrastructure; it only shows a preview.
  * **`terraform apply`:** Makes **permanent changes** to the cloud infrastructure and updates the `tfstate` file to reflect these changes.

### 27\. What is Terraform Import?

(See Q9 and Q22). It's the utility command used to **read an existing, manually created infrastructure resource** from the cloud and **record it in the Terraform state file**, thus bringing it under Terraform management.

### 28\. What is the use of Taint in Terraform?

The **`terraform taint`** command is used to **mark a resource as tainted** in the state file. This forces Terraform to consider the resource damaged or corrupted. The next `terraform apply` will automatically plan to **destroy the old resource and recreate a new one** in its place. **Note:** HashiCorp now **recommends using `terraform apply -replace=resource_address`** instead of `taint`.

### 29\. How do we check the benchmarking structure of a Terraform compilation? (Checking syntax and formatting)

  * **Syntax Check:** **`terraform validate`** (checks configuration structure, variable types, and syntax).
  * **Formatting Check:** **`terraform fmt`** (rewrites configuration files to a canonical format and style).

### 30\. Where can I have the state files in Terraform?

1.  **Local:** `terraform.tfstate` (only for single-user development).
2.  **Remote Backends (Best Practice):**
      * **AWS S3** (often with DynamoDB for locking).
      * **Azure Storage Blob** (often with a lease lock).
      * **Terraform Cloud/Enterprise.**

### 31\. How is stateful store in Terraform in S3?

1.  **Backend Configuration:** The S3 bucket and region are defined in the **`backend "s3"`** block during `init`.
2.  **Storage:** The `tfstate` file is encrypted and stored as an **object** in the S3 bucket.
3.  **Locking:** A separate resource (usually an **AWS DynamoDB table**) is used to enforce state locking. Before making any changes, Terraform attempts to write a lock record to the DynamoDB table, preventing simultaneous access by other users.

### 33\. What are Terraform modules and have you written any?

(See Q3). **Modules** are reusable, self-contained Terraform configurations. They allow you to encapsulate complex infrastructure patterns (e.g., an entire VPC with subnets, NAT Gateways, and Security Groups) and use them multiple times across different environments or projects with minimal code.

*Answer based on experience:* Yes, I have written modules for common components like an **EKS Cluster**, a **secure S3 bucket**, and a **custom security group** configuration.

### 34\. How do you handle Terraform state locking?

State locking is crucial for team environments using a remote backend.

  * **Mechanism:** When a user runs `terraform plan` or `apply`, Terraform attempts to acquire a lock on the state file via the remote backend (e.g., using a **DynamoDB table** with S3).
  * **Conflict:** If another user already holds the lock, the current command will wait or fail with an error, preventing conflicting modifications and state file corruption.

### 35\. How do you manage secrets in Terraform?

(See Q12). The general principle is to fetch secrets dynamically at runtime from dedicated secret management tools rather than storing them in code. (e.g., **AWS Secrets Manager Data Source** or **Vault Provider**).

### 36\. How do you manage the same infrastructure for different environments in Terraform?

1.  **Workspaces:** Use **`terraform workspace new [env_name]`** (simple separation).
2.  **Directory Structure (Preferred for complexity):** Create separate directories for each environment (`prod/`, `stage/`), each with its own remote backend and configuration (`main.tf`, `variables.tf`). Variables for each environment are stored in **`prod.auto.tfvars`**, etc.
3.  **Modules:** Use the same **module** (e.g., VPC module) across all environments, passing different **input variables** (e.g., CIDR block, instance type) for customization.

### 37\. How do you define module dependency in Terraform?

Dependency between modules is implicitly created when the **output** of one module is used as an **input variable** for another module.

*Example:*

```hcl
module "vpc" {
  # ... creates a VPC ...
}

module "subnets" {
  vpc_id = module.vpc.vpc_id # Implicit dependency
}
```

### 38\. Can you import manually created resources into Terraform?

(See Q9 and Q22). **Yes**, using the **`terraform import`** command.

### 39\. A Terraform `apply` command failed halfway through. What do you do to recover and ensure the state is consistent with the infrastructure?

1.  **Check State Lock:** Ensure the state lock has been released. If not, investigate the failure and manually **`terraform force-unlock [lock_id]`** if safe.
2.  **Investigate Error:** Fix the configuration error or cloud API issue that caused the failure.
3.  **Re-run `apply`:** Run **`terraform apply`** again. Terraform is idempotent; it will reconcile the infrastructure by identifying which resources were successfully created (and are in the state file) and which failed (and need to be created or updated).

### 40\. Can you explain the Terraform plan and its purpose?

(See Q18). The `terraform plan` command is a **read-only operation** that **determines the actions required** to achieve the desired state defined in the configuration files.

  * **Purpose:** To provide a **safe preview** of changes before they are applied, preventing accidental destruction or creation of resources. It acts as a mandatory review step in most CI/CD pipelines.

### 41\. Why did you choose Terraform over Boto3 for infrastructure provisioning?

  * **Declarative (Terraform):** Defines *what* the infrastructure should look like. Terraform handles the *how* (ordering, dependency, reconciliation).
  * **Imperative (Boto3):** Defines *how* to achieve the state step-by-step (e.g., `create_vpc`, then `create_subnet`). Requires complex code for error handling, state management, and updates.
  * **Cloud Agnostic (Terraform):** Can manage AWS, Azure, GCP, and other services with a single tool/language. Boto3 is AWS-specific.

### 42\. Write a terraform script for VPC architecture for production.

A basic, high-level example using a module structure would look like this:

**`main.tf` (calling the module):**

```hcl
module "vpc" {
  source  = "hashicorp/vpc/aws"
  version = "3.16.0"

  name = "production-vpc"
  cidr_block = "10.0.0.0/16"

  azs             = ["us-east-1a", "us-east-1b", "us-east-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  # Standard for Production: Highly Available NAT Gateways
  enable_nat_gateway = true
  single_nat_gateway = false 

  tags = {
    Environment = "Production"
    ManagedBy   = "Terraform"
  }
}
```

*Note: In a real-world scenario, you would use a custom, internal VPC module tailored to your company's standards instead of a public one.*

-----
