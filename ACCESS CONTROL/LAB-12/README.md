# ✅ **PortSwigger Lab: Multi-step process with no access control on one step**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step](https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step)

## ⚙️ **Difficulty:** Practitioner

📂 **Category:** Access control → Multi-step workflows → Authorization bypass

---

### 📚 **Description**

This lab demonstrates a multi-step workflow where one intermediate step in a process lacks proper access control checks. The application performs a multi-stage operation (for example, creating or approving content) where an authenticated user may access an intermediate page or endpoint that should only be reachable after previous steps. However, that intermediate step is not protected and can be accessed directly, enabling an attacker to skip earlier checks and perform privileged actions (such as approving content or deleting users) without proper authorization.

**Goal:** Find the unprotected step in the multi-step process, access it directly, and use the available functionality to delete the user `carlos`.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

A multi-step server-side process relies on a specific sequence of actions to enforce permissions, but one step does not validate whether the requester has the right to perform the action. Attackers can directly access or replay the unprotected step and reach a privileged state.

#### ✅ Why it's dangerous

* Missing access control on an intermediate step undermines the security of the entire workflow.
* Attackers can bypass intended checks and reach outcomes reserved for privileged users (approve content, delete users, escalate privileges).
* Multi-step flows must validate state and authorization at every step, not just at entry points.

#### ✅ Where to look

* Multi-step flows (wizards, approval processes, staged actions) and the endpoints they call.
* Intermediate URLs, hidden forms, or AJAX endpoints that are expected to be called only as part of the workflow.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker inspects the normal multi-step flow to identify each request/endpoint involved.
* One intermediate endpoint that performs a privileged action (or moves the process to a privileged state) does not verify the caller’s authorization or prior state.
* By calling this endpoint directly (or replaying the request with modified parameters), the attacker triggers the privileged action and completes the lab by deleting `carlos`.

**Impact:** Authorization checks must be enforced at every step of a workflow; missing checks undermine security and permit privilege escalation or unauthorized actions.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Explored the site and executed the multi-step workflow as an authenticated user to capture each request in Burp Proxy.
2. Reviewed the captured requests to identify intermediate endpoints and any state tokens or parameters passed between steps.
3. Tested direct access to each intermediate endpoint (via Repeater) to see whether the server enforced prior-step validation or authorization.
4. Found an unprotected intermediate step that returned a privileged response or allowed a privileged action.
5. Replayed the request targeting the victim `carlos` (if applicable) and used the returned functionality to delete the user.

---

### 📸 Screenshots 
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-12/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-12/images/2.png)

---

### 📝 What I Learned

✔ Multi-step workflows require authorization checks at every stage — never assume earlier steps enforce all necessary checks.                         
✔ Capture and replay intermediate requests during testing to verify that stateful protections are enforced server-side.                         
✔ Workflow security is often overlooked; include workflow step validation in threat modeling and code reviews.                         

---

### 🔐 Mitigation Techniques

✔ Enforce server-side state validation for each step — ensure that an endpoint verifies the process state and the caller's authorization before acting.                 
✔ Use single-use, server-side tokens to track multi-step workflows and validate them at each transition.                                                   
✔ Log and monitor direct access to intermediate endpoints and alert on unexpected sequences.                                                                    

---

### 🧾 Notes

* The lab requires identifying an unprotected intermediate step in a multi-step process and using it to delete the victim `carlos`.
* Only perform these workflow tests in authorized lab environments.

---

### 👤 Author

Harbeer-Singh

