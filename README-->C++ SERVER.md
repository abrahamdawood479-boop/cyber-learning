# 🚀 Low-Level C++ Network Server & Memory Matrix

## 📋 Project Overview
This project is a low-level network server built natively in C++ that models the core mechanics of the TCP/IP protocol suite. The architecture establishes a dedicated IPv4 listener socket, coordinates a synchronous TCP 3-way handshake (`SYN` -> `SYN-ACK` -> `ACK`), interfaces directly with the OS kernel to manage hardware RAM blocks, parses incoming client streams, and logs payloads dynamically into a matrix storage database before transmitting a transaction verification echo.

---

## 🧠 Core Engineering Breakthroughs & Theory Learned
Along this development sprint, I explored how software logic tightly integrates with physical computer architecture. I solidified my grasp on variable address pointers, anonymous structures, execution scopes, and fixed-dimensional C-style memory grids.

### 🛡️ Unsigned Integer Wrap-Arounds (`size_t` vs. `ssize_t`)
A major milestone was breaking down the structural risks when handling network return metrics:
* **`size_t` (Unsigned):** Used strictly for measuring data allocations (like string lengths). It cannot evaluate negative numbers.
* **`ssize_t` (Signed):** Track bytes successfully transmitted across socket boundaries, retaining the critical capability to return a `-1` system failure flag.

**The Exploit Mechanic:** If an architectural design mistake forces a network error code (`-1`) into an unsigned `size_t` container, the bit array slips out of bounds. The system misinterprets that `-1` as the maximum possible positive value. This forces an integer overflow, triggering a massive, volatile allocation flag that can blow past buffer safety fences and corrupt adjacent data slots inside the RAM blast radius.

---

## 🧮 Database Indexing & Mathematical Formula
To avoid slow, resource-heavy sorting loops, this system utilizes pure integer calculations to instantly map sequential byte streams into precise grid slots:

```cpp
int row = db.total_writes / 5; // Integer division targets the vertical row
int col = db.total_writes % 5; // Modulo (remainder) targets the horizontal column
```

### Visual Representation of the 5x5 Matrix:
```text
         Col 0   Col 1   Col 2   Col 3   Col 4
Row 0:  [ 0 ]   [ 1 ]   [ 2 ]   [ 3 ]   [ 4 ]
Row 1:  [ 5 ]   [ 6 ]   [ 7 ]   [ 8 ]   [ 9 ]
Row 2:  [10 ]   [11 ]   [12 ]   [13 ]   [14 ]
Row 3:  [15 ]   [16 ]   [17 ]   [18 ]   [19 ]
Row 4:  [20 ]   [21 ]   [22 ]   [23 ]   [24 ]
```

### Trace Example (Mapping Packet 6):
If our global tracking counter hits `6`, the server evaluates the grid coordinates like this:
1. **Row Math:** `6 / 5 = 1.2` → C++ drops the decimal entirely, landing cleanly on **Row 1**.
2. **Column Math:** `6 % 5 = 1` → 5 divides into 6 exactly once, leaving a leftover remainder of **1**.
* **Final Location:** The payload maps directly to **Row 1, Column 1** (Slot 6 on the grid)!



TERMINAL DEBUG/LAUNCH CODE:
g++ -std=c++17 main.cpp -o production_server && ./production_server

nc 127.0.0.1 80 (use netcat (nc) to connect to server as if you were an external client)
