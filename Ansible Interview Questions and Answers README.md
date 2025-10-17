---

# ANSIBLE — 75 Questions & Answers (complete & interview-ready)

---

### 1. What is Ansible, and what is its purpose?

**Answer:** Ansible is an open-source automation tool for configuration management, application deployment, and orchestration. It’s agentless, uses SSH (or WinRM for Windows), is written in Python, and uses human-readable YAML playbooks to ensure consistency and repeatability across environments.

---

### 2. What are the main components of Ansible?

**Answer:** Inventory (hosts/groups), Playbooks (YAML task files), Modules (units of work like `yum`, `copy`), Plugins (connection, callback, lookup, etc.), Facts (system info collected from hosts), and Roles (structured reusable playbooks).

---

### 3. What language do you use in Ansible?

**Answer:** Playbooks are written in **YAML**. Templates use **Jinja2** syntax. Ansible itself is implemented in **Python**.

---

### 4. What are the requirements to install Ansible?

**Answer:** Control node: Linux/macOS with Python 3.x. Managed nodes: SSH access (or WinRM for Windows) and Python 2.7+/3.x installed (many modern systems have Python 3). No agent required on managed nodes.

---

### 5. Can Ansible be installed on Windows?

**Answer:** There’s no native Windows control-node install. Use **WSL** (Windows Subsystem for Linux), Cygwin, or a Linux VM as the control node. Managed Windows hosts are supported via **WinRM**.

---

### 6. What are Ansible Facts?

**Answer:** Facts are automatically gathered host variables (OS, IP, memory, CPU, distribution, etc.). Collected with the `setup` module. Accessible as `ansible_facts` or shorthand `ansible_<fact>`.

---

### 7. How do you fetch server details using Ansible?

**Answer:** Use the `setup` module:

```bash
ansible all -m setup
ansible all -m setup -a 'filter=ansible_hostname'
```

You can also use `gather_facts: yes/no` in playbooks.

---

### 8. What is idempotency in Ansible? How do you handle it?

**Answer:** Idempotency means running tasks/playbooks multiple times leaves the system in the same desired state (no unintended changes). Use Ansible’s idempotent modules (e.g., `apt`, `yum`, `lineinfile`), add state checks, and avoid raw shell commands unless necessary with proper guards.

---

### 9. What are handlers in Ansible?

**Answer:** Handlers are special tasks that run only when notified (i.e., when another task reports `changed`). Commonly used for service restarts after config changes. Defined under `handlers:` and referenced via `notify:`.

---

### 10. How do you use handlers in Ansible and how are they triggered?

**Answer:** Example:

```yaml
tasks:
  - name: Update nginx config
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

Handler runs at the end of the play for hosts where the notifying task returned `changed: true`.

---

### 11. What are variables in Ansible, and how are they used?

**Answer:** Variables store dynamic values used in playbooks, templates, inventories, `group_vars` and `host_vars`. Example:

```yaml
vars:
  app_port: 8080

tasks:
  - debug: msg="App runs on {{ app_port }}"
```

There is a variable precedence order (defaults < vars_files < inventory < play vars < extra vars).

---

### 12. How do you use variables and templates in Ansible?

**Answer:** Use Jinja2 templates (`.j2`) and `template:` module. Example:

```yaml
- template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

Template contains `{{ variable }}` placeholders.

---

### 13. What is Ansible Pull?

**Answer:** `ansible-pull` makes a node pull playbooks from a VCS (e.g., Git) and apply them locally — useful for a pull-based approach. Example:

```bash
ansible-pull -U https://github.com/repo.git playbook.yml
```

---

### 14. What is the command to run a playbook?

**Answer:**

```bash
ansible-playbook playbook.yml
ansible-playbook -i inventory.ini playbook.yml
```

---

### 15. How do you check the syntax of a playbook file?

**Answer:**

```bash
ansible-playbook --syntax-check playbook.yml
```

---

### 16. What is an ad-hoc command?

**Answer:** One-line commands to run a module/task without a playbook. Example:

```bash
ansible all -m ping
ansible webservers -m shell -a "uptime"
```

---

### 17. Have you worked on Ansible ad-hoc commands? Give examples.

**Answer:** Yes — quick admin tasks such as restarting services (`ansible all -m service -a "name=nginx state=restarted"`), checking disk usage (`ansible all -a "df -h"`), or gathering package info.

---

### 18. What is `hosts` used for and what are `tasks`; can we write something before `hosts`?

**Answer:** `hosts:` defines the target group or hosts for a play. `tasks:` is the list of actions to run on those hosts. You can have `vars:`, `pre_tasks:`, `post_tasks:`, `roles:` before `tasks` within a play. `hosts` must be present in each play.

---

### 19. What is the loop feature in Ansible?

**Answer:** `loop` repeats a task for multiple items. Example:

```yaml
- name: Create users
  user:
    name: "{{ item }}"
    state: present
  loop:
    - dev
    - test
    - prod
```

There are also `with_items`, `loop_control`, and looping over dictionaries/lists of dicts.

---

### 20. How do you use conditionals (`when`) in Ansible?

**Answer:** `when:` controls task execution. Example:

```yaml
- name: Install Apache on CentOS
  yum:
    name: httpd
    state: present
  when: ansible_facts['os_family'] == 'RedHat'
```

---

### 21. How do you skip CentOS-specific tasks?

**Answer:** Use a conditional:

```yaml
- debug: msg="Not CentOS"
  when: ansible_facts['os_family'] != 'RedHat'
```

Or use `tags` and `--skip-tags` to avoid groups of tasks.

---

### 22. How do you run only a specific task in a long playbook?

**Answer:** Tag the task and run with `--tags`:

```yaml
- name: Install Apache
  yum: name=httpd state=present
  tags: install
```

Run: `ansible-playbook site.yml --tags install`

---

### 23. Name some commonly used Ansible modules.

**Answer:** `yum`, `apt`, `service`, `systemd`, `copy`, `template`, `user`, `group`, `file`, `lineinfile`, `command`, `shell`, `uri`, `ec2`, `s3`, `docker_image`, `docker_container`.

---

### 24. Have you worked on custom modules? Explain.

**Answer:** Yes — custom modules are Python scripts placed under `library/`. Use `from ansible.module_utils.basic import AnsibleModule` to parse args and return `exit_json()`/`fail_json()`. Useful when built-in modules don’t cover a specific API or business logic.

---

### 25. What are Ansible plugins?

**Answer:** Plugins extend Ansible: connection plugins, callback plugins, lookup, filter, inventory, and action plugins. They let you customize execution, output formatting, data retrieval, and more.

---

### 26. How do you write a custom inventory plugin?

**Answer:** Write a Python plugin under `plugins/inventory/` implementing `parse()` and returning host/group JSON. Configure `ansible.cfg` to enable it. Use official Inventory Plugin API and follow plugin schema and docs.

---

### 27. What are Ansible Roles?

**Answer:** Roles structure playbooks into standardized directories (tasks, handlers, vars, defaults, templates, files, meta). They enable reuse and shareability (e.g., via Ansible Galaxy).

---

### 28. Directory structure of Ansible roles

**Answer:** Typical:

```
roles/
  webserver/
    tasks/main.yml
    handlers/main.yml
    templates/
    files/
    vars/main.yml
    defaults/main.yml
    meta/main.yml
```

---

### 29. What is Ansible Galaxy?

**Answer:** A public repository for reusable roles and collections. Install with `ansible-galaxy install <role>` and publish your roles there.

---

### 30. What are Ansible Collections?

**Answer:** Collections bundle roles, modules, plugins, and playbooks into a distributable unit. Install via `ansible-galaxy collection install <namespace.collection>`.

---

### 31. How do you use Ansible Collections in a playbook?

**Answer:** Reference collection-qualified modules. Example:

```yaml
- hosts: all
  tasks:
    - amazon.aws.ec2:
        key_name: mykey
        instance_type: t2.micro
```

---

### 32. How do you use tags in Ansible?

**Answer:** Add `tags:` to tasks/plays and run with `--tags`/`--skip-tags`. Useful to run subsets (e.g., `--tags install,configure`).

---

### 33. How do you encrypt data in Ansible?

**Answer:** Use **Ansible Vault** to encrypt files:

```bash
ansible-vault encrypt secrets.yml
ansible-vault edit secrets.yml
ansible-playbook site.yml --ask-vault-pass
```

Or use Vault ID and password files for automation.

---

### 34. What is Ansible Vault?

**Answer:** A feature to encrypt sensitive data (passwords, keys) used in playbooks/vars so you can store secrets in VCS safely.

---

### 35. How do you use Ansible Vault to manage secrets in playbooks?

**Answer:** Put secrets in an encrypted file (e.g., `vault.yml`), include it with `vars_files:` and supply the vault password or `--vault-id` when running playbooks.

---

### 36. How do you manage configuration drift with Ansible?

**Answer:** Re-run playbooks periodically (cron, Jenkins, Tower), use idempotent tasks, and use Ansible Tower/AWX scheduling to enforce desired state and correct drift.

---

### 37. Error handling in Ansible — options and example

**Answer:** Use `ignore_errors`, `failed_when`, `register`, `when`, and structured `block`/`rescue`/`always`. Example:

```yaml
- block:
    - service: name=nginx state=restarted
  rescue:
    - debug: msg="Nginx restart failed!"
```

---

### 38. Task reporting `changed` every time — troubleshooting

**Answer:** Check if the module is non-idempotent or using `shell/command`. Use `changed_when: false` or adjust task to perform checks (e.g., check file content before writing) to make it idempotent.

---

### 39. What is Ansible Lint, and why is it useful?

**Answer:** `ansible-lint` checks playbooks and roles for best practices, style issues, and common errors — helps maintain quality and consistency.

---

### 40. Example playbook to install and start Apache (fixed & correct)

**Answer:**

```yaml
- name: Install and start Apache
  hosts: webservers
  become: yes
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Ensure httpd is started and enabled
      service:
        name: httpd
        state: started
        enabled: yes
```

---

### 41. Sample playbook to install Nginx on Debian and RedHat families

**Answer:**

```yaml
- hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx on Debian
      apt:
        name: nginx
        state: present
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install Nginx on RedHat
      yum:
        name: nginx
        state: present
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Ensure nginx is running and enabled
      service:
        name: nginx
        state: started
        enabled: yes
```

---

### 42. Example playbook to check service status

**Answer:**

```yaml
- hosts: webservers
  become: yes
  tasks:
    - name: Gather service facts
      service_facts:

    - name: Print nginx state
      debug:
        msg: "{{ ansible_facts.services['nginx.service'].state }}"
      when: "'nginx.service' in ansible_facts.services"
```

---

### 43. Restart Nginx if config changes (template + handler)

**Answer:**

```yaml
tasks:
  - name: Deploy nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: Restart Nginx

handlers:
  - name: Restart Nginx
    service:
      name: nginx
      state: restarted
```

---

### 44. Using Ansible for application deployment — typical steps

**Answer:** Pull code (git), install dependencies, configure environment (templates + vars), run DB migrations, restart services, and perform health checks. Use roles for each stage and idempotent modules.

---

### 45. Using Ansible for OS hardening — examples

**Answer:** Set file permissions, disable root SSH, enforce password complexity, close unused ports, apply security updates, install audit/monitoring agents. Use roles and `ansible-lint` for consistent policies.

---

### 46. Using Ansible with Docker — basic example

**Answer:**

```yaml
- name: Pull nginx image
  community.docker.docker_image:
    name: nginx
    source: pull

- name: Run nginx container
  community.docker.docker_container:
    name: web
    image: nginx
    published_ports: "80:80"
    state: started
```

---

### 47. Setting up a Jump Host (ProxyJump) in inventory

**Answer:** Example inventory line:

```
server1 ansible_host=10.0.0.10 ansible_ssh_common_args='-o ProxyJump=jumpuser@bastion.example.com'
```

Alternatively configure `ssh_config` or `ansible.cfg` SSH arguments.

---

### 48. How to optimize playbook execution?

**Answer:** Increase `forks`, disable facts where not needed (`gather_facts: no`), use `async`/`poll: 0` for long tasks, use caching (fact_caching), limit hosts with `--limit`, and use efficient modules instead of shell loops.

---

### 49. Ansible execution strategies — Linear vs Free

**Answer:** `linear` (default) runs tasks sequentially per host. `free` lets hosts run tasks independently (useful when tasks for hosts don’t depend on each other). Set `strategy: free` in playbook header.

---

### 50. Managing dynamic inventory in AWS — example plugin config

**Answer:** Use the AWS EC2 inventory plugin:

```yaml
plugin: amazon.aws.ec2
regions: [ap-south-1]
filters:
  instance-state-name: running
```

Then `ansible-inventory -i aws_ec2.yml --list` to view hosts.

---

### 51. How to run a playbook with a specific inventory?

**Answer:**

```bash
ansible-playbook -i inventory/dev_hosts.ini site.yml
```

You can also pass a comma-separated host list: `-i 10.0.0.10,`

---

### 52. How to integrate Ansible with Terraform?

**Answer:** Use Terraform to provision infra and output IPs (`terraform output -raw web_ip`), then feed those into Ansible inventory (dynamic inventory or `-i $(terraform output -raw web_ip),`) to configure instances.

---

### 53. How to use Ansible in CI/CD pipelines (Jenkins example)?

**Answer:** In pipeline step run `ansible-playbook -i inventory/prod site.yml`. Store credentials securely (Vault, Jenkins credentials), and use linting & tests (Molecule) as pipeline stages.

---

### 54. What is Ansible Tower / AWX?

**Answer:** Tower (commercial Red Hat) and AWX (upstream) provide a web UI, RBAC, scheduling, logging, credential management, job templates, and centralized automation control for Ansible.

---

### 55. Difference between Ansible and Terraform

**Answer:** Ansible is for configuration management (push, no central state), Terraform is for infrastructure provisioning (declarative, maintains state). They complement each other: Terraform creates infra, Ansible configures it.

---

### 56. How does Ansible differ from Chef and Puppet?

**Answer:** Ansible is agentless and push-based using YAML. Chef/Puppet are typically agent-based and use Ruby/DSL with different models (often pull). Ansible tends to be simpler to start with.

---

### 57. Why is Ansible preferred?

**Answer:** Simplicity (YAML), agentless architecture (SSH), idempotency, large community, good cloud and CI/CD integrations.

---

### 58. Best practices in production with Ansible

**Answer:** Keep playbooks modular (roles), maintain in Git, use Ansible Vault for secrets, adopt CI (linting & Molecule), use dynamic inventories, enable logging, and test in staging.

---

### 59. Why do you use Ansible? (How to answer in interview)

**Answer (example):** “I use Ansible to automate deployments, enforce consistent configurations, reduce manual errors, and integrate infra provisioning with CI/CD. It speeds up delivery and improves reliability.”

---

### 60. How do you test Ansible playbooks?

**Answer:** Use `--syntax-check`, `--check` (dry-run), `ansible-lint`, and `Molecule` to test roles locally (with Docker/Vagrant). Also run in staging with monitoring and automated smoke tests.

---

### 61. Multi-tier application deployment — approach

**Answer:** Use roles per tier (web/app/db), separate inventories and group_vars, a `site.yml` to orchestrate order, and health checks. Example:

```yaml
- hosts: web
  roles: [web]
- hosts: app
  roles: [app]
- hosts: db
  roles: [db]
```

---

### 62. Rolling updates with zero downtime — pattern

**Answer:** Use `serial` to update hosts in batches, drain traffic from load balancer, deploy, run health checks, and re-add to LB. Example `serial: 2` for batch of two hosts.

---

### 63. Handling secret management — best practice

**Answer:** Use Ansible Vault for secrets, avoid plaintext credentials in repos, use `--vault-id` or vault password files for automation, and integrate with external secret managers (HashiCorp Vault, AWS Secrets Manager) if needed.

---

### 64. Idempotency issues — how to debug and fix

**Answer:** Run with `-vvv` to identify cause, check module choice (replace `shell` with module), add `creates`/`removes` or conditional checks, and use `changed_when`/`check_mode` where appropriate.

---

### 65. Dynamic inventory for frequently changing infra

**Answer:** Use cloud inventory plugins (AWS, Azure, GCP) or dynamic scripts that query the cloud provider and return JSON inventory. Filter by tags, region, and instance-state.

---

### 66. Optimizing slow playbooks — tips

**Answer:** Increase forks (`-f`), enable fact caching, gather minimal facts (`gather_facts: no` where possible), use `async` for long-running tasks, and reduce unnecessary tasks and network hops.

---

### 67. Migrating legacy shell scripts to Ansible

**Answer:** Break the script into discrete steps and convert to modules (`apt`, `yum`, `file`, `user`). Keep idempotency in mind and replace multi-step shells with declarative tasks.

---

### 68. Managing dev/staging/prod environments

**Answer:** Use separate inventory directories (`inventories/dev`, `inventories/prod`), `group_vars` & `host_vars`, and environment-specific vars files. Use CI pipelines to select environment.

---

### 69. Delegation & `run_once` — use cases

**Answer:** `delegate_to` runs a task on a specific host (e.g., DB migrations on primary); `run_once: true` ensures the task runs once across all hosts (e.g., create a shared resource).

Example:

```yaml
- name: Run migration on db host
  command: /opt/migrate.sh
  delegate_to: "{{ groups['db'][0] }}"
  run_once: true
```

---

### 70. Fact caching & custom facts — why & how

**Answer:** Fact caching reduces repeated SSH/fact collection (speeds up runs). Enable `fact_caching` in `ansible.cfg`. Custom facts live under `/etc/ansible/facts.d/` as JSON files and are accessible as `ansible_local`.

---

### 71. Advanced loops — handling complex data

**Answer:** Loop over dictionaries/lists of dicts:

```yaml
- name: Create users
  user:
    name: "{{ item.name }}"
    shell: "{{ item.shell }}"
  loop:
    - { name: 'dev', shell: '/bin/bash' }
    - { name: 'test', shell: '/bin/zsh' }
```

Use `loop_control` to set labels and indices.

---

### 72. `import_tasks` vs `include_tasks` — difference and usage

**Answer:** `import_tasks` is static (parsed at play start). `include_tasks` is dynamic (evaluated at runtime) and can use `when`. Use `import_tasks` for fixed task sets and `include_tasks` for conditional/dynamic inclusion.

---

### 73. Advanced error handling with `block/rescue/always`

**Answer:** `block` groups tasks, `rescue` handles failures in the block, `always` runs afterwards regardless of success. Use `failed_when`/`ignore_errors` to tune behavior. Example:

```yaml
- block:
    - do something
  rescue:
    - notify admin
  always:
    - cleanup
```

---

### 74. Ansible Tower/AWX advanced usage

**Answer:** Use Job Templates, Credentials, Inventories, Schedules, and RBAC to manage automation at scale. Integrate with SCM (Git), LDAP/SSO, and notifications. Use surveys for runtime variable input.

---

### 75. Molecule testing & CI/CD integration — workflow

**Answer:** Use Molecule to test roles (`molecule init role`, `molecule test`) with Docker or other drivers. Integrate tests into CI pipelines (Jenkins/GitHub Actions) to run linting, unit tests, and `molecule test` before merging.

---

