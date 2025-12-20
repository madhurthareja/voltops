# VoltOps Core Philosophy

VoltOps is designed to support **electronics and signal-analysis workflows where physical context matters**. The library exposes APIs that correspond to familiar engineering concepts—Ohm’s law, impedance, power, frequency analysis—while explicitly tracking the assumptions under which those concepts are valid.

Rather than acting as a general numerical toolbox, VoltOps focuses on preserving metadata, enforcing basic physical constraints, and documenting intent at the API level.

---

## Operating Assumptions

Unless stated otherwise, computations in VoltOps assume:

* **Linear, time-invariant, lumped systems**
* **Steady-state behavior**, with transient effects treated as a separate modeling choice
* **Closed-form analytical relationships**, with numerical solvers delegated to external libraries when needed
* **Typed physical quantities**, such as voltage or current, rather than unannotated scalars

These assumptions are made explicit to reduce ambiguity and prevent accidental misuse.

---

## Design Considerations

1. **Quantities instead of scalars**
   Electrical values are represented as typed quantities (e.g., `Voltage`, `Current`, `Power`) that carry waveform information such as DC/AC classification, RMS or peak representation, phase reference, and frequency where applicable.

2. **Physical consistency checks**
   Tests emphasize conservation laws, scaling behavior, and dimensional consistency rather than relying solely on fixed numerical outputs.

3. **Layered structure**
   The codebase is organized into layers (`core`, `formulas`, `signal_processing`, etc.) with one-way dependencies. Lower layers remain independent of higher-level abstractions.

4. **Context-preserving outputs**
   Functions return annotated results instead of raw arrays. For example, frequency-domain transforms retain sampling and spectral metadata rather than discarding it.

5. **Intentional delegation**
   Established libraries such as NumPy and SciPy are used for numerical kernels. VoltOps focuses on providing physics-aware structure around these computations rather than re-implementing them.

---

## Development Roadmap (Indicative)

| Area                    | Rationale                                      | Status       |
| ----------------------- | ---------------------------------------------- | ------------ |
| Quantity primitives     | Preserve metadata and enforce valid operations | Implemented  |
| Circuit abstractions    | Express common analytical reductions           | Designed     |
| Measurement helpers     | Model ADC limits, noise, aliasing              | Designed     |
| Signal-aware transforms | Retain context across domains                  | Foundational |
| Symbolic integration    | Support derivations alongside numerics         | Exploratory  |

---

## Implementation Implications

* APIs are written to **express intent explicitly**. For example, voltage is derived through a named relationship rather than implicit arithmetic.
* Documentation focuses on **assumptions and failure modes**, not just usage.
* The architecture keeps metadata handling separate from numerical kernels, allowing future optimization without altering user-facing behavior.

---

## External Dependencies

VoltOps relies on existing libraries for well-established functionality:

| Capability                  | Library                     |
| --------------------------- | --------------------------- |
| Linear algebra and numerics | NumPy, SciPy                |
| Nonlinear solvers           | SciPy                       |
| Symbolic algebra            | SymPy (optional)            |
| Visualization               | External plotting libraries |

---

VoltOps aims to reduce repeated derivations, make assumptions visible, and keep physical meaning intact throughout a computation pipeline. The emphasis is on clarity and correctness rather than breadth or performance.
