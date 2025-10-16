ANSIBLE QUESTIONS AND ANSWERS
---

## **1. What is Ansible, and what is its purpose?**
Ansible is an open-source automation tool used for configuration management, application deployment, and IT orchestration. It uses a simple, human-readable YAML language for defining tasks and doesn’t require any agent installation on managed nodes. Ansible’s lightweight design, ease of use, and ability to handle complex tasks make it ideal for DevOps automation, enabling efficient management of infrastructure at scale
---

## **2. Main components of Ansible**

Ansible’s architecture includes several core components:

Control Node: The machine where Ansible is installed and from which commands are executed.
Managed Nodes: The servers managed by Ansible.
Inventory: A file listing all the hosts that Ansible manages, categorized by groups.
Modules: Scripts run by Ansible to perform tasks on managed nodes.
Playbooks: YAML files where automation tasks are defined in a structured format.
Plugins: Extend Ansible’s functionality for connection management, caching, and logging.
These components work together to automate tasks across multiple nodes.


---
## ** extra question How does Ansible differ from other automation tools like Chef and Puppet?

Ansible is one of several popular automation tools, and interviewers may ask about its unique features.

Answer: “Ansible differs from tools like Chef and Puppet in several ways:

Agentless: Ansible doesn’t require an agent on managed nodes, making it lightweight and easy to manage.
Push-based model: Ansible uses a push model, while Chef and Puppet are typically pull-based.
Configuration language: Ansible uses YAML, which is human-readable, while Chef uses Ruby, and Puppet uses its own DSL.
Learning curve: Ansible’s simplicity and YAML syntax make it beginner-friendly, while Chef and Puppet may require more setup and specialized knowledge.”

## ** extra question What is the purpose of Ansible Galaxy?

This question checks your knowledge of reusable content in Ansible.

Answer: “Ansible Galaxy is a public repository of reusable Ansible content, such as roles and collections, which can be used to streamline and accelerate development. It allows users to download, share, and publish roles created by others, making it easy to leverage community-contributed modules. Ansible Galaxy simplifies complex configurations, allowing users to integrate proven roles for tasks like database setup, web server configuration, and more.


## ** extra question What are some best practices for using Ansible in a production environment?

This question assesses your understanding of Ansible’s optimization and security in real-world applications.

Answer: “Some best practices for using Ansible in production include:

Use version control: Store playbooks and configurations in Git to track changes and collaborate.
Structure inventory files: Use dynamic inventory for cloud-based environments and organize groups logically.
Secure sensitive information: Use Ansible Vault to encrypt sensitive data, like passwords and API keys.
Test in staging: Always test playbooks in a staging environment before deploying them to production.
Leverage roles and Galaxy: Use roles and Galaxy to structure playbooks, making them modular and reusable
## **3. What language do you use in Ansible?**

Ansible uses **YAML (Yet Another Markup Language)** for playbooks — human-readable, simple, and indentation-based.

---

## **4. Requirements to install Ansible**

* **Control Node:** Linux/macOS, Python 3.x
* **Managed Nodes:** SSH access, Python 2.7+ or 3.x
  No agent is needed on managed nodes.

---

## **5. Can Ansible be installed on Windows?**

* Direct installation on Windows is not possible.
* Use **WSL (Windows Subsystem for Linux)** or a Linux VM to control hosts.

---

## **6. What are Ansible Facts?**

Facts are system properties automatically gathered from managed nodes.

* Include hostname, OS, IP, memory, CPU, etc.
* Gathered using the `setup` module:

```bash
ansible all -m setup
```

---

## **7. Fetching server details using Ansible**

Use `setup` module to fetch facts:

```bash
ansible all -m setup
ansible all -m setup -a 'filter=ansible_hostname'
```

---

## **8. What is idempotency in Ansible? How do you handle it?**

Idempotency ensures running a playbook multiple times **won’t change the system** if it’s already in the desired state.

* Built-in modules are usually idempotent.
* Custom scripts should include checks before making changes.

---

## **9. What is "handlers" in Ansible?**

Handlers are tasks in Ansible that are triggered only when notified by another task. They are typically used for actions that should occur conditionally, like restarting a service after configuration changes. For example, if a configuration file is updated, a handler can be notified to restart the associated service, ensuring that the new settings are applied.”


---

## **10. How to use handlers in Ansible and how are they triggered?**

```yaml
tasks:
  - name: Update Nginx config
    copy:
      src: nginx.conf
      dest: /etc/nginx/nginx.conf
    notify: Restart Nginx

handlers:
  - name: Restart Nginx
    service:
      name: nginx
      state: restarted
```

Handlers run only if the notifying task reports a change.

---

## **11. What are Variables in Ansible, and how are they used?**

In Ansible, Variables allow for customization and dynamic task execution. They can be defined at multiple levels, including playbooks, inventory files, and external files. Variables help make playbooks more flexible, allowing for configuration changes without modifying the playbook itself. For example, a variable for app_version can be used to dynamically set the application version in different environments.

vars:
  app_port: 8080
tasks:
  - debug: msg="The application runs on port {{ app_port }}"
```

---

## **12. How do you use variables and templates in Ansible?**

Templates use **Jinja2 syntax** to insert variables into files.

```yaml
- template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

---

## **13. What is Ansible Pull?**

Allows nodes to **pull configuration from Git** instead of being pushed from the control node.

```bash
ansible-pull -U https://github.com/repo.git playbook.yml
```

---

## **14. Command to run a playbook file**

```bash
ansible-playbook playbook.yml
ansible-playbook -i inventory.ini playbook.yml
```

---

## **15. Command to check the syntax of a playbook file**

```bash
ansible-playbook --syntax-check playbook.yml
```

---

## **16. What is an ad-hoc command?**

One-liner commands for quick tasks without a playbook.

```bash
ansible all -m ping
ansible webservers -m shell -a "uptime"
```
What are Ansible Playbooks, and how do they differ from ad-hoc commands?

Interviewers ask this question to understand your familiarity with structured task automation in Ansible.

Answer: “Ansible Playbooks are YAML files used to define a sequence of tasks that Ansible should execute on managed nodes. They allow for complex, reusable configurations and support variables, conditionals, and loops. Ad-hoc commands, on the other hand, are one-time commands executed from the command line without a playbook. While ad-hoc commands are useful for quick tasks, playbooks are essential for 

---

## **17. Have you worked on Ansible ad-hoc commands?**

Yes — used for:

* Restarting services: `ansible all -m service -a "name=nginx state=restarted"`
* Checking uptime or disk usage: `ansible all -a "df -h"`

---

## **18. What is "hosts" used for and what is "tasks"; can we write something before "hosts"?**

* **hosts:** Target server/group
* **tasks:** List of actions
* **Pre-tasks** or **vars** can appear before `hosts`.

---

## **19. What is loop feature in Ansible?**

Loops repeat tasks for multiple items.

```yaml
- name: Create multiple users
  user:
    name: "{{ item }}"
    state: present
  loop:
    - dev
    - test
    - prod
```

---

## **20. How do you use conditionals (`when`) in Ansible?**

```yaml
- name: Install Apache only on CentOS
  yum:
    name: httpd
    state: present
  when: ansible_facts['os_family'] == "RedHat"
```

---

## **21. Skipping CentOS tasks**

```yaml
- debug: msg="Not CentOS"
  when: ansible_facts['os_family'] != "RedHat"
```

---

## **22. How to run only a specific task in a long playbook?**

Use **tags**:

```yaml
- name: Install Apache
  yum: name=httpd state=present
  tags: install
```

Run it with: `ansible-playbook site.yml --tags install`

---

## **23. What are Ansible Modules, and can you name a few commonly used ones?

This question assesses your knowledge of Ansible modules, which are essential for automation.

Answer: “Ansible Modules are reusable scripts that perform specific tasks on managed nodes, like installing software, managing files, or restarting services. Modules are the building blocks of Ansible tasks. Some commonly used modules include:

apt: Manages packages on Debian-based systems.
yum: Manages packages on RedHat-based systems.
copy: Copies files to managed nodes.
service: Manages services (start, stop, restart).
user: Manages user accounts.

Ansible has over 1,000 modules, making it versatile for a wide range of automation tasks.”
---

## **24. Have you worked on custom modules? Explain**

Yes — custom modules are written in Python under `library/`.

* Use `AnsibleModule` for argument parsing.
* Example: Validate application configs or fetch API data.

---

## **25. What are Ansible Plugins?**

Plugins extend functionality:

* Action, Callback, Lookup, Connection.
* Customize execution, output, or data retrieval.

---

## **26. What is an Inventory file in Ansible, and how is it structured?

The inventory file is essential in Ansible, and interviewers want to see if you understand its purpose and structure.

Answer: “An Inventory file in Ansible lists the hosts (managed nodes) and organizes them into groups. The inventory file can be static (a simple text file) or dynamic (integrating with cloud providers). A typical inventory file structure looks like this:

ini

[webservers]
server1.example.com
server2.example.com

[databases]
db1.example.com
db2.example.com
This structure allows users to define hosts and groups, enabling targeted task execution on specific machines.

---

## **27. What are Ansible Roles?**

Roles are **reusable, structured playbooks** separating tasks, handlers, variables, templates, and files.

---

## **28. Directory structure of Ansible roles**

```
roles/
  webserver/
    tasks/
    handlers/
    templates/
    files/
    vars/
    defaults/
    meta/
```

---

## **29. Ansible Galaxy**

A repository for reusable roles and collections:

```bash
ansible-galaxy install geerlingguy.nginx
```

---

## **30. What are Ansible Collections?**

Bundles of modules, roles, and plugins — installed via:

```bash
ansible-galaxy collection install amazon.aws
```

---

## **31. How do you use Ansible Collections in a playbook?**

```yaml
- hosts: all
  tasks:
    - amazon.aws.ec2:
        key_name: mykey
        instance_type: t2.micro
```

---

## **32. How do you use tags in Ansible?**

```yaml
tasks:
  - name: Install Apache
    yum: name=httpd state=present
    tags: install
```

Run with: `--tags install`

---

## **33. How to encrypt in Ansible**

Use **Ansible Vault**:

```bash
ansible-vault encrypt secrets.yml
ansible-vault decrypt secrets.yml
ansible-vault edit secrets.yml
```

---

## **34. What is Ansible Vault?**

Encrypts sensitive data (passwords, keys) in playbooks or vars for secure automation.

---

## **35. How do you use Ansible Vault to manage secrets?**

```yaml
vars_files:
  - vault.yml
```

Run playbook:

```bash
ansible-playbook playbook.yml --ask-vault-pass
```

---

## **36. How do you manage configuration drift with Ansible?**

* Re-run playbooks periodically
* Idempotency ensures systems stay in the desired state
* Corrects drift automatically.

---

## **37. Error handling in Ansible**

* `ignore_errors`
* `failed_when`
* `block/rescue/always`
  **Example:**

```yaml
- block:
    - service: name=nginx state=restarted
  rescue:
    - debug: msg="Nginx restart failed!"
```

---

## **38. Task reporting “changed” every time — troubleshooting**

* Check module idempotency
* Use `changed_when: false` if needed
* Use `-vvv` for verbose debug

---

## **39. What is Ansible Lint, and why is it useful?**

Checks playbooks for best practices, syntax errors, and maintainability:

```bash
ansible-lint playbook.yml
```

---

## **40. Example playbook**

```yaml
- name: Install Apache
  hosts: webservers
  become: yes
  tasks:
    - yum:
```


name=httpd state=present
- service: name=httpd state=started

````

---

## **41. Sample playbook to install Nginx**
```yaml
- hosts: webservers
  become: yes
  tasks:
    - apt: name=nginx state=present
      when: ansible_os_family == "Debian"
    - yum: name=nginx state=present
      when: ansible_os_family == "RedHat"
    - service: name=nginx state=started enabled=yes
````

---

## **42. Example playbook to check service status**

```yaml
- hosts: webservers
  become: yes
  tasks:
    - service_facts:
    - debug: msg="{{ ansible_facts.services['nginx.service'].state }}"
```

---

## **43. Restart Nginx if config changes**

```yaml
tasks:
  - template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf
    notify: restart nginx

handlers:
  - service: name=nginx state=restarted
```

---

## **44. Using Ansible for application deployment**

* Clone repo
* Install dependencies
* Configure environments
* Restart services

---

## **45. Using Ansible for OS hardening**

* Set permissions
* Disable root SSH
* Apply password policies
* Install patches

---

## **46. Using Ansible with Docker**

```yaml
- community.docker.docker_image: name=nginx source=pull
- community.docker.docker_container: name=web image=nginx ports="80:80"
```

---

## **47. Setting up Jump Host**

```ini
server1 ansible_host=10.0.0.10 ansible_ssh_common_args='-o ProxyJump=jumpuser@bastion.example.com'
```

---

## **48. Optimizing playbook execution**

* Increase `forks`
* Use `gather_facts: no`
* Async tasks
* Use tags and caching

---

## **49. Ansible Execution Strategies**

* **Linear:** Default, sequential tasks
* **Free:** Tasks run independently across hosts

```yaml
strategy: free
```

---

## **50. Managing dynamic inventory in AWS**

Use **AWS EC2 inventory plugin**:

```yaml
plugin: amazon.aws.ec2
regions: [ap-south-1]
filters: { instance-state-name: running }
```

```bash
ansible-inventory -i aws_ec2.yml --list
```

---

## **51. Run playbook with a specific inventory**

```bash
ansible-playbook -i inventory/dev_hosts.ini site.yml
```

---

## **52. Integrate Ansible with Terraform**

* Terraform provisions infrastructure
* Ansible configures it using Terraform outputs:

```bash
ansible-playbook -i $(terraform output -raw webserver_ip), setup.yml
```

---

## **53. Use Ansible with CI/CD pipelines**

* Build → Deploy → Verify
* Example Jenkins step:

```groovy
sh 'ansible-playbook -i inventory/prod site.yml'
```

---

## **54. What is Ansible Tower?**

* Web UI for automation
* RBAC, scheduling, logging
* Centralized management of inventories, credentials, and playbooks

---

## **55. Difference between Ansible and Terraform**

| Feature  | Ansible           | Terraform                   |
| -------- | ----------------- | --------------------------- |
| Purpose  | Config management | Infrastructure provisioning |
| Language | YAML              | HCL                         |
| Model    | Push              | Declarative                 |
| State    | No                | Maintains state file        |

---

## **56. Difference from Chef and Puppet**

* Agentless, push model, simple YAML
* Chef: Ruby, pull model
* Puppet: DSL, pull model

---

## **57. Why Ansible is preferred**

* Agentless, simple, idempotent
* Strong community
* Cloud & CI/CD integration

---

## **58. Best practices in production**

* Git version control
* Ansible Vault for secrets
* Modular roles
* Test in staging
* Dynamic inventories
* Logging & linting

---

## **59. Why are you using Ansible?**

“I automate deployments, OS configurations, Docker, microservices, and monitoring setups. It reduces manual effort, maintains consistency, and improves reliability.”

---

## **60. How do you test Ansible playbooks?**

* `--syntax-check`
* `--check` mode
* Ansible Lint
* Molecule testing for roles
* Staging verification

```bash
ansible-playbook site.yml --check --diff
```

Absolutely! Here's how we can **add the scenario-based questions in Mihir-style** to your existing document. I’ve kept it **concise, structured, and to-the-point**, with YAML snippets where needed:

---

# **Scenario-Based Ansible Questions (Professional Level)**

## **61. Multi-Tier Application Deployment**

**Scenario:** Deploy web, app, and database servers.
**Answer:**

* Use **roles** for each tier: `web`, `app`, `db`.
* Directory structure:

```
roles/
  web/tasks/main.yml
  app/tasks/main.yml
  db/tasks/main.yml
inventory/production/hosts
group_vars/
  all.yml
  web.yml
  app.yml
  db.yml
```

* `site.yml` includes roles:

```yaml
- hosts: web
  roles: [web]

- hosts: app
  roles: [app]

- hosts: db
  roles: [db]
```

---

## **62. Rolling Updates with Zero Downtime**

**Scenario:** Update application without downtime.
**Answer:**

* Use **load balancer** to drain traffic.
* Update servers in **batches (`serial`)**.
* Health check after update.
* Add back to load balancer.

```yaml
- hosts: web_servers
  serial: 2
  tasks:
    - name: Drain traffic
      uri: url="http://lb/api/drain/{{ inventory_hostname }}" method=POST

    - name: Update web role
      include_role: name=web

    - name: Health check
      uri: url="http://{{ inventory_hostname }}/health" status_code=200 retries=5 delay=10

    - name: Restore traffic
      uri: url="http://lb/api/add/{{ inventory_hostname }}" method=POST
```

---

## **63. Handling Secret Management**

**Scenario:** Manage passwords/API keys securely.
**Answer:**

* Use **Ansible Vault**.
* Encrypt secrets: `ansible-vault encrypt secrets.yml`
* Use in playbooks:

```yaml
- hosts: all
  vars_files: [secrets.yml]
  tasks:
    - name: Use API key
      uri:
        url: "https://api.example.com/data"
        headers: {Authorization: "Bearer {{ api_key }}"}
```

* Run playbook: `ansible-playbook playbook.yml --ask-vault-pass`

---

## **64. Idempotency Issues**

**Scenario:** Task is not idempotent.
**Answer:**

* Identify task via `-v` flag.
* Check current state before changes.
* Example fix:

```yaml
# Non-idempotent
- lineinfile: path=/etc/example.conf line="new configuration"

# Idempotent
- lineinfile: path=/etc/example.conf line="new configuration" state=present
```

---

## **65. Dynamic Inventory**

**Scenario:** Servers frequently added/removed.
**Answer:**

* Use **inventory plugins** (AWS, Azure, GCP).
* Example AWS EC2:

```yaml
plugin: aws_ec2
regions: [us-east-1]
filters: {tag:Environment: production}
```

* List hosts: `ansible-inventory -i aws_ec2.yml --list`

---

## **66. Optimizing Playbook Performance**

**Scenario:** Playbook execution is slow.
**Answer:**

* Increase **forks**: `ansible-playbook -f 10`
* Use **async** for long tasks:

```yaml
- name: Long task
  command: /path/to/script
  async: 3600
  poll: 0
```

* Limit hosts: `--limit web_servers`
* Delegate to localhost if needed.

---

## **67. Handling Legacy Scripts**

**Scenario:** Migrate shell scripts to Ansible.
**Answer:**

* Break scripts into discrete tasks.
* Use **Ansible modules** instead of shell commands.
* Example:

```bash
# Shell
apt-get update
apt-get install -y nginx
```

```yaml
# Ansible
- apt: update_cache=yes
- apt: name=nginx state=present
```

---

## **68. Managing Multiple Environments**

**Scenario:** Handle dev, staging, prod environments.
**Answer:**

* Use separate **inventory files** and **group_vars**.

```
inventories/
  development/hosts
  staging/hosts
  production/hosts
group_vars/all.yml
```

* Run playbook with specific environment:

```bash
ansible-playbook -i inventories/staging/hosts site.yml
```
---

## **69. Delegation & `run_once`**

**Scenario:** Run a task only on a specific host or once across all hosts.

**Answer:**
Use `delegate_to` to target a host and `run_once: true` to execute a task just once.

```yaml
- name: Gather OS info from DB host only
  shell: cat /etc/os-release
  delegate_to: "{{ groups['db'][0] }}"
  run_once: true
```

✅ **Explanation:**

* `delegate_to` sends the task to the host you specify.
* `run_once` ensures it executes only once, not on all hosts.

---

## **70. Fact Caching & Custom Facts**

**Scenario:** Reduce fact gathering time and create custom facts.

**Answer:**

1. **Enable fact caching** in `ansible.cfg`:

```ini
[defaults]
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_cache
fact_caching_timeout = 86400
```

2. **Create custom facts** on a managed host:

```bash
# /etc/ansible/facts.d/myfact.fact
{
  "department": "devops",
  "owner": "mihir"
}
```

```yaml
- name: Print custom fact
  debug:
    msg: "{{ ansible_local.myfact.department }}"
```

✅ **Explanation:**

* Fact caching avoids repeated SSH/fact gathering for speed.
* Custom facts let you store host-specific metadata.

---

## **71. Advanced Loops**

**Scenario:** Loop through complex data structures with multiple attributes.

**Answer:**

```yaml
- name: Create users with shells
  user:
    name: "{{ item.name }}"
    shell: "{{ item.shell }}"
    state: present
  loop:
    - { name: 'dev', shell: '/bin/bash' }
    - { name: 'test', shell: '/bin/zsh' }
```

✅ **Explanation:**

* Loops can handle dictionaries and lists.
* Use `loop_control` if you want to customize index or labels.

```yaml
loop_control:
  label: "{{ item.name }}"
```

---

## **72. Conditional Includes (`import_tasks` vs `include_tasks`)**

**Scenario:** Include tasks dynamically based on conditions.

**Answer:**

```yaml
- import_tasks: common.yml   # Static, loaded at playbook start
- include_tasks: optional.yml
  when: ansible_os_family == "RedHat"  # Dynamic
```

✅ **Explanation:**

* `import_tasks` → Static, parsed once.
* `include_tasks` → Dynamic, evaluated during runtime, supports `when`.

---

## **73. Advanced Error Handling**

**Scenario:** Handle critical and non-critical task failures gracefully.

**Answer:**

```yaml
- block:
    - name: Restart web service
      service:
        name: nginx
        state: restarted
  rescue:
    - debug: msg="Restart failed, alert admin!"
  always:
    - debug: msg="This runs regardless of success/failure."
```

✅ **Explanation:**

* `block` → main tasks
* `rescue` → run if block fails
* `always` → run regardless
* Combine with `ignore_errors` or `failed_when` for fine-grained control.

---

## **74. Ansible Tower / AWX Advanced Usage**

**Scenario:** Manage credentials, schedules, and inventory in enterprise environments.

**Answer:**

* Use **Job Templates** to run playbooks.
* Assign **inventories** and **credentials** per template.
* Apply **RBAC** for user access.
* Schedule recurring jobs for maintenance or deployments.

✅ **Tip:** Tower integrates with Git, LDAP, and logging systems — perfect for enterprise automation.

---

## **75. Molecule Testing / CI/CD Integration**

**Scenario:** Test roles locally and integrate with CI/CD pipelines.

**Answer:**

1. **Molecule test** for roles:

```bash
molecule init role web --driver-name docker
molecule test
```

2. **CI/CD integration** (example with Jenkins):

```groovy
pipeline {
  stages {
    stage('Deploy') {
      steps {
        sh 'ansible-playbook -i inventory/prod site.yml'
      }
    }
  }
}
```

✅ **Explanation:**

* Molecule verifies your role logic locally.
* CI/CD integration ensures automated deployments and reduces human error.

---
