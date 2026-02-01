# Pulse Optimization with Qiskit Dynamics

This project demonstrates the calibration of a superconducting qubit by optimizing microwave pulse parameters to maximize gate fidelity. It uses `qiskit-dynamics` to simulate the quantum system and `scipy.optimize` for the optimization loop.

## Project Structure
- `pulse_optimization.ipynb`: Main Jupyter Notebook containing the simulation, usage of GaussianSquare and RaisedCosine pulses, and optimization tasks.
- `requirements.txt`: Python dependencies.

## Installation setup

1. **Clone the repository** (if applicable) or download the files.
2. **Create a virtual environment** (recommended):
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```
3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

1. Launch Jupyter Notebook:
   ```bash
   jupyter notebook pulse_optimization.ipynb
   ```
2. Run all cells to see the results.
   - **Section 4**: Optimizes the width of a GaussianSquare pulse to achieve a high-fidelity X-gate.
   - **Section 5**: Visualization of population dynamics.
   - **Section 6**: Optimizes a Raised Cosine pulse shape to achieve a high-fidelity X-gate.

## Summary of Learning

- **Qiskit Dynamics**: Learned how to define a Hamiltonian model (static + drive terms) and solve the Shrodinger equation in the rotating frame using `Solver`.
- **Optimization**: Used `scipy.optimize.minimize` (specifically Nelder-Mead) to tune physical parameters (pulse width) given a simulated objective function (fidelity).
- **Pulse Characteristics**: Learned the trade-off between strong/short pulses (fast but risk leakage/spectral splatter) vs weak/long pulses (precise but risk decoherence), and how sigma controls the transition.
- **Pulse Shapes**: Learned the difference between **GaussianSquare** (infinite tails, needs sigma truncating) vs **Raised Cosine** (finite support, strictly zero outside duration), and how this impacts simulation efficiency.
- **Version Compatibility**: Navigating the version constraints between `qiskit` 2.0+ and `qiskit-dynamics` which relies on the legacy `qiskit.pulse` module.
- **Challenges**:
  - **Discrete vs Continuous**: Qiskit Pulse `GaussianSquare` requires integer duration in samples (dt), which creates a stepped landscape for optimization. Nelder-Mead handles this better than gradient-based methods like BFGS which require smoothness.
  - **Optimizer Choice**: Didn't know that **Nelder-Mead** was at all. I learned that it is a robust, gradient-free local optimizer suitable for noisy or non-smooth landscapes (like discrete pulse samples), whereas other methods might get stuck or fail to compute gradients.
  - **Pulse Shaping Physics**: Understanding how the **sigma** parameter (edge smoothness) radically changes the effective "energy" of a pulse. A sigma change (e.g., from 20 to 5) drastically alters the optimal `width` needed for a $\pi$-pulse, moving it from "short and fat" to "sharp and thin".
- **Next Steps**:
  - Investigate **Gradient-based optimization** using JAX auto-differentiation (as shown in advanced tutorials) for multi-parameter pulse shaping (Optimal Control).
  - Implement **Realistic Simulation** incorporating:
    - **Lindblad Dynamics**: Modeling decoherence (T1 relaxation and T2 dephasing).
    - **Higher Energy Levels**: Accounting for leakage to the $\ket{2}$ state (anharmonicity).
    - **Control Electronics**: simulating transfer functions of AWGs and cables (distortions).
    - **Crosstalk**: Modeling interactions with neighboring qubits.
    - **SPAM Errors**: Incorporating State Preparation and Measurement errors.
