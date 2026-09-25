# 11 kV Feeder Protection Coordination and Reactive Power Compensation Study Using CYME

## Project Overview

This project presents an **11 kV radial distribution feeder study developed in CYME / CYMDIST**. The project combines:

- Load-flow and feeder modeling
- Short-circuit analysis
- Phase overcurrent protection using **50/51 relays**
- Current-transformer selection
- Time-current characteristic (TCC) coordination
- Sequence-of-operations verification
- Shunt capacitor modeling for reactive power compensation and voltage support

The main protection objective was to coordinate an upstream and downstream feeder breaker so that faults on the downstream feeder are cleared selectively by the nearest protective device, while the upstream breaker remains available as backup protection.

A shunt capacitor bank was also included in the feeder model to represent reactive power compensation and its influence on feeder voltage and reactive power flow.

---

## Software

- **CYME / CYMDIST**
- CYME Version 7.1

---

## System Description

The modeled system is an **11 kV radial distribution feeder** supplied from a utility source.

The model includes:

- 11 kV utility source
- Radial overhead feeder sections
- Distribution loads
- 11 kV feeder breakers
- Current transformers
- Electronic phase overcurrent relays
- Shunt capacitor bank
- Protective device coordination model

---

## Shunt Capacitor Modeling

A shunt capacitor bank was connected on the downstream feeder to represent **reactive power compensation** and feeder-voltage support.

The capacitor was reviewed and adjusted during the modeling process to obtain a practical representation of the feeder's reactive power compensation.

Key points considered included:

- Capacitor connection location
- Reactive power contribution
- Effect on feeder voltage and reactive power flow
- Rated-voltage compatibility with the 11 kV network
- CYME validation warnings related to capacitor voltage rating

The capacitor bank was approximately:

**600 kVAr**

The model also highlighted the importance of checking equipment rated voltage against the network operating voltage. CYME generated a warning when the capacitor equipment rating was entered as **12 kV** while connected to the **11 kV** system.

The capacitor was retained as part of the final network model because it provides a more realistic feeder representation, but the project does **not** claim to be a full capacitor-sizing or optimization study.

---

## Protection Philosophy

Two phase overcurrent relays were coordinated.

### Upstream Feeder Relay — SEC_11KV_01

- Protection function: **50/51**
- Relay type: Electronic
- Manufacturer: MULTILIN
- Model: **MULTILIN 350 IEC SI**
- Characteristic: **IEC Short Inverse**
- CT ratio: **600/5 A**
- Primary pickup: **550 A**
- Time Dial: **0.40**
- Breaker rating: **630 A, 12 kV, 16 kA**

### Downstream Feeder Relay — SEC_11KV_02

- Protection function: **50/51**
- Relay type: Electronic
- Manufacturer: MULTILIN
- Model: **MULTILIN 350 IEC SI**
- Characteristic: **IEC Short Inverse**
- CT ratio: **600/5 A**
- Primary pickup: **550 A**
- Time Dial: **0.05**
- Breaker rating: **630 A, 12 kV, 16 kA**

The downstream relay was intentionally configured to operate faster than the upstream relay for faults within the downstream feeder zone.

---

## CT and Pickup Selection

CYME calculated a feeder full-load current of approximately:

**452.6 A**

The original **200/5 A CT** was too small for the calculated feeder current.

The final phase CT ratio was therefore selected as:

**600/5 A**

The phase-overcurrent pickup was selected as:

**550 A**

This corresponds to approximately:

**550 / 452.6 = 1.22 pu**

of feeder full-load current.

This keeps the pickup above normal feeder loading while retaining sufficient sensitivity to the calculated fault currents.

---

## Short-Circuit Results

Representative short-circuit currents calculated for the downstream feeder were:

| Fault Type | Fault Current |
|---|---:|
| LLL | 4574.7 A |
| LLG | 4231.4 A |
| LL | 3961.8 A |
| LG | 3060.6 A |

The upstream three-phase fault current used for verification was approximately:

**8741.2 A**

---

## Sequence of Operations Verification

The protection settings were validated using CYME's **Sequence of Operations** analysis.

### Downstream Feeder Fault Results

| Fault Type | Fault Current | Operating Breaker | Trip Time |
|---|---:|---|---:|
| LLL | 4574.7 A | SEC_11KV_02 | 0.078 s |
| LLG | 4231.4 A | SEC_11KV_02 | 0.079 s |
| LL | 3961.8 A | SEC_11KV_02 | 0.080 s |
| LG | 3060.6 A | SEC_11KV_02 | 0.085 s |

For all tested downstream faults, **SEC_11KV_02 operated while SEC_11KV_01 remained closed**, confirming selective phase-overcurrent coordination for the tested cases.

### Upstream Fault Verification

For an upstream fault of approximately:

**8741.2 A**

breaker **SEC_11KV_01** operated at approximately:

**0.221 s**

This confirmed correct upstream protection operation while maintaining downstream selectivity.

---

## TCC Coordination

A combined TCC was generated using CYME's **Branch Device Coordination** function.

The final TCC demonstrated:

- **SEC_11KV_02** as the faster downstream protective device
- **SEC_11KV_01** as the slower upstream backup device
- Clear time separation between the two IEC Short Inverse characteristics
- Proper selective coordination for the tested feeder faults

### Final Relay Settings

| Relay | Pickup | CT Ratio | Time Dial |
|---|---:|---:|---:|
| SEC_11KV_01 | 550 A | 600/5 A | 0.40 |
| SEC_11KV_02 | 550 A | 600/5 A | 0.05 |

---

## Reclosing Configuration

For the coordination study, reclosing was effectively disabled by configuring the breaker sequence for:

**One operation to lockout**

This prevented repeated open-close cycles during persistent fault simulations and allowed the relay selectivity to be evaluated clearly.

---

## Engineering Conclusion

The CYME study demonstrated successful selective coordination between the upstream and downstream 11 kV feeder breakers.

The original **200/5 A phase CT ratio** was unsuitable for the calculated feeder full-load current of approximately **452.6 A**. The phase CT ratio was therefore revised to **600/5 A**, providing a more appropriate current-transformation range for normal load and fault conditions.

A phase-overcurrent pickup of **550 A** was selected, remaining above normal feeder current while preserving sufficient fault sensitivity.

The downstream relay **SEC_11KV_02** was configured with an IEC Short Inverse characteristic and a time dial of **0.05**, while the upstream relay **SEC_11KV_01** used the same characteristic with a time dial of **0.40**.

Sequence-of-operations studies confirmed that the downstream breaker cleared **LLL, LLG, LL, and LG faults** without unnecessary operation of the upstream breaker. An upstream fault was also correctly cleared by **SEC_11KV_01**.

The combined TCC verified the intended grading relationship between the two protective devices.

The model also included a **shunt capacitor bank of approximately 600 kVAr** to represent reactive power compensation and feeder-voltage support. The capacitor setup was reviewed for location, reactive contribution, and equipment voltage-rating compatibility with the 11 kV network.

Overall, the project demonstrates practical CYME experience in:

- Distribution feeder modeling
- Load-flow interpretation
- Short-circuit analysis
- CT selection
- 50/51 relay configuration
- TCC coordination
- Sequence-of-operations verification
- Shunt capacitor modeling
- Reactive power compensation
- Protection-study troubleshooting

---

## Recommended Repository Structure

```text
CYME-11kV-Feeder-Protection-Coordination/
│
├── README.md
│
├── CYME_Project/
│   └── 11KV_Feeder_Protection_Coordination.xst
│
├── Images/
│   ├── 01_One_Line_Diagram.png
│   ├── 02_Load_Flow_Result.png
│   ├── 03_Shunt_Capacitor_Setup.png
│   ├── 04_Upstream_Relay_Settings.png
│   ├── 05_Downstream_Relay_Settings.png
│   ├── 06_CT_Settings_600_5.png
│   ├── 07_Combined_TCC.png
│   ├── 08_LLL_Sequence.png
│   ├── 09_LLG_Sequence.png
│   ├── 10_LL_Sequence.png
│   ├── 11_LG_Sequence.png
│   └── 12_Upstream_Fault_Sequence.png
│
├── Results/
│   ├── Short_Circuit_Results.pdf
│   └── Protection_Coordination_Summary.pdf
│
└── Documentation/
    └── Engineering_Notes.md
```

---

## Recommended Screenshots

For a concise but professional portfolio presentation, include:

1. **Overall one-line diagram**
2. **Load-flow result**
3. **Shunt capacitor setup / capacitor location**
4. **Upstream relay settings**
5. **Downstream relay settings**
6. **600/5 A CT settings**
7. **Combined TCC plot**
8. **LLL Sequence of Operations**
9. **LLG Sequence of Operations**
10. **LL Sequence of Operations**
11. **LG Sequence of Operations**
12. **Upstream fault Sequence of Operations**

The **combined TCC**, **one-line diagram**, and **Sequence of Operations** screenshots should be treated as the most important portfolio images.

---

## Key Skills Demonstrated

- CYME / CYMDIST
- 11 kV distribution feeder modeling
- Load-flow analysis
- Short-circuit analysis
- Current-transformer selection
- 50/51 overcurrent protection
- IEC inverse-time relay characteristics
- TCC coordination
- Selectivity and backup protection
- Sequence-of-operations analysis
- Shunt capacitor modeling
- Reactive power compensation
- Distribution voltage support
- Protection troubleshooting

---

## Disclaimer

This project was developed as an engineering training and portfolio exercise.

The protection and capacitor settings shown are specific to the modeled study system and should not be applied directly to a real power system without a complete engineering study, equipment verification, utility requirements, applicable standards, and protection philosophy review.
