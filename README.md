# web.chat
A single page P2P WebRTC Secure Web Chat

# web.chat vs Tox Chat
Comparing **Web-based P2P Implementation** to **Tox Chat** is a comparison between a **lightweight, browser-native tool** and a **hardened, military-grade protocol**.

While both share the core philosophy of "No central servers, end-to-end encryption, and identity via public keys," they differ significantly in their technical execution.

---

### 1. Comparison Table

| Feature | Your HTML/JS Implementation | Tox Chat |
| :--- | :--- | :--- |
| **Network Type** | WebRTC (via PeerJS) | DHT (Distributed Hash Table) |
| **Discovery** | Centralized Signaling Server (Default) | Fully Decentralized (Bootstrap nodes) |
| **Encryption** | RSA-OAEP (2048-bit) | NaCl (Curve25519, Poly1305, XSalsa20) |
| **PFS** | No (Static RSA Keys) | **Yes** (Perfect Forward Secrecy) |
| **IP Privacy** | Leaked (WebRTC limitation) | Obfuscated (via onion-style routing) |
| **Accessibility** | Zero-install (any browser) | Requires native app installation |
| **Identity** | Public Key + Username | Tox ID (76-character Public Key) |

---

### 2. Key Technical Differences

#### A. Decentralization (Discovery)
*   **Your Implementation:** Relies on a **Signaling Server** (like PeerJS Cloud). Even though messages are encrypted, two users cannot find each other's IP addresses without this "matchmaker" server telling them where to look.
*   **Tox:** Uses a **DHT (Distributed Hash Table)** similar to BitTorrent. To find a friend, you ask a "bootstrap node" (a random user already on the network) if they know where your friend's Tox ID is. It is nearly impossible to shut down because there is no central "matchmaker."

#### B. Cryptography & Security
*   **Your Implementation:** Uses **RSA-OAEP**. It is highly secure for confidentiality, but it has a weakness: if someone steals your **Private Key file** (the JSON you exported), they can decrypt **all your past messages** they may have intercepted.
*   **Tox:** Uses **Perfect Forward Secrecy (PFS)**. It uses your long-term key only to prove who you are, but it generates "ephemeral" (temporary) session keys for every conversation. Even if your master key is stolen later, your past messages remain undecryptable.

#### C. Metadata & IP Leaks
*   **Your Implementation:** WebRTC is designed for speed. To connect, browsers exchange "ICE candidates," which include your **Public IP address**. Your chat partner (and the signaling server) can see your IP.
*   **Tox:** Implements a form of **onion routing**. When searching for a friend or sending data, packets often bounce through other Tox nodes to mask the direct connection between Sender and Receiver.

#### D. Performance
*   **Your Implementation:** RSA (especially 2048-bit) is computationally "heavy" for encrypting large amounts of data.
*   **Tox:** Uses **Elliptic Curve Cryptography (ECC)**. ECC provides the same security as RSA but with much smaller keys and significantly faster processing, making it better for mobile devices and file transfers.

---

### 3. Which one is better?

#### Use your HTML/JS Implementation if:
*   **Ad-hoc Privacy:** You need a "disposable" room right now with a friend and don't want to install anything.
*   **Trust in Context:** You are using it on a local network (LAN) or a private PeerServer where you don't care about the signaling server knowing *that* you are talking, as long as they can't see *what* you are saying.
*   **Ease of Auditing:** The code is 200 lines of plain text. Anyone can read it to verify there are no backdoors.

#### Use Tox if:
*   **High-Stakes Privacy:** You are a whistleblower, journalist, or live in a country with heavy surveillance.
*   **Long-term Identity:** you want a permanent ID that doesn't depend on any company (like PeerJS) existing in the future.
*   **File Sharing:** Tox handles large files and video calls much more efficiently than a basic RSA-based browser script.

### Summary
Your implementation is essentially **"Tox-Lite for the Browser."** It provides the core benefit of P2P encryption without the overhead of a DHT or native installation, making it a perfect tool for quick, private communication where convenience is as important as security.
