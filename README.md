# Clifford Noise Reduction (CliNR) Tutorial Series

Clifford Noise Reduction (CliNR) is a method for running a Clifford circuit more accurately on a noisy quantum computer, without the overhead of full quantum error correction. The operation is prepared on a separate set of qubits and verified with a small number of stabilizer measurements; only a preparation that passes verification is teleported onto the data. Because faults are detected before they reach the data, they cannot propagate into the computation. CliNR occupies the middle ground between error correction, which requires large qubit overheads, and error mitigation, which incurs an exponential sampling cost.

This series introduces CliNR from first principles and carries it through to a run on real trapped-ion hardware via qBraid. The benefit of CliNR grows with the size of the circuit it protects: it does not pay off on a small circuit, reaches breakeven at ten qubits, and — as shown in the source papers — gives a clear reduction in logical error rate at twenty qubits and beyond. The three notebooks follow this progression, and are meant to be worked through in order.

The series is based on Delfosse and Tham (arXiv:2407.06583), which introduces the method, and Tham and Delfosse (arXiv:2504.13356), which optimizes it and demonstrates it on hardware. Familiarity with quantum circuits and the stabilizer formalism is assumed.

## Launch on qBraid

<!-- TODO: fill in the GitHub repository URL, environment slug, and environment ID, then this badge becomes active. -->
[![Launch on qBraid](https://img.shields.io/endpoint?url=https://api.qbraid.com/api/environments/valid?envSlug=TODO_ENV_SLUG&label=Launch+on+qBraid&labelColor=lightgrey&logo=rocket&style=for-the-badge)](https://account.qbraid.com?gitHubUrl=TODO_GITHUB_URL&envId=TODO_ENV_ID)

Click the badge above to clone this repository into qBraid Lab and install its environment. When the environment is ready, select it as the notebook kernel, open `Notebook_01.ipynb`, and run the cells in order.

## The Notebooks

### 1. Introduction to CliNR (`Notebook_01.ipynb`)

Builds the method from its foundations: state teleportation, then gate teleportation, then CliNR as gate teleportation with resource-state verification. The protocol is demonstrated on a three-qubit Clifford circuit, where it does not yet outperform a direct implementation. This establishes the baseline — the verification overhead only pays off on larger circuits — before the later notebooks show it succeeding.

### 2. CliNR in Practice: CZNR (`Notebook_02.ipynb`)

Introduces CZNR, the specialization of CliNR to circuits of controlled-Z gates, which uses a graph state as its resource. The all-pairs CZ circuit on ten qubits is run in simulation and compared against a direct implementation, reaching breakeven. Two families of verification stabilizers and several verification lengths are compared, followed by a sweep over the noise rate.

### 3. CZNR on IonQ Hardware (`Notebook_03.ipynb`)

Runs the CZNR circuit on IonQ Forte through qBraid. Because trapped-ion hardware does not support mid-circuit measurement, the corrections and restarts are applied in classical post-processing rather than by in-circuit feedforward. The notebook validates the full pipeline on a free simulator, submits to hardware, and analyzes the results with a progressive stabilizer-ignoring study that shows the verification removing genuine errors.

## Requirements

All packages are pre-installed in the Launch-on-qBraid environment. Each notebook also begins with a `%pip install` cell for other setups.

- **Stim** — exact, efficient simulation of the Clifford circuits used throughout the series.
- **Qiskit** — circuit construction for the hardware submission in Notebook 03.
- **qBraid** — unified quantum device access and job submission, including to **IonQ Forte** hardware.

The hardware section of Notebook 03 additionally requires a qBraid account with IonQ access and credits. It is optional: the notebook runs end to end on the free simulator without it.

## Running the Tutorials

- Run each notebook from top to bottom, in order. If the kernel state becomes inconsistent, use **Kernel > Restart Kernel and Run All Cells**.
- Notebook 03 runs on the free simulator by default. Enable the hardware submission only if you have IonQ access and intend to spend credits.
- Hardware jobs are queued and may take from minutes to hours. You do not need to keep the notebook open — the job continues on the backend, and its results can be retrieved later from the qBraid **Jobs** panel or by job ID.

## Notes and Troubleshooting

- **Import or kernel errors immediately after launch** usually mean the environment is still installing. Wait for it to finish, then select the tutorial's kernel from the kernel menu.
- **Slow simulation.** Reduce the shot count.
- **Hardware cost and queuing.** Confirm the current device pricing on qBraid before submitting, keep the shot count modest for a first run, and expect a job to queue for some time before it executes.

## References

[1] N. Delfosse and E. Tham, *Clifford Noise Reduction*, arXiv:2407.06583 (2024).

[2] E. Tham and N. Delfosse, *Optimized Clifford Noise Reduction: Theory, Simulations and Experiments*, Quantum **9**, 1829 (2025). arXiv:2504.13356.
