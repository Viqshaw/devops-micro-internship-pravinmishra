# Assignment 02 — Provision Linux VMs with Terraform and Run Ansible Ad-Hoc Commands

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision three or four Ubuntu Linux Virtual Machines on either Microsoft Azure or Amazon Web Services.

You will configure SSH key-based authentication, organize the servers using a custom Ansible inventory, and run Ansible ad-hoc commands across individual hosts and inventory groups.

---

# Task 1 — Create the Multi-Host Lab Structure

## Goal

Create a separate project directory for the multi-host lab and prepare the Terraform, Ansible, and documentation files.

This project will use the Git repository and Ansible controller prepared in Assignment 01.

### Evidence

#### Screenshot 1 — Terminal showing the complete `ansible-adhoc-lab` project structure

![alt text](screenshots/02.1.1-structure.png)

---

#### Screenshot 2 — Terminal showing `git status --short` with the new project files and updated `.gitignore`

![alt text](screenshots/02.1.2-GitStatus.png)

---

### Notes

Created a separate ansible-adhoc-lab project directory containing the Terraform configuration, Ansible files, and project documentation for the multi-host lab. Updated .gitignore to exclude sensitive files and unnecessary generated files. Verified the project structure and Git status in preparation for version control.

---

# Task 2 — Create the Terraform Configuration

## Goal

Create the Terraform configuration required to provision three or four Ubuntu Linux VMs on your selected cloud platform.

Complete only one option:

- Option A — Microsoft Azure
- Option B — Amazon Web Services

Do not configure both providers for this assignment.

### Evidence

#### Screenshot 3 — Terraform configuration showing the three or four server roles and the `for_each` or `count` implementation

![alt text](screenshots/02.2.1-roles.png)

---

#### Screenshot 4 — Terraform configuration showing SSH restricted to the controller IP and HTTP allowed only for web hosts

![alt text](screenshots/02.2.2-ssh.png)

---

#### Screenshot 5 — Terraform output configuration showing how public IP addresses are associated with the server roles

![alt text](screenshots/02.2.3-output.png)

---

### Notes

Configured Terraform for the AWS multi-host lab to provision Ubuntu VMs for the web1, app1, and db1 roles using for_each. Configured SSH access restrictions and allowed HTTP traffic only for the web host. Added outputs to associate public IP addresses with the server roles for easier access and Ansible inventory setup.

---

# Task 3 — Provision the Infrastructure with Terraform

## Goal

Initialize and validate the Terraform configuration, review the execution plan, provision the selected three or four VMs, and retrieve their public IP addresses.

### Evidence

#### Screenshot 6 — Final `terraform apply` output showing `Apply complete`

![alt text](screenshots/02.3.1-apply.png)

---

#### Screenshot 7 — `terraform output public_ips` showing the role-to-IP mapping for all three or four VMs

![alt text](screenshots/02.3.2-outputs.png)

---

#### Screenshot 8 — Azure Portal or AWS Management Console showing all three or four VMs in the `Running` state, with their role-based names visible

![alt text](screenshots/02.3.3-console.png)

---

### Notes

Initialized and validated the Terraform configuration, reviewed the execution plan, and provisioned the three AWS Ubuntu VMs for web1, app1, and db1. Retrieved the public IP addresses using terraform output public_ips and verified that all instances were running in the AWS Management Console.

---

# Task 4 — Verify SSH Key-Based Access

## Goal

Verify that each managed VM can be accessed from the Ansible controller using SSH key-based authentication.

### Evidence

#### Screenshot 9 — Terminal showing successful SSH hostname output from all VMs

![alt text](screenshots/02.4.1-verify.png)

---

### Notes

Verified SSH key-based authentication from the Ansible controller to all three AWS Ubuntu VMs (web1, app1, and db1). Confirmed successful connections by running the hostname command on each server, ensuring passwordless SSH access was working correctly.

---

# Task 5 — Create the Custom Ansible Inventory

## Goal

Create an Ansible inventory file that groups the managed VMs by role.

The inventory allows Ansible to run commands against all servers, or only specific groups such as `web`, `app`, or `db`.

### Evidence

#### Screenshot 10 — `inventory.ini` showing the `web`, `app`, and `db` groups

![alt text](screenshots/02.5.1-inventory.png)

---

#### Screenshot 11 — Output of `ansible-inventory -i inventory.ini --graph`

![alt text](screenshots/02.5.2-output.png)

---

### Notes

Created a custom Ansible inventory grouping the managed VMs into web, app, and db groups using their respective IP addresses. Verified the inventory structure with ansible-inventory -i inventory.ini --graph to confirm that each server was assigned to the correct group for targeted ad-hoc commands.

---

# Task 6 — Run Ansible Ad-Hoc Commands

## Goal

Run Ansible ad-hoc commands from the controller to verify connectivity, check server information, and manage packages and services across inventory groups.

This task proves that the inventory is working and that Ansible can control multiple managed VMs without writing a playbook.

### Evidence

#### Screenshot 12 — Output of `ansible all -i inventory.ini -m ping`

![alt text](screenshots/02.6.1-ping.png)

---

#### Screenshot 13 — Output of `ansible all -i inventory.ini -m command -a "uptime"`

![alt text](screenshots/02.6.2-uptime.png)

---

#### Screenshot 14 — Output of `ansible web -i inventory.ini -m apt -a "name=nginx state=present update_cache=yes" --become`

![alt text](screenshots/02.6.3-apt.png)

---

#### Screenshot 15 — Output of `ansible web -i inventory.ini -m service -a "name=nginx state=started enabled=yes" --become`

![alt text](screenshots/02.6.4-service.png)

---

#### Screenshot 16 — Output of `ansible all -i inventory.ini -m apt -a "name=htop state=present update_cache=yes" --become`

![alt text](screenshots/02.6.5-htop.png)

---

#### Screenshot 17 — Output of `ansible web -i inventory.ini -m command -a "systemctl is-active nginx"`

![alt text](screenshots/02.6.6-systemctl.png)

---

### Notes

Executed Ansible ad-hoc commands to verify connectivity, check system uptime, and manage packages and services across the managed VMs. Successfully tested Ansible ping, installed and started Nginx on the web host, enabled it at boot, installed htop on all hosts, and verified that Nginx was active.

---

# LinkedIn Post Required

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/dqsX_8Q6`

---

#### Screenshot — Published LinkedIn post

![alt text](screenshots/02.7.1-linkedin.png)

---

# Assignment Questions

Answer the following in your own words:

**1. What is the purpose of an Ansible inventory file?**

An Ansible inventory file lists the managed servers and organizes them into groups. It allows Ansible to identify which servers to connect to and where to execute commands.

---

**2. What is the difference between the `web`, `app`, and `db` groups in your inventory?**

The web group contains web servers that handle HTTP requests and run services such as Nginx. The app group contains application servers that run application logic, while the db group contains database servers that store and manage data.

---

**3. What does the Ansible `ping` module verify?**

The Ansible ping module checks whether Ansible can connect to a managed server and execute a basic Python-based task. It helps confirm that SSH access and the remote Ansible environment are working correctly.

---

**4. Why do package installation commands require `--become`?**

Package installation usually requires administrator privileges to modify system files and install software. The --become option allows Ansible to execute the command with elevated privileges, usually through sudo.

---

**5. When would you use an ad-hoc command instead of a playbook?**

I would use an ad-hoc command for quick, one-time tasks such as checking server uptime, testing connectivity, installing a package, or checking a service's status. For repeated tasks or complex configurations that require multiple steps, I would use a playbook.

---

**6. What is one challenge you faced while setting up SSH or inventory, and how did you fix it?**

One challenge I faced was configuring SSH access between the controller and the managed servers. I resolved it by checking the SSH key configuration, verifying the server IP addresses, and ensuring that the security group rules allowed SSH traffic from the appropriate source. I then tested connectivity before running Ansible commands.

---

# Required Files

Confirm that the following files are included in your assignment workspace:

- [ ] `ansible-adhoc-lab/README.md`
- [ ] `ansible-adhoc-lab/terraform/providers.tf`
- [ ] `ansible-adhoc-lab/terraform/main.tf`
- [ ] `ansible-adhoc-lab/terraform/variables.tf`
- [ ] `ansible-adhoc-lab/terraform/outputs.tf`
- [ ] `ansible-adhoc-lab/ansible/inventory.ini`
- [ ] Updated `.gitignore`

---

# Submission Instructions

- Add all required screenshots from the tasks.
- Full Name must be visible in required screenshots.
- Mention whether you used Azure or AWS.
- Mention whether you used the three-VM option or four-VM option.
- Add the public IP addresses of the VMs, redacted if preferred.
- Add your `inventory.ini` proof.
- Add a short explanation of what you learned.
- Answer all assignment questions clearly in your own words.
- Add your LinkedIn post URL.
- Do not expose SSH private keys, Terraform state files, cloud credentials, passwords, access keys, secret keys, account IDs, or subscription IDs.

---

# Completion Checklist

- [ ] Task 1: `ansible-adhoc-lab` project structure created
- [ ] Task 1: `.gitignore` updated for Terraform files
- [ ] Task 2: Terraform configuration created
- [ ] Task 2: Server roles defined for either three or four VMs
- [ ] Task 2: `count` or `for_each` used
- [ ] Task 2: SSH restricted to the controller public IP
- [ ] Task 2: HTTP allowed only for web hosts
- [ ] Task 2: Terraform output maps roles to public IPs
- [ ] Task 3: Terraform initialized successfully
- [ ] Task 3: Terraform configuration validated
- [ ] Task 3: Terraform apply completed successfully
- [ ] Task 3: All selected VMs are running
- [ ] Task 4: SSH key-based access works for every VM
- [ ] Task 5: `inventory.ini` contains `web`, `app`, and `db` groups
- [ ] Task 5: `ansible-inventory -i inventory.ini --graph` shows the correct groups
- [ ] Task 6: `ansible all -i inventory.ini -m ping` returns `SUCCESS`
- [ ] Task 6: Ad-hoc commands run successfully
- [ ] Task 6: `--become` was used for package and service tasks
- [ ] Task 6: Nginx is active on the `web` group
- [ ] Screenshots 1–17 are included
- [ ] Assignment questions are answered
- [ ] LinkedIn post published
- [ ] LinkedIn post URL added
- [ ] No sensitive information is exposed

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra and The CloudAdvisory, focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## Resources

- DMI Official Website: [https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme)
- University: [https://university.pravinmishra.com?utm_source=github&utm_medium=readme](https://university.pravinmishra.com?utm_source=github&utm_medium=readme)
- Discord Community: [https://discord.pravinmishra.com?utm_source=github&utm_medium=readme](https://discord.pravinmishra.com?utm_source=github&utm_medium=readme)
- Blog: [https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*