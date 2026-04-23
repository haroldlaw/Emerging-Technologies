# Emerging Technologies

Quantum computing coursework project implementing and analysing the **Deutsch** and **Deutsch-Jozsa** algorithms using [Qiskit](https://qiskit.org/) and the Aer simulator — no real quantum hardware required.

## Project Overview

This project explores one of the earliest demonstrations that quantum computers can solve certain problems exponentially faster than any classical computer. The Deutsch-Jozsa algorithm determines whether a binary function is *constant* (always 0 or always 1) or *balanced* (half 0, half 1) with a **single query**, whereas the best classical deterministic approach requires up to 2ⁿ⁻¹ + 1 queries.

The notebook walks through the problem from first principles — starting with purely classical Python, building intuition for quantum oracles, and culminating in a full Qiskit implementation that runs on a statevector simulator.

## Key Features

- **Classical baseline** — enumerate and randomly generate Boolean functions; measure how many queries classical algorithms need
- **Quantum oracles** — build constant and balanced oracles as `QuantumCircuit` objects using standard gate decompositions
- **Deutsch's algorithm** — single-qubit version implemented and verified against all four possible single-bit functions
- **Deutsch-Jozsa algorithm** — scaled to n-bit inputs; tested on constant and balanced oracles of varying width
- **Simulation** — all circuits run through `qiskit_aer.AerSimulator`; results visualised with `matplotlib`

## Quick Start

```bash
git clone https://github.com/haroldlaw/Emerging-Technologies.git
cd Emerging-Technologies
pip install -r requirements.txt
jupyter notebook problems.ipynb
```

Open `problems.ipynb` and run all cells from top to bottom (**Kernel → Restart & Run All**).

## Setup

It is recommended to use a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

## Quick Verification

After installation, confirm Qiskit is working:

```python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator

qc = QuantumCircuit(1, 1)
qc.h(0)
qc.measure(0, 0)
result = AerSimulator().run(qc, shots=1024).result()
print(result.get_counts())  # {'0': ~512, '1': ~512}
```

## Project Structure

```
Emerging-Technologies/
├── problems.ipynb      # Main notebook — all five problems
├── requirements.txt    # Python dependencies
├── README.md           # This file
└── .gitignore
```

### Notebook Problems

| # | Title | Description |
|---|-------|-------------|
| 1 | **Generating Random Boolean Functions** | Create and enumerate all Boolean functions f: {0,1}ⁿ → {0,1}; classify as constant or balanced |
| 2 | **Classical Testing** | Implement classical oracle-query algorithms; count queries needed to determine function type |
| 3 | **Quantum Oracles** | Construct constant and balanced quantum oracles using X, CNOT, and Toffoli gates |
| 4 | **Deutsch's Algorithm** | Implement the 1-bit version with Hadamard gates; verify with Aer simulation |
| 5 | **Deutsch-Jozsa Algorithm** | Generalise to n-bit inputs; demonstrate single-query quantum advantage over classical |

## Algorithm Background

### Deutsch's Problem
Given a function f: {0,1} → {0,1}, determine whether it is constant or balanced using as few evaluations as possible.

- **Classical**: requires up to 2 queries (worst case)
- **Quantum (Deutsch)**: always 1 query

### Deutsch-Jozsa Problem
Given f: {0,1}ⁿ → {0,1} promised to be either constant or balanced, determine which with minimum queries.

- **Classical (deterministic)**: requires up to 2ⁿ⁻¹ + 1 queries
- **Quantum (Deutsch-Jozsa)**: always 1 query — exponential speedup

### Oracle Construction
Oracles are implemented as unitary `QuantumCircuit` operations on n + 1 qubits. The ancilla qubit is initialised in the |−⟩ state via an X and H gate before the oracle, enabling phase kickback — the mechanism that encodes the function's type in the phase of the query register.

## Educational Content

The notebook is structured to build understanding incrementally:

1. **Classical intuition first** — Problems 1 and 2 establish what a Boolean function is and how a classical algorithm queries it
2. **Bridge to quantum** — Problem 3 introduces oracles as quantum circuits, showing the gate-level construction for each function type
3. **Core algorithm** — Problems 4 and 5 implement and verify the quantum speedup, with circuit diagrams and simulation output

Key quantum concepts covered:
- Superposition and the Hadamard transform
- Phase kickback
- Quantum oracle (black-box) model
- Measurement and basis states

## Requirements

See `requirements.txt`. Core dependencies:

| Package | Purpose |
|---------|---------|
| `qiskit` | Quantum circuit construction and transpilation |
| `qiskit-aer` | Local statevector/shot-based simulation |
| `numpy` | Numerical operations |
| `matplotlib` | Circuit diagrams and result plots |
| `jupyterlab` / `notebook` | Running the notebook |

Python 3.10 or later is recommended.

## Contributing

This is a coursework submission. Feedback and discussion are welcome via GitHub Issues.

## License

This repository is for educational purposes.

## References

- Deutsch, D. (1985). *Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer*. Proceedings of the Royal Society A.
- Deutsch, D. & Jozsa, R. (1992). *Rapid Solution of Problems by Quantum Computation*. Proceedings of the Royal Society A.
- Nielsen, M. A. & Chuang, I. L. (2010). *Quantum Computation and Quantum Information*. Cambridge University Press.
- [Qiskit Documentation](https://docs.quantum.ibm.com/)
- [Qiskit Textbook — Deutsch-Jozsa Algorithm](https://learning.quantum.ibm.com/tutorial/deutsch-jozsa-algorithm)

## Educational Impact

The Deutsch-Jozsa algorithm was historically significant as the first proof that quantum computers can solve a well-defined problem *provably* faster than any classical deterministic computer. While the problem itself is artificial, it introduced the **oracle model** and **phase kickback** — concepts central to Grover's search algorithm, Shor's factoring algorithm, and modern quantum speedup results.