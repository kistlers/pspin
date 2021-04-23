<div align="center"><img src="overview.svg" alt="PsPIN architecture overview" /></div>

# PsPIN: A RISC-V in-network accelerator for flexible high-performance low-power packet processing

PsPIN [1] is an implementation of the sPIN programming model [2] based on PULP [3]. This repository includes the RTL code implementing PsPIN, the runtime software, and a set of examples to get started. We provide a toolchain that allows to define, build, and test new handlers through cycle-accurate simulations. 

**Workflow summary:** The RTL code is verilated into C++ modules that are compiled together with the functional models into two libraries: `libpspin.so` and `libpspin_debug.so`. To write your own handler, you need to define the handler code and a simulation driver. The handler code must be compiled with RISC-V GCC (link here). The simulation driver interfaces to `libpspin.so` for (1) initializing the simulation; (2) defining the content of L2 handler memory; (3) defining the handlers to offload; (4) defining and injecting packets to process; (5) handle events generated by the execution of the handlers (e.g., packets being sent or writes/reads to/from host memory). By linking against `libpspin_debug.so` you make the simulation a `waves.vcd` that can be explored with any value-change-dump editor (e.g., GTKWave http://gtkwave.sourceforge.net/).

**Repo organization:** The repositority has the following structure:

 - `hw/`: Hardware components and simulation logic.
   - `hw/deps/`: (RTL) Dependencies from the PULP platform (https://github.com/pulp-platform). Some of them have been adapted to fit in the PsPIN design. **License:** SolderPad 0.51.
   - `hw/src/`: (RTL) PsPIN components. **License:** SolderPad 0.51.
   - `hw/verilator_model/`: (functional) Components implementing the NIC model shown by the above figure. **License:** Apache 2.0.
 - `sw/`: Software components. **License:** Apache 2.0.
   - `sw/pulp-sdk/`: Dependencies from the PULP SDK adapted to fit the PsPIN design. 
   - `sw/rules/`: Makefile rules used to ease simulutions setups and runs. 
   - `sw/runtime/`: HPUs runtime code and support functions for the handlers. 
   - `sw/script/`: utilities for extracting data from the simulation output. 
 - `examples/`: Examples of sPIN handlers. **License:** Apache 2.0.
    - `examples/*/driver/`. Simulation driver.
    - `examples/*/handlers/`. Handlers code.

## Dependencies
 - Verilator > v4.100 (https://www.veripool.org/wiki/verilator)
 - RISC-V GCC toolchain (source: https://github.com/pulp-platform/pulp-riscv-gnu-toolchain; prebuilt: link here)

## Getting started
Here is a walkthrough to setup the simulation and run example handlers. 

### 1. Setting up the environment 
 

### 2. Verilating the hardware
```
cd hw/verilator_model/
```

Compile `libpspin.so`:
```
make release
```

(Optional) Compile `libpspin_debug.so`:
```
make debug
```


### 3. Compile handlers and simulation driver

### 4. Run! 

### 5. Check data


## Citation

Please include this citation if you use this work as part of your project:

```
@inproceedings{pspin,
	title={A RISC-V in-network accelerator for flexible high-performance low-power packet processing},
	author={Di Girolamo, Salvatore and Kurth, Andreas and Calotoiu, Alexandru and Benz, Thomas and Schneider, Timo and Beranek, Jakub and Benini, Luca and Hoefler, Torsten},
	booktitle={2021 ACM/IEEE 48th Annual International Symposium on Computer Architecture (ISCA)},
	year={2021}
}
```

## References

[1] Di Girolamo Salvatore, Kurth Andreas, Calotoiu Alexandru, Benz Thomas, Schneider Timo, Beranek Jakub, Benini Luca, Hoefler Torsten. "A RISC-V in-network accelerator for flexible high-performance low-power packet processing." 2021 ACM/IEEE 48th Annual International Symposium on Computer Architecture (ISCA). IEEE, 2021. 

[2] Hoefler Torsten, Salvatore Di Girolamo, Konstantin Taranov, Ryan E. Grant, and Ron Brightwell. "sPIN: High-performance streaming Processing in the Network." In Proceedings of the International Conference for High Performance Computing, Networking, Storage and Analysis, pp. 1-16. 2017.

[3] Rossi, Davide, Francesco Conti, Andrea Marongiu, Antonio Pullini, Igor Loi, Michael Gautschi, Giuseppe Tagliavini, Alessandro Capotondi, Philippe Flatresse, and Luca Benini. "PULP: A parallel ultra low power platform for next generation IoT applications." In 2015 IEEE Hot Chips 27 Symposium (HCS), pp. 1-39. IEEE, 2015.
