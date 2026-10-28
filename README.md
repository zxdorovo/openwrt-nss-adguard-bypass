# Smart Routing Bypass for Qualcomm NSS on OpenWrt

This project provides a structural architecture and configuration guide for intercepting hardware-accelerated network traffic on **OpenWrt builds with Qualcomm NSS (Network Subsystem)**. 

It specifically solves the conflict where hardware token-forwarding (SFE/NSS) bypasses the standard Linux netfilter stack, preventing local services like **AdGuard VPN (CLI)** and **AdGuard Home** from filtering encrypted traffic.

## Network Topology & Data Flow
1. **Client Device** ➡️ Sends HTTPS/DNS requests.
2. **Qualcomm NSS Engine** ➡️ Attempts to offload packets at the hardware layer.
3. **Passwall 2 & Sing-Box (TPROXY)** ➡️ Intercepts specific ports (53, 80, 443) before the hardware layer, bypassing NSS routing loops.
4. **Local Proxy Loopback (127.0.0.1)** ➡️ Routes traffic into the AdGuard VPN CLI client.
5. **AdGuard CA Certificate** ➡️ Validates and filters TLS/HTTPS connections locally.

## Current QA & Testing Status
- [x] Base OpenWrt deployment with custom hardware cooling.
- [x] Sing-Box core configuration.
- [x] Passwall 2 firewall table ruleset injection.
- [ ] AdGuard VPN CLI validation (License Pending).
