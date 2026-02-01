# Pulse Optimization with Qiskit Dynamics

This project demonstrates the calibration of a superconducting qubit by optimizing microwave pulse parameters to maximize gate fidelity. It uses `qiskit-dynamics` to simulate the quantum system and `scipy.optimize` for the optimization loop.

## Project Structure
- `pulse_optimization.ipynb`: Main Jupyter Notebook containing the simulation, usage of GaussianSquare and RaisedCosine pulses, and optimization tasks.
- `requirements.txt`: Python dependencies.
- `create_notebook.py`: Script used to generate the notebook programmatically.
- `verify_pulse.py`: Script for quick verification of the core logic.

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
   *Note: This project requires `qiskit<1.0` (e.g., 0.46.x) as `qiskit-dynamics` currently depends on `qiskit.pulse` which was removed in Qiskit 2.0.*

## Usage

1. Launch Jupyter Notebook:
   ```bash
   jupyter notebook pulse_optimization.ipynb
   ```
2. Run all cells to see the results.
   - **Section 4**: Optimizes the width of a GaussianSquare pulse to achieve a high-fidelity X-gate.
   - **Section 5**: Visualization of population dynamics.
   - **Section 6**: Optimizes a Raised Cosine pulse shape using the Nelder-Mead method.

## Summary of Learning

- **Qiskit Dynamics**: Learned how to define a Hamiltonian model (static + drive terms) and solve the Shrodinger equation in the rotating frame using `Solver`.
- **Pulse Control**: Understood how `InstructionToSignals` converts abstract Qiskit Pulse schedules into time-dependent signals for the solver.
- **Optimization**: Used `scipy.optimize.minimize` (specifically Nelder-Mead) to tune physical parameters (pulse width) given a simulated objective function (fidelity).
- **Challenges**:
  - **Discrete vs Continuous**: Qiskit Pulse `GaussianSquare` requires integer duration in samples (dt), which creates a stepped landscape for optimization. Nelder-Mead handles this better than gradient-based methods like BFGS which require smoothness.
  - **Version Compatibility**: Navigating the version constraints between `qiskit` 2.0+ and `qiskit-dynamics` which relies on the legacy `qiskit.pulse` module.
- **Next Steps**:
  - Investigate **Gradient-based optimization** using JAX auto-differentiation (as shown in advanced tutorials) for multi-parameter pulse shaping (Optimal Control).
  - Include realistic noise models (Lindblad dynamics) to optimize for robustness against decoherence.

## License
Open Source.
