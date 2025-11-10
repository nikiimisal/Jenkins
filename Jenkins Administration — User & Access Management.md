# Jenkins Administration — User & Access Management 

> Comprehensive guide for Jenkins Administrators to configure **user accounts**, **access control**, **ACLs (Access Control Lists)**, and **LDAP integration** for secure user management. This document also explains what happens if permissions are not properly configured.

---

## 🧰 Prerequisites

* Jenkins is already installed and accessible.
* Admin-level access to Jenkins UI.
* Network connectivity to the Jenkins instance (IP and port accessible).

---

## 👤 User Creation in Jenkins

1. Go to **Manage Jenkins → Manage Users**.
2. Click **Create User**.
3. Enter details: Username, Password, Full Name, Email.
4. Click **Create User**.

🧩 **Important Note:** If a user is created **without any access restrictions** and your Jenkins IP:PORT is reachable over the internet, that user can log in and — depending on default settings — might:

* View or even **modify CI/CD pipelines**.
* Delete existing jobs or configurations.
* Trigger unwanted builds or deployments.

💡 Always configure permissions (Matrix or Role-Based) immediately after creating any new user.

---

## 🔐 Access Control Configuration

Go to **Manage Jenkins → Configure Global Security**.
Here you control two main areas:

* **Security Realm:** Defines *who* can log in.
* **Authorization:** Defines *what* users can do once logged in.

If **Authorization** is not configured, users might have **global read/write permissions**, which poses a security risk.

---

## ⚙️ Required Plugins

### 🔸 Matrix Authorization Strategy Plugin

* Provides granular, checkbox-style permission control per user/group.
* Without it, all authenticated users may have unnecessary access.

### 🔸 Role-Based Authorization Strategy Plugin (RBAC)

* Lets you define **roles** (Admin, Developer, Viewer) and assign them to users.
* Prevents users from editing or deleting pipelines that don’t belong to them.

### 🔸 LDAP / Active Directory Plugin

* Integrates Jenkins with enterprise authentication systems.
* Enables centralized user login management using corporate credentials.

🧠 **LDAP Concept :** LDAP (Lightweight Directory Access Protocol) allows Jenkins to use an organization’s directory service for authentication, ensuring users and groups are managed centrally, not locally.

### 🔸 Audit Trail Plugin

* Logs user actions (login, job changes, deletions, plugin installs).
* Helps detect unauthorized activity.

Install Plugins via: **Manage Jenkins → Plugins → Available → Search → Install → Restart Jenkins.**

---

## 🧩 Matrix Authorization Strategy Setup

1. Go to **Manage Jenkins → Configure Global Security**.
2. Under **Authorization**, select **Matrix-based security**.
3. Add users and assign permissions manually.
4. Save changes.

✅ Example:

| User      | Read | Build | Configure | Administer |
| --------- | ---- | ----- | --------- | ---------- |
| admin     | ✔️   | ✔️    | ✔️        | ✔️         |
| developer | ✔️   | ✔️    | ❌         | ❌          |
| viewer    | ✔️   | ❌     | ❌         | ❌          |

⚠️ **If not configured:**
A basic user might access the Jenkins web UI and perform admin-level actions if the default security policy is permissive — leading to unauthorized deletion or modification of CI/CD pipelines.

---

## 🧭 Role-Based Access (RBAC) Setup

1. Go to **Manage Jenkins → Configure Global Security**.
2. Select **Role-Based Strategy** under Authorization.
3. Save and go to **Manage Jenkins → Manage and Assign Roles**.

**Manage Roles:** Define global/project-specific roles.
**Assign Roles:** Link users to roles.

| Role      | Permissions                            |
| --------- | -------------------------------------- |
| Admin     | Full control (system, jobs, plugins)   |
| Developer | Build and configure assigned jobs only |
| Viewer    | Read-only access                       |

⚠️ **If not applied:**
Without RBAC or Matrix Authorization, *every authenticated user could act as an Admin*, risking pipeline tampering or deletion.

---

## 🔑 ACL (Access Control List)

ACLs determine what actions users can take and where — global, folder, or job-level.

### Types of ACLs

1. **Global ACL:** Controls access to all Jenkins resources.
2. **Folder-level ACL:** Manages permissions within a specific folder.
3. **Item-level ACL:** Defines permissions for specific jobs/pipelines.
4. **Node-level ACL:** Restricts control over build agents/nodes.

⚠️ **Without ACL enforcement:**
Any user could access job configurations, environment variables, or even stored credentials — leading to possible CI/CD pipeline compromise.

---

## 🧱 LDAP / Active Directory Integration

To integrate Jenkins with LDAP:

1. Go to **Manage Jenkins → Configure Global Security**.
2. Under **Security Realm**, select **LDAP**.
3. Fill details:

   * Server: `ldap://yourcompany-ldap.com`
   * Root DN: `dc=yourcompany,dc=com`
   * User Search Base: `ou=users,dc=yourcompany,dc=com`
   * Manager DN (optional): `admin@yourcompany.com`
4. Test and Save.

✅ **After LDAP Integration:**

| Role      | User Type     | Access                      |
| --------- | ------------- | --------------------------- |
| Admin     | Local Jenkins | Full system control         |
| Developer | LDAP user     | Job creation & build rights |
| Viewer    | LDAP user     | View-only rights            |

💡 **LDAP ensures** centralized authentication and automatic user sync — no need to create users manually.

---

## 🧠 Security Reminder

If your Jenkins IP (e.g., `http://<public-ip>:8080`) is accessible online and you’ve created a user without restricting access:

* That user could directly log in from the internet.
* Modify, trigger, or delete your CI/CD jobs.
* Inject malicious scripts into build pipelines.

🛡️ **To prevent this:**

* Always enable **Matrix or Role-based security** immediately.
* Restrict access to Jenkins via **firewall** or **VPN**.
* Use **HTTPS** instead of HTTP.
* Regularly review user permissions.

##

---

### 💡 In Short

A Jenkins Administrator must ensure that every user created is assigned a **specific role and ACL**, and **authorization is always configured**. Without proper control, even a normal user with access to your Jenkins IP could manipulate or delete your CI/CD pipelines.
