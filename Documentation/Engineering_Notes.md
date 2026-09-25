# Engineering Notes

## Project Title

**11 kV Feeder Protection Coordination and Reactive Power Compensation Study Using CYME / CYMDIST**

---

## 1. Purpose

These notes summarize the engineering decisions, troubleshooting steps, and final settings used in the CYME 11 kV feeder portfolio project.

The study focused on:

- 11 kV radial feeder modeling
- Short-circuit analysis
- Phase overcurrent protection using 50/51 relays
- CT selection
- TCC coordination
- Sequence-of-operations verification
- Shunt capacitor modeling
- Reactive power compensation
- CYME protection-model troubleshooting

---

## 2. System Model

The modeled network is an 11 kV radial feeder supplied from a utility source.

Main components:

- Utility source: `UTILITY_11KV_250MVA`
- Upstream feeder section: `SEC_11KV_01`
- Downstream feeder section: `SEC_11KV_02`
- Additional downstream section containing the shunt capacitor
- Overhead lines
- Three-phase loads
- Two feeder breakers
- Two phase overcurrent relays
- Current transformers
- Shunt capacitor bank

---

## 3. Breakers

Final breaker equipment used for the feeder protection study:

- Rated voltage: **12 kV**
- Continuous current: **630 A**
- Breaking current: **16 kA**
- Making current peak: **40 kA**
- Minimum delay time: **50 ms**

The breaker equipment identifier used in the study was:

`CB_11KV_630A_16KA`

---

## 4. Phase Overcurrent Relays

Both feeder breakers were equipped with electronic phase overcurrent protection.

Relay family:

- Manufacturer: **MULTILIN**
- Model: **MULTILIN 350 IEC SI**
- Protection function: **50/51**
- Characteristic: **IEC Short Inverse**

### Upstream Relay — SEC_11KV_01

- CT ratio: **600/5 A**
- Primary pickup: **550 A**
- Time Dial: **0.40**

### Downstream Relay — SEC_11KV_02

- CT ratio: **600/5 A**
- Primary pickup: **550 A**
- Time Dial: **0.05**

The downstream relay was intentionally configured to operate faster than the upstream relay.

---

## 5. CT Selection

The initial phase CT ratio was:

**200/5 A**

CYME calculated a feeder full-load current of approximately:

**452.6 A**

With a 200/5 A CT, the secondary current at full load would be:

`452.6 × 5 / 200 ≈ 11.3 A`

This was too high for a nominal 5 A CT secondary.

The CT ratio was therefore changed to:

**600/5 A**

At 452.6 A primary current, the secondary current becomes:

`452.6 × 5 / 600 ≈ 3.77 A`

This is a much more suitable operating range.

---

## 6. Pickup Selection

The final primary pickup was selected as:

**550 A**

Relative to the feeder full-load current:

`550 / 452.6 ≈ 1.22`

The pickup is therefore approximately **122% of full-load current**.

This keeps the 51 element above normal feeder loading while remaining well below the calculated fault currents.

---

## 7. Short-Circuit Results

Representative downstream fault currents:

| Fault Type | Fault Current |
|---|---:|
| LLL | 4574.7 A |
| LLG | 4231.4 A |
| LL | 3961.8 A |
| LG | 3060.6 A |

Representative upstream three-phase fault current:

**8741.2 A**

---

## 8. Sequence of Operations Results

### Downstream Faults

The following results were obtained at the downstream feeder:

| Fault Type | Breaker | Trip Time |
|---|---|---:|
| LLL | SEC_11KV_02 | 0.078 s |
| LLG | SEC_11KV_02 | 0.079 s |
| LL | SEC_11KV_02 | 0.080 s |
| LG | SEC_11KV_02 | 0.085 s |

For all tested downstream faults:

- `SEC_11KV_02` operated
- `SEC_11KV_01` remained closed

This verified selective operation for the tested cases.

### Upstream Fault

For an upstream fault of approximately:

**8741.2 A**

the upstream breaker operated as follows:

- Breaker: `SEC_11KV_01`
- Trip time: **0.221 s**

This confirmed correct upstream protection operation.

---

## 9. TCC Coordination

A combined TCC was generated using CYME's:

**Analysis → Protective Device Coordination → Branch Device Coordination**

The downstream node was selected and the branch was traced back to the source.

The combined plot showed:

- `SEC_11KV_02` as the faster curve
- `SEC_11KV_01` as the slower backup curve

Final time dial settings:

| Relay | Time Dial |
|---|---:|
| SEC_11KV_01 | 0.40 |
| SEC_11KV_02 | 0.05 |

The final TCC demonstrated the intended grading relationship.

---

## 10. Reclosing Configuration

The Sequence of Operations initially showed repeated:

**Open → 2 s → Close → Open → 2 s → Close**

The source was the Reclosing Unit sequence.

The original settings were approximately:

- Number of operations first: 2
- Number of operations to lockout: 4
- Reclosing times: 2.0 s

For the protection coordination study, the sequence was changed to:

- **1 operation**
- **1 operation to lockout**

This removed the repeated reclosing cycle and produced a clean single-trip protection sequence.

---

## 11. Shunt Capacitor

A shunt capacitor was included on `SEC_11KV_03`.

Final capacitor data shown in the project:

- ID: `CAP_600KVAR_11KV`
- Number: `CAP_END_600KVAR`
- Type: **Fixed**
- Rated voltage: **11.0 kVLN**
- Rated power: **600 kVAr total**
- Losses: **0 kW**
- Location: **At To Node**

The capacitor was included to represent:

- Reactive power compensation
- Voltage support
- Reactive power-flow effects

The capacitor was not treated as a full optimization study; it was included as part of the practical feeder model.

---

## 12. Important CYME Troubleshooting Notes

### TCC / Protective Device Coordination Crash

CYME 7.1 initially crashed when opening or running TCC-related functions.

The TCC database folder was:

`C:\Program Files (x86)\CYME\CYME\TCCLib`

Files observed:

- `cymtcc.mdb`
- `cymtcc.ldb`

A stale `cymtcc.ldb` lock file remained after CYME had closed.

The lock file was safely renamed while CYME was closed:

`cymtcc.ldb` → `cymtcc.ldb.old`

CYME then created a new lock file and the TCC functions started working again.

### Corrupted Breaker / Protection Object

The original breaker/protection association also caused CYME protection functions to fail.

The successful repair was:

1. Remove the problematic breaker.
2. Add a new breaker.
3. Recreate the relay and CT association.
4. Reconfigure the relay and TCC data.

After this, Protective Device Analysis populated the fault-current data correctly.

### CT Editing

The CT ratio was read-only from the relay's Controlled Breakers screen.

The actual CT ratio was changed by opening the **Current Transformer object** directly from the one-line diagram.

---

## 13. Final Protection Settings

| Parameter | SEC_11KV_01 | SEC_11KV_02 |
|---|---:|---:|
| Relay | MULTILIN 350 IEC SI | MULTILIN 350 IEC SI |
| Function | 50/51 | 50/51 |
| Curve | IEC Short Inverse | IEC Short Inverse |
| CT Ratio | 600/5 A | 600/5 A |
| Pickup | 550 A | 550 A |
| Time Dial | 0.40 | 0.05 |
| Reclosing | 1 operation to lockout | 1 operation to lockout |

---

## 14. Engineering Conclusion

The final CYME model demonstrated successful selective phase-overcurrent coordination between the upstream and downstream 11 kV feeder breakers.

The key engineering decisions were:

- Increase CT ratio from 200/5 A to 600/5 A
- Select a 550 A phase-overcurrent pickup
- Use a faster downstream time dial of 0.05
- Use a slower upstream backup time dial of 0.40
- Disable repeated reclosing for the coordination study
- Verify coordination using both TCC and Sequence of Operations
- Retain the 600 kVAr shunt capacitor as part of the feeder's reactive-power compensation model

The tested LLL, LLG, LL, and LG downstream faults were cleared by the downstream breaker without unnecessary upstream operation.

---

## 15. Portfolio Notes

Recommended evidence to include in the repository:

- One-line diagram
- Load-flow result
- Shunt capacitor settings
- CT 600/5 A settings
- Upstream relay settings
- Downstream relay settings
- Combined TCC
- LLL Sequence of Operations
- LLG Sequence of Operations
- LL Sequence of Operations
- LG Sequence of Operations
- Upstream fault Sequence of Operations
- Short-circuit results PDF
- Protection coordination summary PDF

---

## Disclaimer

This project is an engineering training and portfolio study.

The settings shown are specific to this modeled CYME system and should not be transferred directly to a real installation without a complete protection study, field data, manufacturer documentation, utility requirements, and applicable engineering standards.
