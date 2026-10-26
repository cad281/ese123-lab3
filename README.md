# ESE 123 Lab 3: SPICE Simulation & Soldering

**Interactive page:** https://cad281.github.io/ese123-lab3/

All labs: [⌂ Home](https://cad281.github.io/ese123-fall2026/) · [Lab 1: DC Power & Ohm's Law](https://cad281.github.io/ese123-lab1/) · [Lab 2: Digital Multimeter](https://cad281.github.io/ese123-lab2/) · [Lab 3: SPICE & Soldering](https://cad281.github.io/ese123-lab3/) · [Lab 4: Function Generator & Scope](https://cad281.github.io/ese123-lab4/) · [Lab 5: Capacitors & Scope](https://cad281.github.io/ese123-lab5/) · [Lab 6: Operational Amplifiers](https://cad281.github.io/ese123-lab6/) · [Lab 7: Assembly Language & Buzzer](https://cad281.github.io/ese123-lab7/)

A TA-written guide for ESE 123 students: **what** you do in Lab 3, **why** it matters and **how** to do it. The interactive page builds the LTspice schematic step by step, runs a draggable DC sweep, reduces the TP1 network by hand, and animates a solder joint.

---

## What this lab is about

| You will… | Using… |
|---|---|
| Draw a circuit and solve it with **.OP** (operating point) | LTspice (free from Analog Devices) |
| Sweep the input with **.DC** and read values with cursors | LTspice plot window |
| Solve a series-parallel network **by hand** and in SPICE | The TP1 circuit (R1–R5) |
| **Solder** your first parts onto the project board | R1–R5, J2 header, GND wire loop |
| **Verify** the board: resistance (power off) and voltage (power on) | DMM + programming tray |

## Why it matters

- **Simulate before you build.** SPICE is the industry-standard circuit solver. A two-minute simulation catches mistakes that would cost hours on the bench.
- **Three independent answers** (hand calculation, simulation, measurement) that agree are strong evidence the circuit is right. When they disagree you know where to look.
- **Soldering** is how every part gets onto your project board this semester. Good joints now mean a working clock later.
- **Check before you power up.** A quick resistance check with the power off finds wrong parts, missing joints and shorts without risking damage.

## How to do it, step by step

### Part 1: LTspice divider (steps 1–19)
1. **File → New Schematic.** Right-click → **Draft → Component → res**: click to place R1, click again for R2, **Esc**.
2. **Draft → Component → voltage** to place V1, **Esc**.
3. **Draft → Label Net → GND (global node 0)**. Every SPICE circuit needs ground: all voltages are measured from it.
4. **Draft → Draw Wire**: connect everything (squares mark junctions). **Esc**.
5. Hover the middle wire (named N00x), right-click → **Label Net → Vout**.
6. Right-click each part for values: **R1 = 1k (top), R2 = 2k (bottom), V1 = 3**. Remember **MEG** = mega; `M` means milli.
7. Right-click → **View → SPICE Netlist**. Each line: name, node, node, value. Screenshot it.
8. **Draft → SPICE directive → `.op`**, **Save As** (lab drive isn't writeable), **Run**. Expect **V(vout) = 2 V**, 1 mA through both resistors, **I(V1) = −1 mA**.

### Part 2: DC sweep (steps 20–26)
1. Edit the directive to **`.DC V1 0 10 0.1`** and **Run**.
2. Click the top node (a line of slope 1) and the Vout node (slope 2/3).
3. Click the **V(vout)** label for a cursor; drag it to **V(vout) ≈ 3.1416 V**. Expect **V1 ≈ 4.712 V**. Screenshot with the coordinate box.
4. Click the source to plot its current: **negative**, because SPICE reports current *into* the + terminal.

### Part 3: TP1 by hand and in SPICE (steps 27–30)
1. **R1 ∥ R2** (both VDD → TP1): 9.1k ∥ 13k = **5.353 kΩ**.
2. **R3 ∥ R4 ∥ R5** (all TP1 → GND): 22k ∥ 18k ∥ 27k = **7.244 kΩ**.
3. Divider: **TP1 = 5 × 7.244 / (5.353 + 7.244) = 2.875 V**. Photograph your work.
4. Measure the real R1–R5 with the DMM (all within 5%), rebuild the circuit in LTspice with measured values, run `.op`.

### Part 4: soldering (steps 31–51)
1. Write your name in the white box (solder side).
2. Insert **R1 9.1k (white-brown-red-gold), R2 13k (brown-orange-orange), R3 22k (red-red-orange), R4 18k (brown-grey-orange), R5 27k (red-violet-orange)** flat on the component side; bend leads to hold them.
3. At a soldering station, board in the fixture component-side down. For each joint: **heat pad + lead together (1–2 s) → feed solder into the joint → remove solder → remove iron → don't move it while it cools.** Trim leads.
4. **GND wire loop:** ~30 mm bare wire, U-shape ~2.5 mm wide, through the GND holes, ~5 mm proud. Solder both ends.
5. **J2:** long pins into the component fixture, board slid over the steel pins, **solder the centre pin only**, remove the board, then solder the other two.

### Part 5: verification (steps 52–64)
1. Power off: **Ω from TP1 to VDD ≈ 5.35 kΩ** (R1∥R2); **Ω from TP1 to GND (screw hole) ≈ 7.24 kΩ** (R3∥R4∥R5). If either is wrong, fix it before powering.
2. Board on the programming tray, adapter plugged in, rocker switch right side down (yellow LED on).
3. **DC volts VDD–GND** (≈ 5 V) and **TP1–GND** (≈ 2.87 V). Power off and unplug.
4. LTspice: **`.DC V1 4 6 0.01`**, cursor at your measured VDD, compare with measured TP1. Screenshot.
5. Store the board in your section's slot and **wash your hands**.

## What you should observe, and why

| Observation | Why it happens |
|---|---|
| V(vout) is a straight line, slope 2/3 | A divider always outputs the same fraction of its input. |
| I(V1) is negative | SPICE uses the passive sign convention for every part, sources included. |
| TP1 ≈ 2.87 V at 5 V | Two parallel groups make a simple 5.35k / 7.24k divider. |
| Measured TP1 differs slightly from the nominal calculation | 5% resistor tolerance and the adapter isn't exactly 5.00 V. With measured values the match is much closer. |
| Shiny cone-shaped joint = good; dull ball = cold | Solder only bonds (wets) when both metals are hot enough. |

## Formula sheet (what, when, example)

### Voltage divider

```
Vout = Vin · Rbottom / (Rtop + Rbottom)
```

**Use it when:** Any time two resistors (or two resistor groups) are in series and you want the voltage at the node between them.

**Example:** 3 V, 1k top, 2k bottom → 3 × 2/3 = 2 V (the .OP answer).

### Inverse divider

```
Vin = Vout · (Rtop + Rbottom) / Rbottom
```

**Use it when:** You know the output you want and need the input (step 24).

**Example:** Vout = π → Vin = π × 3/2 = 4.712 V.

### Parallel resistors

```
1/Rp = 1/Ra + 1/Rb + 1/Rc …
```

**Use it when:** Parts connected between the same two nodes, like R1 and R2 (VDD to TP1) or R3, R4, R5 (TP1 to GND).

**Example:** 9.1k ∥ 13k = 5.353 kΩ;  22k ∥ 18k ∥ 27k = 7.244 kΩ.

### Two in parallel (shortcut)

```
Rp = Ra·Rb / (Ra + Rb)
```

**Use it when:** Exactly two resistors in parallel: product over sum.

**Example:** 9100 × 13000 / 22100 = 5353 Ω.

### TP1 voltage

```
VTP1 = VDD · Rbot / (Rtop + Rbot)
Rtop = R1∥R2, Rbot = R3∥R4∥R5
```

**Use it when:** Step 27 hand calculation, and step 64 comparison at your measured VDD.

**Example:** 5 × 7.244 / 12.597 = 2.875 V.

### Ohm’s law

```
I = V / R
```

**Use it when:** Finding currents in the .OP results or checking SPICE by hand.

**Example:** 3 V / 3 kΩ = 1 mA through R1 and R2.

### SPICE source current sign

```
I(V1) = −(current leaving the + terminal)
```

**Use it when:** Reading a negative source current in LTspice: passive sign convention for every part.

**Example:** Source delivering 1 mA shows I(V1) = −1 mA.

### SPICE prefixes

```
f p n u m k MEG G = 10⁻¹⁵ 10⁻¹² 10⁻⁹ 10⁻⁶ 10⁻³ 10³ 10⁶ 10⁹
```

**Use it when:** Typing component values. SPICE ignores units and is case-insensitive, so m and M both mean milli.

**Example:** 10MEG = 10 MΩ;  10M = 10 mΩ (a classic mistake).

### .DC sweep directive

```
.DC <source> <start> <stop> <step>
```

**Use it when:** Seeing how outputs change as an input voltage changes.

**Example:** .DC V1 0 10 0.1 → 101 solutions;  .DC V1 4 6 0.01 for step 64.

### 5% resistor colour code

```
band1 band2 × 10^band3, gold = ±5%
black0 brown1 red2 orange3 yellow4 green5 blue6 violet7 grey8 white9
```

**Use it when:** Picking the right resistor before soldering.

**Example:** brown-grey-orange = 1, 8, ×1000 = 18 kΩ (R4).

### Percent difference

```
(measured − predicted) / predicted × 100%
```

**Use it when:** Comparing measured TP1 with the hand calculation and the SPICE sweep.

**Example:** Measured 2.861 V vs 2.875 V → −0.5%.


## Practice questions

Same concepts and formulas as the Brightspace quiz, with new numbers (not the real quiz questions). Try each one before opening the answer.

**Q1.** A 5 V source drives R1 = 2.2 kΩ (top) and R2 = 3.3 kΩ (bottom). Find Vout and the current.

<details><summary>Show answer</summary>

**3.0 V, 0.909 mA**. Vout = 5 × 3.3/5.5 = 3.0 V. I = 5/5.5k = 0.909 mA.

</details>

**Q2.** For the same divider, what input gives Vout = 2.5 V?

<details><summary>Show answer</summary>

**4.17 V**. Vin = Vout(R1 + R2)/R2 = 2.5 × 5.5/3.3 = 4.17 V.

</details>

**Q3.** VDD = 5 V. Between VDD and TP: 10 kΩ ∥ 15 kΩ. Between TP and GND: 12 kΩ ∥ 24 kΩ ∥ 24 kΩ. Find VTP.

<details><summary>Show answer</summary>

**2.5 V**. Top: 10k·15k/25k = 6 kΩ. Bottom: 24k ∥ 24k = 12k, then 12k ∥ 12k = 6 kΩ. Equal halves → 5 × 6/12 = 2.5 V.

</details>

**Q4.** With the power off, what resistance should the DMM show between TP and VDD in the previous circuit?

<details><summary>Show answer</summary>

**6 kΩ**. Only the top group connects TP to VDD (VDD and GND are not connected to each other on an unpowered board), so you read Rtop = 6 kΩ.

</details>

**Q5.** A 5% resistor is orange-white-red-gold. What is its value and allowed range?

<details><summary>Show answer</summary>

**3.9 kΩ, 3705–4095 Ω**. Orange 3, white 9, red ×100 → 3900 Ω. ±5% = ±195 Ω.

</details>

**Q6.** A netlist line reads R3 N002 0 4.7m. What value did SPICE use, and what was probably intended?

<details><summary>Show answer</summary>

**4.7 mΩ; probably 4.7 MΩ**. SPICE ignores case: m = milli. Mega must be typed MEG: 4.7MEG.

</details>

**Q7.** How many solutions does .DC V1 0 5 0.05 compute?

<details><summary>Show answer</summary>

**101**. (5 − 0)/0.05 = 100 steps, plus the starting point = 101 solutions.

</details>

**Q8.** A 6 V source drives 2 kΩ in total. What does LTspice report for I(V1)?

<details><summary>Show answer</summary>

**−3 mA**. 3 mA flows out of the source’s + terminal; SPICE reports current into +, so −3 mA.

</details>


## Report (one PDF)

- Netlist screenshot (2 pts)
- DC sweep screenshot with the π cursor and coordinate box (2 pts)
- Photo of your TP1 hand calculation (2 pts)
- 4–6 V sweep screenshot compared with your measured TP1 (2 pts)

---
*Made for ESE 123 students by a TA. Values shown are nominal; your measured resistors will make your numbers slightly different.*
