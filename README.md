# SEID-Auth: Secure EVCCID Authorization

**Automatic, cryptographically secure EV charging authorization using ISO 15118-20 mTLS and the Vehicle Certificate EVCCID**

---

## What is SEID-Auth?

SEID-Auth (Secure EVCCID Authorization) is a proposed method for automatic authorization of Electric Vehicles at charging stations. It uses the EVCCID — a vehicle identifier embedded in the Vehicle Certificate — extracted during the mandatory mTLS 1.3 handshake defined by ISO 15118-20.

### The Problem

| Method | Security | Complexity | PKI beyond base 15118-20 |
|---|---|---|---|
| **Autocharge** (MAC-based) | Low — MAC easily spoofed | Low | None |
| **Plug & Charge** (ISO 15118) | High — Full PKI with contract certs | High | MO sub-CAs, cert pool, contract certs |
| **SEID-Auth** (this proposal) | **High — mTLS + OEM PKI** | **Low–Medium** | **None** |

### How It Works

```
EV plugs in → mTLS 1.3 handshake → Vehicle Certificate validated
→ EVCCID extracted from cert → OCPP Authorize → Charging begins
```

The EV's identity is cryptographically proven during the TLS handshake. No MAC addresses. No contract certificates. No certificate pool (beyond what 15118-20 already requires). Just the Vehicle Certificate that ISO 15118-20 already mandates.

### Why Now?

The EU's AFIR regulation (Commission Delegated Regulation (EU) 2025/656) mandates that from **January 1, 2027**, all newly installed or renovated public and private chargers must support **ISO 15118-20**. Every new charger in Europe will have the mTLS infrastructure SEID-Auth needs — making this approach immediately deployable at scale.

## Documentation

- **[Whitepaper](SEID-Auth-Whitepaper.md)** — Full technical specification with protocol flows, security analysis, and implementation guidance

## Who Benefits?

**EVSE manufacturers** are the most critical actor — the SECC firmware extracts the EVCCID from the already-validated Vehicle Certificate and sends it via OCPP. Since they're already implementing ISO 15118-20 mTLS, the incremental effort is minimal.

**CSMS providers** need to support the new EVCCID IdToken type and enrollment flows. For those already supporting MAC-based Autocharge, the architecture is identical — just a better identifier.

**Fleet operators / private CPOs** are the primary early beneficiaries. Fleet owners who operate their own chargers (depot charging, logistics hubs, bus depots) get seamless plug-and-charge authorization without the complexity of Plug & Charge's multi-party ecosystem.

## Key Design Decisions

- **Identifier:** EVCCID from the Vehicle Certificate Subject CN (not MAC address, not public key hash)
- **Security:** mTLS 1.3 proof-of-possession — the EVCCID alone is not secret, but combined with mTLS it is spoof-proof
- **No additional PKI:** V2G Root CAs and OEM Root Certificates are already required by ISO 15118-20 for mTLS — SEID-Auth adds nothing on top
- **OCPP integration:** Proposes new `EVCCID` IdToken type for OCPP 2.0.1/2.1, with interim guidance using `Central` type
- **Enrollment:** Two models — First-See-and-Link (universal) and Fleet Pre-Registration (for private CPOs)
- **Certificate renewal:** EVCCID typically persists across renewals; re-enrollment flow for rare EVCCID changes

## Scope

SEID-Auth applies to **all ISO 15118-20 sessions**, including:
- CCS-equipped passenger EVs using ISO 15118-20
- Heavy-duty vehicles using Megawatt Charging System (MCS)
- Private fleet charging at depots and corporate facilities
- Any vehicle where ISO 15118-20 mTLS is the communication protocol

## Standards Alignment

| Standard | Relationship |
|---|---|
| **ISO 15118-20** | Uses existing mTLS mandate and EVCCID definition — no changes needed |
| **OCPP 2.0.1 / 2.1** | Proposes new IdToken type; compatible with existing Authorize flow |
| **EU AFIR (2025/656)** | ISO 15118-20 mandate from Jan 2027 ensures infrastructure readiness |
| **ISO 3780** | EVCCID includes WMI for manufacturer identification |

## Contributing

This is a community proposal published for open review. We welcome:
- **Discussion** via [GitHub Issues](../../issues)
- **Improvements** via [Pull Requests](../../pulls)
- **Feedback** from EVSE manufacturers, CSMS providers, fleet operators, CPOs, OEMs, and standards body participants

## License

This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

---

*SEID-Auth is an independent community proposal. It is not affiliated with or endorsed by the Open Charge Alliance, CharIN, ISO, or any vehicle manufacturer.*
