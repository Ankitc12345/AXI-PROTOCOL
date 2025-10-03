# AXI Master-Slave Implementation in Verilog

This project contains Verilog implementations of an **AXI Master** and an **AXI Slave**, along with their individual testbenches and a combined testbench to verify proper communication between them.

---

# 📂AXI (Advanced eXtensible Interface) Documentation




AXI (Advanced eXtensible Interface) is part of the AMBA protocol family defined by ARM, widely used in SoC designs. It provides **high-performance, high-bandwidth, and low-latency** communication between master and slave components.

---

## 1. FEATURES
- AXI is used to communicate between **Master** and **Slave**.
- It has **five independent channels**:
  - Write Address
  - Write Data
  - Write Response
  - Read Address
  - Read Data
- Decoupled handshakes (VALID/READY) → independent flow control.
- Out-of-order transactions supported with IDs.
- Burst-based transfers (multiple beats per address phase).

👉 **Note:** Transfer of data occurs only when **VALID (from master)** and **READY (from slave)** are both asserted at the same clock signal.

---

## 2. AXI Channels Overview

| Channel | Direction | Purpose |
|---------|-----------|---------|
| **AW** (Write Address) | Master → Slave | Carry write address + control signals |
| **W** (Write Data) | Master → Slave | Carry write data + last beat |
| **B** (Write Response) | Slave → Master | Carry write response |
| **AR** (Read Address) | Master → Slave | Carry read address + control signals |
| **R** (Read Data) | Slave → Master | Carry read data + response |

---

## 3. AXI Write Transaction

A write consists of **3 phases**:

1. **Write Address (AW)**
   - Master provides address, id, burst, burst length, burst size, etc.
   - Handshake: `AWVALID + AWREADY`.

2. **Write Data (W)**
   - Master sends data, with `WLAST = 1` on final beat.
   - Handshake: `WVALID + WREADY`.

3. **Write Response (B)**
   - Slave responds once write completes.
   - Response: `BRESP` with ID.
   - Handshake: `BVALID + BREADY`.

---

## 4. AXI Read Transaction

A read consists of **2 phases**:

1. **Read Address (AR)**
   - Master provides address, id, burst, burst len, burst size, etc.
   - Handshake: `ARVALID + ARREADY`.

2. **Read Data (R)**
   - Slave responds with one or more beats.
   - Last beat signaled by `RLAST=1`.
   - Response: `RRESP`.
   - Handshake: `RVALID + RREADY`.

---

## 5. Signal List

### 5.1 Write Address Channel (AW)
- `AWID` → Transaction ID  
- `AWADDR` → Start address  
- `AWLEN` → Burst length  
- `AWSIZE` → Bytes per beat  
- `AWBURST` → Burst type (FIXED, INCR, WRAP)  
- `AWLOCK` → Atomic/exclusive ops  
- `AWCACHE` → Cache control  
- `AWPROT` → Privileged, secure, instruction/data  
- `AWQOS` → Quality of service  
- `AWREGION` → Region identifier  
- `AWUSER` → User-defined  
- Handshake: `AWVALID/AWREADY`  

### 5.2 Write Data Channel (W)
- `WDATA` → Write data  
- `WSTRB` → Byte lane valid mask  
- `WLAST` → Last beat in burst  
- `WUSER` → User-defined  
- Handshake: `WVALID/WREADY`  

### 5.3 Write Response Channel (B)
- `BID` → Transaction ID  
- `BRESP` → Response (OKAY=00, EXOKAY=01, SLVERR=10, DECERR=11)  
- `BUSER` → User-defined  
- Handshake: `BVALID/BREADY`  

### 5.4 Read Address Channel (AR)
- `ARID` → Transaction ID  
- `ARADDR` → Start address  
- `ARLEN` → Burst length  
- `ARSIZE` → Bytes per beat (log2)  
- `ARBURST` → Burst type  
- `ARLOCK` → Atomic/exclusive ops  
- `ARCACHE` → Cache control  
- `ARPROT` → Privileged, secure, instruction/data  
- `ARQOS` → Quality of Service  
- `ARREGION` → Region identifier  
- `ARUSER` → User-defined  
- Handshake: `ARVALID/ARREADY`  

### 5.5 Read Data Channel (R)
- `RID` → Transaction ID  
- `RDATA` → Read data  
- `RRESP` → Response code  
- `RLAST` → Last beat  
- `RUSER` → User-defined  
- Handshake: `RVALID/RREADY`  

---

## 6. Burst Types
- **FIXED (00)** → All beats at same address  
- **INCR (01)** → Incrementing address  
- **WRAP (10)** → Increment but wrap around aligned boundary  

---

## 7. Burst Size
Data size per beat (bytes):

| Code | Bytes |
|------|-------|
| 000  | 1     |
| 001  | 2     |
| 010  | 4     |
| 011  | 8     |
| 100  | 16    |
| 101  | 32    |
| 110  | 64    |
| 111  | 128   |

---

## 8. Burst Length
- Number of beats in burst.  

---

## 9. Responses
- **OKAY (00)** → Successful transfer  
- **EXOKAY (01)** → Exclusive access OK  
- **SLVERR (10)** → Slave error  
- **DECERR (11)** → Decode error (no slave)  

---

## 10. Optional Features
- **AxLOCK** → Atomic/exclusive sequences  
- **AxCACHE** → Cache hints  
- **AxPROT** → Security & privilege attributes  
- **AxQOS** → Arbitration hints  
- **AxREGION** → Region info  
- **AxUSER** → User-defined extensions  

---
