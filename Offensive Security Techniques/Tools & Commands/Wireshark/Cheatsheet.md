# Wireshark Reference Sheet

**Wireshark** – Network protocol analyzer for traffic capture, inspection, and troubleshooting.

---

## **Use Cases**

- **Network Troubleshooting**: Spot latency, drops, congestion.
    
- **Security Analysis**: Detect suspicious traffic or malicious payloads.
    
- **Protocol Investigation**: Understand how protocols function across OSI layers.
    

---

## **Merge PCAP Files**

**File** → **Merge**  
→ Combine multiple captures into one PCAP file.

---

## **View Capture Details**

**Statistics** → **Capture File Properties**  
_or_  
Click paper+pen icon (bottom left).

---

## **Packet Layers (Details Pane)**

- **Frame (L1)**: Metadata (time, size).
    
- **Ethernet (L2)**: MAC addresses.
    
- **IP (L3)**: Source/Destination IP.
    
- **TCP/UDP (L4)**: Ports, protocol info.
    
- **Errors**: Reassembly or protocol issues.
    
- **Application (L5)**: Protocol (HTTP, FTP, etc).
    
- **App Data**: Payload (e.g., HTTP body).
    

---

## **Find Packets**

**Edit** → **Find Packet**  
→ Search by string, hex, address, etc.

---

## **Export Files**

**File** → **Export Objects**  
→ Extract files (HTTP, SMB, etc).

**Right-click packet** → **Export Packet Bytes**  
→ Save raw data (e.g., JPEG, exe).

---

## **Expert Info**

Click **circle icon (bottom left)**  
→ View anomalies, warnings, notes.

---

## **Filtering Packets**

**Right-click options:**

- **Apply as Filter** → Source/Dest IP.
    
- **Conversation Filter** → Specific host session.
    
- **Follow TCP Stream** → Reconstruct raw TCP data.
    
    - _Red_: client → server
        
    - _Blue_: server → client
        

**Tip**: Use filter bar or right-click in Packet Details pane to create precise filters.
