# **ZAP Fuzzer Overview**

ZAP Fuzzer is a powerful built-in tool for fuzzing endpoints. Unlike Burp’s free Intruder, it doesn’t throttle speed and includes built-in wordlists, making it highly effective for quick attacks.

---

## **Steps to Fuzz with ZAP**
#### 1. **Capture Request**
- Visit target URL (e.g., `http://SERVER_IP:PORT/test/`) in a browser proxied through ZAP.
- In ZAP’s History tab, right-click the request → `Attack > Fuzz`.
#### 2. **Set Fuzz Location**
- In the request, highlight the part to fuzz (e.g., `test`) and click **Add**. This sets the payload insertion point.
#### 3. **Choose Payloads**
- Click **Add** → choose payload type:
    - **File** or **File Fuzzers** to load wordlists (e.g., from DirBuster).
    - Built-in lists are available; more can be added via ZAP Marketplace.
#### 4. **Apply Processors** _(Optional but Useful)_
- Add transformations to each payload:
    - **MD5**, **Base64**, **URL Encode**, **Prefix/Postfix**, etc.
    - Use **Generate Preview** to see final payloads.
    - Example: Apply **URL Encode** or **MD5 Hash** if needed.
#### 5. **Set Options**
- Configure threads (e.g., 20 for faster fuzzing).
- Choose **Depth-first** or **Breadth-first** processing:
    - Depth: all payloads for one position.
    - Breadth: one payload across all positions.
#### 6. **Run & Analyze**
- Click **Start Fuzzer**.
- Sort results by **Status Code**, **Response Length**, or **RTT**.
- Look for anomalies like:
    - `200 OK` responses
    - Unique content length
    - Delayed response time (useful in timing attacks)

---
### **Tips**
- Use MD5 processor if fuzzing an `auth` cookie hashed from usernames.
- Monitor response body/length to spot successful payloads.