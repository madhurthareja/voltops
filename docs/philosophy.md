# VoltOps Core Philosophy

VoltOps delivers **physics-aware computation for everyday electronics and signal-analysis workflows**. Each public API corresponds to a concept that practicing engineers recognize—Ohm's Law, impedance, FFT-based measurement, and so on. Rather than replicating generic math helpers, VoltOps focuses on wiring realistic metadata, guardrails, and educational context into every computation.

---

## Guiding Assumptions

- **Linear, time-invariant, lumped elements** unless explicitly stated.
- **Steady-state analysis** is the default; transient behavior must be modeled consciously.
- **Closed-form expressions first**. Numerical solvers and optimizers stay in delegated libraries (e.g., SciPy) until they can be wrapped with clear physics-aware semantics.
- **Quantities, not scalars**. VoltOps manipulates `Voltage`, `Current`, `Power`, etc., keeping track of waveform kind (DC, AC RMS, AC peak), phase references, and carrier frequency.

## Non‑Goals (for now)

Reinforcing explicit "no" statements builds trust:

- VoltOps is **not** a SPICE replacement or a general-purpose circuit simulator.
- **No real-time execution guarantees**—runtime safety belongs to your embedded toolchain.
- **No nonlinear semiconductor device physics**. Analytical helpers for BJTs/FETs may arrive later, but they will remain closed-form.
- **No blind duplication of SciPy/NumPy** utilities. We wrap them only when physics-aware metadata adds value.

## Design Principles

1. **Quantities over scalars** – Type-safe values that guard against invalid operations and attach waveform metadata everywhere.
2. **Physical invariants as tests** – Unit tests assert conservation laws, scaling behavior, and dimensional consistency instead of memorizing golden numbers.
3. **Layered architecture** – Packages are separated into `core/` (metadata + quantities), `formulas/`, `signal_processing/`, and future `circuits/` or `numeric/` layers. Lower layers never depend on higher ones.
4. **Context-rich results** – APIs return annotated objects. Even FFT helpers yield spectra with sampling metadata rather than naked arrays.
5. **Delegation with intent** – Expensive numerical kernels continue to live in SciPy/NumPy; VoltOps focuses on keeping the surrounding context faithful to the underlying physics.

## Roadmap Snapshot

| Theme | Why it matters | Current Status |
| --- | --- | --- |
| Quantity primitives | Enforce metadata, catch misuse early | (`voltage`, `current`, `resistance`, etc.) |
| Circuit abstractions | Series/parallel, dividers, equivalents | planned |
| Measurement helpers | ADC quantization, SNR, aliasing guards | planned |
| Signal-aware DSP | FFTs that return annotated spectra | foundational transforms |
| Symbolic hooks | Bridge SymPy for derivations | future exploration |

## How This Shapes Implementation

- **APIs describe intent**: instead of `voltage = i * r`, users call `BasicFormulas.ohms_law(current=i, resistance=r)` and receive a `Voltage` object tied to the originating metadata.
- **Docs teach, not just list**: each recipe explains the physical context, common pitfalls, and how VoltOps keeps track of assumptions.
- **Architecture anticipates acceleration**: should we swap in C++ kernels later, the Python-facing API remains unchanged because all metadata plumbing lives in `core/`.

## Delegations

| Capability | Preferred Library |
| --- | --- |
| Large-scale linear algebra | NumPy / SciPy |
| Nonlinear solvers & optimizers | SciPy |
| Symbolic manipulation | SymPy (optional future layer) |
| Plotting / visualization | Matplotlib, Plotly, etc. (kept outside VoltOps) |

---

VoltOps succeeds when it saves engineers from re-deriving the same relationships, documents the assumptions behind every helper, and keeps metadata honest from input to output. Depth over breadth, physics over convenience, meaning over speed.
