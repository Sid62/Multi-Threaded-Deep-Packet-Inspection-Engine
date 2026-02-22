🚀 Multi Threaded Deep Packet Inspection Engine

A high-performance Deep Packet Inspection (DPI) Engine built in C++ that analyzes network traffic from PCAP files, classifies applications (YouTube, Facebook, Google, etc.), and enforces customizable blocking rules using flow-based inspection and multi-threaded architecture.

This project demonstrates strong fundamentals in:

Computer Networks

Network Security

Systems Programming

Multi-threading & Concurrency

Flow Tracking

Protocol Parsing (Ethernet, IP, TCP, UDP, TLS)

High-performance C++ Development

🧠 What is Deep Packet Inspection (DPI)?
4

Deep Packet Inspection (DPI) is an advanced network filtering technique that examines the payload of packets, not just header metadata.

Unlike traditional firewalls that only check:

Source IP

Destination IP

Ports

DPI analyzes:

TLS Client Hello

Server Name Indication (SNI)

HTTP Host headers

Flow patterns

This enables:

Application detection

Domain-level blocking

Traffic classification

Security monitoring

📌 Project Highlights

✅ Parses raw PCAP files (Wireshark format)
✅ Extracts and processes Ethernet, IPv4, TCP, UDP headers
✅ Implements TLS SNI extraction (HTTPS inspection)
✅ Flow-based tracking using Five-Tuple
✅ Rule-based blocking (IP / App / Domain)
✅ Multi-threaded architecture with load balancing
✅ Thread-safe queues using mutex & condition variables
✅ Detailed traffic analytics and reporting

🏗️ System Architecture
1️⃣ Simple Version (Single-threaded)

File: main_working.cpp

Designed for learning & small traffic captures

2️⃣ Multi-threaded Version (Production-grade)
4

Architecture Flow:

Reader Thread
      ↓
Load Balancer Threads
      ↓
Fast Path Threads (DPI Processing)
      ↓
Output Writer Thread

Key Concepts Used:

Consistent Hashing (5-tuple based)

Producer-Consumer Pattern

Thread-safe Queues

Parallel Flow Processing

📦 Project Structure
packet_analyzer/
├── include/
│   ├── pcap_reader.h
│   ├── packet_parser.h
│   ├── sni_extractor.h
│   ├── rule_manager.h
│   ├── connection_tracker.h
│   ├── thread_safe_queue.h
│   └── dpi_engine.h
│
├── src/
│   ├── main_working.cpp
│   ├── dpi_mt.cpp
│   ├── pcap_reader.cpp
│   ├── packet_parser.cpp
│   ├── sni_extractor.cpp
│   └── types.cpp
│
├── generate_test_pcap.py
├── test_dpi.pcap
└── README.md
🔍 Core Technical Concepts Implemented
1️⃣ Five-Tuple Flow Identification

Each connection is uniquely identified using:

Source IP

Destination IP

Source Port

Destination Port

Protocol

All packets with the same 5-tuple are treated as one flow.

2️⃣ TLS SNI Extraction (HTTPS Inspection)

The system extracts domain names from:

TLS Client Hello

SNI Extension (Type 0x0000)

Example extracted domains:

www.youtube.com

www.facebook.com

github.com

This enables application-level classification even in encrypted HTTPS traffic.

3️⃣ Rule-Based Blocking Engine

Supports three rule types:

Rule Type	Example	Effect
IP	192.168.1.50	Blocks all traffic from source
App	YouTube	Blocks all YouTube connections
Domain	facebook	Blocks matching SNI substring

Flow-level blocking ensures:

Once a flow is identified as blocked, all future packets are dropped.

4️⃣ Multi-threading & Concurrency

std::thread

std::mutex

std::condition_variable

Thread-safe queue

Hash-based workload distribution

Ensures:

High throughput

Efficient CPU utilization

Scalable DPI processing

⚙️ Technologies Used

Language: C++17

Concepts: OOP, Multi-threading, Systems Programming

Networking: Ethernet, IPv4, TCP, UDP, TLS

Build Tool: g++

Testing: Custom PCAP generator (Python)

No external libraries used

🛠️ Build Instructions
Simple Version
g++ -std=c++17 -O2 -I include -o dpi_simple \
    src/main_working.cpp \
    src/pcap_reader.cpp \
    src/packet_parser.cpp \
    src/sni_extractor.cpp \
    src/types.cpp
Multi-threaded Version
g++ -std=c++17 -pthread -O2 -I include -o dpi_engine \
    src/dpi_mt.cpp \
    src/pcap_reader.cpp \
    src/packet_parser.cpp \
    src/sni_extractor.cpp \
    src/types.cpp
▶️ Running the Project

Basic Usage:

./dpi_engine input.pcap output.pcap

With Blocking Rules:

./dpi_engine input.pcap output.pcap \
    --block-app YouTube \
    --block-ip 192.168.1.50 \
    --block-domain facebook

Configure Threads:

./dpi_engine input.pcap output.pcap --lbs 4 --fps 4
📊 Sample Output
Total Packets: 77
Forwarded: 69
Dropped: 8

Application Breakdown:
HTTPS: 50.6%
YouTube: 5.2% (BLOCKED)
DNS: 5.2%
Facebook: 3.9%

Provides:

Traffic distribution

Thread performance stats

Application classification

Blocked traffic metrics

📈 Key Learning Outcomes

Through this project, I strengthened my understanding of:

Low-level network protocol parsing

TLS handshake analysis

Stateful flow tracking

High-performance concurrent system design

Load balancing strategies

Producer-consumer architecture

Secure traffic filtering mechanisms

🚀 Future Improvements

QUIC / HTTP3 support

Real-time traffic monitoring

Bandwidth throttling instead of drop

Web dashboard for analytics

Persistent rule storage

Intrusion detection extensions

🎯 Why This Project Matters

This project simulates real-world DPI systems used by:

ISPs

Enterprise firewalls

Network monitoring tools

Security gateways

Parental control systems

It demonstrates strong capability in:

System-level programming

Networking internals

Performance optimization

Concurrency management
