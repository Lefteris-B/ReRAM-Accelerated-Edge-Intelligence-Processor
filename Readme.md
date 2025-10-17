# ReRAM-Enhanced RISC-V Neuromorphic Processor for Adaptive Edge Intelligence

## Project Overview
We propose a novel System-on-Chip that combines a compact RISC-V processor with a ReRAM-based neuromorphic accelerator, leveraging BM Labs' NVM IP to enable persistent, ultra-low-power machine learning at the edge. Our design targets critical applications in medical monitoring, automotive safety, and space exploration where power efficiency and reliability are paramount.

## Key Innovation
Our design uniquely exploits ReRAM's non-volatility and in-memory computing capabilities to create an adaptive edge processor that:

- Persistent Intelligence: Neural network weights and learned patterns remain in ReRAM through power cycles, eliminating reload overhead
- Hybrid Architecture: RISC-V core for flexibility + dedicated ReRAM accelerator for efficiency
- Adaptive Learning: On-chip weight updates using ReRAM's write capabilities for continuous adaptation
- Multi-Model Storage: Store multiple neural networks (32x32 weight matrices) and dynamically switch based on context
- Sub-milliwatt Operation: Aggressive power gating enabled by non-volatile weight storage

## Technical Approach
### System Architecture
```
┌─────────────────────────────────────────────────────────┐
│                    SoC Top Level                        │
├─────────────────────┬───────────────────────────────────┤
│   RV32I Core        │     ReRAM Neural Accelerator      │
│   (Controller)      │  ┌────────────────────────────┐   │
│                     │  │  NVM IP (32x32 Crossbar)   │   │
│   ┌──────────┐      │  ├────────────────────────────┤   │
│   │   I$     │      │  │    MAC Array (8 units)     │   │
│   └──────────┘      │  ├────────────────────────────┤   │
│                     │  │   Activation Functions     │   │
│   TileLink Bus      │  └────────────────────────────┘   │
├─────────────────────┴───────────────────────────────────┤
│  SRAM Scratchpad    │  SPI/I2C  │  GPIO  │  Timer       │
└─────────────────────────────────────────────────────────┘
```
### Implementation Strategy

1. **Base Platform**: Berkeley Chipyard framework with Rocket RISC-V core
2. **Accelerator Integration**: Custom RoCC (Rocket Custom Coprocessor) interface
3. **NVM Integration**: BM Labs' IP integrated as memory-mapped peripheral
4. **Verification**: Chipyard's existing test infrastructure + custom ReRAM tests
5. **Physical Design**: Hammer flow targeting SKY130 PDK
6. **Final Integration**: GDS generation with optional Caravel wrapper

## Leveraging Berkeley Chipyard Framework
### Why Chipyard is Critical to Our Success
Our project extensively leverages the Berkeley Chipyard agile hardware development framework, enabling rapid SoC development through:
Generator-based design methodology for parameterizable hardware
FIRRTL compiler infrastructure for design optimization and instrumentation
RoCC (Rocket Custom Coprocessor) interface for clean accelerator integration
TileLink interconnect for coherent memory system design
Hammer unified physical design flow for SKY130 tapeout
FireSim integration for FPGA prototyping and validation
RISC-V toolchain for software development and testing

### ReRAM Utilization Strategy

- **Weight Storage**: 32x32 crossbar arrays for dense neural network weights
- **Compute-in-Memory**: Analog MAC operations directly in ReRAM crossbar
- **Multi-Bank Design**: Parallel access to different weight sets
- **Wear Leveling**: Rotation scheme for write operations to extend lifetime
- **Power Optimization**: Selective bank activation based on workload

## Target Applications

### Primary: Adaptive Medical Anomaly Detection
- **Use Case**: Continuous ECG/EEG monitoring with personalized models
- **ReRAM Advantage**: Patient-specific patterns persist across device resets
- **Power Target**: <100μW average with event-driven processing
- **Adaptation**: On-device learning of individual baseline patterns

### Secondary: Automotive Predictive Maintenance
- **Use Case**: Real-time vibration analysis for component failure prediction
- **ReRAM Advantage**: Learned failure signatures retained without power
- **Value**: Early detection prevents catastrophic failures

### Tertiary: Radiation-Hardened Space Classification
- **Use Case**: Satellite image classification with radiation tolerance
- **ReRAM Advantage**: Inherent radiation resistance of ReRAM technology

## Development Milestones

| Date | Milestone | Deliverable |
|------|-----------|-------------|
| Oct 18-20 | Environment Setup | Chipyard + SKY130 PDK configured, NVM IP integrated |
| Oct 21-23 | Core Implementation | RISC-V core + basic RoCC accelerator functional |
| Oct 24-26 | ReRAM Integration | NVM IP connected, basic read/write operations |
| Oct 27-29 | Neural Functions | MAC array, activation functions, inference working |
| Oct 30-31 | Verification | Complete testbench suite, functional validation |
| Nov 1-2 | Physical Design | Hammer flow complete, DRC/LVS clean |
| Nov 3 | Final Submission | GDS files, documentation, video demonstration |

## Verification Plan

- **Unit Tests**: Individual verification of MAC units, NVM interface, control logic
- **Integration Tests**: Full inference path validation with known neural networks
- **Power Analysis**: Measurement of power consumption in different operating modes
- **Reliability Tests**: Retention time, endurance cycling, power-on reset behavior
- **Application Benchmarks**: MNIST digit recognition, ECG arrhythmia detection

## Team Background

- **Experience**: Extensive background with Berkeley Chipyard framework and RISC-V design
- **Relevant Skills**: SoC integration, hardware verification, neural network acceleration
- **Tool Proficiency**: Chisel/Verilog RTL, Hammer physical design flow, FIRRTL transforms

## Risk Mitigation

Given the tight timeline, we've designed fallback positions:

1. **Minimum Viable**: RISC-V + ReRAM as simple weight storage (no in-memory compute)
2. **Expected Delivery**: Above + MAC acceleration with ReRAM weights
3. **Stretch Goal**: Full neuromorphic processing with on-chip learning

## Open Source Commitment

- All RTL code will be released under Apache 2.0 license
- Complete documentation for reproduction
- Chipyard configuration files and Hammer scripts included
- Full AI/LLM prompt logs will be documented if any assistance tools are used

