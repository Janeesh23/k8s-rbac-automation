# Kubernetes RBAC User Onboarding Automation

##  Overview
Managing Kubernetes RBAC manually can be time-consuming and error-prone.  
Each time a new user joins a project, administrators often need to:

- Create a ServiceAccount
- Bind it to the correct Role or ClusterRole
- Generate a kubeconfig for authentication
- Share the kubeconfig securely
- Revoke access when the user leaves

This project automates the entire process using **Ansible**, **AWS S3**, and **SMTP (Gmail)**.  



##  Solution
This Ansible automation provides a **hands-free workflow** for Kubernetes RBAC user management:

1. **ServiceAccount Creation**
   - Creates a ServiceAccount for the user.
   - Supports both `Role` (namespace scoped) and `ClusterRole` (cluster scoped).

2. **Role/ClusterRole Binding**
   - Option to bind existing roles (e.g., `view`, `admin`)  
   - Or create new custom roles based on YAML rules.

3. **Kubeconfig Generation**
   - Builds a user-specific kubeconfig containing:
     - API server endpoint
     - CA certificate
     - ServiceAccount token

4. **Secure Distribution via AWS S3**
   - Uploads the kubeconfig to a private S3 bucket.
   - Generates a presigned URL (time-limited) for secure download.

5. **User Notification**
   - Sends an automated email to the user with the presigned URL.
   - SMTP can be configured using Gmail App Passwords or AWS SES.
   - Secrets (like SMTP credentials) are stored securely in **Ansible Vault**.

6. **Access Revocation**
   - Includes a separate playbook to delete the ServiceAccount and revoke kubeconfig access.

---


