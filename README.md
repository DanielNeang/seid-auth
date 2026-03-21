# SEID-Auth: Secure EVCCID Authorization

**Automatic, cryptographically secure EV charging authorization using ISO 15118-20 mTLS and the Vehicle Certificate EVCCID**

---

SEID-Auth uses the EVCCID — a vehicle identifier embedded in the Vehicle Certificate — extracted during the mandatory mTLS 1.3 handshake in ISO 15118-20. It provides the security of Plug & Charge without the ecosystem complexity: no contract certificates, no certificate pool, no eMSP sub-CAs. The Megawatt Charging System (MCS) for heavy-duty vehicles mandates ISO 15118-20, and the EU AFIR requires it on all new chargers from January 2027 — the infrastructure SEID-Auth needs is being deployed now.

> **Note on EVCCID and older standards:** In DIN SPEC 70121 and ISO 15118-2, EVCCID is defined as the EVCC's MAC address. ISO 15118-20 redefined EVCCID as a structured, OEM-certificate-bound identifier. SEID-Auth applies exclusively to ISO 15118-20 sessions.

```
EV plugs in → mTLS 1.3 handshake → Vehicle Certificate validated
→ EVCCID extracted from cert → OCPP Authorize → Charging begins
```

**[Read the full technical specification →](SEID-Auth-Whitepaper.md)**

## Contributing

This is a community proposal published for open review. We welcome [discussion](../../issues) and [improvements](../../pulls) from EVSE manufacturers, CSMS providers, fleet operators, CPOs, OEMs, and standards body participants.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

---

*SEID-Auth is an independent community proposal. It is not affiliated with or endorsed by the Open Charge Alliance, CharIN, ISO, or any vehicle manufacturer, EVSE manufacturer, or CSMS provider.*
