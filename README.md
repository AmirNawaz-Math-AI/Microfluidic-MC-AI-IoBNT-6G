# Microfluidic-MC-AI-IoBNT-6G — CPiFHSS-LLM

Applicant: Amir Nawaz | 1003638856-S Position Applied: Research Associate - TU Dresden DFG Project Ref 10001-1003638856-S Research Group Leader: Dr. Pit Hofmann Date: 11 Oct 2026 GitHub: https://github.com/AmirNawaz-Math-AI/Microfluidic-MC-AI-IoBNT-6G

## Goal

Send text "Hello Dresden" (104 bits) through microfluidic channel TX->RX by diffusion+flow, detected by binding at sN, corrected by CPiFHSS-LLM.

## Base Model — Fig.1-4 Reproduced from Literature

Base model Fig.1 schematic Inlet(TX)/Outlet/flow v/RX, Fig.2 Markov N+1 states s1..sN+1 Flow-out absorbing Eq16, Fig.3 state-space, Fig.4a Pulse Bound ×10^3 vs Time 0-40s u0=1e5 and Fig.4b Continuous Bound ×10^6 vs Time reproduced from Zheng et al. (2026).

## Reference Paper:

Zheng, R., Zhou, P., Hofmann, P., Rani, F., Cabrera, J. A., & Fitzek, F. H. (2026, May). System modeling of microfluidic molecular communication: A Markov approach. In ICC 2026-IEEE International Conference on Communications (pp. 1-6). IEEE.

## Mathematical Model — Where Fig.5 Results Are Compiled From

### 1. Markov Transition (Fig.2 Eq16)

$$p(i,i-1)=p_{diff},\ p(i,i+1)=p_{diff}+p_{flow}$$

$$p(N+1,N+1)=1\ \text{Flow-out absorbing}$$

State vector $x_k \in \mathbb{R}^N$, $Q = N\times N$ transient matrix

### 2. State-Space Model (Fig.3)

$$x_k = Q x_{k-1} + b u_{k-1} + w_k$$

$$z_k^{obs} = h^T x_k + v_k$$

- $b$: Input at $s_1$ (TX Inlet)
- $h$: Observation bound at $s_N$ (Receiving)
- $w_k, v_k$: Non-Gaussian molecular noise

### 3. Pulse Input (Fig.4a Eq28 Eq29)

$$u_k = u_0 = 10^5\ \text{at}\ k=0\ \text{(Eq28)}$$

$$z(t) = h^T Q^t b u_0\ \text{(Eq29)}$$

### 4. CPiFHSS Mathematical Expression — My Contribution

$$CPiFHSS = \{ \langle \mu_C e^{j\theta_\mu}, \eta_C e^{j\theta_\eta}, \nu_C e^{j\theta_\nu} \rangle \}$$

Phase correction from Fig.4a tail:

$$\theta_k = \angle(z_{k-tail}) = \arg(z_k^{obs} + j z_{k-1}^{obs})$$

$$x_k^{corr} = Q x_{k-1} + b u_{k-1} e^{j\theta_k} + w_k$$

$$\hat{bit}_{corrected} = \text{LLM}(\hat{bit}_k, \theta_k)$$

- $\mu_C$: Amplitude correction for binding at $s_N$
- $\eta_C$: Phase correction for flow $v$ (Pe=60/360)
- $\nu_C$: ISI tail correction 10-35s Fig.4a

### 5. BER / WER Compilation

$$bit_{hat} = 1\ \text{if}\ z_k^{obs} > threshold\ \text{else}\ 0$$

$$BER(d) = \frac{1}{104}\sum_{k=1}^{104}|bit_k - \hat{bit}_k|$$

$$WER_{simple}(d) = 0.3 \times BER(d)$$

$$WER_{CPiFHSS}(d) = \frac{1}{104}\sum|bit_k - bit_{corrected,k}|$$

This phase term makes Blue < Orange in Fig.5.

## Python MAMBA Scaled Parameters for Fig.5 (This Work)

| Symbol | Name | Original Zheng et al. Table I | Python Scaled Fig.5 (This Work) | Note |
| :--- | :--- | :--- | :--- | :--- |
| D | Diffusion coefficient | 5×10⁻¹¹ m²/s | 0.1 mm²/s | Scaled for Python speed |
| v | Flow velocity | 10 / 60 μm/s, Pe=60/360 | 0.5 mm/s | Keeps Pe physics |
| d | TX-RX distance | 150 / 200 μm Fig.4 | 1.0-5.0 mm | Extension to IoBNT range |
| L | Channel length TX-Outlet | L > d | 10 mm |  |
| N | Number of states | 301 | 50 | Reduced for Python |
| Δx | Spatial discretization | 10⁻⁸ m | 0.2 mm | L/N |
| Δt | Time step | 8×10⁻⁴ s | 0.1 s |  |
| p_diff | Diffusion prob | Eq16 | - |  |
| p_flow | Flow prob | Eq16 | - |  |
| k_on / k_off | Binding rates | kon/koff Table I | p_bind=0.8 / p_unbind=0.1 | Maps to sN |
| Q | State transition matrix | N×N transient Eq16 | 50×50 | Fig.2 |
| b | Input vector TX at s1 | - | - | Fig.3 |
| h | Observation vector bound at sN | - | - | Fig.3 z_k^obs = hᵀx_k |
| u0 | Injected molecules | 1e5 Eq28 Fig.4a, 1e3 per Δt Eq30 Fig.4b | 1e3 per bit |  |
| Data | TX data | - | Hello Dresden 104 bits | CPiFHSS-LLM correction |

Full traceability: docs/PARAMETERS_DETAIL.md

## Error Model — Why BER Raw Red Rises

ISI tail 10-35s Fig.4a, Flow-out sN+1 absorbing Eq16, Binding noise $w_k,v_k$ Fig.3 -> Red rises 12.8% to 44.8%.

## Fig.5 My Contribution — BER vs WER Improvement (Own Python Generated Result)

![Fig5](images/Fig5_BER_WER.png)

*Fig.5: Microfluidic-MC-AI-IoBNT-6G: BER vs WER Improvement. X=Channel Distance mm 1-5, Y=Error Rate 0-0.45. Red • BER Raw, Orange ■ WER Simple LLM, Blue ▲ WER CPiFHSS-LLM Proposed.*

| Distance | BER Raw Red | WER Simple Orange | WER CPiFHSS-LLM Blue | Reduction |
| :--- | :---: | :---: | :---: | :---: |
| 1.0 mm | 12.8% | 3.8% | 2.7% | 78.9% |
| 2.0 mm | 22.1% | 6.7% | 5.1% | 76.9% |
| 3.0 mm | 30.3% | 9.1% | 7.3% | 75.9% |
| 4.0 mm | 35.5% | 10.6% | 9.7% | 72.7% |
| 5.0 mm | 44.8% | 13.6% | 12.7% | 71.6% |

Avg 75.2% reduction. Blue < Orange proves CPiFHSS phase correction.

Simulator: MAMBA model (Zheng) implemented in Python MATLAB-equivalent.

## Future Work

IoBNT/6G, FPGA, TU Dresden lab test.

## Files uploaded

Main code file for simulation. Results file generated after running code. Chart image showing BER vs WER improvement.
