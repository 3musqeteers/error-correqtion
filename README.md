# error-correqtion

This notebook includes a function called five_qubit_error_code_circuit that receives as its input an initial state $x \in \{0, 1\}$ and a probability $p \in [0,1]$. The function returns a circuit which prepares a fault-tolerant 5-qubit logical qubit for $x$, runs each of the 5 qubits through a random gate which is sampled to be identity with probability $1-3p$ and one of the three Pauli gates each with probability $p$, then attempts to correct the error, if any. Then the notebook does a simple analysis of the probability that the error is successfully corrected, which is the case in particular if one or fewer Pauli gates were sampled.

### Usage:

```python
# Create the circuit for the five-qubit error correction code
circuit = five_qubit_error_code_circuit(initial_state, probability)

# Create a simulator backend and run the circuit to test if it works correctly
simulator = AerSimulator()

# Transpile the circuit for the backend
compiled_circuit = transpile(circuit, simulator)

# Run the circuit
job = simulator.run(compiled_circuit, shots=1)

# Save the statevector
result = job.result()
data = result.data()

# Test the statevector for equality to the initial state
code_value_test(data['statevector'], initial_state)
```

### Gate input

* First $4$ qubits: syndrome qubits, to be measured during the run
* Next $5$ qubits: physical qubits encoding one logical qubit

### Reference

This 5-qubit code is based on [R. Laflamme et al.](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.77.198) ([arXiv](https://arxiv.org/abs/quant-ph/9602019)) and [C. H. Bennett at al.](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.54.3824) ([arXiv](https://arxiv.org/abs/quant-ph/9604024)). Our convention follows [S. J. Devitt et al.](https://iopscience.iop.org/article/10.1088/0034-4885/76/7/076001) ([arXiv](https://arxiv.org/abs/0905.2794)) and the [Wikipedia article](https://en.wikipedia.org/wiki/Five-qubit_error_correcting_code) on the topic.
