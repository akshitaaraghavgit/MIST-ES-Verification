# MIST-ES RISC-V Core — Verification Report

## 1. Project Overview

This report documents the functional verification, feature-specific testing, architecture testing, and benchmark evaluation performed on the MIST-ES RISC-V processor project.

**Project:** MIST-ES  
**Repository:** https://github.com/OR5-LABS/MIST-ES  
**Verification environment:** Ubuntu Linux  
**RTL simulator:** Icarus Verilog  
**Benchmark simulator:** Verilator  
**Compiler optimization:** `-O3`  
**Automated regression status:** 14/14 targets passed

### Objectives

- Execute the existing functional verification suite.
- Test the available processor features and interfaces.
- Execute the configured RISC-V ISA tests.
- Run CoreMark and Dhrystone benchmarks.
- Record performance measurements and identify verification limitations.

## 2. Verification Environment

The verification process used the repository's existing RTL, testbenches, build scripts, and benchmark infrastructure.

The tools and components included:

- RISC-V GCC cross-compiler for target program compilation.
- Icarus Verilog for RTL simulation.
- Verilator for benchmark simulation.
- Reference-model co-simulation.
- RVFI and debug verification flows.
- RISC-V ISA and architecture test infrastructure.
- CoreMark and Dhrystone benchmark programs.

The benchmark measurements documented below correspond to the recorded `-O3` runs.

## 3. Automated Regression Testing

The repository's automated regression suite was executed using:

```bash
./run_tests.sh
```

All 13 configured targets passed their recorded regression checks.

| No. | Test | Result |
|---:|---|---|
| 1 | Basic simulation | PASS |
| 2 | Co-simulation | PASS |
| 3 | RVFI verification | PASS |
| 4 | Debug verification | PASS |
| 5 | Physical Memory Protection (PMP) | PASS |
| 6 | Enhanced PMP (ePMP) | PASS |
| 7 | User-privilege functionality (UPriv) | PASS |
| 8 | MML verification | PASS |
| 9 | ECC verification | PASS |
| 10 | AXI-Lite interface | PASS |
| 11 | FPGA build flow | PASS |
| 12 | RISC-V ISA tests | PASS |
| 13 | CoreMark and Dhrystone benchmark scripts | PASS |

### 3.1 Co-simulation Results

The co-simulation flow reported matching instruction-retirement counts between the RTL and reference model.

| Metric | Result |
|---|---:|
| RTL retired instructions | 142 |
| Reference-model retired instructions | 142 |
| Comparison result | MATCH |

This confirms agreement for the tested workload. It does not establish exhaustive correctness for every possible instruction sequence.

### 3.2 Interpretation

A passing test indicates that the corresponding repository flow satisfied its recorded regression criteria. The results demonstrate successful execution of the configured tests, not exhaustive verification of every processor feature or configuration.

## 4. Benchmark Evaluation — `-O3`

CoreMark and Dhrystone were executed using the repository's benchmark infrastructure.

The measurements below are from the recorded runs configured with `-O3`. They represent simulation-based results, not physical FPGA measurements.

### 4.1 CoreMark

**Benchmark:** CoreMark  
**Iterations:** 1,000  
**Optimization:** `-O3`  
**Functional validation:** PASS

| Metric | Result |
|---|---:|
| Iterations | 1,000 |
| Total benchmark ticks | 794,537,062 |
| Average ticks per iteration | 794,537.1 |
| Simulation cycles (`sim_cycles`) | 794,603,595 |
| Benchmark cycles (`bench_cycles`) | 794,537,062 |
| CoreMark/MHz | 1.2586 |
| CRC / functional validation | PASS |

**Measured performance: 1.2586 CoreMark/MHz.**

CoreMark evaluates embedded processor performance using several workloads, including list processing, matrix operations, state-machine processing, and CRC calculations.

The normalized CoreMark/MHz result can support comparisons between implementations when benchmark versions, compiler configurations, processor settings, and measurement methods are comparable.

### 4.2 Dhrystone

**Benchmark:** Dhrystone  
**Number of runs:** 100,000  
**Optimization:** `-O3`  
**Benchmark validation:** PASS

| Metric | Result |
|---|---:|
| Number of runs | 100,000 |
| Reported execution cycles | 146,100,066 |
| Retired instructions | 49,200,029 |
| Cycles per instruction (CPI) | 2.969 |
| Dhrystones/s/MHz | 684 |
| DMIPS/MHz | 0.389 |
| Simulation cycles (`sim_cycles`) | 146,245,593 |
| Benchmark status | PASS |

**Measured performance: 684 Dhrystones/s/MHz and 0.389 DMIPS/MHz.**

CPI represents the average number of execution cycles per retired instruction for this particular benchmark run. It should not be interpreted as a universal CPI for all workloads.

The execution-cycle count and simulation-cycle count are recorded separately because the benchmark output reports different values for these fields.

### 4.3 Benchmark Comparison and Interpretation

The two benchmarks provide complementary performance information.

- **CoreMark/MHz** measures performance across a collection of embedded-C workloads.
- **Dhrystones/s/MHz** and **DMIPS/MHz** describe normalized Dhrystone performance.
- **CPI** indicates average instruction execution cost for the measured Dhrystone workload.
- **CRC validation** checks the functional correctness of the CoreMark execution.

The recorded results establish a performance baseline for the tested configuration. They do not directly establish maximum clock frequency, power consumption, or physical FPGA performance.

## 5. Architecture Testing

The repository's architecture-test flow was executed using:

```bash
make arch
```

The initial run encountered an assembler error involving the `la x0,5b` instruction sequence in the `jalr-01` test.

A workaround was applied to the architecture-test environment, after which the test was reported to pass.

This modification must be documented for reproducibility. The final architecture-test totals should be recorded from the final run's summary. Because the test environment was modified, these results should not be described as an entirely unmodified upstream architecture-test run.

## 6. Build and Configuration Verification

The following repository flows passed the automated regression checks:

- Basic simulation.
- Co-simulation.
- RVFI.
- Debug.
- PMP and ePMP.
- User-privilege functionality.
- MML.
- ECC.
- AXI-Lite.
- FPGA build flow.

These results show that the tested repository flows completed successfully in the configured environment.

Passing the FPGA build target alone does not prove successful programming or operation on physical FPGA hardware.

## 7. Limitations and Considerations

1. **Simulation scope:** The results cover the workloads and configurations tested by the available repository infrastructure.
2. **Co-simulation scope:** Matching retirement counts demonstrate agreement for the tested workload, not exhaustive ISA correctness.
3. **Architecture-test workaround:** The `jalr-01` assembler issue and the modification to the test environment must be preserved in the verification record.
4. **Benchmark reproducibility:** Compiler versions, complete compiler flags, simulator versions, processor configuration, and raw logs should accompany the benchmark results.
5. **FPGA validation:** No physical-board validation is claimed by these results.
6. **Timing analysis:** No maximum operating frequency is claimed without an appropriate timing-analysis result and implementation constraints.
7. **Formal verification:** Passing the available automated suite does not establish complete formal verification or prove the absence of all functional bugs.

## 8. Conclusion

All 13 targets in the MIST-ES automated regression suite passed their recorded checks.

The completed work covers basic simulation, reference-model co-simulation, RVFI and debug flows, memory-protection and privilege-related tests, ECC, AXI-Lite, the configured RISC-V ISA suite, the FPGA build flow, and CoreMark and Dhrystone benchmark execution.

The recorded benchmark results under `-O3` were:

- **CoreMark:** 1.2586 CoreMark/MHz.
- **Dhrystone:** 684 Dhrystones/s/MHz.
- **Dhrystone:** 0.389 DMIPS/MHz.
- **Dhrystone CPI:** 2.969.

These measurements provide a baseline for future performance analysis. Preserving the raw logs, complete compiler flags, tool versions, and architecture-test workaround will make the results more reproducible and useful for future evaluation.
