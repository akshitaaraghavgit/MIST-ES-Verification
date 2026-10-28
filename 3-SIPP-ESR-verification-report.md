# 3-SIPP-ESR Core Verification Report

## 1. Overview

This report documents the verification and testing performed on the 3-SIPP-ESR RISC-V processor project. The work covered environment setup, toolchain identification, architecture tests, regression tests, and benchmark preparation/execution.

The objective was to evaluate functional correctness using the available simulation and verification workflows and document the scope and limitations of the results.

## 2. Verification Environment

- **Operating system:** Ubuntu Linux
- **Verilator:** 5.032
- **RISC-V compiler:** `riscv64-unknown-elf-gcc` 14.2.0+19

The project was tested using the available repository scripts and verification infrastructure.

## 3. Architecture Verification

The architecture test suite was executed using:

```bash
./build.sh arch
```

**Result: 90/90 architecture tests passed.**

This indicates that all 90 tests reported by the architecture test suite completed successfully in the tested environment.

## 4. Regression Testing

The following regression targets were executed:

| Test Target | Result |
|---|---|
| `sim` | PASS |
| `cosim` | PASS |
| `rvfi` | PASS |
| `zcb` | PASS |
| `trig` | PASS |
| `debug` | PASS |
| `priv` | PASS |
| `axi` | PASS |
| `fpga` | PASS |

**Regression result: 9/9 targets reported PASS.**

These results document the outcomes reported by the project's regression scripts. They should not be interpreted as proof of physical FPGA operation.

## 5. Benchmark Testing

### 5.1 Dhrystone

The Dhrystone benchmark build and simulation workflow was used.

The build configuration included:

- Optimization: `-O2`
- Architecture: `-march=rv32imc`
- ABI: `-mabi=ilp32`
- Freestanding-style linking options: `-nostdlib`, `-fno-pic`, and `-Wl,--no-relax`

The run script was invoked with a requested run count of 2,000,000.

The runner uses simulation output checks to identify PASS, FAIL, or TIMEOUT conditions.

**Result documentation:** The benchmark was run during the testing session. Exact numerical metrics and the final run status are not reproduced in this report because the corresponding complete output was not captured here.

### 5.2 CoreMark

The CoreMark benchmark build and simulation workflow was also used.

The build configuration included:

- Optimization: `-O2`
- Architecture: `-march=rv32im`
- ABI: `-mabi=ilp32`
- Options: `-nostdlib`, `-fno-pic`, `-Wl,--no-relax`, and `-fno-builtin`

The run script was configured for 100 iterations with a simulation cycle limit of 200,000,000.

The runner checks simulation output for PASS, FAIL, and TIMEOUT conditions.

**Result documentation:** Benchmark metrics were collected during the testing session, but the exact numerical output and final status are not reproduced here. No specific CoreMark score or performance-per-MHz value is claimed in this report.

### 5.3 Benchmark Interpretation

Benchmark scores depend on the core configuration, compiler and flags, simulation setup, iteration count, and measurement method. Results should only be compared when these conditions are sufficiently consistent.

The benchmark configuration alone does not establish a verified performance score. Exact scores should be reported alongside the corresponding successful run output and configuration.

## 6. Scope and Limitations

The following items are outside the scope of the results documented here:

- Embench execution and results
- FPGA synthesis resource reports, including LUT, flip-flop, BRAM, and DSP utilization
- FPGA timing analysis, including Fmax and critical-path measurements
- Physical FPGA board testing

No claims are made that these activities were completed.

## 7. Overall Summary

The documented verification results are:

- **Architecture tests:** 90/90 passed
- **Regression targets:** 9/9 reported PASS
- **Dhrystone:** Run attempted; exact metrics and final status are not reproduced here
- **CoreMark:** Run configured and metrics collected during the session; exact metrics and final status are not reproduced here
- **Embench, synthesis, timing, and physical-board validation:** Not included in this report

The architecture and regression results provide evidence of successful completion of the reported test suites in the available environment. Benchmark scores and hardware-level performance remain separate measurements and should be documented only when supported by their corresponding output reports.

---
*Report prepared for the 3-SIPP-ESR verification record.*
