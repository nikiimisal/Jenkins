# Jenkins Administration — User & Access Management (Step-by-step)

> This guide focuses on how Jenkins Administrators can create users and assign specific access or roles by using built-in features and plugins like **Matrix Authorization Strategy** and **Role-Based Strategy**. Perfect for those who already have Jenkins setup and want to configure user-level access control.

---

## 🧰 Prerequisites

* Jenkins is already installed and running (UI accessible).
* You have **Administrator** privileges.
* Jenkins has **Manage Jenkins** section available.

---

## 👤 Creating Users in Jenkins

By default, Jenkins allows you to **create users**, but not assign fine-grained access without plugins.

### Steps to Create a New User

1. Go to **Manage Jenkins → Manage Users**.
2. Click **Create User**.
3. Fill details:

   * Username
   * Password
   * Full name
   * Email address
4. Click **Create User**.

Now, the user exists but has default/global permissions (based on current security setup). To assign custom roles or permissions, we must configure authorization strategies and plugins.

---

## 🔐 Configuring Access Control (Step-by-Step)

### 1️⃣ Go to Security Settings

Navigate to:
**Manage Jenkins → Configure Global Security**

Here you will find key sections:

* **Security Realm** → controls authentication (who can log in)
* **Authorization** → controls authorization (what users can do)

---

## ⚙️ Plugins Required for User Access Control

To provide controlled, role-based, or matrix-level access, install the following plugins:

### 🔸 1. Matrix Authorization Strategy Plugin

* **Plugin Name:** Matrix Authorization Strategy
* **Purpose:** Provides fine-grained permission control for individual users or groups.
* **How to install:**

  * Go to **Manage Jenkins → Plugins → Available tab**
  * Search for **Matrix Authorization Strategy**
  * Install and restart Jenkins.

### 🔸 2. Role-Based Authorization Strategy Plugin

* **Plugin Name:** Role Strategy Plugin
* **Purpose:** Allows creation of roles (e.g., Admin, Developer, Viewer) and assigns them to users/groups.
* **How to install:**

  * **Manage Jenkins → Plugins → Available tab**
  * Search for **Role Strategy Plugin**
  * Install and restart Jenkins.

---

## 🧩 Configure Matrix Authorization Strategy

Once installed:

1. Go to **Manage Jenkins → Configure Global Security**.
2. Under **Authorization**, select **Matrix-based security**.
3. Add users by typing their usernames.
4. Check/uncheck permissions (like Build, Configure, Delete, Administer, Read, etc.) based on the access you want.
5. Click **Save**.

### ✅ Example

| User      | Read | Build | Configure | Administer |
| --------- | ---- | ----- | --------- | ---------- |
| admin     | ✔️   | ✔️    | ✔️        | ✔️         |
| developer | ✔️   | ✔️    | ❌         | ❌          |
| viewer    | ✔️   | ❌     | ❌         | ❌          |

---

## 🧭 Configure Role-Based Strategy (RBAC)

Once the **Role Strategy Plugin** is installed:

### Steps

1. Go to **Manage Jenkins → Configure Global Security**.
2. Under **Authorization**, choose **Role-Based Strategy**.
3. Save and return to **Manage Jenkins → Manage and Assign Roles**.

You’ll see two options:

* **Manage Roles:** Create roles and define permissions.
* **Assign Roles:** Assign roles to users.

### Example Roles

| Role Name | Permissions                   |
| --------- | ----------------------------- |
| Admin     | Full control over Jenkins     |
| Developer | Can build, configure own jobs |
| Viewer    | Can view jobs only            |

### Assigning Roles

* Go to **Manage Jenkins → Manage and Assign Roles → Assign Roles**
* Add the user under **Global Roles** or **Project Roles**
* Assign appropriate roles and save.

---

## 🧱 Group and Team Management

If you have multiple users per team:

* Create Jenkins **Folders** for each project or team.
* Assign permissions at **folder level** via Matrix or Role-Based plugins.
* Example: Frontend Team (access to Frontend jobs only), Backend Team (access to Backend jobs only).

You can also integrate external systems like:

* **LDAP Plugin** – to connect Jenkins with corporate directory users.
* **Active Directory Plugin** – for enterprise-level user authentication.

---

## 🔑 Credentials and Secrets Management

While setting up user access, make sure credentials are managed securely:

* Go to **Manage Jenkins → Credentials**.
* Use **global** or **folder-level** domains.
* Never store secrets in plain text or Jenkinsfiles.
* Use the **Credentials Binding Plugin** to inject secrets in pipelines safely.

---

## 🛡️ Security and Auditing

* Install **Audit Trail Plugin** to log user activities.
* Regularly review which users have **Admin** or **Configure** permissions.
* Backup Jenkins Home regularly (`/var/lib/jenkins`).
* Periodica
