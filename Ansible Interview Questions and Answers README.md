ANSIBLE QUESTIONS AND ANSWERS
---

## **1. What is Ansible, and what is its purpose?**

Ansible is an open-source automation tool for **configuration management, application deployment, and orchestration**.

* Automates repetitive IT tasks across servers.
* Agentless — uses SSH and Python.
* Ensures consistency, speed, and reliability across environments.

---

## **2. Main components of Ansible**

* **Inventory:** List of target hosts or groups.
* **Playbooks:** YAML files defining automation tasks.
* **Modules:** Scripts for specific actions (copy, yum, service, etc.).
* **Plugins:** Extend Ansible’s core features (connection, callback).
* **Facts:** System details collected from hosts.
* **Roles:** Structured, reusable playbooks.

---

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

Handlers are tasks triggered **only when notified** by other tasks.

* Commonly used for restarting services after config changes.
* Defined under `handlers:` in playbooks.

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

Variables store dynamic values and make playbooks reusable.

```yaml
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

## **23. Name some modules you have worked on**

* `yum` / `apt`
* `service` / `systemd`
* `copy` / `template`
* `user` / `group`
* `file` / `lineinfile`
* `ec2` / `s3`

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

## **26. How do you write a custom inventory plugin?**

* Written in Python under `plugins/inventory/`.
* Define a `parse()` method returning JSON with hosts and groups.
* Enable in `ansible.cfg`.

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

If you want, I can also **format this into a clean LinkedIn-post style with headings, bold highlights, and snippets** exactly like Mihir Popat’s article so it’s ready to publish or share.

Do you want me to do that?
