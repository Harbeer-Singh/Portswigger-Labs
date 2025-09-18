# ✅ **PortSwigger Lab: User ID controlled by request parameter, with unpredictable user IDs**

## 🔗 **Lab Link:**  
[https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids) :contentReference[oaicite:0]{index=0}

## ⚙️ **Difficulty:** Apprentice  
📂 **Category:** Access control → Insecure Direct Object References / Horizontal privilege escalation with GUIDs :contentReference[oaicite:1]{index=1}

---

### 📚 **Description**

This lab demonstrates a horizontal privilege escalation vulnerability where user accounts are identified by unpredictable GUIDs instead of simple usernames or numeric IDs. The attacker must first discover the GUID for the user `carlos` from somewhere on the site (e.g. a blog post or profile link), then tamper with the `id` parameter in the “My account” request to the `carlos` GUID, to view Carlos’s account page (including his API key). :contentReference[oaicite:2]{index=2}

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

- The application uses GUIDs to represent user IDs, making them not obviously guessable.  
- Despite using unpredictable IDs, the application still trusts a client-supplied `id` parameter without enforcing authorization checks — allowing one user to view another user’s data.  
- Discovery of the other user’s GUID is the key step. Once known, account hijacking or data exfiltration becomes trivial. :contentReference[oaicite:3]{index=3}

#### ✅ Why it's dangerous

- GUIDs or other “unpredictable” IDs are **not sufficient** protection on their own.  
- Once an ID is discovered via public exposure elsewhere on the site, the authorization flaw is exploitable.  
- Attackers can escalate privileges (horizontal) or extract sensitive data (API keys) without password compromises.  

#### ✅ Where to look

- Blog posts, user lists, comments, or user profile links that include user IDs.  
- Any endpoint or request where an `id` parameter or similar identifier influences which user data is returned.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

- A public blog post or something similar has a link to `carlos`’ profile or posts, and that link contains a GUID for `carlos`. The attacker notes this. :contentReference[oaicite:4]{index=4}  
- The attacker logs in as `wiener:peter`, navigates to their own “My account” page, which uses an `id` parameter. In Burp Repeater or browser, they replace that parameter with `carlos`’ GUID. :contentReference[oaicite:5]{index=5}  
- The server returns Carlos’s account page, including his API key because it fails to verify that the logged-in user matches the target `id`. The attacker submits the API key as the lab’s solution. :contentReference[oaicite:6]{index=6}

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Explore public content (e.g., blog posts) until finding a link or post by “carlos” that reveals his GUID in the URL.  
2. Log in using the attacker’s credentials (`wiener:peter`). :contentReference[oaicite:7]{index=7}  
3. View your own “My account” page; capture the request (with the `id=<your GUID>`) in your proxy.  
4. Change the `id` parameter in the request to Carlos’s GUID that you discovered.  
5. Send the modified request; the response reveals Carlos’s API key in the page HTML.  
6. Submit that API key as the solution.  

---

### 📸 Screenshots 
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-6/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-6/images/2.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-6/images/3.png)

---

### 📝 What I Learned

✔ Unpredictable GUIDs improve security marginally, but don’t replace enforced authorization.                        
✔ Always verify that data returned by endpoints corresponds to the authenticated user, not just a client-supplied identifier.              
✔ Public-facing content often leaks identifiers — profile links, authorship pages, or comment systems can expose GUIDs.                 
✔ Authorization logic must be robust regardless of identifier format (numeric, GUID, random token etc.).               

---

### 🔐 Mitigation Techniques

✔ Verify on the server side that the authenticated user has permission to view or modify resources for the given ID — do not solely rely on “ID in the URL”.                             
✔ Use session-based or token-based access controls where the server ignores client-supplied ID parameters for sensitive operations.                              
✔ Conceal or avoid exposing GUIDs in public content unless necessary; if exposed, ensure those endpoints are read-only and don’t leak sensitive data.                        
✔ Use indirect references or token-based access where possible (e.g., “my-account” rather than “account?id=GUID”).                                 

---

### 🧾 Notes

- Lab-provided credentials: `wiener:peter`. Victim: `carlos`. :contentReference[oaicite:8]{index=8}  
- The solution hinges on finding the GUID, then tampering with the `id` parameter. :contentReference[oaicite:9]{index=9}  
- Only perform such testing in labs or authorized environments.

---

### 👤 Author  

Harbeer-Singh  
