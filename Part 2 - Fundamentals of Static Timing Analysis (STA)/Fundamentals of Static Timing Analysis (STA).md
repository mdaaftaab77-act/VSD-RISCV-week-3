
# Part 2 – Fundamentals of Static Timing Analysis (STA)

**Creator & Mentor:** Kunal Ghosh, Co-Founder of VLSI System Design (VSD)

This part focuses on understanding the fundamentals of Static Timing Analysis (STA) — how digital designs are verified for timing closure.
The course provides in-depth knowledge of timing checks, setup/hold analysis, and OCV impact, but does not cover constraints and library modeling.

---

## Table of Contents
1. Introduction to STA
2. Timing Path and Key Parameters
3. Arrival Time
4. Required Time
5. Slack
6. Types of Setup & Hold Analysis
7. Transition (Slew) Analysis
8. Load Analysis
9. Clock Analysis
10. Register to Register (Reg2Reg) Analysis
11. Setup Analysis - Single Clock
12. Timing Graph (DAG)
13. Combinational Logic Analysis
14. Actual Arrival Time (AAT)
15. Required Arrival Time (RAT)
16. Slack Calculation
17. STA Analysis Types
18. Graph-Based Analysis (GBA)
19. Path-Based Analysis (PBA)
20. Setup Time Analysis
21. Jitter and Uncertainty
22. Eye Diagram Analysis
23. Causes of Jitter
24. Slack Calculation with Uncertainty
25. Setup Time - Graphical to Textual Conversion
26. On-Chip Variation (OCV)
27. Pessimism
28. Pessimism Removal
29. Key Takeaways
30. What I Learned
31. Conclusion
32. Credits

---

## Introduction to STA
Static Timing Analysis (STA) is a method to verify the timing of a digital circuit without applying any input vectors.
It checks if all timing paths meet required performance by analyzing propagation delays, setup, and hold times.

STA ensures that every data signal arrives and stabilizes at the correct time relative to its clock signal.

---

## Timing Path and Key Parameters
Assume a circuit typically consists of:

**Launch Flip-Flop → Combinational Logic → Capture Flip-Flop**

*Circuit Diagram: timing_path_example*

### Arrival Time
Time taken for data to propagate from the start point (e.g., Flop clk pin/input port) to the endpoint (e.g., Flop D pin/output port).

### Required Time
Time by which the data must arrive at the capture register to satisfy setup or hold constraints.

### Slack
The difference between Required time and Arrival time.

**Slack = Required Time - Arrival Time**

- Positive Slack → No violation  
- Negative Slack → Timing violation

---

## Types of Setup & Hold Analysis

| # | Timing Type | Description |
|---|--------------|-------------|
| 1 | Register to Register | Between two sequential elements |
| 2 | Input to Register | From input port to register |
| 3 | Register to Output | From register to output port |
| 4 | Input to Output | Combinational path from input to output |
| 5 | Clock Gating Check | Ensures enable signal stability |
| 6 | Recovery/Removal | Checks asynchronous signals like reset |
| 7 | Data to Data | Combinational data dependency |
| 8 | Latch Time Borrow/Given | Borrows time from subsequent stage |

---

## Transition (Slew) Analysis
## Load Analysis
## Clock Analysis

---

## Register to Register (Reg2Reg) Analysis
### Setup Analysis - Single Clock

**Spec:**  
- Clock frequency (F) = 1GHz  
- Clock Period (T) = 1/F = 1ns  

*Circuit Diagram: timing_path_example*

---

## Timing Graph (DAG)
A Directed Acyclic Graph represents all paths and delays.  
We calculate **Arrival Time**, **Required Time**, and **Slack** for each edge.

### Combinational Logic Analysis
Let us convert the combinational part of the circuit into gates.

**Comb Delay**

where,  
a(2), b(2), c(3), d(2) → Cell Delay (in 'ps')  
i1(0), i2(0.3), i3(0.5) → Signal arrival time at inputs (in 'ps')

We convert this circuit into a **Timing Graph (DAG)**.

---

### Actual Arrival Time (AAT)
The Actual Arrival Time is the time at any node where we see the latest transition after the first rising clock edge.
It represents when the signal actually arrives at a particular point in the circuit during propagation.

### Required Arrival Time (RAT)
The Required Arrival Time is the time at any node where we expect the latest transition within the clock cycle.

### Slack Calculation
**Slack(S) = RAT - AAT**  
To meet design expectations → RAT > AAT.

---

## STA Analysis Types

### Graph-Based Analysis (GBA)
- Uses DAG representation.
- Finds worst-case arrival and required times.
- Faster but less accurate (pessimistic).

### Path-Based Analysis (PBA)
- Considers exact timing path from startpoint to endpoint.
- Reduces pessimism and gives realistic timing.

---

## Setup Time Analysis
Let's analyze the time needed for a capture flop to catch data.

*Positive Edge triggered Flip Flop using master-slave configuration*

In this configuration, the first pass transistor logic is a **negative latch**, and the next is a **positive latch**.

**Setup time** is the duration before the rising clock edge during which input **D** must remain valid and stable.

This ensures reliable capture and correct output **Q**.

When clk is negative → *master_slave diagram*

### Example
Input D takes **3 inverter delays + 1 transmission gate delay** to become stable before the rising edge.
This is the **library setup time** — time needed for D to stabilize for proper capture.

---

## Jitter and Uncertainty

### Eye Diagram Analysis
Ideal and real eye diagrams are compared.  
In real cases, jitter causes **eye closure**, reducing the timing margin.

### Causes of Jitter
- Power supply variations  
- Voltage droop → temporary voltage drop causing slower transitions  
- Ground bounce → fluctuation in ground voltage due to simultaneous switching

### Slack Calculation with Uncertainty
Uncertainty (jitter) reduces timing margin.

**Setup Slack = Required Arrival Time - Actual Arrival Time**  

If slack is **negative**, it indicates a **timing violation**.

To avoid setup violation, the following condition must be satisfied:

```
(Tcomb + D1) < (T + D2) - S - SU
```
where,  
Tcomb = Combinational delay  
D1 = Launch Flop clock delay  
D2 = Capture Flop clock delay  
S = Setup time  
SU = Setup Uncertainty

---

## Setup Time - Graphical to Textual Conversion
In the industry, timing reports are textual.  
We must interpret setup and hold violations from these reports.

Example (from previous circuit):  
- **Without OCV:** Slack is positive → no setup violation.  
- **Hold analysis:** `(Tcomb + D1) > H + D2 + HU`  
  - H = Hold time  
  - HU = Hold time uncertainty  

If this condition holds → **no hold time violation**.

---

## On-Chip Variation (OCV)
Variation in delay due to **process**, **voltage**, or **temperature** differences.

**Sources:**
- Etching process variation  
- Oxide thickness variation  

### Clock Pull-in and Push-out
- **Pull-in:** Clock arrives earlier (speedup)  
- **Push-out:** Clock arrives later (slowdown)  

These impact **timing closure** and are analyzed via **OCV corners**.

---

## Pessimism
Pessimism refers to **overestimation of delays or margins** during STA.
It assumes worst-case conditions — making analysis conservative.

### Pessimism Removal
After adjusting for clock/data correlation, pessimism reduces.  
Slack often becomes **positive**, confirming no violation.

---

## Key Takeaways
- STA verifies timing without simulation.  
- Understanding **AAT** and **RAT** is essential.  
- GBA → faster, pessimistic.  
- PBA → accurate, realistic.  
- Checks include setup, hold, recovery, removal, data-to-data, etc.  
- Jitter, droop, and bounce affect slack.  
- Pessimism removal improves timing accuracy.

---

## What I Learned
- How to calculate **arrival**, **required**, and **slack** times.  
- Deep understanding of **setup**, **hold**, and **latch** behavior.  
- Difference between **Graph-Based** and **Path-Based** analysis.  
- Clock vs. data transition behavior.  
- Effect of **uncertainty** and **OCV** on timing.  
- Industry-level STA workflow and report analysis.

---

## Conclusion
This course provided a complete understanding of STA fundamentals — setup/hold checks, slack computation, and variation impacts.  
It strengthened my ability to perform timing verification and ensure reliable digital design performance.

---

## Credits
Special thanks to **Kunal Ghosh Sir** for his clear, structured, and insightful teaching.  
His mentorship made STA easy to grasp and apply effectively.
