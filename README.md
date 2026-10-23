# Relay · Packet-Switched WAN Simulator

[![Live Demo](https://img.shields.io/badge/Live_Demo-Vercel-black?logo=vercel)](https://focused-rutherford.vercel.app)
[![Zero Dependencies](https://img.shields.io/badge/dependencies-0-brightgreen.svg)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**Relay** is an interactive, browser-based wide-area network (WAN) and packet-switching simulator. Watch high-level messages and media attachments split into discrete parcels, route autonomously across network links using queue-aware Dijkstra shortest path calculations, navigate dynamic link failures and congestion, and reassemble in real-time at the destination.

---

## ✦ Key Features

### 1. 3D Isometric & 2D Planar Network Engine
- **3D Canvas Engine**: Pure vanilla Canvas 2D isometric rendering with real-time depth sorting, 3D coordinate projection, packet elevation arcs, packet trails, motes, and particle shatter physics.
- **2D Planar Mode**: Instant toggle (`2` / `3` keys or UI button) to view an orthogonal top-down schematic map.
- **Raymarched Hero Shader**: WebGL fragment shader with refractive glass crystal, chromatic dispersion, and noise distortion reflecting live network packet activity.

### 2. Transport Layer Simulation: TCP Reno (AIMD) vs UDP
- **TCP Reno Congestion Control**:
  - **Slow Start**: Exponential window expansion ($\text{CWND} \leftarrow \text{CWND} + 1$ per ACK until $\text{ssthresh}$).
  - **Congestion Avoidance**: Additive Increase ($\text{CWND} \leftarrow \text{CWND} + \frac{1}{\lfloor\text{CWND}\rfloor}$ per ACK).
  - **Packet Loss / Multiplicative Decrease**: On buffer drop or timeout, $\text{ssthresh} \leftarrow \max(2, \lfloor\text{CWND} / 2\rfloor)$ and $\text{CWND} \leftarrow \max(1, \lfloor\text{CWND} / 2\rfloor)$.
  - **Sliding Sender Window**: Packets are metered and paced according to live window capacity.
  - **Live CWND Telemetry**: Real-time sparkline graph capturing the classic TCP sawtooth waveform.
- **UDP Datagram Mode**: Uncontrolled transmission; packets are dropped without acknowledgment or retransmission.

### 3. Queue-Aware Dijkstra Routing
Every router computes optimal next hops dynamically using link costs that account for physical propagation delay, transmission time, and queue backlog:
$$\text{cost}(l) = \text{prop}(l) + \frac{\text{packet size}}{\text{bandwidth}(l)} + \max(0, \text{busy}(l) - T) + \epsilon$$

Supported routing strategies:
- **Adaptive**: Real-time queue-aware avoidance of congested links.
- **Static**: Shortest physical path based on propagation delay and base capacity.
- **Randomized**: Jitter-infused path distribution.

### 4. Network Topologies
- **National Core Mesh**: 6 interconnected Indian metro switching centers (Delhi, Mumbai, Kolkata, Chennai, Bengaluru, Hyderabad).
- **Resilient Ring**: Benchmark loop topology demonstrating bidirectional rerouting during single fiber cuts.
- **Centralized Star Hub**: All metro nodes route strictly through a central Hyderabad core switch, highlighting choke points.

### 5. Deep Packet Inspection Ticket
Click any router, link, or parcel in flight to inspect:
- **Layer 3 (IPv4)**: Source/Destination IP addresses (`10.0.x.y`), TTL hop limits, ECN bits, Protocol ID (`0x06` TCP / `0x11` UDP).
- **Layer 4 (Transport)**: Virtual ports, sequence numbers, acknowledgment status, and flags (`[SYN]`, `[ACK]`, `[PSH, ACK]`).
- **Live Checksum & Payload**: Bitwise checksum verification (`0xXXXX`) and raw payload extraction.
- **Route Breadcrumbs**: Real-time hop trail (`DEL › HYD › CHE`).

### 6. Realistic Messaging & Media Streaming
- Interactive smartphone client (chatting as Asha, Ravi, Meera, Dev, Kiran, or Campus Group).
- **Media Attachments**:
  - **Images**: Sliced into dozens of packets; watch individual image tiles unveil progressively as packets arrive and reassemble.
  - **Voice Notes**: Simulated audio waveforms and playback.
  - **PDF Documents**: In-browser document viewer.
- **Two-Way ACK Protocol**: Message checkmarks transition in real time:
  $$\text{Pending} \longrightarrow \text{Sent} \longrightarrow \text{Delivered} \longrightarrow \text{Read}$$

### 7. Interactive Scenarios & Diagnostics
- **Guided Scenarios**:
  1. *One message, many parcels*
  2. *Everyone talks at once (Multiplexing)*
  3. *A link fails mid-transfer (Dynamic rerouting)*
  4. *Rush hour at the hub (Buffer overflows & drops)*
  5. *A noisy line (Retransmission timeouts)*
  6. *Parcels arrive out of order (Jitter & reordering)*
  7. *One to many (Multicast routing)*
  8. *Whole message or parcels (Store-and-forward vs packet switching)*
  9. *TCP Reno AIMD (Sawtooth congestion curve)*
  10. *Ring Topology Fiber Cut (Fault-tolerant loop routing)*
- **Diagnostics**:
  - Live sparkline vitals: Throughput, Delivery Latency, Packet Loss, Busiest Link, Congestion Window.
  - Full packet ledger table & event log.
  - CSV trace exporter (`relay-trace.csv`).
  - Comprehensive session report generator with p95 delivery statistics.
  - Map screenshot exporter (`.png`).
  - Command Palette (`Ctrl+K` / `Cmd+K`).

---

## ⌨ Keyboard Shortcuts

| Shortcut | Action |
|:---|:---|
| <kbd>Space</kbd> | Pause / Resume simulation |
| <kbd>.</kbd> or <kbd>&gt;</kbd> | Step forward 0.1s |
| <kbd>R</kbd> | Reset simulation |
| <kbd>2</kbd> / <kbd>3</kbd> | Toggle 2D Planar / 3D Isometric View |
| <kbd>T</kbd> | Cycle Network Topology (Mesh → Ring → Star) |
| <kbd>C</kbd> | Toggle Chaos Mode (random link cuts & heals) |
| <kbd>Ctrl</kbd> + <kbd>K</kbd> | Open Command Palette |
| <kbd>Esc</kbd> | Close Modals / Command Palette |

---

## 🚀 Deployment & Local Development

Relay is built with zero external dependencies and runs natively in any modern web browser.

### Local Preview
```bash
# Clone the repository
git clone https://github.com/lak-is-law/relay-wan.git
cd relay-wan

# Serve using any static HTTP server
python3 -m http.server 3000
# Open http://localhost:3000
```

### Deploy to Vercel
```bash
vercel deploy --prod
```

---

## 📄 License
MIT License. Created for computer networking exploration and interactive systems education.
