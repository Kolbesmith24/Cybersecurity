# Wireshark

**Wireshark** is a powerful network protocol analyzer used for detecting and troubleshooting network problems, identifying security anomalies, and analyzing detailed protocol data. It captures network packets in real-time and provides comprehensive information for debugging and learning about network traffic.

---

## General Overview

Wireshark is widely used by network administrators, security professionals, and penetration testers to capture, inspect, and analyze network traffic. Its ability to decode numerous protocols and visualize packet-level data makes it an invaluable tool for network diagnostics and security investigations.

### Key Use Cases:
1. **Detecting and Troubleshooting Network Problems**: By inspecting captured packets, you can identify network congestion, latency, packet loss, and other issues.
2. **Detecting Security Anomalies**: Wireshark helps in detecting suspicious network traffic, such as unauthorized access attempts or malicious payloads.
3. **Investigating Protocol Details**: It provides deep insights into how various protocols work, making it ideal for learning about network communication.

---

## Merging PCAP Files

Wireshark allows you to merge multiple packet capture (PCAP) files into one file. This is useful when you have multiple captures from different sources or timeframes and want to analyze them together.

### Steps to Merge PCAP Files:
1. Click **File** in the top menu.
2. Select **Merge** from the dropdown menu.
  
This opens a dialog that allows you to select multiple capture files to merge into a single PCAP file for further analysis.

---

## Viewing File Details

To view detailed information about the capture file, such as capture properties, statistics, and more, follow these steps:

### Steps to View Capture File Details:
1. Click on **Statistics** in the top menu.
2. Select **Capture File Properties** from the dropdown menu.

Alternatively, you can click the **button on the bottom left** (with the paper and pen icon) to view the capture file details.

---

## Packet Details

When you click on a specific packet in Wireshark, you can view detailed information about the packet in the **Packet Details Pane**, located in the lower section of the interface. This section is divided into several layers corresponding to the OSI model.

### Breakdown of Packet Layers:

- **Frame (Layer 1)**: Displays details specific to the **Physical Layer** of the OSI model, such as frame number, timestamp, and frame size.
  
- **Source MAC Address (Layer 2)**: Shows the source and destination MAC addresses, corresponding to the **Data Link Layer**.
  
- **Source IP Address (Layer 3)**: Displays the source and destination IPv4 addresses, which are part of the **Network Layer**.
  
- **Protocol (Layer 4)**: Provides details about the transport protocol being used (TCP or UDP) along with source and destination ports, corresponding to the **Transport Layer**.
  
- **Protocol Errors**: Displays any protocol errors related to reassembled segments, particularly TCP.
  
- **Application Protocol (Layer 5)**: This layer shows details of the application protocol used (e.g., HTTP, FTP, SMB), corresponding to the **Application Layer**.
  
- **Application Data**: Displays the data specific to the application protocol. This can include things like HTTP request bodies or FTP commands.

---

## Finding Packets by Content

Wireshark allows you to search for specific content within captured packets. This is useful when you're looking for particular events, errors, or data within the packet capture.

### Steps to Find Packets by Content:
1. Click on **Edit** in the top menu.
2. Select **Find Packet** from the dropdown menu.
  
You can then enter specific keywords, IP addresses, or other criteria to search for packets that match your query.

---

## Exporting Objects (Files)

Wireshark can export files that were transferred over the network and captured in the packet data. This feature is helpful when you want to extract files, such as images, executables, or documents, that were sent over the network.

### Steps to Export Objects:
1. Click on **File** in the top menu.
2. Select **Export Objects** from the dropdown menu.
  
This allows you to choose specific objects (files) from the capture to export.

---

## Expert Information

Wireshark includes an **Expert Information** window that provides a summary of important events, anomalies, and other insights from the capture. This is especially useful for identifying network issues or security threats.

### Steps to Access Expert Information:
- Click the **circle icon on the bottom left** to open the Expert Information menu, which lists important findings and anomalies in the capture.

Additionally, you can **right-click** on a packet (such as a JPEG file) in the packet details pane and select **Export Packet Bytes** to extract the raw packet data.

---

## Filtering Packets

Wireshark offers powerful filtering capabilities to help you isolate specific packets, conversations, or protocols of interest. You can apply filters by using right-click options on packets or using the filter toolbar.

### Right-click Filtering Options:
- **Apply as Filter**: Filters packets based on the selected packet's source IP address.
  
- **Conversation Filter**: Filters only related packets for the selected packet's conversation (helpful for analyzing communication between specific hosts).
  
- **Follow TCP Stream**: Displays the raw TCP traffic stream, allowing you to see unencrypted data (e.g., HTTP or FTP traffic).
  - **Red text**: Data originating from the client.
  - **Blue text**: Data originating from the server.

To further narrow down your analysis, you can right-click a section in the packet details pane and choose **Apply as Filter** to apply that specific filter to the packet capture.

---
