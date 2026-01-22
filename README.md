# MPI Google PageRank - HPC Programming Project

This repository contains the implementation of a parallelized Google PageRank algorithm using MPI, as part of the *High Performance Computing and Optimization* course at Technische Universität Bergakademie Freiberg.

## Project Description

The goal of this project is to compute the PageRank vector and the largest eigenvalue of a left stochastic matrix using the power iteration method, applied to both real and randomly generated web graphs. 
 - The implementation uses **MPI (Message Passing Interface)** to parallelize the computation and evaluate performance through:
    - **Strong scaling** (fixed problem size)
    - **Weak scaling** (problem size scales with number of processes)
A more detailed description of strong/weak scaling results and execution instructions on the HPC cluster is available in **`report_hpc.pdf`**.
---
## Repository Structure

```text
.
├── MPI_GooglePageRank.cpp      # Main MPI PageRank implementation (power iteration + timing)
├── README.md                   # Project overview + build/run instructions
├── Test_GooglePageRank.cpp     # Small 6×6 validation test for correctness
├── pbs_err.txt                 # Sample PBS standard error log from cluster run
├── pbs_out.txt                 # Sample PBS standard output log from cluster run
├── report_hpc.pdf              # Full report: theory + scaling results + run instructions
├── script.pbs                  # PBS job submission script (cluster execution)
└── task_hpc_mpi.pdf             # Original project task sheet (assignment description)
```

---

## Compilation Instructions

Compile the main code using the MPI C++ compiler:

```bash
mpic++ -o hpc_code.out MPI_GooglePageRank.cpp
```

---

## Running the Code

You can execute the compiled binary as follows:

```bash
mpirun -np <num_procs> ./hpc_code.out <MATRIX_SIZE>
```

Where:
- `<num_procs>` is the number of MPI processes
- `<MATRIX_SIZE>` is the size of the web graph matrix (e.g., 10000)

---

## PBS Job Script

To run the code on an HPC cluster using PBS:

```bash
qsub script.pbs
```

**PBS Script Overview:**
- Queue: `teachingq`
- Resources: 1 node, 60 CPUs, 248 GB RAM
- Matrix size: 10000×10000
- Wall time: 5 minutes
- Output: Written to `pbs_out.txt`

---

## Sample Output

**Test Case (6×6 Matrix):**
- Final PageRank vector: `[0.0238, 0.0238, 0.2778, 0.0952, 0.2143, 0.3651]`
- Final Rayleigh Quotient: `1.000000`

**Real Case (10000×10000 Matrix, 60 processors):**
- Final Rayleigh Quotient: `1.000000`
- Execution Time: `0.460119 seconds`

---

## Performance Scaling

### Strong Scaling (Fixed Problem Size(10000×10000))

| Processors | Execution Time (s) |
|------------|--------------------|
| 1          | 18.04              |
| 2          | 9.03               |
| 4          | 4.75               |
| 12         | 1.91               |
| 60         | 0.46               |

### Weak Scaling (Matrix size ∝ Processors)

| Matrix Size | Processors | Execution Time (s) |
|-------------|------------|--------------------|
| 1291×1291   | 1          | 0.30               |
| 2581×2581   | 4          | 0.33               |
| 10000×10000 | 60         | 0.47               |

---

## Theoretical Background

The PageRank algorithm is based on the eigenvalue problem of a left stochastic matrix `P` derived from a web link matrix `L`. The power iteration method is used to compute the dominant eigenvector (PageRank vector), satisfying:

```
Pr = λ_max * r
```

Where:
- `r`: PageRank vector
- `λ_max`: Approximate dominant eigenvalue (≈1)

---

## Testing

- Random reproducible test cases for `n = 100`, `1000`, `10000` using seeded generation.
- Validation against summation conditions (∑ri = 1, ri ≥ 0).
- Visual inspection of the final `P` matrix and `r` vector for test cases.

---
