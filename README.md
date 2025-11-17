# Grover's Algorithm Implementation in Qiskit

This project is an implementation of Grover's quantum search algorithm for $n=4$ qubits, built and executed using Qiskit. The primary goal was to successfully build the circuit from its core components and compare its performance on a local ideal simulator versus real, noisy IBM quantum hardware.

The marked state for this search is **`|1010>`** (binary for 10), as defined in the `phase_oracle` function.

##  Key Components

The algorithm is built from two main unitary operators:

1.  **Phase Oracle ($U_f$)**: This operator identifies the marked state. It applies a phase shift of -1 to the marked state, leaving all other states unchanged. This is implemented by applying a C-Z gate (or an equivalent `mcx` on an auxiliary qubit) controlled by the marked state.
2.  **Diffuser ($V$)**: This operator, also known as the "Grover diffusion operator," amplifies the amplitude of the marked state. It performs a reflection about the average amplitude of all states, which systematically increases the probability of measuring the correct answer.



##  Execution Workflow

1.  **Circuit Construction**: The full Grover circuit is constructed by applying the Phase Oracle and the Diffuser $r$ times.
2.  **Optimal Iterations**: The optimal number of iterations, $r$, is calculated to maximize the probability of success. For $N=2^n$ items and $M=1$ solution, $r$ is:
    $$r \approx \frac{\pi}{4}\sqrt{\frac{N}{M}} = \frac{\pi}{4}\sqrt{2^4} \approx 3$$
3.  **Execution:** The circuit is run twice:
    * **`AerSimulator`**: An ideal, local simulator is used to verify the circuit's correctness.
    * **IBM Quantum Backend**: The circuit is transpiled and sent to a real IBM quantum device (e.g., `ibm_torino`) to observe its performance on noisy intermediate-scale (NISQ) hardware.

##  Results

The results clearly demonstrate the difference between ideal simulation and real-world execution.

* The **AerSimulator** (blue) perfectly isolates the marked state `1010` with nearly 100% probability, as expected from the theory.
* The **IBM Fez** backend (orange) also correctly identifies `1010` as the most probable outcome. However, the presence of noise on the device results in other, incorrect states being measured with non-trivial probability.

![alt text](image.png)

##  Tools Used

* **Python 3.11**
* **Qiskit**: For quantum circuit construction, simulation, and hardware execution.
* **Qiskit-IBM-Runtime**: For managing jobs on real IBM quantum backends.
* **NumPy**: For numerical calculations.
* **Matplotlib / Qiskit Visualization**: For plotting results.