# 4x4 INT8 Systolic Array Accelerator

A weight-stationary 4x4 INT8 systolic array for matrix multiplication, written in Verilog.

## Design

- **Weight-stationary dataflow:** weights are loaded once into each processing element (PE); activations flow across the rows and partial sums accumulate in place.
- **Processing element (`pe_mac.v`):** holds a weight register and a signed 32-bit accumulator, and passes activations, weights and a valid bit to its neighbour every cycle.
- **SIMD MAC (`simd_unit.v`):** a 4-lane INT8 multiply-accumulate datapath shared across the PE to save area.
- **Array (`systolic_array_4x4.v`):** 16 PEs with `load_weight`, `clear_acc` and `valid_in` controls.

This array is also integrated with an AXI4-Lite slave and a 2x2 round-robin crossbar in the [7-Stage-RV32I-Core](https://github.com/lalithkc/7-Stage-RV32I-Core) SoC.

## Files

| File | Purpose |
|---|---|
| `systolic_array_4x4.v` | Top-level 4x4 array |
| `pe_mac.v` | Processing element |
| `simd_unit.v` | 4-lane INT8 MAC |
| `tb_systolic_array.v` | Basic array testbench |
| `tb_systolic_matmul.v` | Matrix-multiply testbench, checks results against expected values |

## Running the simulation

```sh
iverilog -o sim tb_systolic_matmul.v systolic_array_4x4.v pe_mac.v simd_unit.v
vvp sim
```
