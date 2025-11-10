# Jenkins Email Notification Setup — Gmail SMTP 

> A step-by-step and beginner-friendly guide to configure **Email Notifications in Jenkins** using **Gmail SMTP**, ensuring you never miss build status updates.
>also check my linkdine post there i have share some screenshots & info.[click here](https://www.linkedin.com/posts/nikhilmisal_jenkins-devops-cicd-activity-7390594760748658688-NYJU?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFcBGtoBmoaRzra2TlzrfhghK14stiKDRAc)
## 🚀 Overview

Email notifications in Jenkins help you stay informed about build results, failures, and status updates directly in your inbox. Whether you’re managing CI/CD pipelines or monitoring automated jobs, notifications are key to fast issue response.

---

## 🧰 Prerequisites

Before starting, ensure:

* Jenkins is installed and running.
* Internet access for Jenkins server.
* A Gmail account with **2-Step Verification enabled**.
* Generated **App Password** from Google Account.
* "Email Extension" plugin installed in Jenkins.

---

## ⚙️ Step 1 — Configure Gmail SMTP in Jenkins

1. Go to **Manage Jenkins → Configure System**.
2. Scroll to **E-mail Notification** section.
3. Fill in the details:

   * **SMTP Server:** `smtp.gmail.com`
   * **Port:** `465`
   * **Use SSL:** ✔️ Enabled
   * **Use SMTP Authentication:** ✔️ Enabled
   * **Username:** your Gmail ID (e.g., `example@gmail.com`)
   * **Password:** Gmail App Password (not your real password)
4. Click **Test configuration by sending test e-mail**.
5. Enter your target email (e.g., `nik0misal@gmail.com`) and click **Test**.

✅ If setup is correct, you’ll receive a “Jenkins Test Mail” in your inbox.

---

## 🔑 Step 2 — Generate Gmail App Password

Gmail doesn’t allow direct password login for third-party apps (like Jenkins). Follow these steps:

1. Go to **Google Account → Security**.
2. Enable **2-Step Verification** (if not enabled).
3. Go to **App Passwords** section.
4. Select **App:** Mail → **Device:** Other (Custom name like "Jenkins").
5. Copy the **16-character app password**.
6. Use this in Jenkins SMTP password field.

🧠 *Note:* Each app password is unique and can be revoked anytime for security.

---

## 📤 Step 3 — Add Post-Build Email Notification

1. Open any Jenkins job.
2. Click **Configure → Post-build Actions**.
3. Select **E-mail Notification**.
4. Add recipient addresses (comma-separated if multiple).
5. Choose when to send emails (on success/failure/always).
6. Save and build the job.

📩 Jenkins will automatically send mail when a build completes based on your chosen conditions.

---

## 🧩 Step 4 — Verify Email Delivery

After triggering a build:

* Open your Gmail inbox.
* You should receive a mail titled **“Build #X Result”**.
* It includes build logs, result, and timestamp.

If not received:

* Check **Manage Jenkins → System Log** for SMTP errors.
* Verify SSL and SMTP port settings.
* Ensure your Gmail app password hasn’t expired.

---

## ⚠️ Common Issues & Fixes

| Issue                 | Cause                      | Fix                                         |
| --------------------- | -------------------------- | ------------------------------------------- |
| Authentication failed | Wrong password used        | Use App Password instead of normal password |
| SSL Handshake failed  | Port or SSL config issue   | Use Port `465` with SSL enabled             |
| Email not received    | Recipient blocked/filtered | Check Spam folder or Gmail filters          |

---

## 🔐 Security & Best Practices

* Never store plain text passwords — use **Credentials Manager** in Jenkins.
* Prefer **App Passwords** instead of main Gmail password.
* Restrict network access to Jenkins via firewall or VPN.
* Review logs periodically for unauthorized SMTP attempts.

---

## 🧠 Quick Recap (Adaptive Summary)

| Step | Action                | Key Detail                       |
| ---- | --------------------- | -------------------------------- |
| 1    | Configure SMTP        | smtp.gmail.com, Port 465, SSL ✔️ |
| 2    | Generate App Password | From Google Account → Security   |
| 3    | Add Notification      | In Post-Build Actions            |
| 4    | Verify & Test         | Check inbox and logs             |

---

### 💡 In Short

Setting up Jenkins Email Notifications with Gmail ensures you get instant updates about your CI/CD builds. It’s simple, secure, and highly effective for DevOps automation workflows.
