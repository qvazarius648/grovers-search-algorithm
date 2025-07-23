# Grover's Search Algorithm in Qiskit

A clean, well-documented Python implementation of Grover's search algorithm using Qiskit. This project demonstrates how to efficiently find one or more marked items in an unstructured quantum database, showcasing a fundamental quantum advantage.

![Grover Search Results](search\result_plot.png)

## Key Features

- **Multi-Target Search:** Capable of searching for multiple target states simultaneously.
- **Pure Phase Oracle:** Implemented using a phase-flip oracle that requires no ancilla qubits, making it more resource-efficient.
- **Automatic Iteration Calculation:** Automatically determines the optimal number of Grover iterations for the highest success probability.
- **Modular Code:** Structured with clear, single-responsibility functions for the oracle, diffuser, and circuit assembly, making it easy to read and understand.

---

## How It Works

Grover's algorithm provides a quadratic speedup over classical algorithms for unstructured search problems. The magic happens by repeating two key steps to amplify the probability amplitude of the target states.

This implementation closely follows the methodology described in *Fundamentals of Quantum Information* by Hiroyuki Sagawa and Tomahiro Yoshida.

### 1. The Oracle (`U_f`)

The first step is to "mark" the items we're looking for. The oracle is a quantum operator that selectively applies a negative phase (`-1`) to the target states while leaving all other states unchanged.

- If `|x⟩` is a target state: `U_f |x⟩ = -|x⟩`
- If `|x⟩` is not a target state: `U_f |x⟩ = |x⟩`

This phase difference is invisible if you measure right away, but it's the key ingredient for the next step.

### 2. The Diffuser (`D`)

The second step is the amplifier, also known as the "inversion about the mean" operator. This operator takes the marked states and dramatically increases their amplitudes. It's constructed as follows:

`D = H^n * U_0 * H^n`

- `H^n`: A Hadamard gate applied to every qubit, which creates a uniform superposition.
- `U_0`: A special oracle that only marks the `|00...0⟩` state.

When combined, these gates create an operator that reflects every state's amplitude about the average amplitude. This clever trick causes destructive interference for non-target states and constructive interference for the target states, rapidly boosting their measurement probability.

### 3. Iteration

A single Grover operator is the sequence of these two steps: `G = D * U_f`.

This operator `G` is applied `k` times to the initial uniform superposition. The optimal number of iterations `k` is calculated to maximize the probability of measuring a target state:

`k ≈ (π / 4) * sqrt(N / M)`

where `N` is the total number of states (`2^n`) and `M` is the number of target states.

---

## Installation & Requirements

To run this project, you'll need Python 3 and the following libraries. It's recommended to set up a virtual environment.

1.  Create a `requirements.txt` file with the following content:
    ```txt
    qiskit>=1.0.0
    qiskit-aer
    matplotlib
    numpy
    ```

2.  Install the dependencies:
    ```bash
    pip install -r requirements.txt
    ```

---

## Usage

The script is ready to run out of the box.

1.  To execute the search, simply run the Python script from your terminal:
    ```bash
    python your_script_name.py
    ```

2.  To search for different states, modify the `target_keys` list inside the `main()` function at the bottom of the script:
    ```python
    def main():
        # Define the states to find
        target_keys = ['101', '010'] # Example: searching for two 3-qubit states
        
        # ... rest of the code
    ```

The script will automatically calculate the required number of qubits and optimal iterations based on your input.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
